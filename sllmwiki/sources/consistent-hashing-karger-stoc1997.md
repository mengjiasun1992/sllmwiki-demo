## Page 1

Consistent Hashing and Random Trees:

Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web

David Karger

Eric Lehman
Tom Leighton Matthew Levine
Daniel Lewin

Rina Panigrahy

Abstract

We describe a family of caching protocols for distrib-uted n
etworks
that can be used to decrease or eliminate the occurrence of ho
t spots
in the network. Our protocols are particularly designed for
use with
very large networks such as the Internet, where delays cause
d by
hot spots can be severe, and where it is not feasible for every
server
to have complete information about the current state of the e
ntire
network. The protocols are easy to implement using existing
net-
work protocols such as TCP/IP, and require very little overh
ead.
The protocols work with local control, make efficient use of e
xist-
ing resources, and scale gracefully as the network grows.
Our caching protocols are based on a special kind of hashing
that we callconsistent hashing. Roughly speaking, a consistent
hash function is one which changes minimally as the range of t
he
function changes. Through the development of good consiste
nt
hash functions, we are able to develop caching protocols whi
ch do
not require users to have a current or even consistent view of
the
network. We believe that consistent hash functions may even
tually
prove to be useful in other applications such as distributed
name
servers and/or quorum systems.

Intro duction

In this paper, we describe caching protocols for distribute
d net-
works that can be used to decrease or eliminate the occurrenc
es
of “hot spots”.Hot spotsoccur any time a large number of clients
wish to simultaneously access data from a single server. If t
he site
is not provisioned to deal with all of these clients simultan
eously,
service may be degraded or lost.
Many of us have experienced the hot spot phenomenon in the
context of the Web. A Web site can suddenly become extremely
popular and receive far more requests in a relatively short t
ime than
This research was supported in part by DARPA contracts N00014-95-
1-1246 and DABT63-95-C-0009, Army Contract DAAH04-95-1-0607, and
NSF contract CCR-9624239
Laboratory for Computer Science, MIT, Cambridge, MA 02139.
email:
f karger,elehman,danl,ftl,mslevine,danl,rinap
g @theory.lcs.mit.edu
A full version of this paper is availble at:
http://theory.lcs.mit.edu/

f karger,elehman,ftl,mslevine,danl,rinap
g
Department of Mathematics, MIT, Cambridge, MA 02139

it was originally configured to handle. In fact, a site may rec
eive so
many requests that it becomes “swamped,” which typically re
nders
it unusable. Besides making the one site inaccessible, heav
y traffic
destined to one location can congest the network near it, int
erfering
with traffic at nearby sites.
As use of the Web has increased, so has the occurrence and
impact of hot spots. Recent famous examples of hot spots on th
e
Web include the JPL site after the Shoemaker-Levy 9 comet str
uck
Jupiter, an IBM site during the Deep Blue-Kasparov chess tou
r-
nament, and several political sites on the night of the elect
ion. In
some of these cases, users were denied access to a site for hou
rs or
even days. Other examples include sites identified as “Web-s
ite-of-
the-day” and sites that provide new versions of popular soft
ware.
Our work was originally motivated by the problem of hot spots
on the World Wide Web. We believe the tools we develop may be
relevant to many client-server models, because centralize
d servers
on the Internet such as Domain Name servers, Multicast serve
rs,
and Content Label servers are also susceptible to hot spots.

P
ast W
ork

Several approaches to overcoming the hot spots have been pro
-
posed. Most use some kind of replication strategy to store co
pies of
hot pages throughout the Internet; this spreads the work of s
erving
a hot page across several servers. In one approach, already i
n wide
use, several clients share a
proxy cache. All user requests are for-
warded through the proxy, which tries to keep copies of frequ
ently
requested pages. It tries to satisfy requests with a cached c
opy; fail-
ing this, it forwards the request to the home server. The dile
mma
in this scheme is that there is more benefit if more users share
the
same cache, but then the cache itself is liable to get swamped
.
Malpani et al. [6] work around this problem by making a group
of caches function as one. A user's request for a page is direc
ted
to an arbitrary cache. If the page is stored there, it is retur
ned to
the user. Otherwise, the cache forwards the request to all ot
her
caches via a special protocol called “IP Multicast”. If the p
age is
cached nowhere, the request is forwarded to the home site of t
he
page. The disadvantage of this technique is that as the numbe
r
of participating caches grows, even with the use of multicas
t, the
number of messages between caches can become unmanageable.
A tool that we develop in this paper,consistent hashing, gives a
way to implement such a distributed cache without requiring
that
the caches communicate all the time. We discuss this in Secti
on 4.
Chankhunthod et al. [1] developed the Harvest Cache, a more
scalable approach using atreeof caches. A user obtains a page by
asking a nearby leaf cache. If neither this cache nor its sibl
ings have
the page, the request is forwarded to the cache's parent. If a
page
is stored by no cache in the tree, the request eventually reac
hes
the root and is forwarded to the home site of the page. A cache

## Page 2

retains a copy of any page it obtains for some time. The advant
age
of a cache tree is that a cache receives page requests only fro
m its
children (and siblings), ensuring that not too many request
s arrive
simultaneously. Thus, many requests for a page in a short per
iod
of time will only cause one request to the home server of the pa
ge,
and won't overload the caches either. A disadvantage, at lea
st in
theory, is that the same tree is used for all pages, meaning th
at the
root receives at least one request for every distinct page re
quested
of the entire cache tree. This can swamp the root if the number
of
distinct page requests grows too large, meaning that this sc
heme
also suffers from potential scaling problems.
Plaxton and Rajaraman [9] show how to balance the load
among all caches by using randomization and hashing. In part
ic-
ular, they use a hierarchy of progressively larger sets of “v
irtual
cache sites” for each page and use a random hash function to as
-
sign responsibility for each virtual site to an actual cache
in the
network. Clients send a request to a random element in each se
t in
the hierarchy. Caches assigned to a given set copy the page to
some
members of the next, larger set when they discover that their
load
is too heavy. This gives fast responses even for popular page
s, be-
cause the largest set that has the page is not overloaded. It a
lso gives
good load balancing, because a machine in a small (thus loade
d) set
for one page is likely to be in a large (thus unloaded) set for a
nother.
Plaxton and Rajaraman's technique is also fault tolerant.
The Plaxton/Rajaraman algorithm has drawbacks, however. F
or
example, since their algorithm sends a copy of each page requ
est
to a random element in every set, the small sets for a popular p
age
are guaranteed to be swamped. In fact, the algorithm uses swa
mp-
ing as a feature since swamping is used to trigger replicatio
n. This
works well in their model of a synchronous parallel system, w
here
a swamped processor is assumed to receive a subset of the inco
m-
ing messages, but otherwise continues to function normally
. On the
Internet, however, swamping has much more serious conseque
nces.
Swamped machines cannot be relied upon to recover quickly an
d
may even crash. Moreover, the intentional swamping of large
num-
bers of random machines could well be viewed unfavorably by t
he
owners of those machines. The Plaxton/Rajaraman algorithm
also
requires that all communications be synchronous and/or tha
t mes-
sages have priorities, and that the set of caches available b
e fixed
and known to all users.

Our Contribution

Here, we describe two tools for data replication and use them
to
give a caching algorithm that overcomes the drawbacks of the
pre-
ceding approaches and has several additional, desirable pr
operties.
Our first tool,random cache trees, combines aspects of the
structures used by Chankhunthod et al. and Plaxton/Rajaram
an.
Like Chankhunthod et al., we use a tree of caches to coalesce r
e-
quests. Like Plaxton and Rajaraman, we balance load by using
a different tree for each page and assigning tree nodes to cac
hes
via a random hash function. By combining the best features of
Chankhunthod et al. and Plaxton/Rajaraman with our own meth-
ods, we prevent any server from becoming swamped with high
probability, a property not possessed by either Chankhunth
od et
al. or Plaxton/Rajaraman. In addition, our protocol shows h
ow to
minimize memory requirements (without significantly incre
asing
cache miss rates) by only caching pages that have been reques
ted a
sufficient number of times.
We believe that the extra delay introduced by a tree of caches
should be quite small in practice. The time to request a page i
s
multiplied by the tree depth. However, the page request typi
cally
takes so little time that the extra delay is not great. The ret
urn of
a page can be pipelined; a cache need not wait until it receive
s a
whole page before sending data to its child in the tree. There
fore,
the return of a page also takes only slightly longer. Altoget
her, the

added delay seen by a user is small.
Our second tool is a new hashing scheme we callconsistent
hashing. This hashing scheme differs substantially from that used
in Plaxton/Rajaraman and other practical systems. Typical
hashing
based schemes do a good job of spreading load through a known,
fixed collection of servers. The Internet, however, does not
have
a fixed collection of machines. Instead, machines come and go
as they crash or are brought into the network. Even worse, the
information about what machines are functional propagates
slowly
through the network, so that clients may have incompatible “
views”
of which machines are available to replicate data. This make
s stan-
dard hashing useless since it relies on clients agreeing on w
hich
caches are responsible for serving a particular page. For ex
ample,
Feeley et al [3] implement a distributed global shared memor
y sys-
tem for a network of workstations that uses a hash table distr
ibuted
among the machines to resolve references. Each time a new ma-
chine joins the network, the require a central server to redi
stribute
a completely updated hash table to all the machines.
Consistent hashing may help solve such problems. Like most
hashing schemes, consistent hashing assigns a set of items t
o buck-
ets so that each bin receives roughly the same number of items
.
Unlike standard hashing schemes, a small change in the bucke
t set
does not induce a total remapping of items to buckets. In addi
-
tion, hashing items into slightly different sets of buckets
gives only
slightly different assignments of items to buckets. We appl
y con-
sistent hashing to our tree-of-caches scheme, and show how t
his
makes the scheme work well even if each client is aware of only
a constant fraction of all the caching machines. In [5] Litwi
n et al
proposes a hash function that allows buckets to be added one a
t a
time sequentially. However our hash function allows the buc
kets to
be added in an arbitrary order. Another scheme that we can imp
rove
on is given by Devine [2]. In addition, we believe that consis
tent
hashing will be useful in other applications (such as quorum
sys-
tems [7] [8] or distributed name servers) where multiple mac
hines
with different views of the network must agree on a common sto
r-
age location for an object without communication.

Presentation

In Section 2 we describe our model of the Web and the hot spot
problem. Our model is necessarily simplistic, but is rich en
ough
to develop and analyze protocols that we believe may be usefu
l in
practice. In Section 3, we describe our random tree method an
d use
it in a caching protocol that effectively eliminates hot spo
ts under a
simplified model. Independent of Section 3, in Section 4 we pr
esent
our consistent hashing method and use it to solve hot spots un
der a
different simplified model involving inconsistent views.
In Section 5 we show how our two techniques can be effectively
combined. In Section 6 we propose a simple delay model that ca
p-
tures hierarchical clustering of machines on the Internet.
We show
that our protocol can be easily extended to work in this more r
eal-
istic delay model. In Sections 7 and 8 we consider faults and t
he
behavior of the protocol over time, respectively. In Sectio
n 9 we
discuss some extensions and open problems.

A Note on Randomization and Hashing

In several places we make use of hash functions that map objec
ts
into a range. For clarity we assume that these functions map o
bjects
in a truly random fashion, i.e. uniformly and independently
. In
practice, hash functions with limited independence are mor
e plau-
sible since they economize on space and randomness. We have
proven all theorems of this paper with only limited independ
ence
using methods similar to those in [11]. However, in this exte
nded
abstract we only state the degree of independence required f
or re-
sults to hold. Proofs assuming limited independence will ap
pear in

## Page 3

the full version of this paper.

Mo del

This section presents our model of the Web and the hotspot pro
b-
lem.
We classify computers on the Web into three categories. All
requests for Web pages are initiated by
browsers. The permanent
homes of Web pages areservers.Caches are extra machines which
we use to protect servers from the barrage of browser request
s.
Throughout the paper, the set of caches is

C and the number of
caches is

C .
Each server is home to a fixed set of pages. Caches are also
able to store a number of pages, but this set may change over ti
me
as dictated by a caching protocol. We generally assume that t
he
content of each page is unchanging, though Section 9 contain
s a
discussion of this issue. The set of all pages is denoted

P .
Any machine can send a message directly to any other with
the restriction that a machine may not be aware of the existen
ce
of all caches; we require only that each machine is aware of a
t
fraction of the caches for some constant
t . The two typical types
of messages are requests for pages and the pages themselves.
A
machine which receives too many messages too quickly ceasesto
function properly and is said to be “swamped”.
L
atency measures the time for a message from machine
m
to arrive at machine
m
. We denote this quantity

m
m
. In
practice, of course, delays on the Internet are not so simply
char-
acterized. The value of
should be regarded as a “best guess” that
we optimize on for lack of better information; the correctne
ss of
a protocol should not depend on values of
(which could actually
measure anything such as throughput, price of connection or
con-
gestion) being exactly accurate. Note that we do not make lat
ency
a function of message size; this issue is discussed in Sectio
n 3.2.1.
All cache and server behavior and some browser behavior is
specified in our protocol. In particular, the protocol speci
fies how
caches and servers respond to page requests and which pages a
re
stored in a cache. The protocol also specifies the cache or ser
ver
to which a browser sends each page request. All control must b
e
local; the behavior of a machine can depend only on messages i
t
receives.
An adversary decides which pages are requested by browsers.
However, the adversary cannot see random values generated i
n our
protocol and cannot adapt his requests based on observed del
ays
in obtaining pages. We consider two models. First, we consid
er a
static model in which a single “batch” of requests is process
ed, and
require that the number of page requests be at most

R C where
is a constant and
C is the number of caches. We then consider
a temporal model in which the adversary may initiate new requ
ests
for pages at rate at most
; that is, in any time interval
, he
may initiate at most
requests.

Objective

The “hot spot problem” is to satisfy all browser page request
s while
ensuring that with high probability no cache or server is swa
mped.
The phrase “with high probability” means “with probability
at least
N ”, where
N is a confidence parameter used throughout the
paper.
While our basic requirement is to prevent swamping, we also
have two additional objectives. The first is to minimize cach
e mem-
ory requirements. A protocol should work well without requi
ring
any cache to store a large number of pages. A second objective
is,
naturally, to minimize the delay a browser experiences in ob
taining
a page.

Random T
rees

In this section we introduce our first tool, random trees. To s
im-
plify the presentation, we give a simple caching protocol th
at would
work well in a simpler world. In particular, we make the follo
wing
simplifications to the model:

1. All machines know about all caches.

2. m i m j for all

i j .

3. All requests are made at the same time.

This restricted model is “static” in the sense that there is o
nly
one batch of requests; we need not consider the long-term sta
bility
of the network.
Under these restrictions we show a protocol that has good be-
havior. That is, with high probability no machine is swamped
. We
achieve a total delay of

log C and prove that it is optimal. We
use total cache space which is a fraction of the number of requ
ests,
and evenly divided among the caches. In subsequent sections
we
will show how to extend the protocol so as to preserve the good
behavior without the simplifying assumptions.
The basic idea of our protocol is an extension of the “tree of
caches” approach discussed in the introduction. We use this
tree to
ensure that no cache has many “children” asking it for a parti
cu-
lar page. As discussed in the introduction, levels near the r
oot get
many requests for a page even if the page is relatively unpopu
lar,
so being the root for many pages causes swamping. Our techniq
ue,
similar to Plaxton/Rajaraman's, is to use a different, rand
omly gen-
erated tree for each page. This ensures that no machine is nea
r the
root for many pages, thus providing good load balancing. Not
e that
we cannot make use of the analysis given by Plaxton/Rajarama
n,
because our main concern is to prevent swamping, whereas the
y
allow machines to be swamped.
In Section 3.1 below, we define our protocol precisely. In Sec
-
tion 3.2, we analyze the protocol, bounding the load on any ca
che,
the storage each cache uses, and the delay a browser experien
ces
before getting the page.

Proto col

We associate a rooted
d -ary tree, called anabstract tree, with each
page. We use the termnodesonly in reference to the nodes of these
abstract trees. The number of nodes in each tree is equal to th
e
number of caches, and the tree is as balanced as possible (so a
ll
levels but the bottom are full). We refer to nodes of the tree b
y
their rank in breadth-first search order. The protocol is des
cribed
as running on these abstract trees; to support this, all requ
ests for
pages take the form of a 4-tuple consisting of the identity of
the re-
quester, the name of the desired page, a sequence of nodes thr
ough
which the request should be directed, and a sequence of cache
s that
should act as those nodes. To determine the latter sequence,
that is,
which cache actually does the work for a given node, the nodes
are
mapped to machines. The root of a tree is always mapped to the
server for the page. All the other nodes are mapped to the cach
es
by a hash function
h P C C , which must be dis-
tributed to all browsers and caches. In order not to create co
pies of
pages for which there are few requests, we have another param
eter,
q , for how many requests a cache must see before it bothers to st
ore
a copy of the page.
Now, given a hash function
h , and parameters
d and q , our pro-
tocol is as follows:

Browser When a browser wants a page, it picks a random leaf to
root path, maps the nodes to machines with

h , and asks the
leaf node for the page. The request includes the name of the
browser, the name of the page, the path, and the result of the
mapping.

## Page 4

Cache When a cache receives a request, it first checks to see if it
is caching a copy of the page or is in the process of getting
one to cache. If so, it returns the page to the requester (afte
r it
gets its copy, if necessary). Otherwise it increments a coun
ter
for the page and the node it is acting as, and asks the next
machine on the path for the page. If the counter reaches

q , it
caches a copy of the page. In either case the cache passes the
page on to the requester when it is obtained.

Server When a server receives a request, it sends the requester a
copy of the page.

Analysis

The analysis is broken into three parts. We begin by showing t
hat
the latency in processing a request is likely to be small, und
er the
assumption that no server is swamped. We then show that no ma-
chine is likely to be swamped. We conclude by showing that no
cache need store too many pages for the protocol to work prope
rly.
The analysis of swamping runs much the same way, except that
the “weights” on our abstract nodes are now the number of requ
ests
arriving at those nodes. As above, the number of requests tha
t hit a
machine is bounded by the weight of nodes mapped to it.

Latency

Under our protocol, the delay a browser experiences in obtai
ning
a page is determined by the height of the tree. If a request is f
or-
warded from a leaf to the root, the latency is twice the length
of
the path,
log
d

C . If the request is satisfied with a cached copy,
the latency is only less. If a request stops at a cache that is w
ait-
ing for a cache copy, the latency is still less since a request
has
already started up the tree. Note that

d can probably be made large
in practice, so this latency will be quite small.
Note that in practice, the time required to obtain a large pag
e is
not multiplied by the number of steps in a path over which it tr
avels.
The reason is that the page can be transmitted along the path i
n
a pipelined fashion. A cache in the middle of the path can star
t
sending data to the next cache as soon as it receives some; it n
eed
not wait to receive the whole page. This means that although t
his
protocol will increase the delay in getting small pages, the
overhead
for large pages is negligible. The existence of tree schemes
, like the
Harvest Cache, suggests that is acceptable in practice.
Our bound is optimal (up to constant factors) for any protoco
l
that forbids swamping. To see this, consider making
C requests
for a single page. Look at the graph with nodes corresponding
to
machines and edges corresponding to links over which the pag
e
is sent. Small latency implies that this graph has small diam
eter,
which implies that some node must have high degree, which im-
plies swamping.

Swamping

The intuition behind our analysis is the following. First we
analyze
the number of requests directed to the abstract tree nodes of
various
pages. These give “weights” to the tree nodes. We then analyz
e the
outcome when the tree nodes are mapped by a hash function onto
the actual caching machines: a machine gets as many requests
as
the total weight of nodes mapped to it. To bound the projected
weight, we first give a bound for the case where each node is as-
signed to a random machine. This is a weighted version of the
familiar balls-in-bins type of analysis. Our analysis give
s a bound
with an exponential tail. We can therefore argue as in [11] th
at it ap-
plies even when the balls are assigned to bins only

k O log N -
way independently. This can be achieved by using a
k -universal
hash function to map the abstract tree nodes to machines.

We will now analyze our protocol under the simplified model.
In this “static” analysis we assume for now that caches have e
nough
space that they never have to evict pages; this means that if a
cache
has already made

q requests for a page it will not make another
request for the same page. In Theorem 3.1 we provide high prob
a-
bility bounds on the number of requests a cache gets, assumin
g that
all the outputs of the function

h are independent and random. The-
orem 3.4 extends our high probability analysis to the case wh
en h
is a
k -way independent function. In particular we show that it suf
-
fices to have

k logarithmic in the system parameters to achieve the
same high probability bounds as with full independence.

Analysis for Random h

Theorem 3.1 If h is chosen uniformly and at random from the
space of functions

P C C then with probability at least
N , where
N is a parameter, the number of requests a given
cache gets is no more than

log
d

C O

log N
log log N

O

dq log N
log

dq
log N

log N

Note that
log
d

C is the average number of requests per cache
since each browser request could give rise to

log
d

C requests up the
trees. The

log N
log log N term arises because at the leaf nodes of a tree's
page some cache could occur log N
log log N times (balls-in-bins) and the
adversary could choose to devote all
R requests to that page. We
prove the above Theorem in the rest of the section.
We split the analysis into two parts. First we analyze the re-
quests to a cache due to its presence in the leaf nodes of the ab
-
stract trees and then analyze the requests due to its presenc
e at the
internal nodes and then add them up.

Requests to Leaf No des

Due to space limitations, we give a proof that only applies wh
en
N R . Its extension to small
N is straightforward but long.
Observe that the requests for each page are being mapped rand
omly
onto the leaf nodes of its abstract tree. And then these leaf n
odes
are mapped randomly onto the set of caches. Look at collectio
n of
all the leaf nodes and the number of requests (weight) associ
ated
with each one of them. The variance among the “weights” of the
leaf nodes is maximized when all the requests are made for one
page. This is also the case which maximizes the number of leaf
node requests on a cache.
Each page's tree has about

C d leaf nodes. Since a
machine m has a C chance of occurring at a particular leaf node,
with probability

N it will occur in
O

log N
log log N

leaf nodes. In
fact, since there are at most
R requests,
m will occur
O

log N
log log N
times in all those requested pages' trees with probability
R
N .
Given an assignment of machines to leaf nodes so that

m occurs
O

log N
log log N

times in each tree, the expected number of requests
m
gets is
R
C

O

log N
log log N

which is
O

log N
log log N

. Also, once the as-
signment of machine to leaf nodes is fixed, the number of reque
sts
m gets is a sum of independent Bernoulli variables. So by Cher-
noff bounds
m gets
O

log N
log log N

log N requests with probability
N . So we conclude that
m gets
O

log N
log log N

log N with
probability at least
R N . Replacing
N by N

and assum-
ing N R we can say that the same bound holds with probability
N . It is easy to extend this proof so that the bound holds
even for

N R .

## Page 5

Requests to Internal No des

Again we think of the protocol as first running on the abstract
trees.
Now no abstract internal node gets more than

dq requests because
each child node gives out at most
q requests for a page. Consider
any arbitrary arrangement of paths for all the

R requests up their
respective trees. Since there are only
R requests in all we can bound
the number of abstract nodes that get

dq requests. In fact we will
bound the number of abstract nodes over all trees which recei
ve
between j and j requests where
q j log dq . Let
n
j denote the number of abstract nodes that receive between

j
and j requests. Let
r
p be the number of requests for page

p .
Then

P
r
p

R . Since each of the

R requests gives rise to at
most log d C requests up the trees, the total number of requests is
no more than R log d C . So,

log dq
X

j

j
n
j

R log
d

C (1)

Lemma 3.2 The total number of internal nodes which receive at
least

q x requests is at most
R x if
x

Proof (sketch):Look at the tree induced by the request paths, con-
tract out degree 1 nodes, and count internal nodes.

For x there can clearly be no more than
R log
d

C requests.
The preceding lemma tells us that

n
j , the number of abstract nodes
that receive between

j and
j requests, is at most
R
j except for
j . For
j , n
j will be at most

R log
d

C . Now the probabil-
ity that machine

m assumes a given one of these
n
j nodes is

C .
Since assignments of nodes to machines are independent the p
rob-
ability that a machine
m is receives more than
z of these nodes is at
most

n
j
z

C

z
en
j

C z

z . In order for the right hand side

to be as small as
N we must have z

n
j
C

log N
log

C
n
j

log N

.

Note that the latter term will be present only if

C
n
j

log N . So

z is
O

n
j
C

log N
log

C
n
j

log N

with probability at least
N .

So with probability at least

log dq N the total number of
requests received by
m due to internal nodes will be of the order of

log dq
X

j

j

n
j
C

log N
log

C
n
j

log N

log
d

C O

dq log N
log

dq
log N

log N

By combining the high probability bounds for internal and le
af
nodes, we can say that a machine gets

log
d

C O

log N
log log N

O

dq log N
log

dq
log N

log N

requests with probability at least
O

log dq
N

. Replacing
N by
N log dq and ignoring
log log dq in comparision with
dq we get
Theorem 3.1.

Tightness of the high p
robabilit
y b ound In this section we
show that the high probability bound we have proven for the nu
m-
ber of requests received by a machine

m is tight.

Lemma 3.3 There exists a distribution of
R requests to pages
so that a given machine

m gets log
d

C

log N
log log N
dq log N
log

dq
log N

requests with probability at least
N .

Proof:Full paper.

Analysis for k w
a
y Indep endent h We now extend our high
probability analysis to functions
h that are chosen at random from
a k -universal hash family.

Theorem 3.4 Ifh is chosen at random from a
k -universal hash
family then with probability at least
N a given cache receives
no more than log d C O k q N d k k dq
log k dq
log k

requests.

Proof:The full proof is deferred to the final version of the paper.
This result does not follow immediately from the results of [
11],
but involves a similar argument.

Setting
k log N we get the following corollary.

Corollary 3.5The high probability bound proved in theorem 3.1
for the number of requests a cache gets holds even if
h is selected
from a log N -universal hash family.

In fact, this can be shown to be true for all the bounds that we w
ill
prove later, i.e., it suffices
k to be logarithmic in the system size.

Storage

In this section, we discuss the amount of storage each cache m
ust
have in order to make our protocol work. The amount of storage
required at a cache is simply the number of pages for which it r
e-
ceives more than

q requests.

Lemma 3.6 The total number of cached pages, over all machines,
is

O log R log
d

C

R
q

with probability at least

R

. A

given cache
m has O
q

log R cached copies with high proba-
bility.

Proof (sketch):The analysis is very similar to that in proof of The-
orem 3.1. We again play the protocol on the abstract trees. Si
nce a
page is cached only if it requested

q times, we assign each abstract
node a weight of one if it gets more than
q requests and zero other-
wise. These abstract nodes are then mapped randomly onto the
set
of caches. We can bound the total weight received by a particu
lar
cache, which is exactly the number of pages it caches.

Consistent Hashing

In this section we define a new hashing technique called
consis-
tent hashing. We motivate this technique by reference to a simple
scheme for data replication on the Internet. Consider a sing
le server
that has a large number of objects that other clients might wa
nt to
access. It is natural to introduce a layer of caches between t
he
clients and the server in order to reduce the load on the serve
r. In
such a scheme, the objects should be distributed across the c
aches,
so that each is responsible for a roughly equal share. In addi
tion,
clients need to know which cache to query for a specific object
.
The obvious approach is hashing. The server can use a hash fun
c-
tion that evenly distributes the objects across the caches.
Clients
can use the hash function to discover which cache stores a obj
ect.
Consider now what happens when the set of active caching ma-
chines changes, or when each client is aware of a
differentset of
caches. (Such situations are very plausible on the Internet
.) If the
distribution was done with a classical hash function (for ex
ample,
the linear congruential function

x ax b mo d p ), such in-
consistencies would be catastrophic. When the range of the h
ash
function (

p in the example) changed, almost every item would be

## Page 6

hashed to a new location. Suddenly, all cached data is useles
s be-
cause clients are looking for it in a different location.
Consistent hashing solves this problem of different “views
.” We
define a viewto be the set of caches of which a particular client is
aware. We assume that while views can be inconsistent, they a
re
substantial: each machine is aware of a constant fraction of
the cur-
rently operating caches. A client uses a consistent hash fun
ction
to map a object to one of the caches in its view. We analyze and
construct hash functions with the following consistency pr
operties.
First, there is a “smoothness” property. When a machine is ad
ded
to or removed from the set of caches, the expected fraction of
ob-
jects that must be moved to a new cache is the minimum needed
to maintain a balanced load across the caches. Second, over a
ll the
client views, the total number of different caches to which a
object
is assigned is small. We call this property “spread”. Simila
rly, over
all the client views, the number of distinct objects assigne
d to a
particular cache is small. We call this property “load”.
Consistent hashing therefore solves the problems discusse
d
above. The “spread” property implies that even in the presen
ce
of inconsistent views of the world, references for a given ob
ject are
directed only to a small number of caching machines. Distrib
uting
a object to this small set of caches will insure access for all
clients,
without using a lot of storage. The “load” property implies t
hat
no one cache is assigned an unreasonable number of objects. T
he
“smoothness” property implies that smooth changes in the se
t of
caching machines are matched by a smooth evolution in the loc
a-
tion of cached objects.
Since there are many ways to formalize the notion of consis-
tency as described above, we will not commit to a precise defin
i-
tion. Rather, in Section 4.4 we define a “ranged hash function
” and
then precisely define several quantities that capture diffe
rent as-
pects of “consistency”. In Section 4.2 we construct practic
al hash
functions which exhibit all four to some extent. In Section 4
.4, we
discuss other aspects of consistent hashing whihc, though n
ot ger-
mane to this paper, indicate some of the richness underlying
the
theory.

Denitions

In this section, we formalize and relate four notions of cons
istency.
Let I be the set of items and

B be the set of buckets. Let
I jI j
be the number of items. Aviewis any subset of the buckets
B .
A ranged hash functionis a function of the form
f

B
I
B . Such a function specifies an assignment of items to buckets
for every possible view. That is,
f V i is the bucket to which
item i is assigned in view

V . (We will use the notation
f
V

i in
place
f V i from now on.) Since items should only be assigned
to usable buckets, we require

f
V

I V for every view
V .
A ranged hash familyis a family of ranged hash functions. A
random ranged hash functionis a function drawn at random from
a particular ranged hash family.
In the remainder of this section, we state and relate some rea
-
sonable notions of consistency regarding ranged hash famil
ies.
Throughout, we use the following notational conventions:

F is a
ranged hash family,
f is a ranged hash function,
V is a view,
i is an
item, and

b is a bucket.

Balance:A ranged hash family isbalancedif, given a particu-
lar view
V a set of items, and a randomly chosen function selected
from the hash family, with high probability the fraction of i
tems
mapped to each bucket is

O j V j .

The balance property is what is prized about standard hash
functions: they distribute items among buckets in a balance
d fa-
sion.

Monotonicity:A ranged hash function
f ismonotone if for all
views V V B , f V i V implies
f
V

i f
V

i . A

ranged hash family ismonotone if every ranged hash function in it
is.

This property says that if items are initially assigned to a s
et
of buckets
V
and then some new buckets are added to form

V
,
then an item may move from an old bucket to a new bucket, but not
from one old bucket to another. This reflects one intuition ab
out
consistency: when the set of usable buckets changes, items s
hould
only move if necessary to preserve an even distribution.

Spread: Let V V V be a set of views, altogether containing
C distinct buckets and each individually containing at least
C t
buckets. For a ranged hash function and a particular item
i , the
spread i is the quantity

f f
V
j

i g

V
j

. Thespreadof a hash
function
f is the maximum spread of an item. Thespreadof
a hash family is

if with high probability, the spread of a random
hash function from the family is
.

The idea behind spread is that there are

V people, each of whom
can see at least a constant fraction (
t ) of the buckets that are
visible to anyone. Each person tries to assign an item

i to a bucket
using a consistent hash function. The property says that acr
oss the
entire group, there are at most
i different opinions about which
bucket should contain the item. Clearly, a good consistent h
ash
function should have low spread over all items.

Load: Define a set of

V views as before. For a ranged hash
function
f and bucket
b , theload b is the quantity

S
V

f
V

b

.
The loadof a hash function is the maximum load of a bucket. The
loadof a hash family is
if with high probability, a randomly cho-
sen hash function has load

. (Note that
f
V

b is the set of items

assigned to bucket

b in view V .) The load property is similar to
spread. The same
V people are back, but this time we consider a
particular bucket

b instead of an item. The property says that there
are at most

b distinct items that at least one person thinks be-
longs in the bucket. A good consistent hash function should a
lso
have low load.
Our main result for consistent hashing is Theorem 4.1 which
shows the existence of an efficiently computable monotonic r
anged
hash family with logarithmic spread and balance.

Construction

We now give a construction of a ranged hash family with good
properties. Suppose that we have two random functions

r
B and

r
I .
The function

r
B maps buckets randomly to the unit interval, and

r
I
does the same for items.
f
V

i is defined to be the bucket
b V
that minimizes
j r
B

b r
I

i j . In other words,
i is mapped to the
bucket “closest” to

i . For reasons that will become apparent, we ac-
tually need to have more than one point in the unit interval as
soci-
ated with each bucket. Assuming that the number of buckets in
the
range is always less than
C , we will need
log C points for each
bucket for some constant

. The easiest way to view this is that
each bucket is replicated

log C times, and then
r
B maps each
replicated bucket randomly. In order to economize on the spa
ce to
represent a function in the family, and on the use of random bi
ts,
we only demand that the functions

r
B and

r
I map points

log C -
way independently and uniformly to
. Note that for each point
we pick in the unit interval, we need only pick enough random b
its
to distinguish the point from all other points. Thus it is unl
ikely
that we need more than

log number of points
bits for each point.
Denote the above described hash family as

F .

Theorem 4.1 The ranged hash family
F described above has the
following properties:

1. F is monotone.

## Page 7

2. Balance: For a fixed view
V , Pr f
V

i b

O
jV j for
i I and b V , and, conditioned on the choice of
r
B , the
assignments of items to buckets are
log C -way independent.

3. Spread: If the number of views

V C for some constant
, and the number of items
I C , then for
i I , i is
O t log C with probability greater than
C

.

4. Load: If

V and I are as above, then for
b B , b is
O t log C with probability greater than
C

.

Proof (sketch):Monotonicity is immediate. When a new bucket
is added, the only items that move are those that are now close
st to
one of the new bucket's associated points. No items move betw
een
old buckets. The spread and load properties follow from the o
bser-
vation that with high probability, a point from
everyview falls into
an interval of length

O tC . Spread follows by observing that
the number of bucket points that fall in this size interval ar
ound
an item point is an upper bound on the spread of that item, sinc
e
no other bucket can be closer in any view. Standard Chernoff a
rgu-
ments apply to this case. Load follows by a similar argument w
here
we count the number of item points that fall in the region “own
ed”
by a bucket's associated points. Balance follows from the fa
ct that
when log C points are randomly mapped to the unit interval,
each bucket is with highu probability responsible for no mor
e than
a O
jV j fraction of the interval. The key here is to count the number
of combinatroially distinct ways of assigning this large a f
raction to
the log C points associated with a bucket. This turns out to be
polynomial in

C . We then argue that with high probability none of
these possibilities could actually occur by showing that in
each one
an additional bucket point is likely to fall. We deduce that t
he ac-
tual length must be smaller than

O jV j . All of the above proofs
can be done with only
log C -way independent mappings.

The following corollary is immediate and is useful in the res
t
of the paper.

Corollary 4.2With the same conditions of the previous theorem,
Pr f
V

i b inany view

O t log C
jV j for

i I and b B .

Implementation

In this section we show how the hash family just dexcrobed can
be implemented efficiently. Specifically, the expected runn
ing time
for a single hash computation will be
O . The expectation is over
the choice of hash function. The expected running time for ad
ding
or deleting a bucket will be
O log C where C is an upper bound
on the total number of buckets in all views.
A simple implementation uses a balanced binary search tree
to store the correspondence between segments of the unit int
erval
and buckets. If there are

C buckets, then there will be
C log C
intervals, so the search tree will have depth
O log C . Thus, a
single hash computation takes

O log C time. The time for an
addition or removal of a bucket is
O log
C since we insert or
delete

log C points for each bucket.
The following trick reduces the expected running time of a ha
sh
computation to
O . The idea is to divide the interval into roughly
C log C equal length segments, and to keep a separate search
tree for each segment. Thus, the time to compute the hash func
tion
is the time to determine which interval
r
I

i is in, plus the time
to lookup the bucket in the corresponding search tree. The fir
st
time is always
O . Since, the expected number of points in each
segment is

O , the second time is
O in expectation.
One caveat to the above is that as the number of buckets grows,
the size of the subintervals needs to shrink. In order to deal
with
this issue, we will use intervals only of length

x for some
x . At
first we choose the largest
x such that

x
C log C . Then,

as points are added, we bisect segments gradually so that whe
n we
reach the next power of
, we have already divided all the segments.
In this way we amortize the work of dividing search trees over
all of
the additions and removals. Another point is that the search
trees in
adjacent empty intervals may all need to be updated when a buc
ket
is added since they may all now be closest to that bucket. Sinc
e the
expected length of a run of empty intervals is small, the addi
tional
cost is negligible. For a more complete analysis of the runni
ng time
we refer to the complete version of the paper.

Some Theorems on Consistent Hashing

In this section, we discuss some additional features of cons
istent
hashing which, though unneccessary for the remainder of the
paper,
demonstrate some of its interesting properties.
To give insight into the monotone property, we will define a
new class of hash functions and then show that this is equival
ent to
the class of monotone ranged hash functions.
A -hash functionis a hash function of the familiar form
f

B
I B constructed as follows. With each item
i I ,
associate a permutation

i of all the buckets
B . Define f
V

i to
be the first bucket in the permutation
i that is contained in the
view V . Note that the permutations need not be chosen uniformly
or independently.

Theorem 4.3 Every monotone ranged hash function is a

-hash
function and vice versa.

Proof (sketch):For a ranged hash function
f , associate item
i with
the permutation

b
f
B

i b
j

f
B f b
b
j

g

i .
Suppose b j is the first element of an arbitrary view
V
in this
permutation. Then

V
V
B f b
b
j

g . Since
f
V

i b
j

V
, monotonicity implies

f
V

i b
j .

The equivalence stated in Theorem 4.3 allows us to reason
about monotonic ranged hash functions in terms of permutati
ons
associated with items.

Universality:A ranged hash family isuniversalif restricting
every function in the family to a single view creates a univer
sal
hash family.

This property is one way of requiring that a ranged hash func-
tion be well-behaved in every view. The above condition is ra
ther
stringent; it says that if a view is fixed, items are assigned r
an-
domly to the bins in that view. This implies that in any view
V ,
the expected fraction of items assigned to

j of the buckets is
j jV j .
Using only monotonicity and this fact about the uniformity o
f the
assignment, we can determine the expected number of items re
as-
signed when the set of usable buckets changes. This relates t
o the
informal notion of “smoothness”.

Theorem 4.4 Let f be a monotonic, universal ranged hash func-
tion. Let

V
and

V
be views. The expected fraction of items

i for
which f V i f V i is jV V j
jV
V
j .

Proof (sketch):Count the number of items that move as we add
buckets from
V
until the view is

V
V
, and then delete buckets
down to V

Note that monotonicity is used only to show an upper bound on
the number of items reassigned to a new bucket; this implies t
hat
one can not obtain a “more consistent” universal hash functi
on by
relaxing the monotone condition.
We have shown that every monotone ranged hash function can
be obtained by associating each item with a random permutati
on

## Page 8

of buckets. The most natural monotone consistent hash funct
ion
is obtained by choosing these permutations independently a
nd uni-
formly at random. We denote this function by

f .

Theorem 4.5 The function
f is monotonic and universal. For item
i and bucket
b each of the following hold with probability at least
N : i t log N V and
b

p
C
I t

tI log N V I C 2.

Proof: Monotonicity and universality are immediate; this leaves
spread and load. Define:

t log N V
r
C
tI

tI log N V I
C

We use i to denote a list of the buckets in
V
V
V
which are ordered as in
i .
First, consider spread. Recall that in a particular view, it
em i
is assigned to the first bucket in
i which is also in the view.
Therefore, if every view contains one of the first

buckets in
i
then in every view item
i will be assigned to one of the first
buckets in
i . This implies that item
i is assigned to at most
distinct buckets over all the views.
We have to show that with high probability every view contain
s
one of the first
buckets in
i . We do this by showing that the
complement has low probability; that is, the probability th
at some
view contains none of the first
buckets is at most
N .
The probability that a particular view does not contain the fi
rst
bucket in
i is at most
t , since each view contains at least
a t fraction of all buckets. The fact that the first bucket is not
in a view only reduces the probability that subsequent bucke
ts are
not in the view. Therefore, the probability that a particula
r view
contains none of the first

buckets is at most
t
t

t log N V
N V . By the union bound, the probability
that even one of the

V views contains none of the first
buckets is
at most
N .
Now consider load. By similar reasoning, every item

i in every
view is assigned to one of the first
t log N V I buckets in
i
with probability at least
N . We show below that a fixed
bucket
b appears among the first
t log N V I buckets in
i for
at most

items
i with probability at least
N . By the union
bound, both events occur with high probability. This implie
s that at
most items are assigned to bucket
b over all the views.
All that remains is to prove the second statement. The ex-
pected number of items

i for which the bucket
b appears among
the first
t log N V I buckets in
i istI log N V I C . Us-
ing Chernoff bounds, we find that bucket

b appears among the first
t log N V I buckets in
i for at most
items
i with probability
at least

N V I N .

A simple approach to constructing a consistent hash functio
n is
to assign random scores to buckets, independently for each i
tem.
Sorting the scores defines a random permutation, and therefo
re has
the good properties proved in the this section. However, find
ing the
bucket an item belongs in requires computing all the scores.
This
could be restrictivly slow for large bucket sets.

Random T
rees in an Inconsistent W
orld

In this section we apply the techniques developed in the last
sec-
tion to the simple hot spot protocol developed in section 3. W
e now
relax the assumption that clients know about all of the cache
s. We

assume only that each machine knows about a
t fraction of the
caches chosen by an adversary. There is no difference in the p
roto-
col, except that the mapping
h is a consistent hash function. This
change will not affect latency. Therefore, we only analyze t
he ef-
fects on swamping and storage. The basic properties of consi
stent
hashing are crucial in showing that the protocol still works
well.
In particular, the blowup in the number of requests and stora
ge is
proportional to the maximum

and of the hash function.

Swamping

Theorem 5.1 If
h is implemented using the
log C -way indepen-
dent consistent hash function of Theorem 4.1 and if each view
con-
sists of
C
C t caches then with probability at least
C
an arbitrary cache gets no more than
O t
log
d

C
log C
dq t log C t log C requests.

Proof (sketch):We look at the different trees of caches for dif-
ferent views for one page,
p . Let
C
C t denote the number
of caches in each tree. We overlay these different trees on on
e an-
other to get a new tree where in each node, there is a
setof caches.
Due to thespreadproperty of the consistent hash function at most
O t log C caches appear at any node in this combined tree
with high probability. In fact since there are only
R requests, this
will be true for the nodes of all the

R trees for the requested pages.
If E pj denotes the event that

m appears in the
j

th node of the
combined tree for page
p then we know from Corrollary 4.2 that
the probability of this event is

O C , where
is theloadwhich
is
O t log C with high probability. We condition on the event that
and are
O t log C which happens with high probability.
Since a cache in a node sends out at most

q requests, each node
in the combined tree sends out at most
q requests. We now adapt
the proof of Theorem 3.1 to this case. In Theorem 3.1 where eve
ry
machine was aware of all the

C caches, an abstract node was as-
signed to any given machine with probability
C . We now assign
and abstract node to a given machine with probability

C . So
we have a scenario with
C
C t caches where each abstract node
sends out up to

q

requests to its parent and
m occurs at each ab-
stract node independently and with probability

C . The rest
of the proof is very similar to that of Theorem 3.1.

Storage

Using techniques similar to those in proof of Theorem 5.1 we g
et
the following lemma. The proof is deferred to the final versio
n of
the paper.

Lemma 5.2 The total number of cached pages, over all machines
is
O log R log
d

C

R
q

with probability of
R

. A

given cache

m has O q log R cached copies with high
probability.

Nonuniform Communication Costs

So far we assumed that every pair of machines can communicate
with equal ease. In this section we extend our protocol to tak
e
the latency between machines,
, into account. The latency of the
whole request will be the sum of the latencies of the machine-
machine links crossed by the request. For simplicity, we ass
ume
in this section that all clients are aware of all caches.
We extend our protocol to a restricted class of functions

. In
particular, we assume that
is anultrametric. Formally, an ultra-
metric is a metric which obeys a more strict form of the triang
le
inequality:

x z max x y y z .

## Page 9

The ultrametric is a natural model of Internet distances, si
nce it
essentially captures the hierarchical nature of the Intern
et topology,
under which, for example, all machines in a given university
are
equidistant, but all of them are farther away from another un
iver-
sity, and still farther from another continent. The logical
point-to-
point connectivity is established atop a physical network,
and it is
generally the case that the latency between two sites is dete
rmined
by the “highest level” physical communication link that mus
t be
traversed on the path between them. Indeed, another definiti
on of
an ultrametric is as a hierarchical clustering of points. Th
e distance
in the ultrametric between two points is completely determi
ned by
the smallest cluster containing both of the points.

Proto col

The only modification we make to the protocol is that when a
browser maps the tree nodes to caches, it only uses caches tha
t
are as close to it as the server of the desired page. By doing th
is,
we insure that our path to the server does not contain any cach
es
that are unnecessarily far away in the metric. The mapping is
done
using a consistent hash function, which is the vital element
of the
solution.
Clearly, requiring that browsers use “nearby” caches can ca
use
swamping if there is only one cache and server near many brows
ers.
Thus, in order to avoid cases of degenerate ultrametrics whe
re there
are browsers that are not close to any cache, and where there a
re
clusters in the ultrametric without any caches in them, we re
strict
the set of ultrametrics that may be presented to the protocol
. The re-
striction is that in any cluster the ratio of the number of cac
hes to the
number of browsers may not fall below
(recall that
R C ).
This restriction makes sense in the real world where caches a
re
likely to be evenly spread out over the Internet. It is also ne
ces-
sary, as we can prove that a large number of browsers clustere
d
around one cache can be forced to swamp that cache in some cir-
cumstances.

Analysis

It is clear from the protocol and the definition of an ultramet
ric that
the latency will be no more than the depth of the tree,

log
d

C , times
the latency between the browser and the server. So once again
we
need only look at swamping and storage. The intuition is that
inside
each cluster the bounds we proved for the unit distance model
ap-
ply. The monotone property on consistent hashing will allow
us to
restrict our analysis to

log C clusters. Thus, summing over these
clusters we have only a
log C blowup in the bound.

Swamping

Theorem 6.1 Let be an ultrametric. Suppose that each browser
makes at most one request. Then in the protocol above, an arbi
-
trary cache gets no more than
log C log
d

C O

log N
log log N
O

dq log N
log

dq
log N

log N requests with probability at least

N where N is a parameter.

Proof (sketch):The intuition behind are proof is the following. We
bound the load on a machine

m . Consider the ranking of machines
m
m
according to their distance from
m . Suppose
m
i asks
for a page from a machine closer to itself than

m . Then according
to our modified protocol, it will never involve

m in the request. So
we need only consider machine
m
i if it asks for a page at least as far
away from itself as

m . It follows from the definition of ultrametrics
that every

m
j ,

j i , is also used in the revised protocol by
m
i .
Intuitively, our original protocol spread load among the ma
-
chines so that the probability a machine got on the path for a p
ar-
ticular page requests was
O log
d

C C . In our ultrametric pro-

tocol,
m
i plays the protocol on a set of at least

i machines. So
m is
on the path of the request from

m
i with probability

O log
d

i i .
Summing over i , the expected load on
m is
O log C .
Stating things slightly more formally, we consider a set of

log C
nested “virtual” clusters
C
i

f m
m
i

g . Note that any
browser in
C
i

C
i will use all machines in

C
i in the protocol. We
modify the protocol so that such a machine uses
onlythe machines
inC i . This only reduces the number of machines it uses. According
to the monotonicity property of our consistent hash functio
ns, this
only increases the load on machine

m .
Now we can consider each C i separately and apply the static
analysis. The total number of requests arriving in one of the
clusters
under the modified protocol is proportional to the number of c
aches
in the cluster, so our static analysis applies to the cluster
. This
gives us a bound of

O log
d

C on the load induced on
m by C
i .
Sumnming over the log C clusters proves the theorem.

Storage

Using techniques similar to those in proof of Theorem 6.1 we g
et
the following lemma.

Lemma 6.2 The total number of cached pages, over all machines
is

O R q log R log
d

C log C with probability of
R

. A
given cache

m has O log C q log R cached copies with high
probability.

Fault T
olerance

Basically, as in Plaxton/Rajaraman, the fact that our proto
col uses
random short paths to the server makes it fault tolerant. We c
on-
sider a model in which an adversary designates that some of th
e
caching machines may bedown , that is, ignore all attempts at com-
munication. Remember that our adversary does not get to see o
ur
random bits, and thus cannot simply designate all machines a
t the
top of a tree to be down. The only restriction is that a specifie
d
fraction
s of the machines in every view must be up. Under our
protocol, no preemptive caching of pages is done. Thus, if a s
erver
goes down, all pages that it has not distributed become inacc
essible
to any algorithm. This problem can be eliminated using stand
ard
techniques, such as Rabin's Information Dispersal Algorit
hm [10].
So we ignore server faults.
Observe that a request is satisfied if and only if all the cache
s
serving for the nodes of the tree path are not down. Since each
node is mapped to a machine (

k -wise) independently, it is trivial
(using standard Chernoff bounds) to lower bound the number o
f
abstract nodes that have working paths to the root. This lead
s to the
following lemma:

Lemma 7.1 Suppose that

d log N . With high probability,
the fraction of abstract-tree leaves that have a working pat
h to the
root is
s

log
d

C
. In particular, if
s O log
d

C , this
fraction is a constant.

The modification to the protocol is therefore quite simple.
Choose a parameter

t , and simultaneously send
t requests for the
page. A logarithmic number if requests is sufficient to give a
high
probability of one of the requests goes through. This change
in the
protocol will of course have an impact on the system. This imp
act
is described in the full paper.
Note that since communication is a chancy thing on the Inter-
net, failure to get a quick response from a machine is not a par
-
ticularly good indication that it is down. Thus, we focused o
n the
tolerance of faults, and not on their detection. However, gi
ven some
way to decide that a machine is down, our consistent hash func
tions
make it trivial to reassign the work to other machines. If a yo
u de-
cide a machine is down, remove it from your view.

## Page 10

Adding Time to the Mo del

So far, we have omitted any real mention of time from our analy
-
ses. We have instead considered and analyzed a single “batch
” of
R requests, and argued that this batch causes a limited amount
of
caching (storage usage) at every machine, while simultaneo
usly ar-
guing that no machine gets swamped by the batch. In this secti
on,
we show how this static analysis carries implications for a t
emporal
model in which requests arrive over time. Recall that our tem
poral
model says that browsers issues requests at a certain rate

.
Time is a problematic issue when modeling the Internet, be-
cause the communication protocols for it have no guarantees
re-
garding time of delivery. Thus any one request could take arb
i-
trarily long. However, we can consider the rate at which serv
ers
receive requests. This seems like an overly simplistic meas
ure, but
the rate at which a machine can receive requests is in fact the
statis-
tic that hardware manufacturers advertise. We consider an i
nterval
of time
, and apply our “requests all come at once” analysis to the
requests that come in this interval.
We can write the bounds from the static analysis on
R requests
as follows:

cache size

a
s

R b
s cache load

a
l

R b
l

Suppose machines have cache size
m . Consider a time interval
small enough to make

R small enough so that
m a
s

R b
s .
In other words, the number of requests that arrive in this int
erval is
insufficient, according to our static analysis, to use stora
ge exceed-
ing m per machine. Thus once a machine caches a page during this
interval, it keeps it for the remainder of the interval. Thus
our static
analysis will apply over this interval. This gives us a bound
on how
many requests can arrive in the interval. Dividing by the int
erval
length, we get the rate at which caches see requests:
a
l

b
l

a
s
m b
s

.
Plugging in the bounds from Section 3, we get the following:

Theorem 8.1 If our machines have

m log N storage, for
some constant
N , then with probability
N , the bound on the
rate of new requests per cache when we have

C machines of size
m is

log
d

C
C

O

dq
m

.

Observe the tradeoffs implicit in this theorem. Increasing
m causes the load to decrease proportionately, but never belo
w
log C C . Increasing
d increases the load linearly (but re-
duces the number of hops on a request path). Increasing

q seems
only to hurt, suggesting that we should always take
q .
The above analysis used the rate at which requests were issue
d
to measure the rate at which connections are established to m
a-
chines. If we also assume that each connection lasts for a fini
te
duration, this immediately translates into a bound on the nu
mber of
connections open at a machine at any given time.

Conclusion

This paper has focused on one particular caching problem—tha
t
of handling read requests on the Web. We believe the ideas hav
e
broader applicability. In particular, consistent hashing
may be a
useful tool for distributing information from name servers
such as
DNS and label servers such as PICS in a load-balanced and faul
t-
tolerant fashion. Our two schemes may together provide an in
ter-
esting method for constructing multicast trees [4].
Another important way in which our ideas could be extended
is in handling pages whose information changes over time, du
e to
either server or client activity. If we augment our protocol
to let the
server know which machines are currently caching its page, t
hen
the server can notify such caches whenever the data on its pag
es
changes. This might work particularly well in conjunction w
ith the
currently under developmentmulticastprotocols [4] that broadcast

information from a server to all the client members of a multi
cast
“group.” Our protocol can be mapped into this model if we assu
me
that every machine “caching” a page joins a multicast group f
or that
page. Even without multicast, if each cache keeps track, for
each
page it caches, of the at most

d other caches it has given the page
to, then notification of changes can be sent down the tree to on
ly
the caches that have copies.
It remains open how to deal with time when modeling the In-
ternet, because the communication protocols have no guaran
tees
regarding time of delivery. Indeed, at the packet level, the
re are not
even guarantees regarding eventual delivery. This suggest
s model-
ing the Internet as some kind of distributed system. Clearly
, in a
model in which there are no guarantees regarding delivery ti
mes,
the best one can hope to prove is some of the classical
livenessand
safetyproperties underlying distributed algorithms. It is not cl
ear
what one can prove about caching and swamping in such a model.
We think that there is significant research to be done on the pr
oper
way to model this aspect of the Internet.
We also believe that interesting open questions remain rega
rd-
ing the method of consistent hashing that we present in this p
aper.
Among them are the following. Is there a
k -universal consistent
hash function that can be evaluated efficiently?? What trade
offs
can be achieved between spread and load? Are there some kind o
f
“perfect” consistent hash functions that can be constructe
d deter-
ministically with the same spread and load bounds we give? On
what other theoretical problems can consistent hashing giv
e us a
handle?

References

[1] Anawat Chankhunthod, Peter Danzig, Chuck Neerdaels, Michael
Schwartz and Kurt Worrell. A Hierarchical Internet Object C
ache. In
USENIX Proceedings, 1996.
[2] Robert Devine. Design and Implementation of DDH: A Distributed
Dynamic Hashing Algorithm. InProceedings of 4th International Con-
ference on Foundations of Data Organizations and Algorithm
s, 1993.
[3] M. J. Feeley, W. E. Morgan, F. P. Pighin, A. R. Karlin, H. M.
Levy
and C. A. Thekkath. Implementing Global Memory Management in a
Workstation Cluster. InProceedings of the 15th ACM Symposium on
Operating Systems Principles, 1995.
[4] Sally Floyd, Van Jacobson, Steen McCanne, Ching-Gung Liu and
Lixia Zhang. A Reliable Multicast Framework for Light-weig
ht Ses-
sions and Application Level Framing,SIGCOMM' 95
[5] Witold Litwin, Marie-Anne Neimat and Donovan A. Schneider.
LH

-A Scalable, Distributed Data Structure. ACM Transactions
on
Database Systems, Dec. 1996
[6] Radhika Malpani, Jacob Lorch and David Berger. Making World Wide
Web Caching Servers Cooperate. InProceedings of World Wide Web
Conference, 1996.
[7] M. Naor and A. Wool. The load, capacity, and availability
of quorum
systems. InProceedings of the 35th IEEE Symposium on Foundations
of Computer Science, pages 214-225, November 1994.
[8] D. Peleg and A. Wool. The availability of quorum systems.
Information
and Computation123(2):210-233, 1995.
[9] Greg Plaxton and Rajmohan Rajaraman. Fast Fault-Tolera
nt Concur-
rent Access to Shared Objects. InProceedings of 37th IEEE Sympo-
sium on Foundations of Computer Science, 1996.
[10] M. O. Rabin. Efficient dispersal of Information for Secu
rity, Load Bal-
ancing, and Fault Tolerance.Journal of the ACM36:335–348, 1989.
[11] Jeanette Schmidt, Alan Siegel and Aravind Srinivasan.
Chernoff-
Hoeffding Bounds for Applications with Limited Independen
ce. In
Proc. 4th ACS-SIAM Symposium on Discrete Algorithms, 1993.


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://people.csail.mit.edu/karger/Papers/web.pdf>
- License: proprietary
