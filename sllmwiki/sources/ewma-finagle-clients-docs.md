# Clients¶

Finagle clients adheres to a simple [interface](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Client.scala>) for construction. The following API supports the creation of both stateful and stateless clients. You want a stateful client if you require requests and response sequences over the same connection. The following illustrates the difference.

Constructor for a **stateless** client:

```
def newService(dest: Name, label: String): Service[Req, Rep]
```

That is, given a logical destination and an identifier, return a function that produces a typed *Service* over which requests can be dispatched. Dispatched requests will be load balanced across all of the resolved hosts utilizing Finagle’s configured load balancer.

An alternative constructor for a **stateful** client producing a typed *ServiceFactory*:

```
def newClient(dest: Name, label: String): ServiceFactory[Req, Rep]
```

Each *Service* acquired from the returned *ServiceFactory* represents a distinct session. Requests dispatched on this *Service* will reuse the established connection to the picked host. Load balancing occurs only per-session, *ServiceFactory.apply*, while per-request on *newService*. Depending on the configured Pooling strategy, *Service.close* typically returns its connection to the pool and does not cut the connection. As a result, it is important to close sessions after use to avoid resource leaks. For example:

```
import com.twitter.finagle.{Service, ServiceFactory}
import com.twitter.finagle.Http
import com.twitter.finagle.http.{Request, Response}
import com.twitter.util.Future

val sessionFactory: ServiceFactory[Request, Response] = Http.client.newClient("example.com:80")

// we establish a session, represented by `svc`
sessionFactory().onSuccess { svc: Service[Request, Response] =>

  // both requests will land on the same host
  val rep1: Future[Response] = svc(Request("/some/path"))
  val rep2: Future[Response] = svc(Request("/some/other/path"))

  // clean up the session so the connection is released into the pool
  svc.close()
}

// session establishment is load balanced. No guarantee as to which endpoint is selected by the load balancer
sessionFactory().onSuccess { ... }
```

## Client Protocol Implementation¶

As of [6.x](<https://twitter.github.io/finagle/guide/changelog.html>), client implementations are encouraged to expose this interface on a Scala object named after the protocol implementation. This results in a uniform way to construct clients, `Protocol.newClient(...)` or `Protocol.newService(...)`. For example:

```
import com.twitter.finagle.Service
import com.twitter.finagle.Http
import com.twitter.finagle.http.{Request, Response}

val twitter: Service[Request, Response] = Http.client.newService("twitter.com")
```

Clients can further furnish the resulting *ServiceFactory* with protocol specific API’s. A common pattern is to expose a `newRichClient` method that does exactly this. For cases like Thrift, where IDLs are part of the rich API, a more specialized API is exposed. See the protocols section on [Thrift](<https://twitter.github.io/finagle/guide/Protocols.html#thrift-and-scrooge>) for more details.

## Transport¶

Finagle clients come with a variety of transport-level parameters that not only wire up TCP socket options, but also upgrade the transport protocol to support encryption (e.g. TLS/SSL) and proxy servers (e.g. HTTP, SOCKS5).

## Transport Security¶

Finagle has robust support for TLS. The most common options such as server validation are accessible directly via the *tls* members of [ClientTransportParams](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/param/ClientTransportParams.scala>) as follows:

```
import com.twitter.finagle.{Service, Http}
import com.twitter.finagle.http.{Request, Response}

val twitter: Service[Request, Response] = Http.client
  .withTransport.tls("twitter.com")
  .newService("twitter.com:443")
```

There are further configuration options including client authentication accessible via [ClientTransportParams](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/param/ClientTransportParams.scala>).

Finagle also supports [SPNEGO](<https://en.wikipedia.org/wiki/SPNEGO>) which is an HTTP specific extension for negotiating security schemes. A common use case for SPNEGO is for authentication in [Kerberos](<https://en.wikipedia.org/wiki/Kerberos_(protocol)>) secured environments.

```
import com.twitter.finagle.{Service, Http}
import com.twitter.finagle.http.{Request, Response}
import com.twitter.finagle.http.SpnegoAuthenticator.ClientFilter
import com.twitter.finagle.http.SpnegoAuthenticator.Credentials.{ClientSource, JAASClientSource}

val jaas: ClientSource = new JAASClientSource(
  loginContext = "com.sun.security.jgss.krb5.initiate",
  _serverPrincipal = "HTTP/SOME_HOST@SOME_DOMAIN"
)

val client: Service[Request, Response] =
  new ClientFilter(jaas).andThen(Http.client.newService("host:port"))
```

### HTTP Proxy¶

There is built-in support for [tunneling TCP-based protocols](<https://tools.ietf.org/html/draft-luotonen-web-proxy-tunneling-01>) through web proxy servers in a default Finagle client that might be used with any TCP traffic, not only HTTP(S). See [Squid documentation](<https://wiki.squid-cache.org/Features/HTTPS>) on this feature.

The following example enables tunneling HTTP traffic through a web proxy server *my-proxy-server.com* to *twitter.com*.

```
import com.twitter.finagle.{Service, Http}
import com.twitter.finagle.http.{Request, Response}
import com.twitter.finagle.client.Transporter
import java.net.SocketAddress

val twitter: Service[Request, Response] = Http.client
  .withTransport.httpProxyTo(
    host = "twitter.com:443",
    credentials = Transporter.Credentials("user", "password")
  )
  .newService("inet!my-proxy-server.com:3128") // using local DNS to resolve proxy
```

While this setup may look somewhat counter intuitive with regards to where the ultimate destination and the proxy server address are applied, it enables a variety of resiliency features by utilizing Finagle’s naming and load balancing subsystems. Given a web proxy server address/name falls under a standard name resolution process, it might be (and should be) backed by a replica set (multiple hosts) to get the greatest out of a client.

Note

There is also a legacy support to web proxy servers available in Finagle via the *Transporter.HttpProxy* stack param. In that case, proxy server is forced to represented as a single *SocketAddress*, which not only introduces a single point of failure within a client (i.e., a client goes offline if a web proxy server is down), but also disables Finagle’s resiliency features such as failure detection and load balancing.

### SOCKS5 Proxy¶

SOCKS5 proxy support in Finagle is designed and implemented exclusively for testing/development (assuming that SOCKS proxy is provided via *ssh -D*), not for production usage. For production traffic, an HTTP(S) proxy should be used instead.

Use the following CLI flags to enable SOCKS proxy on every Finagle client on a given JVM instance (username and password are optional).

```
-com.twitter.finagle.socks.socksProxyHost=localhost \
-com.twitter.finagle.socks.socksProxyPort=50001 \
-com.twitter.finagle.socks.socksUsername=$TheUsername \
-com.twitter.finagle.socks.socksPassword=$ThePassword
```

## Client Modules¶

A default Finagle client is designed to maximize success and minimize latency. Each request dispatched through a client will flow through various modules that help it achieve these goals. The modules are logically separated into three stacks: the *client stack* manages *name* resolution and balances requests across multiple endpoints, the *endpoint stack* provides circuit breakers and connection pooling, and the *connection stack* provides connection life-cycle management and implements the wire protocol.

Fig. 1: A visual representation of each module in a default Finagle client that is configured with three endpoints and connections. Requests flow from left to right.

### Module Composition¶

A materialized Finagle client is a [ServiceFactory](<https://twitter.github.io/finagle/guide/ServicesAndFilters.html#service-factory>). It produces [Services](<https://twitter.github.io/finagle/guide/ServicesAndFilters.html#services>) over which requests can be dispatched. The modules in *Fig. 1* are defined in terms of a *ServiceFactory* and thus are composed via the usual [combinators](<https://twitter.github.io/finagle/guide/ServicesAndFilters.html#composing-services-filters>). An important consequence of this is that modules deeper in the stack can affect the behavior and availability of the client. For example, this is how failure management modules mark entire endpoints as unavailable.

### Observability¶

The *Observe*, *Monitor*, and *Trace* modules export useful information about the internals and behavior of a Finagle client. Client metrics are exported using a [StatsReceiver](<https://github.com/twitter/util/blob/release/util-stats/src/main/scala/com/twitter/finagle/stats/StatsReceiver.scala>) (see the [metrics](<https://twitter.github.io/finagle/guide/Metrics.html#public-stats>) section for more details about specific metric values).

Unhandled exceptions can be handled by installing a custom [MonitorFilter](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/filter/MonitorFilter.scala>) or overriding the default monitor instance. The default setting for `MonitorFilter` is to log all the unhandled exception onto standard output. To override this, use the following sample.

```
import com.twitter.finagle.Service
import com.twitter.finagle.Http
import com.twitter.finagle.http.{Request, Response}
import com.twitter.util.Monitor

val monitor: Monitor = new Monitor {
  def handle(t: Throwable): Boolean = {
    // do something with the exception
    true
  }
}

val twitter: Service[Request, Response] = Http.client
  .withMonitor(monitor)
  .newService("twitter.com")
```

Finally, clients have built-in support for [Tracing](<https://twitter.github.io/finagle/guide/Tracing.html>).

### Retries¶

Every Finagle client contains a *Retries* module in the stack, above load balancers, so that it can retry failures from the underlying modules: circuit breakers, timeouts, load balancers and connection pools. Retries can help improve the client’s logical success rate when subsequent attempts succeed.

For the most part, service owners will be interested in the logical success rate of a clients. Logical requests represent the result of the initial request, after any retries have occurred. Concretely, should a request result in a retryable failure on the first attempt, but succeed upon retry, this is considered a single successful logical request. By default, a Finagle client’s success rate metrics include the individual attempts and this can cause confusion. [MethodBuilder](<https://twitter.github.io/finagle/guide/MethodBuilder.html#methodbuilder>) offers logical metrics scoped to “logical” for both success rate and latency. The deprecated `ClientBuilder` code also offers similar metrics scoped to “tries”.

Failures that are known to be safe to retry (for example, exceptions that occurred before the bytes were written to the wire and protocol level NACKs) will be automatically retried by Finagle. These retries come out of a `RetryBudget` that allows for approximately 20% of the total requests to be retried on top of 10 retries per second in order to accommodate clients that have just started issuing requests or clients that have a low rate of requests per second.

Some failures may also be known as unsafe to retry. If a [Failure](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Failure.scala>) is flagged `NonRetryable`, the *Retries* module will not make any attempts to retry the request and pass along the failure as is. A *NonRetryable* failure may be used in situations where a client determines that a service is unhealthy and wishes to signal that the normal pattern of retries should be skipped. Additionally, a service may reject a request that is malformed and thus pointless to retry. While Finagle respects the *NonRetryable* flag internally, users should also take care to respect it when creating retry filters of their own.

The *Retries* module is configured with two parameters:

1. `RetryBudget` - determines whether there is available budget to retry a request
2. `Backoff` - the backoff 1 policy used to requeue the failed request

It’s highly recommended to use [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Backoff.scala>) instead of using the error-prone `Stream` API directly.

By default, the `RetryBudget` allows for about 20% of the total requests to be immediately (no backoff) retried on top of 10 retries per second in order to accommodate clients that have just started issuing requests or clients that have a low rate of requests per second.

To override this default use the following code snippet.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http
import com.twitter.finagle.service.{Backoff, RetryBudget}

val budget: RetryBudget = ???

val twitter = Http.client
  .withRetryBudget(budget)
  .withRetryBackoff(Backoff.const(10.seconds))
  .newService("twitter.com")
```

The following example 3 shows how to use a factory method `RetryBudget.apply` in order to construct a new instance of `RetryBudget` backed by *leaky token bucket*.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.service.RetryBudget

val budget: RetryBudget = RetryBudget(
  ttl = 10.seconds,
  minRetriesPerSec = 5,
  percentCanRetry = 0.1
)
```

The `RetryBudget` factory method takes three arguments:

1. *ttl* - a time to live for deposited tokens
2. *minRetriesPerSec* - the minimum rate of retries allowed
3. *percentCanRetry* - the percentage of requests that might be retried

While the [RequeueFilter](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/RequeueFilter.scala>) (configured via `Retries`) is inserted into every client’s stack by default so all the failures from the underlying modules will be retried, the [RetryFilter](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/RetryFilter.scala>) handling application level exceptions from the remote server should be applied explicitly.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http
import com.twitter.finagle.util.DefaultTimer
import com.twitter.finagle.service.{RetryBudget, RetryFilter, RetryPolicy}
import com.twitter.finagle.stats.NullStatsReceiver
import com.twitter.util.Try

val twitter = Http.client.newService("twitter.com")
val budget: RetryBudget = RetryBudget()
val policy: RetryPolicy[Try[Nothing]] = ???

val retry = new RetryFilter(
  retryPolicy = policy,
  timer = DefaultTimer,
  statsReceiver = NullStatsReceiver,
  retryBudget = budget
)

val retryTwitter = retry.andThen(twitter)
```

The `RetryPolicy` instance might be constructed in several ways:

1. `RetryPolicy.tries` - retries using jittered backoff 1 between the given number of maximum attempts
2. `RetryPolicy.backoff` - retries using the given backoff 1 policy

The following example 3 constructs an instance of `RetryPolicy` using the given backoff value.

```
import com.twitter.finagle.http.{Response, Status}
import com.twitter.finagle.service.{Backoff, RetryPolicy}
import com.twitter.util.{Try, Return, Throw}
import com.twitter.conversions.DurationOps._

val policy: RetryPolicy[Try[Response]] =
  RetryPolicy.backoff(Backoff.equalJittered(10.milliseconds, 10.seconds)) {
    case Return(rep) if rep.status == Status.InternalServerError => true
  }
```

See [Retries metrics](<https://twitter.github.io/finagle/guide/Metrics.html#id3>) for more details.

Note

It’s highly recommended to *share* a single instance of `RetryBudget` between both `RetryFilter` and `RequeueFilter` to prevent *retry storms*.

### Timeouts & Expiration¶

Finagle provides timeout facilities with fine granularity:

The *Session Timeout* module defines a timeout for session acquisition. That is, it defines the maximum time allotted to a request to wait for an available service/session. Requests that exceed this timeout are failed with a `ServiceTimeoutException`. This module is implemented by the [TimeoutFactory](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/factory/TimeoutFactory.scala>)

The default timeout value for the *Session Timeout* module is unbounded (i.e., `Duration.Top`), which simply means it’s disabled. Although, it’s possible to override the default setting with stack params 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http

val twitter = Http.client
  .withSession.acquisitionTimeout(42.seconds)
  .newService("twitter.com")
```

See [Service Latency metrics](<https://twitter.github.io/finagle/guide/Metrics.html#service-factory-failures>) for more details.

The *Request Timeout* module is a filter and thus gives an upper bound on the amount of time allowed for a request to be outstanding. An important implementation detail of the [TimeoutFilter](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/TimeoutFilter.scala>) is that it attempts to cancel the request when a timeout is triggered. With most protocols, if the request has already been dispatched, the only way to cancel the request is to terminate the connection. Note that HTTP/2 and Mux both have first-class support for request cancellation without needing to tear down the connection.

The default timeout for the *Request Timeout* module is unbounded (i.e., `Duration.Top`). Here is an example 3 of how to override that default.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http

val twitter = Http.client
  .withRequestTimeout(42.seconds)
  .newService("twitter.com")
```

Regarding retries, the timeout is given to each attempt.

As Finagle does not know whether or not a request is idempotent, request timeouts are not retried by default. However this can be configured through a retry policy.

See [Request Latency metrics](<https://twitter.github.io/finagle/guide/Metrics.html#metrics-stats-filter>) for more details.

Note

This module only works with request/response usage and does not support streaming (such as with HTTP).

The *Expiration* module is attached at the connection level and expires a service/session after a certain amount of time. The module is implemented by [ExpiringService](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/ExpiringService.scala>).

The default setting for the *Expiration* module is to never expire a session. Here is how it can be configured 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http

val twitter = Http.client
  .withSession.maxLifeTime(20.seconds)
  .newService("twitter.com")
```

The *Expiration* module for clients takes one parameter: *maxLifeTime* - the maximum duration for which a session is considered alive.

See [Expiration metrics](<https://twitter.github.io/finagle/guide/Metrics.html#idle-apoptosis-stats>) for more details.

Finally, timeouts can be enforced outside of these modules on a per-request level using `Future#within` or `Future#raiseWithin`. 4:

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.http.{Request, Response}
import com.twitter.util.Future

val response: Future[Response] = twitter(request).within(1.second)
```

### Request Draining¶

The *RequestDraining* module guarantees that the client delays closure until all outstanding requests have been completed. It wraps each produced *Service* with a [RefCountedService](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/RefcountedService.scala>).

### Load Balancing¶

Finagle clients come equipped with a load balancer, a pivotal component in the client stack, whose responsibility is to dynamically distribute load across a collection of interchangeable endpoints. This gives Finagle an opportunity to maximize success and optimize request distribution in an attempt to tighten the client’s tail latencies. To achieve this in a non-cooperative distributed environment, the balancer must pass accurate judgments about endpoints based only on its local view. An effective feedback mechanism in such environments is latency; the balancers load metrics make use of this either implicitly or explicitly.

Balancer implementations are split into two parts: A *load metric* and a *distributor*. Each node in the balancer maintains the load metric and a distributor uses the data to select an endpoint.

The default setup for a Finagle client is to use P2C algorithm to distribute load across endpoints, while picking the least loaded one. See P2C + Least Loaded for more details.

There are plenty of useful stats exported from the *Load Balancing* module. See [Load Balancing metrics](<https://twitter.github.io/finagle/guide/Metrics.html#loadbalancer-stats>) for more details.

Use the following code snippet to override the default load balancing strategy for a particular Finagle client (see [Balancers](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/loadbalancer/Balancers.scala>) on how to construct instances of `LoadBalancerFactory`).

```
import com.twitter.finagle.Http
import com.twitter.finagle.loadbalancer.LoadBalancerFactory

val balancer: LoadBalancerFactory = ???
val twitter = Http.client
  .withLoadBalancer(balancer)
  .newService("twitter.com:8081,twitter.com:8082")
```

In addition to the default configuration (i.e., `Balancers.p2c`), the following setups are available.

#### Heap + Least Loaded¶

The distributor is a heap which is shared across requests. Each node in the heap maintains a count of outstanding request. The count is incremented when a request is dispatched and decremented when we receive a response (note the dependence on latency). The heap is min-ordered to allow for efficient access to the least loaded. The distributor inherits all the nice properties of the heap (i.e. selecting the top of the heap is constant time and other common operations take *O(log n)*). This configuration has some limitations. In particular, it’s difficult to use weighted nodes or swap out a load metric without sacrificing the performance of the heap. What’s more, the heap must be updated atomically by each request and thus represents a highly contended resource.

Use `Balancers.heap` to construct an instance of `LoadBalancerFactory`.

```
import com.twitter.finagle.loadbalancer.{Balancers, LoadBalancerFactory}

val balancer: LoadBalancerFactory = Balancers.heap()
```

Note

In order to take an advantage of any of the load balancers supported in Finagle, a client should be configured to talk to a *replica set* (the default finagle stack doesn’t do *sharding* and assumes all hosts in the set are interchangeable.) (see [Names and Naming in Finagle](<https://twitter.github.io/finagle/guide/Names.html#finagle-names>) for more details) rather than a single endpoint.

#### Power of Two Choices (P2C) + Least Loaded¶

The P2C distributor solves many of the limitations that are inherent with the Heap distributor and is the default balancer for Finagle clients. By employing an elegant (and surprising) mathematical phenomenon 5, the algorithm randomly picks two nodes from the collection of endpoints and selects the least loaded of the two. By repeatedly using this strategy, we can expect a manageable upper bound on the maximum load of any server 6. The default load metric for the P2C balancer is least loaded, however, because P2C is fully concurrent 7, it allows us to efficiently implement weighted nodes 8 or different load metrics with minimal per-request costs.

Use `Balancers.p2c` to construct an instance of `LoadBalancerFactory` 3.

```
import com.twitter.finagle.loadbalancer.{Balancers, LoadBalancerFactory}

val balancer: LoadBalancerFactory = Balancers.p2c()
```

#### Power of Two Choices (P2C) + Peak EWMA 2¶

Backed by the P2C distributor, Peak EWMA uses a moving average over an endpoint’s round-trip time (RTT) that is highly sensitive to peaks. This average is then weighted by the number of outstanding requests, effectively increasing our resolution per-request. It is designed to react to slow endpoints more quickly than *least loaded* by penalizing them when they exhibit slow response times. This load metric operates under the assumption that a loaded endpoint takes time to recover and so it is generally safe for the advertised load to incorporate an endpoint’s history. However, this assumption breaks down in the presence of long polling clients.

Use `Balancers.p2cPeakEwma` to construct an instance of `LoadBalancerFactory` 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.loadbalancer.{Balancers, LoadBalancerFactory}

val balancer: LoadBalancerFactory =
  Balancers.p2cPeakEwma(decayTime = 100.seconds)
```

The `p2cPeakEwma` factory method takes two arguments:

1. *decayTime* (default: 10 seconds) - the window of latency observations

#### Aperture + Least Loaded 2¶

Note

The aperture load balancers can be more challenging to reason about than the P2C family of balancers. See [Aperture Load Balancers](<https://twitter.github.io/finagle/guide/ApertureLoadBalancers.html#aperture-load-balancers>) section for more information.

All the previously mentioned configurations operate optimally under high load. That is, without sufficient concurrent load, the previous distributors can degrade to random selection. The Aperture distributor aims to remedy this among other things. By employing a simple feedback controller based on the client’s load, the distributor balances across a subset of servers to meet a specified target load band. The principle of hysteresis is applied to the aperture to avoid rapid fluctuations and dampen the effects of large load spikes.

The benefits of Aperture are promising:

1. A client uses resources commensurate to offered load. In particular, it does not have to open sessions with every service in a large cluster. This is especially important when offered load and cluster capacity are mismatched.
2. It balances over fewer, and thus warmer, services. This also means that clients pay the penalty of session establishment less frequently.
3. It increases the efficacy of least-loaded balancing which, in order to work well, requires concurrent load.

Use `Balancers.aperture` to construct an instance of `LoadBalancerFactory` 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.loadbalancer.{Balancers, LoadBalancerFactory}

val balancer: LoadBalancerFactory =
  Balancers.aperture(
    smoothWin = 32.seconds,
    lowLoad = 1.0,
    highLoad = 2.0,
    minAperture = 10
  )
```

The `aperture` factory method takes five arguments:

1. *smoothWin* (default: 5 seconds) - the window of concurrent load observation
2. \[*lowLoad*, *highLoad*\] (default: \[0.5, 2\]) - the load band used to adjust an aperture size such that a concurrent load for each endpoint stays within the given interval
3. *minAperture* (default: 1) - the minimum size of the aperture

Note

The Aperture load balancer should rarely be configured and we are working to provide broadly applicable defaults.

#### Panic Mode¶

Panic mode is when the load balancer gives up trying to find a healthy node. The LB sends the request to the last pick even if the node is unhealthy. For a given request, panic mode is enabled when the percent of nodes that are unhealthy exceeds the panic threshold. This percent is approximate. For pick2-based load balancers (P2C\* and Aperture\*), interpret this as 1% of requests or less will panic when the threshold is reached. When the percent of unhealthy nodes exceeds the threshold, the number of requests that panic increases exponentially. For round robin, this panic threshold percent does not apply because it is not a pick two based algorithm. Panic mode is disabled for heap LB.

Please, note that this doesn’t mean that 1% of requests will fail since Finagle clients have additional layers of requeues above the load balancer.

Panic mode can be combined with any P2C or Aperture load balancer. The possible modes are: *TenPercentUnhealty*, *TwentyPercentUnhealthy*, *ThirtyPercentUnhealthy*, *FortyPercentUnhealthy*, *FiftyPercentUnhealthy* (default), and *MajorityUnhealthy*.

```
import com.twitter.finagle.loadbalancer.PanicMode

val client = Http.client
  .withLoadBalancer.panicMode(PanicMode.FortyPercentUnhealthy)
```

#### Role of Balancers in Resiliency¶

The balancer’s primary goal is to attempt to optimize request latency. Coincidentally, to do this well, it also needs to properly qualify sessions. These two concerns are treated separately in the client stack. Finagle has dedicated modules which track failures and control the *com.twitter.finagle.Status* of an endpoint. The balancers selection process takes this status into account. However, without protocol support the qualification happens in-band with requests (i.e. it requires failed requests). We are exploring better (explicit) session qualification with protocol support (e.g. Mux).

Additionally, clients must be resilient to instabilities in the service discovery system. Historically, Finagle has employed solutions like *com.twitter.finagle.addr.StabilizingAddr* and *com.twitter.finagle.serverset2.Stabilizer* to validate changes to the balancers endpoint collection. Since we have information about the availability of an endpoint in the balancer, it may represent a viable intersection to validate such changes. Balancers have a “probation” capability built-in behind a client parameter 9.

[Related stats](<https://twitter.github.io/finagle/guide/Metrics.html#loadbalancer-stats>)

### Circuit Breaking¶

The following modules aim to preemptively disable sessions that will likely fail requests. From the perspective of the load balancer, they act as circuit breakers which, when triggered, temporarily suspend the use of a particular endpoint.

There are at least two modules in the client stacks that might be viewed as [circuit breakers](<https://martinfowler.com/bliki/CircuitBreaker.html>):

1. *Fail Fast* - a session-driven circuit breaker
2. *Failure Accrual* - a request-driven circuit breaker

In addition to *Fail Fast* and *Failure Accrual*, some of the protocols (i.e., *Mux*, *HTTP/2*) in Finagle support *Ping-based Failure Detectors* 10 (i.e., [ThresholdFailureDetector](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/liveness/ThresholdFailureDetector.scala>)).

#### Fail Fast¶

The [FailFast](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/FailFastFactory.scala>) module attempts to reduce the number of requests dispatched to endpoints that are likely to fail. It works by marking downed hosts when a connection fails, and launching a background process that repeatedly attempts to reconnect with a given backoff schedule. During the time that a host is marked down, the factory is marked unavailable (and thus the load balancer above it will avoid its use). The factory becomes available again on success or when the back-off schedule runs out.

This module fails closed and returns an exception when it detects a failure. See the FAQ to [better understand](<https://twitter.github.io/finagle/guide/FAQ.html#faq-failedfastexception>) why clients might be seeing `com.twitter.finagle.FailedFastException`’s.

The *Fail Fast* module is enabled by default for all of the Finagle clients except for `Memcached.client` one. The following example demonstrates how to explicitly disable it for a particular client.

```
import com.twitter.finagle.Http

val twitter = Http.client
  .withSessionQualifier.noFailFast
  .newService("twitter.com")
```

Note

Because this module fails closed, Finagle will automatically disable *Fail Fast* when only one host is present in the replica set. This is because, without more replicas, Finagle can not meaningfully handle the failure when breaking the circuit.

[Related stats](<https://twitter.github.io/finagle/guide/Metrics.html#fail-fast-stats>)

#### Failure Accrual¶

The *Failure Accrual* module marks itself as unavailable per-endpoint based on a configurable [policy](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/liveness/FailureAccrualPolicy.scala>). Unlike *Fail Fast*, this module fails open. That is, even if it transitions into an unavailable state, requests will still be allowed to flow through it. However, recall that the availability is propagated through the stack, so the load balancer will avoid using an endpoint where the failure accrual module is unavailable.

When transitioning from an unavailable to an available state, the module is conservative and only allows for a probe request. If the probe fails, it goes back to unavailable regardless of the policy. Put differently, at least one request must succeed before the module starts to apply the policy again.

There are two types of policies out of the box:

1. A policy based on the requests success rate meaning (i.e, an endpoint marked dead if its success rate goes below the given threshold).
2. A policy based on the number of consecutive failures occurred in the endpoint (i.e., an endpoint marked dead if there are at least `N` consecutive failures occurred in this endpoint)

The default setup for the *Failure Accrual* module is a hybrid policy based on the number of consecutive failures (default is 5) and required success rate (default is 80%). The policy is accompanied by an equal jittered backoff 1 (5 to 300 seconds) producing durations for which an endpoint is marked dead.

Use `FailureAccrualFactory.Param` 2 to configure Failure Accrual\`. The following snippets illustrate some examples 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http
import com.twitter.finagle.liveness.{FailureAccrualFactory, FailureAccrualPolicy}
import com.twitter.finagle.service.Backoff

val twitter = Http.client
  .configured(FailureAccrualFactory.Param(() => FailureAccrualPolicy.successRate(
    requiredSuccessRate = 0.95,
    window = 100,
    markDeadFor = Backoff.const(10.seconds)
  )))
  .newService("twitter.com")
```

The `successRate` factory method takes three arguments:

1. *requiredSuccessRate* - the minimally required success rate below which an endpoint marked dead
2. *window* - the window of *requests* to measure success rate on; measured using an exponentially weighted moving average
3. *markDeadFor* - the backoff 1 policy (an instance of [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Backoff.scala>)) used to mark an endpoint dead for a certain amount of time

It’s highly recommended to use [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Backoff.scala>) instead of using the error-prone `Stream` API directly.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http
import com.twitter.finagle.liveness.{FailureAccrualFactory, FailureAccrualPolicy}
import com.twitter.finagle.service.Backoff

val twitter = Http.client
  .configured(FailureAccrualFactory.Param(() => FailureAccrualPolicy.successRateWithinDuration(
    requiredSuccessRate = 0.95,
    window = 5.minutes,
    markDeadFor = Backoff.const(10.seconds),
    minRequestThreshold = 100
  )))
  .newService("twitter.com")
```

The `successRateWithinDuration` factory method takes four arguments:

1. *requiredSuccessRate* - the minimally required success rate below which an endpoint is marked dead
2. *window* - duration over which the success rate is tracked over.
3. *markDeadFor* - the backoff policy (an instance of [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Backoff.scala>)) used to mark an endpoint dead for
4. *minRequestThreshold* - the minimum number of requests required within the past `window` before considering the measured success rate

To configure *Failure Accrual* based on a number of consecutive failures 2, use the following snippet 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http
import com.twitter.finagle.liveness.{FailureAccrualFactory, FailureAccrualPolicy}
import com.twitter.finagle.service.Backoff

val twitter = Http.client
  .configured(FailureAccrual.Param(() => FailureAccrualPolicy.consecutiveFailures(
    numFailures = 10,
    markDeadFor = Backoff.const(10.seconds)
  )))
  .newService("twitter.com")
```

The `consecutiveFailures` factory method takes two arguments:

1. *consecutiveFailures* - the number of failures after which an endpoint is marked dead
2. *markDeadFor* - the backoff policy (an instance of [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/Backoff.scala>)) used to mark an endpoint dead for

FailureAccrualPolicys can also be composed together via the `orElse` method. If multiple policies return a duration on *markDeadOnFailure()*, the maximum duration is used.

Note

It’s highly recommended to use [Backoff](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/Backoff.scala>) API for constructing instances of `Stream[Duration]` instead of using the error-prone Stream API directly.

Finally, it’s possible to completely disable the *Failure Accrual* module for a given client.

```
import com.twitter.finagle.Http

val twitter = Http.client
  .withSessionQualifier.noFailureAccrual
  .newService("twitter.com")
```

The module is implemented by [FailureAccrualFactory](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/liveness/FailureAccrualFactory.scala>). See [Failure Accrual Stats](<https://twitter.github.io/finagle/guide/Metrics.html#failure-accrual-stats>) for stats exported from the `Failure Accrual` module.

### Pooling¶

Many protocols benefit from having persistent connections that are reused across requests. Pooling is designed to balance connection churn and service acquisition latency at the cost of holding resources open.

Depending on the configuration, a Finagle client’s stack might contain up to \_three\_ connection pools stacked on each other: watermark, caching and buffering pools.

The two Finagle protocols that don’t require any connection pooling (multiplexing protocols) are Mux and HTTP/2 as they both maintain just one connection per remote peer. For every other Finagle- supported protocol (i.e., HTTP/1.1, Thrift), there a connection pooling setup built with watermark and caching pools in front of each remote peer.

Note

When HTTP/2 is enabled on an HTTP client (and the transport is successfully upgraded), the session pool caches streams (not connections) multiplexed over a single connection.

The default client stack layers caching and watermark pools which amounts to maintaining the low watermark (i.e., `0`, as long as request concurrency exists), queuing requests above the unbounded high watermark (i.e., `Int.MaxValue`), and applying an unbounded TTL (i.e., `Duration.Top`) for services that are between \[low, high\].

The override the default settings for connection pooling in a Finagle client, use the following example 3.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http

val twitter = Http.client
  .withSessionPool.minSize(10)
  .withSessionPool.maxSize(20)
  .withSessionPool.maxWaiters(100)
  .withSessionPool.ttl(5.seconds)
  .newService("twitter.com")
```

Note

All session pool settings are applied to each host in the replica set. Put this way, these settings are per-host as opposed to per-client.

Thus all the three pools are configured with a single param that takes the following arguments:

1. *minSize* and *maxSize* - low and high watermarks for the watermark pool (note that a Finagle client will not maintain more connections than *maxSize* per host)
2. *maxWaiters* - the maximum number of connection requests that are queued per host when the connection concurrency exceeds the high watermark
3. *ttl*- the maximum amount of time a per-host session is allowed to be cached in a pool

[Related stats](<https://twitter.github.io/finagle/guide/Metrics.html#pool-stats>)

#### Buffering Pool¶

The simplest connection pool implementation available in Finagle is [BufferingPool](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/pool/BufferingPool.scala>) that just buffers up to `bufferSize` connections and produces/closes new ones above it. This pool is disabled by default.

#### Watermark Pool¶

Finagle provides a generic pool that maintains a collection of service instances. Each endpoint the client connects to has an independent pool with high and low watermarks. The [WatermarkPool](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/pool/WatermarkPool.scala>) keeps persistent services up to the lower bound. It will keep making new services up to upper bound if you checkout more than lower bound services, but when you release those services above the lower bound, it immediately tries to close them. This, however, creates a lot of connection churn if your application consistently requires more than lower bound connections.

#### Caching Pool¶

To reduce connection churn, there is a separate facility for caching, with some TTL, services above the lower bound. The [CachingPool](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/pool/CachingPool.scala>) caches *regardless* of whether there are more than lower-bound open services; it’s always caching up to (upper-bound - lower-bound) services. The cache reaches its peak value when you reach your peak concurrency (i.e. “load”), and then slowly decays, based on the TTL.

[Related stats](<https://twitter.github.io/finagle/guide/Metrics.html#pool-stats>)

## Admission Control¶

Clients are configured with the [NackAdmissionFilter](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/filter/NackAdmissionFilter.scala>) which will probabilistically drop some requests to unhealthy clusters. This aims to decrease the request volume to those clusters with little to no effect on a client’s already unhealthy success rate. The filter works by keeping a moving average of the fraction of requests that are [nacked](<https://twitter.github.io/finagle/guide/Glossary.html#glossary-nack>). When this fraction hits a given threshold, the filter will probabilistically drop requests in proportion to that fraction.

```
import com.twitter.conversions.DurationOps._
import com.twitter.finagle.Http

val twitter = Http.client
  .withAdmissionControl.nackAdmissionControl(
    window = 10.minutes,
    nackRateThreshold = 0.75
  )
```

The filter can be configured with the following parameters:

1. `window` The duration over which the average is calculated. Default is 2 minutes.
2. `nackRateThreshold` The rate of rejected requests at which the filter kicks in. Default is 0.5.

[Related stats](<https://twitter.github.io/finagle/guide/Metrics.html#admission-control-stats>)

Here is a brief summary of the configurable params.

> A configuration with a `nackRateThreshold` of N% and a `window` of duration W roughly translates as, “start dropping some requests to the cluster when the nack rate averages at least N% over a window of duration W.”

Here are some examples of situations with param values chosen to make the filter useful:

- Owners of Service A examine their service’s nack rate over several days and find that it is almost always under 10% and rarely above 1% (e.g., during traffic spikes) or 5% (e.g., during a data center outage). They do not want to preemptively drop requests unless the cluster sees an extreme overload situation so they choose a nack rate threshold of 20%. And in such a situation they want the filter to act relatively quickly, so they choose a window of 30 seconds.
- Owners of Service B observe that excess load typically causes peak nack rates of around 25% for up to 60 seconds. They want to be aggressive about avoiding cluster overload and don’t mind dropping some innocent requests during mild load so they choose a window of 10 seconds and a threshold of 0.15 (= 15%).

Note

Client-side admission control may not work well with clients that only very sporadically send requests to their backends. In this case, the view that each client has of the state of the backend is reduced drastically, and its efficiency is degraded. It’s recommended to disable nack admission control (via *withAdmissionControl.noNackAdmissionControl*) for clients experiencing bursty and very low volume (i.e., single digit RPS) traffic.

## Response Classification¶

To give Finagle visibility into application level success and failure developers can provide classification of responses on clients and servers by using [response classifiers](<https://github.com/twitter/finagle/blob/release/finagle-core/src/main/scala/com/twitter/finagle/service/package.scala>). This gives Finagle the proper domain knowledge and improves the efficacy of failure accrual and more accurate [success rate stats](<https://twitter.github.io/finagle/guide/Metrics.html#metrics-stats-filter>).

For HTTP clients and servers, using `HttpResponseClassifier.ServerErrorsAsFailures` often works great as it classifies any HTTP 5xx response code as a failure. For Thrift/ThriftMux clients and servers you may want to use `ThriftResponseClassifier.ThriftExceptionsAsFailures` which classifies any deserialized Thrift Exception as a failure. For a large set of use cases these should suffice. Classifiers get wired up to your client and server in a straightforward manner, for example, in a ThriftMux client:

```
import com.twitter.finagle.ThriftMux
import com.twitter.finagle.thrift.service.ThriftResponseClassifier

ThriftMux.client
  ...
  .withResponseClassifier(ThriftResponseClassifier.ThriftExceptionsAsFailures)
```

In an HTTP server:

```
import com.twitter.finagle.Http
import com.twitter.finagle.http.service.HttpResponseClassifier

Http.server
  ...
  .withResponseClassifier(HttpResponseClassifier.ServerErrorsAsFailures)
```

If a classifier is not specified on a client or server or if a user’s classifier isn’t defined for a given request/response pair then `ResponseClassifier.Default` is used. This gives us the simple classification rules of responses that are `Returns` are successful and `Throws` are failures.

### Custom Classifiers¶

`ResponseClassifier` is a `PartialFunction` from `ReqRep` to `ResponseClass`. Custom classifiers allow the user to tell Finagle what constitutes a failed outcome, and also what to do about it. Users define classifiers in terms of `ReqRep` and `Try`.

`rc`, defined below, is a classifier that tells Finagle that `Throw` means failure, and `Return` means success.

```
val rc: ResponseClassifier = {
  case ReqRep(req, Throw(exc)) => ResponseClass.RetryableFailure
  case ReqRep(req, Return(rep)) => ResponseClass.Success
}
```

A `ReqRep` is a request-response pair. This is so that classifiers can make judgements on both a request and response.

More than just telling Finagle if this `ReqRep` is a successful or failed outcome, it also gives Finagle a hint about what it should do next. Finagle can respond to failed outcomes with some nuance, for example, it may retry the operation.

`ResponseClass` defines three classes of failure:

- `NonRetryableFailure`: Something went wrong, don’t retry.
- `RetryableFailure`: Something went wrong, consider retrying the operation.
- `Ignorable`: Something went wrong, but it can be ignored.

And, of course, `Success` means that the operation succeeded. `Ignorable` does not apply to `Success` because it is a mapping from `FailureFlags.Ignorable` which only applies to `Failure` and not any arbitrary response.

It’s important to note that classifiers are only consulted but not obeyed. For example, a classifier may emit `Ignorable` for a given `ReqRep` but what actually happens depends on how the caller chooses to interpret `Ignorable`. Similarly, just because a classifier emits `RetryableFailure` does not mean the caller will retry the operation.

Now that we’ve covered the basics, let’s look at an example in HTTP. Here is an example that counts HTTP 503s as failures:

```
import com.twitter.finagle.http
import com.twitter.finagle.service.{ReqRep, ResponseClass, ResponseClassifier}
import com.twitter.util.Return

val classifier: ResponseClassifier = {
  case ReqRep(_, Return(r: http.Response)) if r.statusCode == 503 =>
    ResponseClass.NonRetryableFailure
}
```

Note that this `PartialFunction` isn’t total which is ok due to Finagle always using user defined classifiers in combination with `ResponseClassifier.Default` which will cover all cases.

### Thrift and ThriftMux Classifiers¶

Thrift and ThriftMux classifiers require a bit more care as the request and response types are not as obvious. This is because there is only a single `Service` from `Array[Byte]` to `Array[Byte]` for all the methods of an IDL’s service. To make this workable, there is support in Scrooge, `Thrift/ThriftMux.newService`, `Thrift/ThriftMux.newClient` and `Thrift/ThriftMux.serve` code to deserialize the responses into the expected application types so that classifiers can be written in terms of the Scrooge generated request type, `$Service.$Method.Args`, and the method’s response type. Given an IDL:

```
exception NotFoundException { 1: string reason }
exception InvalidQueryException {
  1: i32 errorCode
}

service SocialGraph {
  i32 follow(1: i64 follower, 2: i64 followee) throws (
    1: NotFoundException ex1,
    2: InvalidQueryException ex2
  )
}
```

One possible classifier would be:

```
import com.twitter.finagle.service.{ReqRep, ResponseClass, ResponseClassifier}

val classifier: ResponseClassifier = {
  // #1
  case ReqRep(_, Throw(_: NotFoundException)) =>
    ResponseClass.NonRetryableFailure

  // #2
  case ReqRep(_, Return(x: Int)) if x == 0 =>
    ResponseClass.NonRetryableFailure

  // #3 *Caution*
  case ReqRep(SocialGraph.Follow.Args(a, b), _) if a <= 0 =>
    ResponseClass.NonRetryableFailure

  // #4
  case ReqRep(_, Throw(_: InvalidQueryException)) =>
    ResponseClass.Success
}
```

If you examine that classifier you’ll note a few things. First (#1), the deserialized `NotFoundException` can be treated as a failure. Second (#2), a “successful” response can be examined to enable services using status codes to classify errors. Next (#3), the request can be introspected to make the decision - *HOWEVER* - if an exception is thrown at the Mux layer (ex: `c.t.f.mux.ClientDiscardedRequestException`) there will **NOT** be a match against (#3). This style (#3) should be avoided for Thrift and ThriftMux. Instead, prefer to handle request specific details at the application layer, such as creating a [Filter](<https://twitter.github.io/finagle/guide/ServicesAndFilters.html#filters>) to reject the request, and reserve Response Classification to deal with wire level response concerns. Lastly (#4), the deserialized `InvalidQueryException` can be treated as a successful response.

### Other Details¶

If you have a response classifier that categorizes non-Exceptions as failures, this includes Thrift Responses (#2) or embedded Thrift Exceptions (#1), note that they will be counted in the `StatsFilter` as a `com.twitter.finagle.service.ResponseClassificationSyntheticException` in the `StatsReceiver` to indicate when this happens. See the [FAQ](<https://twitter.github.io/finagle/guide/FAQ.html#what-is-a-com-twitter-finagle-service-responseclassificationsyntheticexception>) for more details.

## MethodBuilder¶

Note

Currently there is `MethodBuilder` support for HTTP and ThriftMux. We are waiting on user interest before expanding to more protocols.

`MethodBuilder` is a collection of APIs for client configuration at a higher level than the [Finagle 6 APIs](<https://twitter.github.io/finagle/guide/Configuration.html#finagle6apis>) while improving upon the deprecated `ClientBuilder`. `MethodBuilder` provides:

- [Logical](<https://twitter.github.io/finagle/guide/MethodBuilder.html#mb-logical-req>) success rate metrics.
- Retries based on application-level requests and responses (e.g. an HTTP 503 response code or a Thrift exception).
- Configuration of per-attempt and total timeouts.

[Learn more](<https://twitter.github.io/finagle/guide/MethodBuilder.html>) about `MethodBuilder`.

## Partition Aware Client¶

Partition Aware Client is a client configured with a partitioning layer in the client stack that understands the service partitioning strategy. It routes requests to the partitioned service shards and supports scatter-gather. Finagle Memcached client and Redis PartitionedClient have the consistent hashing partitioning strategy configuration by default, ThriftMux client has partitioning support as an opt-in configuration by calling `.withPartitioning`.

[Learn more](<https://twitter.github.io/finagle/guide/PartitionAwareClient.html>) about ThriftMux Partition Aware Client.

Footnotes

**1(1,2,3,4,5)**

Most of the backoff strategies implemented in Finagle are inspired by Mark Brooker’s [blog post](<https://www.awsarchitectureblog.com/2015/03/backoff.html>).

**2(1,2,3,4)**

This configuration was developed to target specific problems we encounter at Twitter and should be considered experimental. Note that its API may change as we continue to understand its place in the stack.

**3(1,2,3,4,5,6,7,8,9,10,11)**

Configuration parameters/values provided in this example are only to demonstrate the API usage, not the real world values. We do not recommend blindly applying those values to production systems.

**4**

The *Future#within* variant creates a new future that invokes raise on the future when the timeout occurs. The affects of which are dependent on the producer of the future. In most cases, Finagle will attempt to cancel the request if it hasn’t already been dispatched. If it has been dispatched, the behavior is dependent on the protocol (without protocol support Finagle needs to tear down the session to signal cancellation).

**5**

Michael Mitzenmacher. 2001. The Power of Two Choices in Randomized Load Balancing. IEEE Trans. Parallel Distrib. Syst. 12, 10 (October 2001), 1094-1104.

**6**

The maximum load variance between any two servers is bound by *ln(ln(n))* where n is the number of servers in the cluster.

**7**

Our micro benchmark exposes the stark differences:

```
HeapBalancer.getAndPut           1000  avgt  10 8686.479 ± 261.360 ns/op
P2CBalancer.leastLoadedGetAndPut 1000  avgt  10 1692.388 ± 103.164 ns/op
```

**8**

Weights are built into all the balancers except for the `HeapBalancer`. The API exposed for this is in `com.twitter.finagle.WeightedSocketAddress`. The name resolver that translates logical destinations to `com.twitter.finagle.Addr` can wrap concrete address with a *Double* which influences the balancer’s distributor during the selection process.

**9**

See `com.twitter.finagle.loadbalancer.LoadBalancerFactory#EnableProbation`.

**10**

See *Failure Detectors* section from Alvaro Videla’s [blog post](<https://videlalvaro.github.io/2015/12/learning-about-distributed-systems.html>).

© Copyright 2024 Twitter, Inc.


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://twitter.github.io/finagle/guide/Clients.html>
- License: [Apache-2.0](<https://github.com/twitter/finagle/blob/develop/LICENSE>)
