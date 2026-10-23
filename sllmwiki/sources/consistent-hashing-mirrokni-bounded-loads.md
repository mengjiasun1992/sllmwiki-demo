Consistent Hashing with Bounded Loads

Vahab Mirrokni1, Mikkel Thorup2, and Morteza Zadimoghaddam1

1Google Research, New York. fmirrokni,zadimg@google.com
2University of Copenhagen. mikkel2thorup@gmail.com

July 28, 2017

Abstract

In dynamic load balancing, we wish to allocate a set of clients (balls) to a set of servers (bins) with
the goal of minimizing the maximum load of any server and also minimizing the number of moves after
adding or removing a server or a client1. We want a hashing-style solution where we given the ID of a
client can efficiently find its server in a distributed dynamic environment. In such a dynamic environ-
ment, both servers and clients may be added and/or removed from the system in any order. The most
popular solutions for such dynamic settings are Consistent Hashing [KLL+97,SML+03] or Rendezvous
Hashing [TR98]. However, the load balancing of these schemes is no better than a random assignment of
clients to servers, so with n of each, we expect many servers to be overloaded with (log n= log log n)
clients. In this paper, we aim to design hashing schemes that achieve any desirable level of load balanc-
ing, while minimizing the number of movements under any addition or removal of servers or clients.
In particular, we consider a problem with m balls and n bins, and given a user-specified balancing
parameter c = 1+" > 1, we aim to find a hashing scheme with no load above dcm=ne, referred to as the
capacity of the bins. Our algorithmic starting point is the consistent hashing scheme where current balls
and bins are hashed to a unit cycle, and a ball is placed in the first bin succeeding it in clock-wise order.
In order to cope with given capacity constraints, we apply the idea of linear probing by forwarding the
ball on the circle to the first non-full bin. We show that in our hashing scheme when a ball or bin is
inserted or deleted, the expected number of balls that have to be moved is within a multiplicative factor
of O( 1"2 ) of the optimum for " 1 (Theorem 3) and within a factor 1 + O( log cc ) of the optimum for
" 1 (Theorem 2). Technically, the latter bound is the most challenging to prove. It implies that we
for superconstant c, we only pay a negligible cost in extra moves. We also get the same bounds for the
simpler problem where we instead of a user specified balancing parameter have a fixed bin capacity C
for all bins, and define c = 1 + " = Cn=m.

Supported in part by Advanced Grant DFF-0602-02499B from the Danish Council for Independent Research under the Sapere
Aude research career programme.
1Throughout this paper, we use balls as clients, and also bins as servers, interchangeably.

arXiv:1608.01350v3 [cs.DS] 27 Jul 2017

## 1 Introduction

Load balancing in dynamic environments is a central problem in designing several networking systems and
web services [SML+03, KLL+97]. We wish to allocate clients (also referred to as balls) to servers (also
referred to as bins) in such a way that none of the servers gets overloaded. Here, the load of a server is
the number of clients allocated to it. We want a hashing-style solution where we given the ID of a client
can efficiently find its server. Both clients and servers may be added or removed in any order, and with
such changes, we do not want to move too many clients. Thus, while the dynamic allocation algorithm
has to always ensure a proper load balancing, it should aim to minimize the number of clients moved after
each change to the system. For every update in the system, we need to change the allocation of clients to
servers. For simplicity, we assume that the updates (ball and bin additions and removals) do not happen
simultaneously and will be operated one at a time, so that we have time to finish changing the allocation
before we get another update.
Note that such allocation problems become even more challenging when we face hard constraints in
the capacity of each server, that is, each server has a capacity and the load may not exceed this capacity.
Typically, we want capacities close to the average loads.
There is a vast literature on solutions in the much simpler case where the set of servers is fixed and only
the client set is updated, and we shall return to some of these in Section 1.4. For now, we focus on solutions
that are known to work in our fully-dynamic case where both clients and servers can be added and removed
in an arbitrary order. This also rules out solutions where only the last added server may be removed2.
The above problem formulation is very general, and does not assume anything about the ratio between the
number m of clients and the number n of servers, e.g., processors are cheap, so one can imagine systems
with a large number of servers, but there could also be many clients. One could also imagine a balanced
system in which m = n.
The classic solution to the scenario where both clients and servers can be added and removed is Con-
sistent Hashing [SML+03,KLL+97] where the current clients are assigned in a random way to the current
servers. While consistent hashing schemes minimize the expected number of movements, they may result
in hugely overloaded servers, and they do not allow for explicit capacity constraints on the servers. The
basic point is that the load balancing of consistent hashing [KLL+97,SML+03] is no better than a random
assignment of clients to servers. The same issue holds for Rendezvous or Highest Random Weight Hash-
ing [TR98]3. Hence, with m clients and n servers, we expect a good load balancing if m=n = (log n),
but the balance is lost with smaller loads, e.g., with m = n, we expect many servers to be overloaded with
(log n= log log n) clients. In this paper, with n clients and n servers, we get a guaranteed max-load of 2,
while only moving an expected constant number of clients each time a client or server is added or removed.
Our general result is described below.
In this paper, we present an algorithm that works with arbitrary capacity constraints on the servers. For
the purpose of load balancing, the system designer can specify a balancing parameter c = 1+", guaranteeing
that the maximum load is at most dcm=ne. While maintaining this hard balancing constraint, we limit the
expected number of clients to be moved when clients or servers are inserted or removed.
Even without capacity constraints, the obvious general lower bounds for moves are as follows. When a
client is added or removed, at least we have to move that client. When a server is added or removed, at least
2This rules out the external memory techniques [Lar88] where blocks (playing the role of fixed capacity servers) can only be
added to and removed from the top of the current memory.
3The popular name “Randezvous Hashing” appears to stem from an earlier technical report, but even in that report, randezvous
is used about a more general situation, and not about the Highest Random Weigth Hashing that has now become known popurlarly
known as Randezvous hashing.

1

we have to move the clients belonging to it. On the average, we therefore have to move least mn clients when
a server is added or removed.
With our solution, while guaranteeing a balancing parameter c = 1 + " 2, when a client is added or
removed, the expected number of clients moved is O( 1"2 ). When a server is added or removed, the expected
number of clients moved is O( m"2n) (Theorem 3). These numbers are only a factor O( 1"2 ) worse than the
general lower bounds without capacity constrains. For balancing parameter c 2, our expected number of
moves is increased by a factor 1 + O(log cc ) over the lower bounds (Theorem 2). The bound for c 2 is
the most challenging to prove. It implies that for superconstant c, we only expect to pay a negligible cost in
extra moves.
From a more practical perspective, it is useful that our algorithm provides a simple knob, the load
balancing parameter c = 1 + ", which captures the tradeoff between load balancing and stability upon
changes in the system. This gives a more direct control to the system designer in meeting explicit balancing
constraints.
We get the same bounds for the simpler problem where we instead of a user specified balancing param-
eter have a fixed bin capacity C for all bins and define c = 1 + " = Cn=m. An interesting case of light
loads is the unit capacity case where each server can only handle a single client at the time.

Applications. Consistent hashing has found numerous applications [ OV11,GF04] and early work in this
area [KLL+97, SMK+01, SML+03] has been cited more than ten thousand times. To highlight the wide
variety of areas in which similar allocation problems might arise, we just mention a few more important
references: content-addressable networks [RFH+01], peer-to-peer systems and their associated multicast
applications [RD01, CDKR02]. Our algorithm is very similar to consistent hashing, and should work for
most of the same applications, bounding the loads whenever this is desired. In fact, our work has already
found two quite different industrial applications; namely Google’s cloud system [MTZ16] and Vimeo’s
video streaming [Rod16]. Both systems had to handle the lightly loaded case. Also, in both cases, load
balancing was not an objective to maximize, but rather a hard constraint, e.g., in the Vimeo blog post
[Rod16], Rodland describes how no server is allowed to be overloaded, and how he found a load balancing
parameter c = 1:25 to be satisfactory for Vimeo’s video steaming. We shall return to this later.

### 1.1 Background: Consistent hashing

The standard solution to our fully-dynamic allocation problem is consistent hashing [SML+03,KLL+97].

Simple consistent hashing. In the simplest version of consistent hashing, we hash the active balls and
bins onto a unit circle, that is, we hash to the unit interval, using the hash values to create a circular order
of balls and bins. Assuming no collisions, a ball is placed in the bin succeeding it in the clockwise order
around the circle. One of the nice features of consistent hashing is that it is history-independent, that is, we
only need to know the IDs of the balls and the bins and the hash functions, to compute the distribution of
balls in bins. If a bin is closed, we just move its balls to the succeeding bin. Similarly, when we open a new
bin, we only have to consider the balls from the succeeding bin to see which ones belong in the new bin.
With m balls, n bins, and a fully random hash function h, each bin is expected to have m=n balls. This
is also the number of balls we expect to move when a bin is opened or closed.
One problem with simple consistent hashing as described above is that the maximum load is likely to be
(log n) times bigger than the average. This has to do with a big variation in the coverage of the bins. We
say that bin b covers the interval of the cycle from the preceding bin b0 to b because all balls hashing to this

2

interval land in b. When n bins are placed randomly on the unit cycle, on the average, each bin covers an
interval of size 1=n, but we expect some bins to cover intervals of size (log nn ), and such bins are expected
to get (m log nn ) balls. The maximum load is thus expected to be a factor (log n) above the average.
A related issue is that the expected number of balls landing in the same bin as any given ball is almost
twice the average. More precisely, consider a particular ball x. Its expected distance to the neighboring
bin on either side is exactly 1=(n + 1), so the expected size of the interval between these two neighbors is
2=(n + 1). All balls landing in this interval will end in the same bin as x; namely the bin b succeeding x.
Therefore we expect 2(m 1)=(n + 1) 2m=n other balls to land with x in b. Thus each ball is expected
to land in a bin with load almost twice the average. If the load determines how efficiently a server can serve
a client, the expected performance is then only half what it should be.
In [KLL+97] they addressed the above issue using virtual bins as described below.

Uniform bin covers. To get a more uniform bin cover, [KLL+97] suggests the use of virtual bins. The
virtual bin trick is that the ball contents of d = O(log n) bins is united in a single super-bin. The d bins
making up a super bin are called virtual bins. We have only n0 = n=d super bins and these super bins
represent the servers. A super bin covers the intervals covered by its d virtual bins. The point is that for any
constant " > 0, if we pick a large enough d = O(log n), then with high probability, each super bin covers a
fraction (1 ")=n0 of the unit cycle.
We note that many other methods have been proposed to maintain such a uniform bin cover as bins
are added and removed (see, e.g., [BSS00, GH05, Man04, KM05, KR06]). Different implementations have
different advantages depending on the computational model, but for the purpose of our discussion below, it
does not matter much which of these methods is used.
We also note that what corresponds to a perfectly uniform bin cover can be obtained using Rendezvous
or Highest Random Weight Hashing [TR98]. A hash function assigns a random weight h(q; b) to each ball
q and bin b. A ball q is placed in the current bin b that maximizes h(q; b). If h is min-wise independent
[BCFM00], then b is uniformly random among the current bins, corresponding to a perfectly uniform bin
cover. An issue with this scheme is that to place the ball q, we need to know all the current bins.
With a uniform bin cover, balls distribute uniformly between bins. However, with n balls and n bins,
we still expect many bins with ((log n)=(log log n)) balls even though the average is 1. On the positive
side, in the heavily loaded case when m=n is large, e.g., m=n = !(log n), all loads are (1 ")m=n w.h.p.
However, in this paper, we want a good load balancing for all possible load levels.

### 1.2 Our algorithmic solution: Respecting bin capacities via forwarding.

We describe our full algorithm in this subsection and provide some intuition behind its logic. An important
part of our solution to the load balancing problem is to introduce a capacity for each bin that may not be
exceeded by the load of the bin. In general, such a capacity could be a direct result of a resource constraint on
the bin, but here we will use it to enforce a strict load balancing, guaranteeing for some balancing parameter
c > 1, that no bin has more than dcm=ne balls.
In the description below, we will first describe the system for the case where each bin comes with its
own fixed capacity. At the end of the section, we will elaborate on the exact vector of capacities to achieve
desirable load balancing.
Our starting point is simple consistent hashing (without virtual bins). Bins and balls are hashed to a
unit circle with a fixed hash function that is not changed once the system is is started. For efficiency, the
hash function should be fixed at random, but the system described below is well-defined for any given hash

3

function. For now, we assume that balls and bins all hash to distinct locations so that we get a complete
cyclic ordering of the balls and bins along the cycle. A ball hashes to the bin following it in clockwise order,
but if the number of balls hashing to a bin exceed its capacity, then some of the balls have to be placed in
other bins.

Simple insertions via forwarding. Suppose for now that we are only adding balls to a fixed set of bins.
To deal with bin capacities, if a ball hashes to a full bin, then it is forwarded around the circle until it finds a
bin that is not full, and then it is placed in that bin.
This forwarding is the basic idea behind linear probing. It appears not to have been suggested before in
the context of Consistent Hashing.

General system invariant between updates. The location or placement of a ball may depend on the order
in which the balls are added, but regardless of the insertion order, the forwarding from full bins maintains a
simple invariant that we discuss more generally before discussing other updates to the system.
If a ball q hash to a bin b but is located in some other bin b0, then we say that q passes all the bins from
b to the bin preceeding b0 in clockwise order. The above forwarding from full bins maintains.

Invariant 1. No ball passes a non-full bin.

In Section 2.1 we prove that Invariant 2.1 uniquely determines the load of each bin. This is for a given
set of balls and bins with given capacities, but independent of history.
As balls and bins are added and removed we will maintain Invariant 1. More precisely, after each of
these updates, we assume we have time to settle the system, moving balls so as to restore Invariant 1 and
satisfy all capacity constraints before we get another update to the system.

Searching a ball in the bins Assuming Invariant 1, it is easy to find a ball q among the bins. First we
search the bin b that q hashes to. If q is not found and if b is full, we move to the next bin b0 and recurse until
either we find q or we reach a bin b0 without q that is not full. In the latter case, by Invariant 1, we conclude
that q is not in the system. If q is to be inserted, it should be inserted in b0.

General insertions and forwarding from overfull bins Above, when we inserted a ball, we just placed
it in the first non-empty bin. We refer to this as simple insertions. We shall here allow for more flexible
non-deterministic insertions.
Temporarily, we allow a bin to be overfull in the sense that the load exceeds the capacity. When a ball
arrives, it is first placed in the bin it hashes to, which may become overfull. If the system has an overfull
bin, we forward any of its balls to the next bin.
In relation to Invariant 1, we view an overfull bin as full, not non-full. This means that Invariant 1 is
never violated in the above process, so we are done when there are no overfull bins.
The previously described simple insertions correspond to always forwarding the most recent arrival, but
now we are free to choose which ball to forward. For example this allows a history independent system
using the idea from [BG07]. It assumes a linear order on all balls which is independent of the order in which
they are inserted. When placing balls in bins, we place them as if they were inserted lowest to highest using
simple insertions. To maintain this history independence with general insertions, we always forward to the
highest balls from an overfull bin.
When we later analyze the efficiency of our system, we will allow for general insertions with no restric-
tions on which balls get forwarded from overfull bins.

4

We will generally use the number forwardings from overfull bins as an upper bound on the number of
balls moved, but note that the actual number of moves may be much smaller, e.g., with the simple insertion,
we are just moving the inserted ball directly to the first non-full bin.

Removing a bin Removing a bin has the same effect as setting its capacity to 0. It is now overfull, and
then we forward balls from overfull bins arbitrarily until no bin is overfull. Invariant 1 was never violated.

Deleting balls and filling holes When we delete a ball from a bin, we may have to replace it. It is
convinient to talk about it as filling holes, where the number of holes in a bin is the number of balls it is
missing to get full. When we delete a ball, we create a new hole.
Our concern about a new hole in a bin b is if b was full and passed by a ball q in some later bin b0. The
hole renders b non-full and then Invariant 1 is violated. We can then fill the hole by moving q to b, but this
creates a new hole in b0 that we may have to fill recursively. Invariant 1 is restored when no hole is passed.
The process never violates any capacity constraint, so we are done.
Inspired by deletions in linear probing, an efficient way to fill holes, starting from the first hole in some
bin b, is to scan the bins one by one. When we get to a bin b0 with a ball q that hash to or before b, we fill
b with q, and proceed from b0. This way we never consider the same bin twice, and we can stop when we
meet a non-full bin. We shall discuss an even more efficient implementation in Section 5 where we maintain
the number of balls passing each bin. This allows us to stop as soon as we create a hole in a bin that is not
passed. For now, however, we are focussed on bounding the number of moves.

Adding a bin When a bin is added, it starts with as as many holes as its capacity. We then keep filling
holes as long as possible so Invariant 1 is restored. No capacity constraint is violated in this process.

Changing Capacities for Load balancing in Dynamic Environments. We will now show how to set and
change capacities to achieve good load balancing. For a given load balancing parameter c = 1 + " > 1,
we want to guarantee that no bin has more than dcm=ne balls. One possibility would be to just say that
all bins had capacity dcm=ne, but then adding a single ball could force us to increase the capacity of all
bins, completely changing the configuration. As a result, we need to be careful about enforcing the above
capacity constraints across all bins. In particular, to minimize the number of capacity changes when balls
are inserted or deleted, we aim for a total bin capacity of dcme. Assuming a linear ordering of the bins, we
let the lowest dcme nbcm=nc bins have capacity dcm=ne while the rest have capacity bcm=nc. We refer
to the former bins as big bins and the latter bins as small bins, though the difference is only 1. Moreover, as
an exception to the above rule, we will never let the capacity drop below 1, that is, if cm < n, then all bins
have capacity 1.
For a given set of balls and bins, with a given linear order on the bins, the above protocol tells us the
exact capacity of each bin. When we add or removing a ball, it affects the capacity of at most dce bins: if
the ball is added, we increment capacity, and if it is deleted, we decrement capacity. If a bin is inserted or
deleted, it changes at most at dcm=ne capacities. If capacities are increased, we may have to fill holes, and
if capacities are decreased, we may have to forward balls.

Separating capacity changes from ball and bin updates. For the sake of later analysis and efficiency, we
make the rule that if a ball is inserted or a bin is deleted, we first do all the required capacity increases, one
by one, settling the system after each capacity increase by hole filling. When the capacities have all settled,
we add the ball or remove the bin, upon which we do the final settlement by forwarding from overfull bins.

5

Conversely, when a ball is deleted or a bin is inserted, we settle by hole filling before we decrease any
capacity. Next we do the required capacity decreases one by one, forwarding from overfull bins after each,
until we are back to the desired settled system satisfying Invariant 1 and all capacity constraints.

Concrete random hash functions The system as described above always works correctly with a fixed
hash function, but an adversary knowing our hash function could force us to make way too many moves. In
order to claim efficiency, we use a hash function that is fixed randomly and independent of the operations
performed on the system. Thus we can think of the updates and searches for balls in bins as chosen in
advance by an adversary that is oblivious to what goes on inside the system.
In practice, we work with hash functions with a limited range [r] = f0; ::::; r 1g. Mapping this range
to a circle, position 0 succeeds r 1. We assume that the hash function for balls is independent of the hash
function for bins. Unless otherwise stated, the bounds in this paper only assume that r n and that both
the ball hash function and the bin hash function are 5-independent (simple tabulation [PT12] will also work
even though it is only 3-independent). With limited-range hash functions, we may have collisions. To get a
complete cyclic order, we do the following tie breaking: if two balls or two bins hash to the same location,
then the one with the lower ID precedes the one with the higher ID. Moreover, if a ball and a bin hash to the
same location, the ball precedes the bin. This implies that the bins hashing to a given position x will always
be filled bottom-up.

### 1.3 Our Results: Theoretical and Empirical

In this subsection, we state our main theorems and other results on the above system. Subject to the capacity
constraints, our main focus in this paper is the expected number of balls that have to be moved when a ball
or bin is inserted or deleted. Our bounds will hold no matter which balls we decide to move when a bin
is overfull or a hole has to be filled, as long as we follow the above description. We shall discuss how to
efficiently identify the balls to be moved in Section 5.
Mathematically, the most interesting case is when c = !(1). We note that inserting a ball results in
up to dce bins increasing their capacity. Nevertheless, besides placing the new ball, we will prove that the
expected number of ball moves is O((log c)=c) = o(1).
The general result is

Theorem 2. For a given load balancing parameter c 2, the expected number of bins visited in a search
is 1 + O((log c)=c). When a ball is inserted or deleted, the expected number of other balls that have to be
moved between bins is O((log c)=c). When a bin is inserted or deleted, besides moving O(m=n) expected
balls hashing directly to the bin, we expect to move O((m=n)(log c)=c) other balls.

For the insertion and deletion of bins, Theorem 2 implies that the expected number of moves is O(m=n).
We distinguish the balls hashing directly to the bin so as to allow a direct comparison with simple consistent
hashing without capacity constraints [KLL+97]. For simple consistent hashing, the balls affected by the
insertion or deletion of a bin are exactly the balls hashing to it. We expect O(m=n) such balls (previously,
we have said that exactly m=n balls were expected to hash to any given bin, but that was assuming ideal
fully random hash functions). With our capacity constraints, we only expect to move O((m=n)(log c)=c)
other balls. The price we pay for guaranteeing a maximum load of dcm=ne is thus only a multiplicative
factor 1 + O((log c)=c) = 1 + o(1) in the expected number of ball moves.
For c 2 (1; 2], we parameterize by " = c 1 > 0.

6

Theorem 3. For a given load balancing parameter c = 1 + " 2 (1; 2], the expected number of bins visited
in a search is O(1="2). When a ball is inserted or deleted, the expected number of other balls that have to
be moved between bins is O(1="2). When a bin is inserted or deleted the expected number of balls that have
to be moved is O(m=(n"2)).

The bounds of Theorem 3 are similar to those obtained in [PT12] for linear probing. The challenge here
is to deal with the fact that bins are randomly placed, as opposed to linear probing where every hash location
has a bin of size 1. Nevertheless we will be able to reuse some of the key lemmas from [PT12]. The proof
of Theorem 2 is far more challenging, and the main focus of this paper.

Remark 4. The bounds from Theorems 2 and 3 also hold in the simpler case where all bins have a fixed
capacity C and we define c = 1 + " = Cn=m. We note that our updates change the value of m and n,
hence of c = Cn=m. For the bounds to apply, we always use the smaller value of c in connection with each
update. Thus, for the bounds on the moves in connection with a ball insertion or bin removal, we use the
value of c before the update. For the bounds on the moves in connection with a ball deletion or bin addition,
we use the value of c after the update.

Additional results. In this paper, we are also going to discuss how to efficiently find the balls to be moved
in connection with system updates. We are also going to discuss high probability bounds that are, essentially,
a factor O(log n) worse than the above expected bounds. Finally, we note that in practice, it may be relevant
to study our problem with weighted balls. In this case, the sum of the weights of the balls assigned to a
bin should not exceed its capacity. With weights, the results get less clean, e.g., a single weight might be
bigger than the allowed capacity. Also, there may be situations where we have to move a large number of
light balls to maintain balance. However, if we use integer capacities as described above, and if no balls has
weight above 1, then the above results hold if we replace “number of balls” with “weight of balls”. We note
that in the non-weighted case, the probability bounds in this paper are about sums of binary 0 1 variables.
However, the extend easily to sums of real variables in range [0; 1]. To avoid corner cases, we should assume
no ball has weight zero, i.e. all weights are bounded below from zero by some constant.

Empirical Results and Industrial use. We confirm effectiveness of our algorithm in various practical
settings via an empirical study in section 7. Furthermore, we note that versions of our algorithm have been
deployed in a number of industrial applications. The first industrial application of our algorithm was in
Google’s Cloud Pub/Sub as described in [MZ17]. This application needed good load balancing in a variety
of instances with different characteristics including the lightly loaded case. History independence was also
necessary in that application.
Surprisingly, within a few months of the first release of this paper on arXiv [MTZ16], our algorithm
got picked up by the video streaming company Vimeo. In a Vimeo blog post [Rod16], citing our paper,
Rodland describes how Vimeo used our algorithmic idea to solve the scaling issues they had in handling
almost a billion requests per day (they have 170 million users). They implemented a version of algorithm
with balancing parameter 1.25, and then the problems vanished as seen in Figure 1. In [Rod16], they also
explain that they had given up on traditional consistent hashing and power of two choices because of load
balancing problems. Finally [Rod16] explains that an open source version of the code has been realased
HAProxy 1.7.0, and we expect that other companies will start using it.
The Vimeo story is a good example of how theory can have impact. We offer a simple algorithmic
solution to the load balancing issue. We are not claiming to have a theoretical model that captures all the
important aspects of performance since it depends on the concrete implementation context. What we do

7

Figure 1: This figure is copied from [Rod16]. It shows Vimeo’s use of shared cache bandwidth over time.
The bandwidth problems disappeared when they switched to a version of our algorithm on October 26.

offer is a theoretical analysis, showing that for every possible input, the algorithm has very good expected
performance on important combinatorial parameters, including guaranteed load balancing for all possible
inputs. This is something that can never be verified by tests, but it is very valuable for the trust in an online
dynamic system that is meant to run “forever” not knowing future inputs.

Note about simplicity and general applicability. Consistent hashing is a simple versatile scheme that has
been implemented in many different systems with different constraints and performance measures [ OV11,
GF04]. Our consistent hashing scheme, respecting capacities via forwarding, is almost as simple, and
should work with most of these implementations. The classic implementation of consistent hashing is the
distributed system Chord [SMK+01, SML+03] which has more than ten thousand citations. The Chord
papers [SMK+01,SML+03] give a thorough description of the many issues affecting the design. Below we
only consider the aspects affecting the message complexity of finding a ball among the bins.
In Chord, they have a system of pointers so that given an arbitrary point on the cycle, they can find the
next bin in the clockwise order using O(log n) messages. This is how they find the bin a ball hashes to.
In simple consistent hashing, this is where the ball is to be found. With our forwarding, starting from the
bin a ball hashes to, we need to visit succeeding bins until we either find the ball or a non-full bin. Chord
does maintain explicit successor pointers between neighboring bins, so we only have to pay O(1) extra
messages per bin visited. By Theorem 3, we expect to visit O(1="2) bins, so our total expected message
cost is O(log n + 1="2). The extra O(1="2) is negligible if " = !(1=

p
log n).
Of course, there may be other systems with different costs associated, but generally we assume it to be
significantly more expensive to find the bin a ball hashes to than it is to go from one bin to its succeeding
neighbor. Since the costs depend on the implementation context, our focus here is on fundamental combi-
natorial properties like loads, the number of bins searched, and the number of balls that are moved when the
system is updated.

### 1.4 Power of multiple choices as an alternative?

We have proposed applying the idea of forwarding in the style of linear probing to bound the loads in
consistent hashing. The reader may be wondering if alternative ideas in dynamic load balancing can be
applied to deal with our natural problem. Here, we discuss them, and highlight their drawbacks or challenges

8

to convert them to working solution. In particular, we discuss the possibility of instead using the power of
multiple choices [ABKU99, PR01, Mit01, BCSV00, TW14, Voc03]. The discussion is speculative in that
none of these techniques have been analyzed in our dynamic context where both balls and bins can be added
and removed. We can try to guess what may happen, but have limited knowledge without a proper analysis.
The most basic form is that we have a fixed set of n bins. Balls are added, one by one. Each ball is
given d uniformly random bins to choose from, and picks the least loaded, breaking ties arbitrarily. With
m balls, w.h.p., we end up with a maximum load of m=n + ln ln n
ln d + (1) [BCSV00]. An interesting
twist suggested by Vocking is that if several bins have the same smallest load, we always pick the left-
most [Voc03]. Surprisingly, this reduces the max load to m=n + (1 + ln ln nd ) [Voc03]. We note that to get
a constant ratio between maximum and average load when the average load is constant, we do need a super
constant number of choices, e.g., d = (log log n) with left-most choice.
The above mentioned bounds are proved in the ideal case where we pick uniformly between the bins.
Consider now first the case of simple consistent hashing where both balls and bins are placed at random on
a unit circle, and a ball goes to the succeeding bin in clockwise order. This case was studied in [BCM03,
BCM04], where it was proved that if m = n, then the maximum load is O(log log n). However, with a
concrete example, [Wie07] showed that we cannot in general hope to get max-load m=n + O(log log n)
when m n log log n. This is again for the case of simple consistent hashing where bins are just placed
randomly on the circle. However, using, e.g., virtual bins, we know that that we can obtain a more uniform
bin cover such that each bin represents a fraction (1 ")=n of the unit cycle and where a ball lands in a bin
with this probability. With " = 1=2, the main result from [Wie07] implies that using the power of d choices,
w.h.p., we get a maximum load of m=n + O(log log n
log d ).
We still have to consider what happens in our dynamic case where balls may be deleted and bins can
be added and/or removed. The results from [CFM+98] indicate that to delete a ball, it may suffice to just
remove it without moving any other balls. However, if a bin is removed, we have to move all its balls. In
order to claim any bounds, we would need a careful analysis, but the best we can possibly hope for is to
match the above bounds for the basic case where m balls are just added picking uniformly between n fixed
bins.

Cuckoo hashing We will now discuss how the power of multiple choices could possibly be used in the
style of Cuckoo hashing [PR04] to provide balancing guarantees like those in our solution. With user
specified load balancing parameter c = (1 + "), we assigned capacities bcm=nc or dcm=ne to each bin in
such a way that the total capacity was dcme and such that we only changed at most dce bin capacities in
connection with each ball update. We then used forwarding in the style of linear probing to respect these
capacities. Obviously, one could try to adapt many other hash table schemes to respect these capacities.
Most notably, we could hope to adapt Cuckoo hashing [PR04].
We now review the basic results for Cuckoo hashing in the ideal case where we have a fixed set of n
bins, all of the same capacity C, and where each of m balls gets d uniformly independent choices between
these bins. The basic feasibility question is how large C has to be before we expect to be able to place all
balls. This question is studied in [FKP16], but here we also want to update the system efficiently as balls
and bins are inserted and remvoed. In the original Cuckoo hashing [PR04], the capacities are all 1. With 2
choices, the balls are placed into n = (2+")m bins for any constant ". However, [FPSS05] proves that using
d = O(log(1=")) choices, we can place the balls in only n = (1 + ")m bins, so the maximum load of 1 is at
most (1 + ") times the average load, as desired for load balance (1 + "). For efficient insertions, [FPSS05]
prove that if d 5 + 3 ln(1="), then a ball can be placed in dO(log(1=")) = (1=")O(log log(1=")) expected time
and in o(n) time with high probability. The high probability bound on the insertion time is improved to a

9

polylog in [FPS13]. Moreover, using a larger value of d = O

log(1=")
"

, it was recently shown [FJ17] that
the expected search time can be reduced to O(1) independent of ". In [DW07] they consider having only
d = 2 choices, but instead they increase the capacity. They show that a bin capacity of size O(log(1="))
suffices to distribute the balls with load balance (1 + "). For efficient insertions, they prove that if the
capacity is above 16 ln(1="), then a ball can be placed in log(1=")O(log log(1=") expected time. The bounds
require fairly complex hash functions, but [DW07] states that the hash functions can be implemented in
O(n5=6) space. A further discussion of hash functions for Cuckoo hashing is found in [ADW14].
We note that it is would require a full analysis to figure out to which degree the above bounds transfer to
our fully dynamic case where bins can be added and removed, and where capacities adapt to the load based
on the balancing parameter. Also, we would most likely need to use virtual bins to get a reasonably uniform
bin cover. The best we can hope for is to match the above bounds for the simpler case with uniformly
distributed choices between a fixed set of bins. This may indeed be possible, and would be interesting.

Stepping back, the first contribution of this paper is to raise the problem of getting consistent hashing to
obey a user specified load balance paremeter c = (1+") for all possible inputs. We present the first solution
with proven bounds on the efficiency of searches, ball and bin updates. When " is a positive constant, our
expected bounds are only a constant factor from the general lower bounds.
It may very well be possible to get a different solution based on Cuckoo hashing instead of linear
probing, getting bounds similar to those reviewed above for a fixed set of bins, all with the same capacity.
However, even if this can be done, it would not always be better than our solution. One of the attractions of
d 2 choices is that a ball only has to be searched in d bins, but these are d random bins, as opposed to the
single segment of consecutive bins searched with our solutions. If we, as Vimeo, use balancing parameter
1 + " = 1:25, then, by Theorem 3, we only expect to consider a constant number of consecutive bins. Thus,
if, as in the Chord implementation, the cost of accessing a random bin is much bigger than that of getting
from a bin to its successor, then our search is nearly twice as efficient as checking 2 random choices (for
successful searches, in expectation, we can bring 2 down to 1.5). With Cuckoo hash tables, it is often seen
as an advantage that the d choices can be checked in parallel, but here we imagine a large system processing
many requests in parallel, and then the total number of messages is important. With updates we have the
same advantage of only working within a single segment of bins whereas Cuckoo hashing has to consider
arbitrary bins. However, the Cuckoo updates are still to be defined and analyzed for us to understand their
efficiency when bins are inserted and deleted and capacities change. Moreover, for the implementation of
Cuckoo hashing, we probably need the added complexity of, e.g., virtual bins to get a more uniform bin
cover. Thus, we expect our linear forwarding solution to remain relevant regardless of future developments
with multiple choices. The situation is like linear probing versus Cuckoo hashing for regular hash tables.
Both are very important solutions to an extremely important problem, and each has its advantages in different
contexts, e.g., with Cuckoo hashing, the queries are easy while the updates are more challenging. For our
problem of consistent hashing with bounded loads, we prove here that the forwarding of linear probing
works in a simple practical solution. A corresponding analysis for Cuckoo hashing is yet to be done.

## 2 High Level Analysis

To analyze the expected number of moves, we shall use a general probabilistic understanding of configura-
tions encountered. By a configuration, we refer to the situation between updates where the allocation has
settled to satisfy Invariant 1 and with no overfull bins.

10

### 2.1 Uniqueness of loads

As a first step in understanding configurations, we argue that Invariant 1 uniquely determines the load of all
bins. This is for a given set of balls and bins with given capacities, but independent of history. This is a
very simple generalization of the argument that the set of cells that are filled by linear probing is unique and
history independent.
First we note that Invariant 1 implies that which bins are full is independent of the order in which the
balls are inserted. It only depends on which balls and bins are present and where they hash to. More
precisely, we have

Lemma 5. A bin b is full if and only if there is an interval of consecutive bins B = b1; :::; bk ending in
b = bk such that the total number of balls hashing to these bins is at least as big as their total capacity.

Proof. If b is full, we take B = b1; :::; bk to be the maximum interval of full bins, that is, the bin b0 preceding
b1 is not full. By Invariant 1, this means that no balls hashing to or before b0 can end in B, so B must be
filled with balls hashing to B. In the other direction, the result is trivial if all balls hashing to B end in B,
since there are enough balls to fill all bins. However, if a ball hashing to B ends up after bk, then bk is full
by Invariant 1.

In fact, the hashing of balls and bins determines completely the number of balls landing in any given bin
b. If the bin is full, this follows from Lemma 5. Otherwise, the number of balls landing in b is determined
by Lemma 6 below.

Lemma 6. If a bin b is not full, consider the longest interval b1; : : : ; bk of full bins leading to b, that is, b
succeeds bk and the predecessor b0 of b1 is not full. If the predecessor of b is empty, with have k = 0, hence
no bins in b1; : : : ; bk.
Then the balls landing in b1; ; bk; b are exactly the balls hashing to these bins. If s balls hash to
b1; ; bk; b and b1; ; bk have capacities C1; ; Ck, then we have exactly s

Pk
i=1 Ck balls landing
in b.

Proof. The result follows by Invariant 1 together with the fact that b and b0 are not full.

Summing up the two previous lemmas, we have

Lemma 7. The set of balls and the set of bins with their capacitities uniquely determine how many balls
land in each bin.

### 2.2 Expected distance to non-full bin

Suppose we have m balls and n bins that are currently in the system. We refer to these balls and bins as
active. We will also study passive balls and bins that are not currently in the system, yet which have hash
values that will be used if they get inserted. For some c > 1, the total capacity will be exactly cm. Since no
bin has capacity below 1, we always have cm=n 1. In our analysis, we will only assume that each bin has
a capacity between cm=(2n) and 2cm=n, and that the concrete capacites are independent of the hashing of
balls and bins. We note that this is always satisfied when bin capacities are at least 1 and differ by at most 1.
Theorem 8 below gives our main technical understanding of configurations for larger c. It does not make
any assumptions about how we reached the configuration as long as Invariant 1 is satisfied with the desired
bin capacities. Theorem 8 may seem too complicated, but this is exactly the technical challenge of this
paper: the devil in the details. We need to exploit every detail of the theorem to get the bounds we have

11

claimed in the introduction. The proof of Theorem 8 is based on a very delicate analysis of the interaction
between the balls and bins.

Theorem 8. Consider a configuration with m active balls and n active bins and total capacity cm for some
c 2. Suppose, moreover, that each bin has capacity between cm=(2n) and 2cm=n. Then

(a) Starting from the hash location of a given passive ball or active or passive bin, the expected number
of consecutive full bins is O(1=c).

(b) If we start from a given active bin of capacity at least 2, the expected number of consecutive full bins
is O((log c)=c2).

(c) The expected number of balls hashing directly to any given active bin is O(m=n). The expected
number of balls forwarded into the bin is O((m=n)((log c)=c2)). Finally, if a bin i is not active, and
its active successor i0 is given an extra capacity of one, then the expected number of full bins starting
from i0 is O((log c)=c2).

The above statements are satisfied if the balls and bins are hashed independently, each using 5-independent
hash functions or simple tabulation hashing. The statement of (c) may seem a bit cryptic, and will make
more sense in the context of the analysis it is used in below.

The worst-case for our bounds is when the capacities are 1 and 2. This case explains why (a) would not
work for an active ball since an active ball by itself could fill a bin of capacity 1. However, when a ball q
is inserted, we do forwarding to the nearest non-full bin in the configuration before q is insertion where q is
still passive, and therefore Theorem 8 (a) gives the expected number of fulls bins passed.

Corollary 9. With balancing parameter c = 1 + " 2 (1; 2], the expected number of bins visited in a search
is 1 + O((log c)=c) if c 2, and O(1="2) if c 2.

Proof. If the ball q searched is not in the system, then the search is to only up to and including the first non-
full bin. However, if q is in the system, the latest it can be placed is if was added last, which corresponds is
the first non-full bin if in the system without q. So in both cases, the expected search is bounded as one plus
the number of consecutive full bins starting from an passive ball as in Theorem 8 (a).

### 2.3 Bounding the expected number of moves

We are now going to prove Theorem 2 applying Theorem 8 to settled configuations. If the settled configua-
tions has m active balls, n active bins, and total capacity cm, then we need to make sure that c c where
c is the fixed parameter chosen to control the bin capacity. The bounds only get better with larger c. We
know that that before and after every update we have total capacity dcm=ne = cm=n so c c. However,
we will also argue about intermediate configurations, e.g., when we insert a ball, we first implement capac-
ity increases one by one, settling into a valid configuration after each capacity increase, but these capacity
increases will only increase c since m has not increased yet.
The moves have already been described in Subsection 1.2, but we review them below for the sake of
analysis.
First we discuss plain inserations and deletions without any changes to bin capacities.

12

Plain insertion Consider an insertion of a ball q hashing to some bin i. For now, we ignore that some
capacities have to be increased. First we place q in i, which may become overfull. If so, we forward some
ball to the succeeding bin i0, repeating until we reach a bin that has room because it was not full. The
important thing to note is that we move at most one ball per full bin encountered, and that we stop when we
meet a bin that was not full before the insertion. By Theorem 8 (a) applied to the configuration before the
insertion, the expected number of moves, excluding the initial placement, is O(1=c) = O(1=c).

Plain deletion When we delete a ball q from a bin i, we create a hole in i that we try to fill with a ball
q0 that has passed bin i in the sense that q0 resides in a later bin i0 but hashes to bin i or some earlier bin.
If we succeed, we recursively try to fill the new hole left by q0 in i0. Suppose the last hole created is in
bin i00. We have now completed the deletion with a new valid configuration. We know that all bins from i
to the bin preceding i00 are full while i00 is not full. The number of moves, including the initial deletion of
b, is bounded by the number of bins from i to i00. By Theorem 8 (a) applied to the configuration after the
deletion, it follows that the expected number of moves, including the final removal, is 1 + O(1=c) but now
c is measured right after the deletion where we have only m 1 balls so c = dcm=nem=(m 1) > c.

Full deletion To complete a deletion, we have to do c 1 capacity decreases. Since the ball has been
removed, we have m0 = m 1 balls during the capacity decreases that all together bring the the capacity
down to dcm0e. Doing one capacity decrease at the time, we know that every configuration encountered has
total capacity D dcm0e, and hence c = D=m0 c. We recall that the bins picked for capacity decreases
are chosen independently of the random choices made by the hash functions.
When we decrease the capacity of a bin i, if the bin now has one ball more than its capacity, we perform
exactly the same process as when we hashed a ball to a full bin, forwarding a ball until we reach a bin that
was not already full. The number of moves is bounded by the number of consecutive full bins starting from
i in the configuration before the capacity of bin i was decreased.
A crucial observation is that since the lowest possible capacity is 1, bin i had capacity at least two before
the decrease, so by Theorem 8 (b) applied to the configuration before the capacity decrease, the expected
number of moves is O((log c)=c2) = O((log c)=c2). We are doing at most dce such capacity decreases, so
the total expected number of moves, including the plain deletion of q itself, is bounded by

1 + O(1=c) + (1 + c)O((log c)=c2) = 1 + O((log c)=c):

Full insertion A full insertion is symmetric to a full deletion, doing c 1 capacity increases before we do
the plain insertion. The expected number of moves, including the the addition of q itself is 1+O((log c)=c).

Bin updates and forwarding When we do bin updates, there may be a lot possibilities for which balls to
move. We will argue that the concrete choices do not matter for our overall bounds.
Note that for the placement of balls in bins according to Invariant 1, we can think of all passive bins as
active bins with capacity zero. Then adding a bin with capacity C is like increasing the capacity to C, while
removing it is like decreasing its capacity to 0. The lemma below considers the general impact of changing
the capacity of a bin.

Lemma 10. Suppose we have a valid configuration, and decrease the capacity of some bin i from C+ to
C while leaving all other capacities unchanged. We assume that the total capacity remains strictly bigger
than the total load. Then the total number of forwardings done from overfull bins to reach a new valid

13

configuration depends only on which balls are in the system. The number of forwardings also bounds the
maximal number of moves done if we conversely increase the capacity from C to C+, filling holes to
restore Invariant 1.

Proof. The proof of Lemma 10 is fairly standard, thinking of forwarding as a flow of balls.
We know from Lemma 7 that the balls and bins with capacities uniquely determine the load of any bin.
In particular, this determines all bin loads before we do the capacity decrease.
When the capacity of bin i is decreased to from C+ to C, bin i may become overfull, and then we have
to forward some balls. Then, repeatedly, we take an arbitrary ball from any overfull bin and forward it to
the next bin, until no overfull bin remains. When we forward from a ball from an overfull bin it remains at
least full, so if i0 is the last bin we forward from, then all bins from i to i0 are full. Since the total capacity
remains strictly bigger than the total load, we cannot keep forwarding the whole way around the cycle.
Let i0; : : : ; ik be the bins from i to i0. For j = 0; : : : ; k 1, let Qj be the set of balls that got forwarded
from ij. Then Q0 are all balls forwarded from i0 so jQ0j is exactly the number of balls with which b0
exceeded the new capacity C. For j = 1; : : : ; k 1, Qj consists of balls that either started in bin ij or
came from Qj1. Since bin ij ends up full, jQjj jQj1j is exactly the number of balls bin ij needed to fill
its capacity. We have jQjj = jQj1j if bin ij was full before the capacity decrease; otherwise jQjj < jQj1j.
The process stops at bin ik because adding jQk1j balls to bin ik does not exceed its capacity.
From the above description, it follows that the numbers jQ0j; : : : ; jQk1j are uniquely determined by
the bin loads before the capacity decrease, and these were uniquely determined by the balls and bins with
capacites. In particular, this determines the total number of forwardings

Pk1
j=1 jQjj.
We now consider the reverse operation, increasing the capacity of bin i from C to C+. Even before
the capacity increase, the total capacity exceeds the total number of balls, so there is some first non-full bin
i00 following i in the clockwise order. By Invariant 1 there is no ball located after bin i00 that hash to bin i00
or before.
After increasing the capacity of bin i, we get C+ C new holes in bin i, and then start trying to fill
new holes recursively: if we have a new hole in some bin i0, we look for a ball hashing to a later bin that
hash to bin i0 or an earlier bin. We know that we never have to look past bin i00. Let i0 be the bin furthest
from i that loses a ball, creating a new hole that cannot be filled.
Let i0; : : : ; ik be the bins from i to i0. For j = 0; : : : ; k 1, let Qj be the set of balls that were used
to fill a hole in bins i0; : : : ; ij with a ball starting from bins ij+1; : : : ; ik. In particular Q0 is the set of balls
moved to fill the holes in i, and then

Pk1
i=0 jQjj is an upper bound on the number of moves to fill holes.
We now observe that Q0; : : : ; Qk1 is a forwarding sequence from a valid configuration after the capac-
ity increase for bin i and to a valid configuration before the capacity increase, that is, we get back to the start
if we for j = 1; : : : ; k1, forward the balls in Qj from bin ij to ij+1. As we saw above, this implies that the
balls and bins with capacites uniquely determines jQ0j; : : : ; jQk1j, hence also our upper bound

Pk1
j=1 jQjj
on the number of hole filling moves.

The strength of Lemma 10 is that when analyzing the number moves in connection with a capacity
change, then we can look at forwardings in any order we want, and get bounds that hold for any sequence
of moves following the description in Subsection 1.2.

Closing a bin When closing a bin i, we are going to lose its capacity C dcm=ne. Our first action is to
increase by one the capacities of C other bins. Doing the increases first, we make sure that the total capacity
before every increase is always at least cm. An increase is the inverse of the decrease that we studied

14

under full deletions, so the expected number of moves for each increase is bounded by O((log c)=c2). The
expected total number of moves resulting from all C bin increases is thus

C O((log c)=c2) = O((m=n)(log c)=c):

We are now ready to start closing i, having made sure that the total capacity remains above cm. The closing
have the same effect as setting the capacity to zero, which may become overfull. We now have to forward
balls from overfull bins. By Lemma 10, when bounding the number of forwarding, we can perform them in
any order that is convinient for the analysis.
The first forwarding we do in our analysis is to transfer all the balls in i to its successor i0. The balls
from i either hashed directly to i, or they were forwarded from the predecessor of i. By Theorem 8 (c),
we expect to have O(m=n) balls that hashed directly to i and O((m=n)(log c)=c) forwarded to i from its
predecessor. The balls from i are now all moved to i0. Bin i is no longer active.
Now i0 may be overfull, and then we repeatedly forward balls from overfull bins until no overfull bins
remain and Invariant 1 is restored. Let i00 be the last bin receiving a ball from this forwarding. Then the
total number of forwardings is at most C times the number of bins from i0 to the predecessor of i00. All bins
from i0 to i00 are now full, but if we gave i an extra capacity of one, then i00 would have received one less
ball, hence not be full. Applying by Theorem 8 (c), to this situation, we know that the expected number
full bins starting from i0 to the predecessor of i00 is O((log c)=c2), so the expected number of forwardings
is C O((log c)=c2) dcm=neO((log c)=c2) = O((m=n)(log c)=c). It might seem that we could here
replace C with its expectation O(m=n), but then we would be multiplying two expectations that are not
independent.
Summing up, we have proved that when closing i, besides the transfer to i of O(m=n) expected balls
hashing directly to i, we have O((m=n)(log c)=c) ball moves. Here n is the number of bins before the
closing.

Opening a bin When we open a bin it is like increasing its capacity from zero to the desired capacity.
Thanks to Lemma 10, we can bound the number of moves simply refering to the bound from above on the
number of forwards used when we above closed a bin, decreasing its capacity to zero. The subsequenct
unit capacity decreases are also symmetric to the unit capacity increases we did when we closed a bin, so
get the same overall bounds for opening a bin as we did for closing a bin, that is, besides the transfer to i
of O(m=n0) expected balls hashing directly to i, we have O((m=n0)(log c)=c) ball moves. Here n0 is the
number of bins after the opening of bin i, but this only makes our bound better than if we used the number
n = n0 1 of balls before the opening.
This completes the proof of Theorem 2 assuming the correctness of Theorem 8.

### 2.4 Small capacities

In the same way that we proved Theorem 2 assuming Theorem 8, we can prove Theorem 3 using the
following theorem.

Theorem 11. Consider a configuration with m active balls and n active bins and total capacity (1 + ")m
for some " 2 (0; 1]. Suppose, moreover, that each bin has capacity between (1+")n=(2n) and 2(1+")m=n.
Then starting from the hash location of a given passive ball or active or passive bin, the expected number of
consecutive full bins is O(1="2).
Here we assume that balls and bins are hashed independently, each using 5-independent hash functions
or simple tabulation hashing.

15

As in the analysis for Theorem 2, we will be operating with an " " such that the total capacity is
exactly (1 + ")m. Applying Theorem 11 will then yield a bound of O(1="2) = O(1="2). However, it could
be that while " 1, we end up with " > 1. In this case, we will instead apply Theorem 8, and get a bound
of O((log(1 + "))=(1 + ")) = O(1) = O(1="2).
We note that Theorem 11 is much simpler than Theorem 8. The point is that Theorem 8 is used to prove
a loss by a factor 1 + o(1) relative to simple consistent hashing without capacities. For Theorem 3, the loss
factor is O(1="2), which is much less delicate to deal with, e.g., for the number of balls in a bin, instead
of analyzing the expected number, which is O(m=n), we can just use the hard capacity, which is at most
2(1 + ")m=n = O(m=n).

## 3 Analysing expectations with large capacities

In this section, we are going to prove Theorem 8.

### 3.1 Basic probability bounds

We first briefly review the probability bounds we will use, and what demands they put on the hash functions
used. The general scenario is that we have some bounded random variables X1; : : : ; Xn = O(1) and
X =

Pn
i=1 Xi. Let = E[X]. Assuming that the Xi are 4-independent, we have the fourth moment bound

Pr[jX j x] = O
( + 2)=x4
: (1)

Deriving (1) is standard (see, e.g., [PPR09]). We will typically have the variable Xi denoting that a ball (or
a bin) hash to a certain interval, which may be defined based on the hash of a certain query ball q. If the
hash function is 5-independent, the Xi are 4-independent.
The fourth moment bound is very useful when 1. For smaller , we are typically looking for upper
bounds x on X, and there we have much better bounds in the combinatorial case where Xi indicates if ball
(or bin) i lands in some interval. Suppose we know that the Xi are a-independent for some a x. The
probability that a given balls land in the interval is pa, so the expected number of such a-sets is

m
a

pa. If
we get x or more balls in the interval, then this includes at least

x
a

such a-sets in the interval. Thus, by
Markov’s inequality, with independence a x,

Pr[X x] = (a=a!)=(xa=a!) = O((=x)a) for a = O(1) (2)

where xa is defined to be x(x 1) (x a + 1). We shall only use (2) with a 3. One advantage
to this is that our results will hold, not only with 5-independent hashing, but also with simple tabulation
hashing. The point is that in [PT12] it is shown that while simple tabulation is only 3-independent, it does
satisfy the fourth moment bound (1) even with a fixed hashing of a given query ball. According to the
experiments [PT12], simple tabulation hashing is an order of magnitude faster than 5-independent hashing
implemented via a degree-6 polynomial.

### 3.2 Proof of Theorem ??

3.2 Proof of Theorem 8

We now focus on the proof of Theorem 8. Cheating a bit, we will assume c 64. We will instead handle
c < 64 as a small capacity in Section 4 where we consider any c = 1 + " = O(1).
Also, to increase readability, since the parameter c c does not appear in the analysis, we will just write
c instead of c below. First we focus on proving Theorem 8 (a) and (b) as restated in the following lemma.

16

Lemma 12. Starting from the hash location of a given q, which is either a passive ball or an active or
passive bin, the expected number of consecutive full bins is O(1=c). If q is a given bin with capacity at least
2, the expected number of consecutive full bins is O((log c)=c2).

Proof. We bound the expected number d of consecutive full bins around h(q). These should include q if
q is a bin, and the bin q would hash to if q is a passive ball. If this bin is not full, then d = 0 and we are
done. Otherwise, let I = (a; b] 3 h(q) be the interval covered by these full bins, that is, a is the location of
non-full bin preceding the first full bin, and b is the location of the last full bin. In our analysis, we assume
that h(q) is fixed before the hashing of any other balls and bins.
We will study the event that t d < t+ and ` < jIj `+. First we note that d t and jIj `+
imply the event:

A(t; `+): Enough balls to fill t bins hash to (h(q) `+; h(q) + `+).

Also d < t+ implies that at most t+ 2 bins land in I n fbg = (a; b). We are discounting position b,
because we could have additional bins hashing to b that are not full. Note that we could have h(q) = b.
No matter how I = (a; b] 3 h(q) is placed, we have either (a; b) [h(q) d`=2e; h(q)), or (a; b)
[h(q); h(q) + d`=2e). Thus d < t+ and jIj > ` imply the event:

B(t+; `): Either at most t+ 2 bins hash to [h(q) d`=2e; h(q)) or at most t+ 2 bins hash to
[h(q); h(q) + d`=2e).

Since balls and bins hash independently, after h(q) has been fixed, the events A(t; `+) and B(t+; `) are
independent, so

Pr[t d < t+ ^ ` < jIj `+] Pr[A(t; `+)] Pr[B(t+; `)]:

For i = 0; 1; : : :, let t = 2i, t = t, and t+ = 2t. Moreover, define `(t) = b8tr=nc. Recall here that r n
is the range we hash balls and bins to to. We will bound Pr[t d < 2t] as follows.

Pr[t d < 2t] Pr[t d < 2t ^ jIj `(t)]

+

dlog2 ceX

j=0

Pr[t d < 2t ^ 2j`(t) < jIj 2j+1`(t)]

+ Pr[t d < 2t ^ c`(t) < jIj]
Pr[A(t; `(t))] (3)

+

dlog2 ceX

j=0

Pr[A(t; 2j+1`(t))] Pr[B(2t; 2j`(t))]

+ Pr[B(2t; c`(t))]:

The main motivation for the definition of `(t) is that with ` = s`(t), s 1, we can get a good fourth
moment bound on Pr[B(2t; `)].
We consider the case where among all bins different from q (which may be a ball or a bin), at most
x = t+ 2 = 2(t 1) hash to [h(q) d`=2e; h(q)). We note that there are at least n 1 bins that
are different from q. We will pay a factor 2 in probability to cover the equivalent case where they hash to
[h(q); h(q) + d`=2e). The expected number of bins different from q hashing to [h(q) d`=2e; h(q)) is

17

(n 1)d`=2e=r, and we want this to be at least 2x = 4(t 1). This is indeed the case with ` `(t)
since `(t) = b8tr=nc 8(t 1)r=(n 1). Since n 2, we have
(n 1)d`=2e=r ns`(t)=(4r) st 1:
Applying the fourth moment bound (1), we now get
Pr[B(2t; s`(t))] = O(( + 2)=( 2(t 1))4) = O(1=2) = (1=(st)2): (4)
Next, we will develop different bounds for Pr[A(t; s`(t))] that are all useful in different contexts. The
interval (h(b) s`(t); h(b) + s`(t)) has length 2s`(t) 1, so the expected number of active balls hashing
to it is < 2s`(t)n=r = O(stm=n). However, to satisfy Pr[A(t; s`(t))], we need enough balls to fill t bins
in that interval. Their total capacity is x tcm=(2n). Suppose we also know that x a and that the balls
hash a-independently. Since we only assume 3-independence, a 3. However, if q is a ball, with h(q)
fixed, we are left with 2-independence. Now, from (2), we get the bound
Pr[A(t; s`(t))] = O((=x)a) = O((s=c)a): (5)
Thus, from (3), we get

Pr[t d < 2t] Pr[A(t; `(t))] + (

dlog2 ceX
j=0

Pr[A(t; 2j+1`(t))] Pr[B(2t; 2j`(t))]) + Pr[B(2t; c`(t))]

=O(1=ca + (

dlog2 ceX
j=0

(2j+1=c)a(1=(2jt)2)) + 1=(ct)2 (6)

=

8
<
:

O(1=c) if a = 1
O(1=c2 + (log c)=(ct)2) if a = 2
O(1=c3 + 1=(ct)2) if a = 3
When we start from the hash of an passive ball or bin, or an active bin whose capacity might be only one,
we can use a = 1 for t = 1 bin and a = 2 for t 2 [2; c]. We now get
E[d [d c]]

X
t=2j;j=0;:::;blog2 cc

2t Pr[t d < 2t]

= O(1=c) +

X
t=2j; j=1;:::;blog2 cc

t O(1=c2 + (log c)=(ct)2)

= O(1=c):
Above, for a Boolean expression B, we have [B] = 1 if B is true, and [B] = 0 if B is false.
In the case where we start from a bin q with capacity at least 2, we can use a = 2 for t = 1 bin. After
the fixing of h(q), the balls are still hashed 3-independently and the capacity is at least 3 with t 2 bins, so
for t 2 [2; c], we can use a = 3. Thus, when q is a bin with capacity at least 2, we get
E[d [d c]] =

X
t=2j;j=0;:::;blog2 cc

2t Pr[t d < 2t]

= O((log c)=c2) +

X
t=2j; j=1;:::;blog2 cc

t O(1=c3 + 1=(ct)2)

= O((log c)=c2):

18

For t c, we are instead going to use a fourth moment bound on Pr[A(t; `(t)]. To satisfy A(t; `(t)), we
need x tcm=(2n) active balls to hash to (h(q) `(t); h(q) + `(t)) but the expectation is only =
(2`(t) 1)n=r 2b8tr=ncn=r 16tm=n. Since c 64, we have x 2. Moreover, 16 since
cm=n 1 and t c. Hence, by (1), we get
Pr[A(t; `(t))] = O(( + 2)=(t )4) = O(2=t4) = O((tm=n)2=((tcm=n)4)) = O(1=(tc)2) (7)
For t c, this replaces the 1=ca term in (6), so we get

Pr[t d < 2t] Pr[A(t; `(t)] +

dlog2 ceX
j=0

Pr[A(t; 2j+1`(t))] Pr[B(2t; 2j`(t))] + Pr[B(2t; c`(t))]

=O(1=(ct)2) +

dlog2 ceX
j=0

(2j+1=c)a(1=(2jt)2 + 1=(ct)2)

=

O(log c)=(ct)2) if a = 2
O(1=(ct)2) if a = 3
In the general case where we start from any passive ball, or active or passive bin, we have a = 2 for t 2,
so we get
E[d [d c]] =

X
t=2jc;j=0;:::;1

2t Pr[t d < 2t]

=

X
t=2jc; j=0;:::;1

2t O((log c)=(ct)2) = O(((log c)=c2),

and then
E[d] = E[d [d < c]] + E[d [d c]] = O(1=c) + O(((log c)=c2) = O(1=c):
This completes the proof of the first statement of the theorem.
For the case where q is a bin with capacity at least 2, we have a = 3 for t 2, and therefore
E[d [d c]] =

X
t=2jc; j=0;:::;1

2t Pr[t d < 2t]

= O(

X
t=2jc; j=0;:::;1

t O(1=(ct)2)
= O(1=c2):
Therefore, when q is a bin with capacity at least 2,
E[d] = E[d [d < c]] + E[d [d c]] = O(((log c)=c2) + O(1=c2) = O(((log c)=c2):
This completes the proof of the theorem.
We will now prove Theorem 8 (c) restated below.
Lemma 13. The expected number of balls hashing directly to any given active bin q is O(m=n). The
expected number of balls forwarded into q from its predecessor q is O(m=n(log c)=c2). Finally, if a bin is
not active, and its active successor q+ is given an extra capacity of one, then the expected number of of full
bins starting from q+ is O((log c)=c2).
19

Proof. For the first statement, we note that the expected number of balls hashing to each location h(q) i
is n=r for any 0 i r. These are not added to q if some bin hashes to [h(q) i; h(q)), which is
an independent event because balls and bins hash independently. The expected number of bins hashing to
[h(q) i; h(q)) is = i(n 1)=r. For i r=(n 1), we have 1, and then, by (1), the probability
of getting no bins in [h(q) i; h(q)) is O(( + 2)=( 0)4) = O(1=2) = O((r=(ni))2). The expected
number of balls hashing directly to q is thus bounded by

n=r

0

@br=(n 1)c +

1X

i=br=(n1)c+1

(r=(ni))2

1

A = O(m=n):

We also have to consider the probability that the preceding bin q forwarded balls to q. For this to happen,
we would need q to be filled even if we increased its capacity by 1, and then q would have capacity at
least 2. This is bounded by the probability of having an interval I 3 h(q) 1 with d 1 consecutive full
bins including one with capacity at least 2. This is what we analyzed in the proof of Lemma 12, so we get
Pr[d 1] E[d] = O((log c=c2). By the capacity constraint, the maximal number of balls that can be
forwarded to and end in q is 2cm=n, so the expected number is

O((log c=c2)2cm=n = O((m=n)(log c)=c):

Next we ask for the expected number d of full bins starting from the active successor q+ of a given passive
bin q, when q+ is given an extra capacity of one. Again this implies that q+ has capacity at least 2, and then
the analysis from the proof of Lemma 12 implies that E[d] = O((log c)=c2).

## 4 Small capacities

In this section we will prove Theorem 11 restated below with " instead of " and allowing any positive
" = O(1) instead of just " 2 (0; 1].

Lemma 14. Consider a configuration with n active balls and active n bins and total capacity (1 + ")m for
some " = O(1). Suppose, moreover, that each bin has capacity between (1 + ")n=(2n) and 2(1 + ")m=n.
Then starting from the hash location of a given passive ball or active or passive bin, the expected number of
consecutive full bins is O(1="2).
Here we assume that balls and bins are hashed independently, each using 5-independent hash functions
or simple tabulation hashing.

We shall sometimes use a weighted count for number YI of bins hashing to an interval I, where the
weight of a bin is its capacity divided by the average capacity (1 + ")m=n. These weights are between 1=2
and 2. The total capacity of the bins hashing into I is precisely YI(1 + ")m=n.
Below we choose such that (1 + )=(1 ) = (1 + "). For " 1, we have "=3, but we also have
for any " = O(1) that = ("). For d 6, our goal is to show that the probability of getting d consecutive
full bins is
O(1=d24) = O(1=d2"4): (8)

It will then follow that the expected number of full bins is O(1="2), as required for Lemma 14.
Before doing the probabilistic analysis, we consider the combinatorial consequences of having d con-
secutive full bins.

20

Lemma 15. Let p be the hash location of an open or closed bin or an inactive ball from which the number
of successive full bins is d 2. If we do not have d balls hashing directly to p, there there is an interval I
containing p of length at least ` = bdr=(2n)c, where either

(i) the number balls XI landing in I is XI (1 + )mjIj=r, or

(ii) the weighted number of bins YI in I is YI (1 )njIj=r.

Proof. Let I = (a; b] be the longest interval covered by consecutive full bins around p. More precisely, a
is the hash location of the non-full bin preceding the first full bin in the interval. We note here that the first
full bins hash strictly after a because bins hashing to the same location always get filled bottom-up. Since
the preceding bin at a is not full, bins in I can only be filled with balls hashing to I.
Since we do not have d bins hashing to p, and we had d full bins starting from p, we must have p < b, so
p 2 (a; b). Also, we have at least d full bins in I, so I must contain at least d(1 + ")n=(2n) balls. Suppose
jIj `. We can then expand I in either end to an interval I+ I of length ` = bdr=(2n)c, and then
XI+ XI (1 + ")dn=(2n) (1 + ")njI+j=r > (1 + )njI+j=r, so (i) is satisfied for I+. Thus we may
assume that jIj ` + 1.
We now look at the interval I = (a; b) which contains p and is of length at least `. All bins in I are
full. Even though our last full bin hashed to b, we have to exclude b because we might also have non-full
bins hashing to b.
Since all bins hashing to I are filled with balls hashing to I, we have XI (1 + ")YIm=n. We
now reach a contradiction if (ii) and (i) are false for I, for then

XI < (1 + )mjIj=r = (1 + ")(1 )mjIj=r < (1 + ")YIm=n:

We note that applying (2) with a = 2, we get that the probability of d bins hashing to p is O(1=d2) =
O(1=d24). Hence it suffices to bound the probability of (i) and (ii). We will do this using a technical result
from [PT12].
To state the result from [PT12], we first reconsider the standard fourth moment bound (1). We are
hashing m balls into [r]. Let = n=r be the density of the balls in r. Let I be an interval that may depend
on the hash location of the inactive query ball, and let XI be the number of active balls hashing to I. Then
E[XI] = jIj, and hence we can state (1) as

Pr[jXI jIjj jIj] = O

jIj + (jIj)2
(jIj)4

(9)

Now [PT12] considered the general event D`;;p that, for a given p 2 [r] that may depend on the hash value
of an inactive query ball, there exists an interval I containing p of length at least ` such that the number of
keys XI in I deviates from the mean jIj by at least jIj. As a perfect generalization of (1), [PT12] proved

Pr[D`;;p] = O

` + (`)2
(`)4

(10)

The proof of (10) in [PT12] only assumes (9), even though (9) only considers a single interval whereas (10)
covers all intervals around a given point of some minimal length.
The bound (10) immediately covers the probability that there exists an interval I 3 p of length at
least ` which satisfies (i). As stated earlier, we may assume that d 6, and since r n, we get that

21

` = bdr=(2n)c dr=(3n). Moreover, the total capacity is (1 + ")m O(m), and the minimum bin
capacity is 1, so n O(m), and therefore ` dn=(3n) = (1). Thus the probability of (i) is bounded by

O
(` + (`)2)=(`)4
= O(1=(4d2)) = O(1=(4d2)):

The analysis above could be made tighter if m n, but then the error probability would just be dominated
by that for (ii) on bins studied in the next part.
We now want to limit the probability of getting too few bins, that is, for some I 3 p containing p of
length at least ` dr=(3n),

(ii) the weighted number of bins in I is YI (1 )njIj=r

It makes no difference to (10) from [PT12] that we apply it to bins instead of balls. We note the bin counts
are weighed with weights below 2. This is not an issue because weights where considered in [PT12],
and the equations (1) and (10) both hold for weights bounded by a constant. Another technical issue is
if we in Lemma 14 start from an active query bin at p. Then this bin is always included in the interval
I 3 p. It should only have been included with probability jIj=r. A simple solution is to only count
the start bib with this probability, yielding a slightly smaller value Y 0I YI. We then instead bound the
probability that Y 0I (1 )njIj=r, which is implied by (ii). Now the bin density is exactly = m=r and
` (n=r)dr=(3n) d=3 2, so the probability of (ii) is bounded by

O
(` + (`)2)=(`)4
= O(d2=(d)4) = O(1=(4d2):

We have thus shown that O(1=(4d2)) bounds the probability of both (i) and (ii) in Lemma 15, hence the
probability of getting d 1=2 successive full bins. It follows that the expected number of successive bins
is bounded by

1=2 +

1X

d=d1=2e

O(1=(4d2)) = O(1=2) = O(1="2)

This completes the proof of Lemma 14, and hence of Theorem 11.

## 5 Computing moves when the system is updated

In this paper, we have not been concerned about computing the moves that have to be done. In many
applications this is a non-issue since we can afford to recompute the configuration from scratch before and
after each update. These are also the applications where history independence matters. Our more efficient
computation of moves is based on a global simulation of the system in RAM. Our implementation is tuned
to yield the best theoretical bounds.
We will now first describe an efficient implementation when balls are just inserted in historical order,
much like in the standard implementation of linear probing. We will describe how to compute the moves
associated with an update in expected time proportional to the bounds we gave above for the expected
number of moves.
Given a ball q, we want to find the bin it hashes to in expected constant time. This is the bin succeeding
q clockwise when bins and balls are hashed to the cycle. For that we use an array B of size t = (n).
Entry i is associated with the interval Ii = [i=t; (i + 1)=t). In B[i], we store the head of a list with the bins
hashing to Ii in sorted order. The list has expected constant size. When a ball q comes, we compute the
interval Ii 3 h(q), and check B[i] for its succeeding bin. If L[i] has no bin hashing after q, we go to Li+1.

22

The expected time is O(1). As bins are inserted and deleted, we use standard doubling/halving techniques
in the back ground to make sure that L always have (n) entries.
For each bin, we store the balls landing in it in a hash table. Recall here that when a ball is inserted, we
first find the bin it hashes to, but if it is full, we have to place it in the first non-full succeeding bin. Such
insertions are trivially implemented in time proportional to the number of bins considered, which is exactly
what we bounded when we considered the number of moves.
We now turn to deletions. A deletion is essentially like a deletion in linear probing. When we take out a
ball q from a bin b, we try to refill the hole by looking at succeeding bins b0 for a ball q0 that hash to or before
b. We then move q0 to b, and recurse to fill the hole left by q0 in b0. The last hole created is the bin where
q would land if q was inserted last in the current configuration. We are willing to pay time proportional to
number of bins from the one q hashed to, and to the one it would land in if inserted last.
To support efficient deletions, we let each bin b have a forwarding count for the number of balls that
hashed preceding b but landed in a bin succeeding b. Also, we divide the balls landing in b according to
which bins they originally hashed to. More precisely, we maintain a doubly-linked list with the bins that
balls landing in b hashed to, sorted in the same order as the bins appear on the cycle. With each of these
preceding bins b, we maintain a list with the balls that hashed to b and landed in b. The total space for
these lists is proportional to the number of balls landing in b, so the total space remains linear. We assume
that we can access the sorted bin list from each end.
Now, if a deletion of a ball q creates a hole in b, then this hole can be filled if and only if the forwarding
counter of b is non-zero. If so, we consider the succeeding bins b0 one by one. In b0, we go to the beginning
of the sorted bin list to find the first bin b that a ball landing in b0 hashed to. If b equals or precedes b, we
can use any ball q0 from the list of b in b0 and fill the hole in b. Checking the bin b0 and possibly moving a
ball takes constant time. If a hole is created in b0 and b0 has non-zero forwarding counter, we recurse.
Another issue is that we have is to locate a ball to be deleted in one of the above lists. For that we
employ an independent hash table for the current balls point to their location. The hash table could be
implemented with linear probing which works in expected constant time using the same 5-independent or
simple tabulation hash function that we used to map balls to the cycle [PPR09,PT12]. Below it is assumed
that we always update the hash table with the location of the balls as we move them around. An alternative
solution would be that we searched the ball starting from the bin it hashed to, and then only used a local
hash table for each bin.
When a ball q is inserted, we have to update the above information. If it hashes to a bin b and lands in
a bin b0, then we have to increment the forwarding counter for all bins starting from b and going to the bin
preceding b0—if b = b0, no forwarding counter is incremented. Now inside b0, we have to go backwards in
the list of bins that balls landing in b0 hashed to, searching for b, inserting b if necessary. Next we add q to
the list of b in b0. The bins considered inside b0 is a subset of the bins we passed from b to b0, so the time
bound is not increased.
When bins are inserted and deleted, we implement the effect using insertion and deletion of the affected
balls as described above.
The most interesting challenge is that when a ball is inserted or deleted, we have to change the capacity
of c bins. This becomes hard when c = !(1) where we only have O(1) expected time available.
As the system is described above, we let the lowest dcme nbcm=nc bins have large capacity dcm=ne
while the rest have small capacity bcm=nc. However, we only have to guarantee that no bin has capacity
above dcm=ne.
We now relax things a bit. With no history independence, we just maintain the list of current bins in the
order they were inserted (if one is deleted and reinserted, we count it as new). The large bins form a prefix

23

of this list. We also relax the requirement on the number of large bins to be dcme nbcm=nc c. This
means that when a ball is inserted or deleted, it doesn’t have to be exactly c bins whose capacity we change.
Next we partition the list of current bins into groups of length at most c with the property that the combined
length of any two consecutive groups is at least c. This partition is easily maintained in constant time per
bin update, including pointers so that we can from each bin can find the head of the group it belongs to.
The changes to bin capacities are now done for one or two groups at the time, telling only the group
heads what the capacity is. Thus, to check if a bin is full, we have to ask the group head about the capacity.
Now, when we change the capacity of a group, we find out if the change in capacity means that some
bins need changes to their information. A bin requiring action is called critical. More precisely, a large bin
is critical if it is full. A small bin is critical if it is full and its forwarding count is non-zero. This implies that
it would be full if it became large. By Lemma 12, a bin is critical with probability O((log c)=c2), so we only
expect O((log c)=c) = o(1) critical bins in a group. To identify critical bins efficiently, we divide groups
into subgroups of size at most

p
c, using the same algorithm as we used for dividing into groups. For each
group and for each subgroup, we count the number of critical bins. Now a group only has a critical bin with
probability O((log c)=c), which is our expected cost to check if there are critical bins. If so, we check which
subgroups have a positive count of critical bins. If a subgroup has critical bins, we find them by checking all
the bins in the subgroup. Altogether, we pay O(

p
c) time per critical bin, so the expected time to identify
the critical bins is O(

p
c(log c)=c2)) = o(1).
We next consider an implementation with history independence. For history independence, we first do
a random permutation of the bin and ball IDs. We can use the classic (x) = ax + b mod p where p is a
random prime and a and b are uniformly random in Zp. These permuted IDs are still history independent.
The point now is that if we have a set X of (k) permuted IDs, then we can maintain order in this set by
bucketing based on b(x)=kc. The buckets are then in relative sorted order, and we can easily maintain
order within each bucket since each ID is expected to end in bucket with O(1) other IDs. As usual, we can
use doubling/halving if the set X grows or decreases by a constant factor. Now it is easy to maintain an
ordered list of permuted bin IDs where a prefix of these are large bins, just like in our original description.
Also, for each bin b, we maintain the list of balls that hash to it, ordered based on the permuted IDs. We
note that the balls in this list are extracted based on the hashing to the unit cycle which is independent of the
permutation of the ball IDs. The balls hashing to b are placed in b and succeeding bins based on the ordering
of this list, that is, first we fill b with the balls with the lowest permuted IDs. In particular, the list of balls
hashing to b but landing in bin b0 is just a segment of the sorted list of balls hashing to b.

## 6 High Probability Bounds

We are now going to present high probability bounds, that is, bounds that holds with probability 1 1=n
for any desired constant . The analysis assumes fully random hashing. The 5-independent hashing that
sufficed for our expected bounds does not suffice for high probability. However, some of our high probability
bounds can also be obtained with simple tabulation [PT12].

Theorem 16. With balancing parameter c = (1+") = O(1), w.h.p., the maximal number of bins considered
in connection with a search is O((log n)="2). This bound also holds for the number of balls that have to be
moved when a ball is added or removed. When a bin is added or removed, w.h.p., the number of balls to
move is O((m=n)(log n)="2).
With balancing parameter c = !(1), w.h.p., the maximal number of bins considered in connection with
a search is 1 + O ((log n)=c) if m > n=2, and 1 + O

log n
(cm=n) log(n=m)

if m n=2. This bound also

24

holds for the number of balls that have to be moved when a ball is added or removed. When a bin is added
or removed, w.h.p., the number of balls to move is and O((m=n) log n) if m > n=2, and O

log n
log(n=m)

if
m n=2.

If m = (n), the above bounds are at most a log-factor worse than the expected bounds. In the very
lighlty loaded case where m = o(n), things have to get worse, e.g., even for simple consistent hashing
without forwarding, we expect some bins to have

log n
log(n=m)

balls, matching our high probability bound
for the number of balls to be moved when a bin is closed.
The rest of this section is devoted to proving Theorem 16. We will often study some run R of consecutive
bins full bins, preceeded by an non-full bin at some position x. Let y be the position of the last bin in R.
Because the preceeding bin is not full, all balls landing in the bins of R must hash to the interval (x; y]
covered by R. We note that the bin succeeding R could be in position y, colliding with the last bin of R.

Large capacities We first study the case with super constant balancing parameter c = !(1). What makes
this case tricky to analyze is that we in connection with ball updates have to make up to dce capacity changes.
Recall that bins have capacity cm=n rounded up or down. Also, recall that cm=n 1.
For a given number d, we will bound the probability that a given bin q is in a maximal run R of d full
bins. Based on d we are going to pick a threshold `. If the run covers an interval I of length at most `, then
I (h(q) `; h(q) + `], and then we know that at least dcm=(2n) hash to (h(q) `; h(q) + `]. We call this
the ball event. On the other hand, if the R covers interval of length bigger `, then, either we have at most d
bins landing in [h(q) d`=2e; h(q)) or at most d bins landing in [h(q); h(q) + d`=2e). We call this the left
and right bin events.
It is convenient to reparameterize with s = `n=(dr), and we will always have s 6.
We will now bound the probability of the ball event. The expected number of balls that hash to (h(q)
`; h(q) + `] is A = 2`m=r = 2dsm=n. Using the Poisson bound, the probability that at least xA =
dcm=(2n) hash to this interval is bounded by

PA = (eA=xA)xA = ((e2dsm=n)=(dcm=(2n))dcm=(2n) = ((4es=c)dcm=(2n): (11)

Next we consider any one of the two bin events. In both cases, we want at most d balls to hash into an
interval I of length at `=2. Ignoring the bin q, we expect at least B = (`=2)(n 1)=r = (ds=3) bins to
land in I, and (ds=3) 2. Therefore, by the Chernoff bounds, the probability of each bin event is bounded
by
PB = exp(B=8) = exp(ds=24): (12)

Now, if n = (m), we pick s = c=(8e), which is bigger than 6 since c = !(1). With this parameter
choice, the probability that any of our events happen is bounded by exp((dc)), which hence also bounds
the probability that bin q is in a maximal run of length d. It follows that, w.h.p., the maximal run length is
O((log n)=c). In particular, it follows that for some sufficiently large c = O(log n), w.h.p., there is no full
bin.
If n = o(m), we pick s = 24(cm=n) ln(n=m). Then

PA = (4es=c)cm=(2n) = (96em=n lg(n=m))cm=(2n) = (m=n)(dcm=n),

while
PB = exp(ds=24) = (m=n)dcm=n

25

Thus we conclude that the probability that a bin q is in a maximal run of length d is bounded by
(m=n)(dcm=n). It follows that w.h.p., the maximal run length is O((log n)=(cm=n log(n=m)).
Above we studied the maximal run length, which also bounds the number of bins we have to search if
the first bin is full. It also bounds the number of balls that have to be moved when a ball is added or removed,
except for the effect of changing dce bin capacities, each by 1. Recall here that adding and removing a ball
has the same cost by symmetry, so it suffices to consider a ball removal where capacities are decreased by 1.

dce capacity decreases We will now study the number of moves in connection with at most dce capacity
decreases. Let C be the set of affected bins. The cost is bounded by the sum over the bins p 2 C of the
length of the maximal run of full bins containing p. We already have a bound on the maximal run length that
we can multiply by c, but we will show that, w.h.p., the sum of these run lengths is only a constant factor
bigger than the maximal run length. Since one bin makes no difference here, we can assume that c = dce.
The argument below assumes that c

p
m. The case c >

p
n is quite extreme and can be handled with a
different argument.
Based on the previous analysis for a single run, we argue that any run of full bins is contained in an
interval of length at most O((log n)r=n)). To see this, recall that we gave high probability bounds on (1)
the maximum length d of a run of full bins, and (2) the length ` such that intervals of length ` have more
than d bins. Here (1) and (2) implies that no run of full bin can span an interval longer than `. For the case
where n = (m), we had s = c=(4e) and d = O((log n)=c), hence

` = sdr=n = (c=(4e)O((log n)=c)r=n = O((log n)r=n):

For the case, n = !(m), we had s = 24(cm=n) ln(n=m) and O((log n)=(cm=n log(n=m)), hence

` = sdr=n = 24((cm=n) log(n=m))O((log n)=((cm=n) log(n=m)))r=n = O((log n)r=n):

We fix the hashing of the bins in C before we start hashing any of the other balls and bins. We say that two
bins from C are well-separated if they are at least 4` apart on the cycle. We will partition C into a constant
number of sets, C1; ::; CO(1) so that bins in the same set are all well-separated. To do that we partition the
cycle into intervals of length between 4` and 8`. Consider one of these intervals I. A bin from C lands in it
with probability O((log n)=n) and c = jCj

p
n, so with high probability, we get only a constant number
of bins from C in any I. To get our first set C1, we pick an arbitrary bin from every other interval, and
likewise we get C2 picking a bin from each of the other intervals. We have now picked a bin from every
interval, so if we repeat this a constant number of times, we get the desired partitioning C1; ::; CO(1).
We now focus on any one of the well-separated Ck. Let ck = jCkj c. For each bin p 2 Ck, we have a
run of full bins, and we want to bound the probability that the total length of these runs is d.
The run of full bins that includes p 2 Ck covers some interval Ip of length `p, and let ` =

P
p2Ck `p.
As when we studied the run of a single bin, we are going to define a threshold length ` = O((log n)r=
and s = `n=(dr). Our combined ball event is that there exists intervals Ip of total length ` = ` with at
least dcm=(2n) hashing to them. Our new combined bin event is that there exists intervals Ip of total length
` = ` with at most d bins hashing inside of them (not including the last position). If both events fail, then
the total run length cannot be d.
For each p, we will use a coarse overestimate `+p of `p. We say that `p is on level 0 if `p `=ck.
Otherwise `p is on level j where 2j1`=ck < `p 2j`=ck. We set `+p = 2j`=ck. With these overestimates,
we have Ip I+p = (h(p) `+p ; h(p) + `+p ] for all p. The bins in Ck are well-separated so that there is no
overlap between any two I+p0. Moreover,

P
p2Ck `

+
p ` +

P
p2Ck 2`p 3`

, so j P
p2Ck jI

+
p j 6`

. Our
combined ball event implies that ` ` and that dcm=(2n) balls hash to

S
p2Ck Ip =

S
p2Ck I

+
p .

26

Assume the I+p were fixed. They have total length at most 6`, so the expected number of balls hashing to
S
p2Ck I

+
p is at most m(6`=r) = 6dsm=n. The probability that at least dcm=(2n) balls hash to

S
p2Ck I

+
p
is therefore at most
((e6dsm=n)=(dcm=(2n))dcm=(2n) = (12es=c)dcm=(2n): (13)
The important point here is that there are only 4ck possible choices for the I+i . More precisely, for each
level j 0, we have a bit map telling which of bins from level j that are on level > j. There are at
most ck=2j1 bins on level j, which is the size of the bit map for that level. The total number of bits
needed to describe all I+p is thus at most 4ck, so there at most 24ck combinations. This means that even
when we quantify over all possible combinations of the intervals I+p , as long as their total length is `, the
total probability of the combined ball event is bounded by

24ck((12es=ck)dcm=(2n):

For the bins, we use the same idea, but starting from level 1 when `=(2ck) `p < `=ck. We now use
an underestimates `p of `p. We set `p = 0 if `p < `=(2ck). Otherwise, if `p is on level j 1, we set
`p = 2j2`=ck. We also have, for each bin a bit that tells if the interval Ip is mostly before or mostly after
h(q). If before, then Ip contains Ip = [h(p) `p ; h(p)). Otherwise Ip contains Ip = [h(p); h(p) + `p ).
Including the before/after bitmaps, we can completely describe all the Ip using 6ck bits, so there are at most
26ck combinations. Moreover, the total size of the small `p < `=(2ck) is at most `=2. For the `p `=(2ck),
we get `+p `p=4, so we conclude that

P
p2Ck `

p `=8. Assume that the I
p are fixed. The combined
bin event implies that at most d bins hash to them. Considering only the n c bins outside C, we expect
at least (n c)(`=8)=r = (n c)sd=(8n) sd=10 bins. With s 20, the probability of the bin event is
exp((sd=10)=8) = exp(sd=80). Considering all possible configurations of the bins Ip , we get that the
probability of the combined bin event is bounded by 26ck exp(sd=80).
If n = (m), we pick s = ck=(24e) and conclude that the probability of combined ball and bin events
is bounded by 2O(ck) exp((dck)) = exp((dck)) for d = !(1). This bounds the probability that the
sum of run lengths from the bins in Ck is d, so we conclude that, w.h.p., the total run length is O((log n)=c).
There are only O(1) sets Ck in C, so we get the same bound on the total run length over all bins in C.
We would like to follow the same idea in the lightly loaded case where n = o(m). If we set
s = 80(cm=n) ln(n=m), we get that the probability of the combined ball and bin events is bounded by
2O(ck)(m=n)(dcm=n). However, this time, the 2O(ck) may be so large. One issue is that ck may be much
larger than d.
We now note that if d is total number of bins in the maximal runs of full bins involving bins in Ck, then
at most d bins from Ck can be full. Let us assume we guess the number d0 d of full bins from Ck, and
let C0 Ck be the subset of these bins. We can then apply the the preceeding argument to C0 instead of Ck
with d0 instead of ck. Note that we are not conditioning on the bins from C0 all being full. We are merely
studing the probability that d is the total number of bins in the maximal runs of full bins involving bins in
C0. To get bounds for Ck, we have to sum over all d0 d over all

ck
d0

choices of C0 Ck. If d0 = O(1),
we are done, for then we involve only O(1) maximal runs, so we may assume that d0 = !(1).
When C0 is fixed, we only have 26d

0
choices for the coarse interals, so for given d0 = !(1), the total
number of configurations we have to consider is

ck
d0

26d

0
< (ckd

0
=d0!)26d

0
< ckd

0
:

We will now sharpen our probability bounds for the combined bin and ball events. Setting s =
160(cm=n) ln(n=m), we get that the probability of the combined bin event is exp(sd=80) =

27

(m=n)2dcm=n. For n = !(m), this upper bound is maximized when m=n is minimized, and we know
that m=n 1=c, so (m=n)2dcm=n) (1=c)2d which is at least the square of ckd

0
. Thus, covering all
combinations, we conclude that the combined bin event has probability at most (m=n)dcm=n).
When it comes to the ball bound, we need to exploit a subtle point that we also used in our expected
bound; namely that when we decrease the capacity, it only has effect if the bin was full before the decrease,
and then the capacity was at least 2. Our ball event assuming that all bins in C0k are full thus implies that
at least 2d0 balls hash to the intervals I+p for p 2 C0k. We also know that we need dcm=(2n) to fill d bins.
Poisson bound from (13) can be refined to

P = (12es=c)maxfdcm=(2n);2d

0g
= (12e160(cm=n) ln(n=m)=c)maxfdcm=(2n);2d

0g

= O((m=n) log(n=m))maxfdcm=(2n);2d

0g
: (14)

For n=m = !(1) and n=m c=4, this bound grows with n=m, so in this case,

P O(log c=c)2d 1=c1:5d:

On the other hand, for c=4 < n=m c, we use the 2d0 bound, and conclude that

P O(log c=c)2d

0
1=c1:5d

0
:

Thus, including our factor cd

0
from the union over all configurations, we get the probability of the ball event
is bounded by

cd

0
P P 1=3 (O(m=n) log(n=m))maxfdcm=(2n);2d

0g=3
(m=n)(dcm=(n)):

This completes the proof that when n = !(n), w.h.p., the total run length over all bins in C is
O((log n)=((cm=n) log(n=m))).

Bin updates The last thing we need to analyze is the number of balls that may be moved when a bin is
added or removed. The two are symmetric, but the easiest to think about is a bin removal. The direct effect
is that the balls in the bin are forwarded to the next been, but if it is full, they are further forwarded. Then the
total number of affected balls is the bin capacity dcn=me times 1 plus the maximal number of full bins in a
run. We already have high probability bounds on the maximal run length. In addition, we have to do dcn=me
bin capacity changes. We already gave high probability bounds on the number of balls that had to be moved
with up to c capacity changes, so now we just have to multiply that number with ddcn=me=ce 1 + n=m.
Thus, when a bin is added or removed, if m = (n), then, w.h.p., the number of balls moved is bounded by

(1 + cm=n)(1 + O((log n)=c)) + (1 + n=m)O((log n)=c) = O(cm=n + (n=m)(log n)):

If m = o(n), then, w.h.p., the number of balls moved is bounded by

(1 + cn=m)
1 + O

log n
(cm=n) log(n=m)

+ (1 + n=m)O

log n
(cm=n) log(n=m)

):

= O
cn=m +

log n
log(n=m)

:

However, if we get no full bins, then there is no forwarding, so if a bin is removed, then this only affects
the ball that were in the bin that are now transferred to the successor. Our high probability bound on the

28

maximal run length was, in fact, based on a bound on how many balls we could have in an interval, large
enough to have several bins, with high probability, so multiplying that number with the capacity, we get a
high probability upper bound on how many balls hash directly to any given bin. For m = (n), the bound
is
dcm=neO ((log n)=c) = O ((m=n)(log n)) :

If this bound is below bcm=nc, then there are no bins, so this bound replaces the previous cm=n term. Thus
for m = (n), we conclude, w.h.p., that if a bin added or removed, then the total number of balls moved is
O ((log n)=c), as claimed in Theorem 16.
Likewise, for m = o(n), w.h.p., the maximal number of balls hashing to a bin is

dcm=neO

log n
(cm=n) log(n=m)

=

log n
log(n=m)

:

Again this replaces the previous cm=n. Thus for m = o(n), we conclude, w.h.p., that if a bin added or
removed, then the total number of balls moved is

log n
log(n=m)

, as claimed in Theorem 16. This completes
the proof of Theorem 16 when c = !(1).

Small capacities For the cases where c = O(1), we get all he bounds from Theorem 16 follows if we
prove, w.h.p., that the longest run of full bins has length O((log n)="2).
As in Section 4, we say that the weight of a bin is its capacity divided by the (1+")m=n. These weights
are between 1=2 and 2. Suppose there a run of full bins of total weight at least d. We consider the shortest
prefix R of this run of length at weight at least d. The bins have weight less than d + 2. Let (x; y] be the
interval covered by these bins. Here x is the location of the first bin and y is the location of the first bin after
R (for technical reasons, we made the tie breaking such that if a ball and bin landed in the same position,
then the ball proceeds the bin). We now have at least d(1 + ")m=n balls landing in [x; y]. We also know
that the total weight of the bins landing in (x; y) is at most d + 2.
Let ` be any value. If x y `, then we conclude that we have d(1 + ")m=n balls landing in some
interval of length ` + 1. On the other hand, if x y > `, then bins of weight at most d + 2 landing in an
interval of length at most ` 1.
We divide the cycle into at most 2n parts of length at most r=n 1. Inside an interval of length ` 1,
we can have an interval of length at least ` = ` 1 2r=n ` 3r=n. This interval can be chosen in at
most 2n ways, and it that consists of a whole number of parts.
We will pick `d as a function of d.
Define such that (1 + ") = (1 + )2=(1 )2. Then = ("). We set ` = d(r=n)=(1 )2.
We will have d (lg n)=2. Then ` (d + 2)(r=n)=(1 ). It follows that the expected weight of
bins in such an interval I is (d + 2)=(1 ). Using standard Chernoff bounds, since each bin has weight
below 2, the probability of this event is

exp(((d + 2)=2)2=2)) exp((d"2)):

We can pick d = O((log n)="2) large enough that the probability of an underfull interval of length ` is
1=n.
We now consider the other error event: that there is an interval of length `+ with too many balls. The
bad interval should consist of a minimal number of pieces so as to have length at least ` + 1. It will have
length at most `+ = ` + 1 + 2r=n ` + 3r=n `(1 + ). The number of balls expected in such an interval
is m`(1 + )=r = d(1 + )(m=bins)=(1 )2. The error event is that we getting d(1 + ")m=n, which is

29

(1 + ) times bigger than the expectation. By definition, (1 + ")m=n 1, so by Chernoff bounds, the error
probability is
exp((d=(1 ))2=3)) exp((d"2)):

Again, we can drive the error probability down for the at most 2n possible combinations. This completes
our proof of Theorem 16.

Tabulation hashing The bounds from Theorem 16 with load balancing c = (1 + ") = O(1) also hold
with simple tabulation, which has somewhat weaker Chernoff bounds [PT12]. These Chernoff bounds
are weaker in the exponent by a constant factor, and this only costs us a constant factor in the maximal
number of consecutive full bins, and that suffices for c = (1 + ") = O(1). For c = !(1), we have to
deal with bin capacity changes, which may not be sufficiently independent with simple tabulation, but for
c = (log n)1+o(1), we can still get the bounds of Theorem 16 if we use twisted tabulation [PT13].

## 7 Simulation Results

To validate the consistency property of our hashing scheme which is theoretically analysed in Theorems 2,
and 3, we present the following empirical results. We generated thousands of instances, and tracked the
number of ball movements in each operation, and the distribution of bin sizes. We picked the number of
bins, n, the average balls per bin ratio r = mn , and " as follows:

n 2 f10; 20; 40; 70; 100; 150; 200; 300; 450; 600; 800; 1000; 2000g.

r = mn 2 f0:5; 0:8; 1; 1:2; 1:5; 2; 3; 5; 10g

" 2 f0:05; 0:1; 0:2; 0:3; 0:4; 0:5; 0:6; 0:7; 0:8; 0:9; 1; 1:2; 1:5; 1:8; 2; 2:3; 2:5; 2:8; 3g

Figures 2 and 3 show the distribution of bin loads for our algorithm and Consistent Hashing algorithm
respectively. Figures 4 and 5 depict the average number of movements in our algorithm for each value of
". We start with Figure 2 that shows the distribution of bin loads. The three plots represent three values of
" 2 f0:1; 0:3; 0:9g. We expect the load of each bin to be at most d(1 + ")m=ne. To unify the results of
various simulations with different average loads (m=n), we divide the loads of bins by m=n, and sort the
normalized bin loads in a decreasing order (breaking ties arbitrarily). The y coordinate is the normalized
load, and the x coordinate shows the fractional rank of the bin in this order. For instance if a bin’s load is
greater than 35% of other bins, its x coordinate will be 35%. As we expect, no bin has normalized load more
than 1 + ". A significant fraction of bins have normalized load 1 + ", and the rest have normalized loads
distributed smoothly in range [0; 1 + "]. The smaller the " is, the more we expect bins to have normalized
loads equal to 1 + ", and consequently having a more uniform load distribution.

30

Figure 2: Bin loads divided by average load m=n for our algorithm.

As shown above, the maximum load never exceeds 1 + " times the average load in our algorithm.
However, you can see below that there is no constant upper bound on maximum load for Consistent
Hashing algorithm. We simulate consistent hashing with n balls and n bins for three different values
of n 2 f200; 1000; 8000g. As expected, the maximum load grows with n, it is expected to be around
log(n)= log log(n). The percentage axis is rescaled to highlight the more interesting bin sizes.

Figure 3: Bin loads divided by average load for Consistent Hashing algorithm.

Figure 4 depicts the number of movements per ball operation. Theorems 2, and 3 suggests that the
expected number of movements per ball operation is O(log(c)=c), and O(1="2) respectively where c is
1 + ". The solid red curve in Figure 4 depict the average normalized ball movements in all simulations for
each value of ". The bars show the standard deviation of these normalized movements. The dashed black
line is the upper bound on these numbers of movements predicted by Theorems 2 and 3 with the following
formula:

f(") =

2="2 if " < 1
1 + log(1 + ")=(1 + ") if " 1

31

Figure 4: Number of movements per ball operation for different values of ".

Our results predict that unlike the ball operations, the average movements per bin operation (insertion or
deletion of a bin) is proportional to average density of bins r = mn . Therefore we designate Figures 5 and 4
to bin and ball operations respectively. We start by elaborating on Figure 5. Theorems 2 and 3 suggest that
the average number of movements per bin operation is O(r log(c)=c), and O(r="2) respectively where c is
1 + ". To unify the results of all our simulations, we normalize the number of movements in bin operations
with dividing them by r. The solid red curve in Figure 5 depict the average normalized bin movements in
all simulations for each value of ". The bars show the standard deviation of these normalized movements.
The dashed black line is the upper bound function, f(") (defined above), on these numbers of movements
predicted by Theorems 2 and 3. Similarly Figure 4 shows the relation between the number of movements
and " for ball insertions and deletions. These are the actual number of movements and are not normalized
by the average density r.

Figure 5: Normalized number of movements per bin operation for different values of ".

32

Acknowledgements We thank Andrew Rodland from Vimeo for putting our algorithmic idea into practice,
and for letting use the plot from [Rod16] presented in Figure 1 (with a changed background color to make
printing easy).

References

[ABKU99] Yossi Azar, Andrei Z. Broder, Anna R. Karlin, and Eli Upfal. Balanced allocations. SIAM J.
Comput., 29(1):180–200, 1999.

[ADW14] Martin Aumuller, Martin Dietzfelbinger, and Philipp Woelfel. Explicit and efficient hash fami-
lies suffice for cuckoo hashing with a stash. Algorithmica, 70(3):428–456, 2014.

[BCFM00] Andrei Z. Broder, Moses Charikar, Alan M. Frieze, and Michael Mitzenmacher. Min-wise
independent permutations. Journal of Computer and System Sciences, 60(3):630–659, 2000.
See also STOC’98.

[BCM03] John W. Byers, Jeffrey Considine, and Michael Mitzenmacher. Simple load balancing for dis-
tributed hash tables. In Peer-to-Peer Systems II, Second International Workshop, IPTPS 2003,
Berkeley, CA, USA, February 21-22,2003, Revised Papers, pages 80–87, 2003.

[BCM04] John W. Byers, Jeffrey Considine, and Michael Mitzenmacher. Geometric generalizations of the
power of two choices. In SPAA 2004: Proceedings of the Sixteenth Annual ACM Symposium on
Parallelism in Algorithms and Architectures, June 27-30, 2004, Barcelona, Spain, pages 54–63,
2004.

[BCSV00] Petra Berenbrink, Artur Czumaj, Angelika Steger, and Berthold Vocking. Balanced allocations:
the heavily loaded case. In Proceedings of the thirty-second annual ACM symposium on Theory
of computing, pages 745–754. ACM, 2000.

[BG07] G. E. Blelloch and D. Golovin. Strongly history-independent hashing with applications. In
Proc. 48th IEEE Symposium on Foundations of Computer Science (FOCS), pages 272–282,
2007.

[BSS00] Andre Brinkmann, Kay Salzwedel, and Christian Scheideler. Efficient, distributed data place-
ment strategies for storage area networks (extended abstract). In Proceedings of the Twelfth
annual ACM Symposium on Parallel Algorithms and Architectures, SPAA, July 9-13, 2000, Bar
Harbor, Maine, USA, pages 119–128, 2000.

[CDKR02] Miguel Castro, Peter Druschel, Anne-Marie Kermarrec, and Antony IT Rowstron. Scribe: A
large-scale and decentralized application-level multicast infrastructure. Selected Areas in Com-
munications, IEEE Journal on, 20(8):1489–1499, 2002.

[CFM+98] Richard Cole, Alan M. Frieze, Bruce M. Maggs, Michael Mitzenmacher, Andrea W. Richa,
Ramesh K. Sitaraman, and Eli Upfal. On balls and bins with deletions. In Randomization
and Approximation Techniques in Computer Science, Second International Workshop, RAN-
DOM’98, Barcelona, Spain, October 8-10, 1998, Proceedings, pages 145–158, 1998.

[DW07] Martin Dietzfelbinger and Christoph Weidling. Balanced allocation and dictionaries with tightly
packed constant size bins. Theor. Comput. Sci., 380(1-2):47–68, 2007.

33

[FJ17] Alan M. Frieze and Tony Johansson. On the insertion time of random walk cuckoo hashing.
In Proceedings of the Twenty-Eighth Annual ACM-SIAM Symposium on Discrete Algorithms,
SODA 2017, Barcelona, Spain, Hotel Porta Fira, January 16-19, pages 1497–1502, 2017.

[FKP16] Nikolaos Fountoulakis, Megha Khosla, and Konstantinos Panagiotou. The multiple-
orientability thresholds for random hypergraphsy. Combinatorics, Probability & Computing,
25(6):870–908, 2016.

[FPS13] Nikolaos Fountoulakis, Konstantinos Panagiotou, and Angelika Steger. On the insertion time
of cuckoo hashing. SIAM J. Comput., 42(6):2156–2181, 2013.

[FPSS05] Dimitris Fotakis, Rasmus Pagh, Peter Sanders, and Paul G. Spirakis. Space efficient hash tables
with worst case constant access time. Theory Comput. Syst., 38(2):229–248, 2005.

[GF04] David A. Grossman and Ophir Frieder. Information Retrieval - Algorithms and Heuristics, Sec-
ond Edition, volume 15 of The Kluwer International Series on Information Retrieval. Kluwer,
2004.

[GH05] George Giakkoupis and Vassos Hadzilacos. A scheme for load balancing in heterogenous dis-
tributed hash tables. In Proceedings of the Twenty-Fourth Annual ACM Symposium on Prin-
ciples of Distributed Computing, PODC 2005, Las Vegas, NV, USA, July 17-20, 2005, pages
302–311, 2005.

[KLL+97] David R. Karger, Eric Lehman, Frank Thomson Leighton, Rina Panigrahy, Matthew S. Levine,
and Daniel Lewin. Consistent hashing and random trees: Distributed caching protocols for
relieving hot spots on the world wide web. In Proceedings of the Twenty-Ninth Annual ACM
Symposium on the Theory of Computing, El Paso, Texas, USA, May 4-6, 1997, pages 654–663,
1997.

[KM05] Krishnaram Kenthapadi and Gurmeet Singh Manku. Decentralized algorithms using both local
and random probes for P2P load balancing. In SPAA 2005: Proceedings of the 17th Annual
ACM Symposium on Parallelism in Algorithms and Architectures, July 18-20, 2005, Las Vegas,
Nevada, USA, pages 135–144, 2005.

[KR06] David R. Karger and Matthias Ruhl. Simple efficient load-balancing algorithms for peer-to-peer
systems. Theory Comput. Syst., 39(6):787–804, 2006. Announced at SPAA’05.

[Lar88] Per- Ake Larson. Dynamic hash tables. Commun. ACM, 31(4):446–457, 1988.

[Man04] Gurmeet Singh Manku. Balanced binary trees for ID management and load balance in dis-
tributed hash tables. In Proceedings of the Twenty-Third Annual ACM Symposium on Principles
of Distributed Computing, PODC 2004, St. John’s, Newfoundland, Canada, July 25-28, pages
197–205, 2004.

[Mit01] Michael Mitzenmacher. The power of two choices in randomized load balancing. Parallel and
Distributed Systems, IEEE Transactions on, 12(10):1094–1104, 2001.

[MTZ16] Vahab S. Mirrokni, Mikkel Thorup, and Morteza Zadimoghaddam. Consistent hashing with
bounded loads. CoRR, abs/1608.01350, v1, September, 2016.

34

[MZ17] Vahab Mirrokni and Morteza Zadimoghaddam. Consistent hashing with bounded loads.
Google Research Blog, April 3, 2017. https://research.googleblog.com/
2017/04/consistent-hashing-with-bounded-loads.html.

[NSW08] Moni Naor, Gil Segev, and Udi Wieder. History-independent cuckoo hashing. In 35th Inter-
national Colloquium on Automata, Languages and Programming, (ICALP), pages 631–642,
2008.

[ OV11] M. Tamer Ozsu and Patrick Valduriez. Principles of Distributed Database Systems, Third Edi-
tion. Springer, 2011.

[PPR09] Anna Pagh, Rasmus Pagh, and Milan Ruzic. Linear probing with constant independence. SIAM
Journal on Computing, 39(3):1107–1120, 2009. See also STOC’07.

[PR01] Rasmus Pagh and Flemming Friche Rodler. Cuckoo hashing. Springer, 2001.

[PR04] Rasmus Pagh and Flemming Friche Rodler. Cuckoo hashing. Journal of Algorithms, 51(2):122–
144, 2004. See also ESA’01.

[PT12] Mihai Patrascu and Mikkel Thorup. The power of simple tabulation-based hashing. Journal of
the ACM, 59(3):Article 14, 2012. Announced at STOC’11.

[PT13] Mihai Patrascu and Mikkel Thorup. Twisted tabulation hashing. In Proc. 24th ACM/SIAM
Symposium on Discrete Algorithms (SODA), pages 209–228, 2013.

[RD01] Antony Rowstron and Peter Druschel. Pastry: Scalable, decentralized object location, and
routing for large-scale peer-to-peer systems. In Middleware 2001, pages 329–350. Springer,
2001.

[RFH+01] Sylvia Ratnasamy, Paul Francis, Mark Handley, Richard Karp, and Scott Shenker. A scalable
content-addressable network, volume 31. ACM, 2001.

[Rod16] Andrew Rodland. Improving load balancing with a new consistent-hashing algo-
rithm. Vimeo Engineering Blog, December 19, 2016. https://medium.com/
vimeo-engineering-blog/improving-load-balancing-with-a-new-
consistent-hashing-algorithm-9f1bd75709ed.

[SMK+01] Ion Stoica, Robert Morris, David Karger, M Frans Kaashoek, and Hari Balakrishnan. Chord:
A scalable peer-to-peer lookup service for internet applications. ACM SIGCOMM Computer
Communication Review, 31(4):149–160, 2001.

[SML+03] Ion Stoica, Robert Morris, David Liben-Nowell, David R. Karger, M. Frans Kaashoek, Frank
Dabek, and Hari Balakrishnan. Chord: a scalable peer-to-peer lookup protocol for internet
applications. IEEE/ACM Trans. Netw., 11(1):17–32, 2003.

[TR98] David Thaler and Chinya V. Ravishankar. Using name-based mappings to increase hit rates.
IEEE/ACM Trans. Netw., 6(1):1–14, 1998.

[TW14] Kunal Talwar and Udi Wieder. Balanced allocations: A simple proof for the heavily loaded
case. In Automata, Languages, and Programming - 41st International Colloquium, ICALP
2014, Copenhagen, Denmark, July 8-11, 2014, Proceedings, Part I, pages 979–990, 2014.

35

[Voc03] Berthold Vocking. How asymmetry helps load balancing. J. ACM, 50(4):568–589, 2003. An-
nounced at FOCS’99.

[Wie07] Udi Wieder. Balanced allocations with heterogenous bins. In SPAA 2007: Proceedings of
the 19th Annual ACM Symposium on Parallelism in Algorithms and Architectures, San Diego,
California, USA, June 9-11, 2007, pages 188–193, 2007.

36


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://arxiv.org/pdf/1608.01350>
- License: [proprietary](<http://arxiv.org/licenses/nonexclusive-distrib/1.0/>)
