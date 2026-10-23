## Page 1

Operating R. Stockton Gaines
Systems Editor

Time, Clocks, and the

Ordering of Events in

a Distributed System

Leslie Lamport
Massachusetts Computer Associates, Inc.

The concept of one event happening before another
in a distributed system is examined, and is shown to
define a partial ordering of the events. A distributed
algorithm is given for synchronizing a system of logical
clocks which can be used to totally order the events.
The use of the total ordering is illustrated with a
method for solving synchronization problems. The
algorithm is then specialized for synchronizing physical
clocks, and a bound is derived on how far out of
synchrony the clocks can become.
Key Words and Phrases: distributed systems,
computer networks, clock synchronization, multiprocess
systems
CR Categories: 4.32, 5.29

Introduction

The concept of time is fundamental to our way of
thinking. It is derived from the more basic concept of
the order in which events occur. We say that something
happened at 3:15 if it occurred after our clock read 3:15
and before it read 3:16. The concept of the temporal
ordering of events pervades our thinking about systems.
For example, in an airline reservation system we specify
that a request for a reservation should be granted if it is
made before the flight is filled. However, we will see that
this concept must be carefully reexamined when consid-
ering events in a distributed system.
General permission to make fair use in teaching or research of all
or part of this material is granted to individual readers and to nonprofit
libraries acting for them provided that ACM's copyright notice is given
and that reference is made to the publication, to its date of issue, and
to the fact that reprinting privileges were granted by permission of the
Association for Computing Machinery. To otherwise reprint a figure,
table, other substantial excerpt, or the entire work requires specific
permission as does republication, or systematic or multiple reproduc-
tion.
This work was supported by the Advanced Research Projects
Agency of the Department of Defense and Rome Air Development
Center. It was monitored by Rome Air Development Center under
contract number F 30602-76-C-0094.
Author's address: Computer Science Laboratory, SRI Interna-
tional, 333 Ravenswood Ave., Menlo Park CA 94025.
© 1978 ACM 0001-0782/78/0700-0558 $00.75

558

A distributed system consists of a collection of distinct
processes which are spatially separated, and which com-
municate with one another by exchanging messages. A
network of interconnected computers, such as the ARPA
net, is a distributed system. A single computer can also
be viewed as a distributed system in which the central
control unit, the memory units, and the input-output
channels are separate processes. A system is distributed
if the message transmission delay is not negligible com-
pared to the time between events in a single process.
We will concern ourselves primarily with systems of
spatially separated computers. However, many of our
remarks will apply more generally. In particular, a mul-
tiprocessing system on a single computer involves prob-
lems similar to those of a distributed system because of
the unpredictable order in which certain events can
occur.
In a distributed system, it is sometimes impossible to
say that one of two events occurred first. The relation
"happened before" is therefore only a partial ordering
of the events in the system. We have found that problems
often arise because people are not fully aware of this fact
and its implications.
In this paper, we discuss the partial ordering defined
by the "happened before" relation, and give a distributed
algorithm for extending it to a consistent total ordering
of all the events. This algorithm can provide a useful
mechanism for implementing a distributed system. We
illustrate its use with a simple method for solving syn-
chronization problems. Unexpected, anomalous behav-
ior can occur if the ordering obtained by this algorithm
differs from that perceived by the user. This can be
avoided by introducing real, physical clocks. We describe
a simple method for synchronizing these clocks, and
derive an upper bound on how far out of synchrony they
can drift.

The Partial Ordering

Most people would probably say that an event a
happened before an event b if a happened at an earlier
time than b. They might justify this definition in terms
of physical theories of time. However, if a system is to
meet a specification correctly, then that specification
must be given in terms of events observable within the
system. If the specification is in terms of physical time,
then the system must contain real clocks. Even if it does
contain real clocks, there is still the problem that such
clocks are not perfectly accurate and do not keep precise
physical time. We will therefore define the "happened
before" relation without using physical clocks.
We begin by defining our system more precisely. We
assume that the system is composed of a collection of
processes. Each process consists of a sequence of events.
Depending upon the application, the execution of a
subprogram on a computer could be one event, or the
execution of a single machine instruction could be one

Communications July 1978
of Volume 21
the ACM Number 7

## Page 2

Fig. 1.

a, CY ,Y

(9 (9 ~o
~ o

P4'

P3

P2'

Pl ~

q7

q6

q5

ql

r 4

r 3

r 2

r 1

event. We are assuming that the events of a process form
a sequence, where a occurs before b in this sequence if
a happens before b. In other words, a single process is
defined to be a set of events with an a priori total
ordering. This seems to be what is generally meant by a
process.~ It would be trivial to extend our definition to
allow a process to split into distinct subprocesses, but we
will not bother to do so.
We assume that sending or receiving a message is an
event in a process. We can then define the "happened
before" relation, denoted by "---~", as follows.
Definition. The relation "---->" on the set of events of
a system is the smallest relation satisfying the following
three conditions: (1) If a and b are events in the same
process, and a comes before b, then a ~ b. (2) If a is the
sending of a message by one process and b is the receipt
of the same message by another process, then a ~ b. (3)
If a ~ b and b ~ c then a ---* c. Two distinct events a
and b are said to be concurrent if a ~ b and b -/-* a.
We assume that a ~ a for any event a. (Systems in
which an event can happen before itself do not seem to
be physically meaningful.) This implies that ~ is an
irreflexive partial ordering on the set of all events in the
system.
It is helpful to view this definition in terms of a
"space-time diagram" such as Figure 1. The horizontal
direction represents space, and the vertical direction
represents time--later times being higher than earlier
ones. The dots denote events, the vertical lines denote
processes, and the wavy lines denote messagesfl It is easy
to see that a ~ b means that one can go from a to b in

' The choice of what constitutes an event affects the ordering of
events in a process. For example, the receipt of a message might denote
the setting of an interrupt bit in a computer, or the execution of a
subprogram to handle that interrupt. Since interrupts need not be
handled in the order that they occur, this choice will affect the order-
ing of a process' message-receiving
events.
2 Observe that messages may be received out of order. We allow
the sending of several messages to be a single event, but for convenience
we will assume that the receipt of a single message does not coincide
with the sending or receipt of any other message.

559

Fig. 2.
cy c~

(9 (9 ~)
O O U

-2 - - - q6 -- ;#.i
Y _
P3' ~ ~ ~ ~ ~ _~~-~ r3

the diagram by moving forward in time along process
and message lines. For example, we have p, --~ r4 in
Figure 1.
Another way of viewing the definition is to say that
a --) b means that it is possible for event a to causally
affect event b. Two events are concurrent if neither can
causally affect the other. For example, events pa and q:~
of Figure 1 are concurrent. Even though we have drawn
the diagram to imply that q3 occurs at an earlier physical
time than 1)3, process P cannot know what process Q did
at qa until it receives the message at p, (Before event p4,
P could at most know what Q was planning to do at q:~.)
This definition will appear quite natural to the reader
familiar with the invariant space-time formulation of
special relativity, as described for example in [1] or the
first chapter of [2]. In relativity, the ordering of events is
defined in terms of messages that could be sent. However,
we have taken the more pragmatic approach of only
considering messages that actually are sent. We should
be able to determine if a system performed correctly by
knowing only those events which did occur, without
knowing which events could have occurred.

Logical Clocks

We now introduce clocks into the system. We begin
with an abstract point of view in which a clock is just a
way of assigning a number to an event, where the number
is thought of as the time at which the event occurred.
More precisely, we define a clock Ci for each process Pi
to be a function which assigns a number Ci(a) to any
event a in that process. The entire system ofclbcks is
represented by the function C which assigns to any event
b the number C(b), where C(b) = C/(b) ifb is an event
in process Pj. For now, we make no assumption about
the relation of the numbers Ci(a) to physical time, so we
can think of the clocks Ci as logical rather than physical
clocks. They may be implemented by counters with no
actual timing mechanism.

Communications July 1978
of Volume 21
the ACM Number 7

## Page 3

Fig. 3.
CY n¢

8 8 8
c~! ~ ~iLql ~ .r 4

We now consider what it means for such a system of
clocks to be correct. We cannot base our definition of
correctness on physical time, since that would require
introducing clocks which keep physical time. Our defi-
nition must be based on the order in which events occur.
The strongest reasonable condition is that if an event a
occurs before another event b, then a should happen at
an earlier time than b. We state this condition more
formally as follows.

Clock Condition. For any events a, b:
if a---> b then C(a) < C(b).

Note that we cannot expect the converse condition to
hold as well, since that would imply that any two con-
current events must occur at the same time. In Figure 1,
p2 and p.~ are both concurrent with q3, so this would
mean that they both must occur at the same time as q.~,
which would contradict the Clock Condition because p2
-----> /93.
It is easy to see from our definition of the relation
"---~" that the Clock Condition is satisfied if the following
two conditions hold.

C 1. If a and b are events in process P~, and a comes
before b, then Ci(a) < Ci(b).
C2. If a is the sending of a message by process Pi
and b is the receipt of that message by process Pi, then
Ci(a) < Ci(b).

Let us consider the clocks in terms of a space-time
diagram. We imagine that a process' clock "ticks"
through every number, with the ticks occurring between
the process' events. For example, if a and b are consec-
utive events in process Pi with Ci(a) = 4 and Ci(b) = 7,
then clock ticks 5, 6, and 7 occur between the two events.
We draw a dashed "tick line" through all the like-
numbered ticks of the different processes. The space-
time diagram of Figure 1 might then yield the picture in
Figure 2. Condition C 1 means that there must be a tick
line between any two events on a process line, and

560

condition C2 means that every message line must cross
a tick line. From the pictorial meaning of--->, it is easy to
see why these two conditions imply the Clock Con-
dition.
We can consider the tick lines to be the time coordi-
nate lines of some Cartesian coordinate system on space-
time. We can redraw Figure 2 to straighten these coor-
dinate lines, thus obtaining Figure 3. Figure 3 is a valid
alternate way of representing the same system of events
as Figure 2. Without introducing the concept of physical
time into the system (which requires introducing physical
clocks), there is no way to decide which of these pictures
is a better representation.
The reader may find it helpful to visualize a two-
dimensional spatial network of processes, which yields a
three-dimensional space-time diagram. Processes and
messages are still represented by lines, but tick lines
become two-dimensional surfaces.
Let us now assume that the processes are algorithms,
and the events represent certain actions during their
execution. We will show how to introduce clocks into the
processes which satisfy the Clock Condition. Process Pi's
clock is represented by a register Ci, so that C~(a) is the
value contained by C~ during the event a. The value of
C~ will change between events, so changing Ci does not
itself constitute an event.
To guarantee that the system of clocks satisfies the
Clock Condition, we will insure that it satisfies conditions
C 1 and C2. Condition C 1 is simple; the processes need
only obey the following implementation rule:

IR1. Each process P~ increments Ci between any
two successive events.

To meet condition C2, we require that each message
m contain a timestamp Tm which equals the time at which
the message was sent. Upon receiving a message time-
stamped Tin, a process must advance its clock to be later
than Tin. More precisely, we have the following rule.

IR2. (a) If event a is the sending of a message m
by process P~, then the message m contains a timestamp
Tm= Ci(a). (b) Upon receiving a message m, process
Pi sets Ci greater than or equal to its present value and
greater than Tin.

In IR2(b) we consider the event which represents the
receipt of the message m to occur after the setting of C i.
(This is just a notational nuisance, and is irrelevant in
any actual implementation.) Obviously, IR2 insures that
C2 is satisfied. Hence, the simple implementation rules
IR l and IR2 imply that the Clock Condition is satisfied,
so they guarantee a correct system of logical clocks.

Ordering the Events Totally

We can use a system of clocks satisfying the Clock
Condition to place a total ordering on the set of all
system events. We simply order the events by the times

Communications July 1978
of Volume 21
the ACM Number 7

## Page 4

at which they occur. To break ties, we use any arbitrary
total ordering < of the processes. More precisely, we
define a relation ~ as follows: if a is an event in process
Pi and b is an event in process Pj, then a ~ b if and only
if either (i) Ci{a) < Cj(b) or (ii) El(a) ----" Cj(b) and Pi
< Py. It is easy to see that this defines a total ordering,
and that the Clock Condition implies that if
a ----> b then a ~ b. In other words, the relation ~ is a
way of completing the "happened before" partial order-
ing to a total ordering, a
The ordering ~ depends upon the system of clocks
Cz, and is not unique. Different choices of clocks which
satisfy the Clock Condition yield different relations ~.
Given any total ordering relation ~ which extends --->,
there is a system of clocks satisfying the Clock Condition
which yields that relation. It is only the partial ordering
which is uniquely determined by the system of events.
Being able to totally order the events can be very
useful in implementing a distributed system. In fact, the
reason for implementing a correct system of logical
clocks is to obtain such a total ordering. We will illustrate
the use of this total ordering of events by solving the
following version of the mutual exclusion problem. Con-
sider a system composed of a fixed collection of processes
which share a single resource. Only one process can use
the resource at a time, so the processes must synchronize
themselves to avoid conflict. We wish to find an algo-
rithm for granting the resource to a process which satis-
fies the following three conditions: (I) A process which
has been granted the resource must release it before it
can be granted to another process. (II) Different requests
for the resource must be granted in the order in which
they are made. (III) If every process which is granted the
resource eventually releases it, then every request is
eventually granted.
We assume that the resource is initially granted to
exactly one process.
These are perfectly natural requirements. They pre-
cisely specify what it means for a solution to be correct/
Observe how the conditions involve the ordering of
events. Condition II says nothing about which of two
concurrently issued requests should be granted first.
It is important to realize that this is a nontrivial
problem. Using a central scheduling process which grants
requests in the order they are received will not work,
unless additional assumptions are made. To see this, let
P0 be the scheduling process. Suppose P1 sends a request
to Po and then sends a message to P2. Upon receiving the
latter message, Pe sends a request to Po. It is possible for
P2's request to reach P0 before Pl's request does. Condi-
tion II is then violated if P2's request is granted first.
To solve the problem, we implement a system of

;~ The ordering < establishes a priority among the processes. If a
"fairer" method is desired, then < can be made a function of the clock
value. For example, if Ci(a) = C/b) andj < L then we can let a ~ b
ifj < C~(a) mod N --< i, and b ~ a otherwise; where N is the total
number of processes.
4 The term "eventually" should be made precise, but that would
require too long a diversion from our main topic.
561

clocks with'rules IR 1 and IR2, and use them to define a
total ordering ~ of all events. This provides a total
ordering of all request and release operations. With this
ordering, finding a solution becomes a straightforward
exercise. It just involves making sure that each process
learns about all other processes' operations.
To simplify the problem, we make some assumptions.
They are not essential, but they are introduced to avoid
distracting implementation details. We assume first of all
that for any two processes P/and Pj, the messages sent
from Pi to Pi are received in the same order as they are
sent. Moreover, we assume that every message is even-
tually received. (These assumptions can be avoided by
introducing message numbers and message acknowledg-
ment protocols.) We also assume that a process can send
messages directly to every other process.
Each process maintains its own request queue which
is never seen by any other process. We assume that the
request queues initially contain the single message To:Po
requests resource, where Po is the process initially granted
the resource and To is less than the initial value of any
clock.
The algorithm is then defined by the following five
rules. For convenience, the actions defined by each rule
are assumed to form a single event.
1. To request the resource, process Pi sends the mes-
sage TIn:P/requests resource to every other process, and
puts that message on its request queue, where T,~ is the
timestamp of the message.
2. When process Pj receives the message T,~:P~ re-
quests resource, it places it on its request queue and sends
a (timestamped) acknowledgment message to P~.'~
3. To release the resource, process P~ removes any
Tm:Pi requests resource message from its request queue
and sends a (timestamped) Pi releases resource message
to every other process.
4. When process Pj receives a Pi releases resource
message, it removes any Tm:P~ requests resource message
from its request queue.
5. Process P/is granted the resource when the follow-
ing two conditions are satisfied: (i) There is a Tm:Pi
requests resource message in its request queue which is
ordered before any other request in its queue by the
relation ~. (To define the relation "~" for messages,
we identify a message with the event of sending it.) (ii)
P~ has received a message from every other process time-
stamped later than Tin. ~
Note that conditions (i) and (ii) of rule 5 are tested
locally by P~.
It is easy to verify that the algorithm defined by these
rules satisfies conditions I-III. First of all, observe that
condition (ii) of rule 5, together with the assumption that
messages are received in order, guarantees that P~ has
learned about all requests which preceded its current

'~ This acknowledgment message need not be sent if Pj has already
sent a message to Pi timestamped later than T ....
" If P, -< Pi, then Pi need only have received a message timestamped
_> T,,, from P/.

Communications July 1978
of Volume 21
the ACM Number 7

## Page 5

request. Since rules 3 and 4 are the only ones which
delete messages from the request queue, it is then easy to
see that condition I holds. Condition II follows from the
fact that the total ordering ~ extends the partial ordering
---~. Rule 2 guarantees that after Pi requests the resource,
condition (ii) of rule 5 will eventually hold. Rules 3 and
4 imply that if each process which is granted the resource
eventually releases it, then condition (i) of rule 5 will
eventually hold, thus proving condition III.
This is a distributed algorithm. Each process inde-
pendently follows these rules, and there is no central
synchronizing process or central storage. This approach
can be generalized to implement any desired synchroni-
zation for such a distributed multiprocess system. The
synchronization is specified in terms of a State Machine,
consisting of a set C of possible commands, a set S of
possible states, and a function e: C× S--~ S. The relation
e(C, S) -- S' means that executing the command C with
the machine in state S causes the machine state to change
to S'. In our example, the set C consists of all the
commands Pi requests resource and P~ releases resource,
and the state consists of a queue of waiting request
commands, where the request at the head of the queue
is the currently granted one. Executing a request com-
mand adds the request to the tail of the queue, and
executing a release command removes a command from
'he queue. 7
Each process independently simulates the execution
of the State Machine, using the commands issued by all
the processes. Synchronization is achieved because all
processes order the commands according to their time-
stamps (using the relation ~), so each process uses the
same sequence of commands. A process can execute a
command timestamped T when it has learned of all
commands issued by all other processes with timestamps
less than or equal to T. The precise algorithm is straight-
forward, and we will not bother to describe it.
This method allows one to implement any desired
form of multiprocess synchronization in a distributed
system. However, the resulting algorithm requires the
active participation of all the processes. A process must
know all the commands issued by other processes, so
that the failure of a single process will make it impossible
for any other process to execute State Machine com-
mands, thereby halting the system.
The problem of failure is a difficult one, and it is
beyond the scope of this paper to discuss it in any detail.
We will just observe that the entire concept of failure is
only meaningful in the context of physical time. Without
physical time, there is no way to distinguish a failed
process from one which is just pausing between events.
A user can tell that a system has "crashed" only because
he has been waiting too long for a response. A method
which works despite the failure of individual processes
or communication lines is described in [3].

7 If each process does not strictly alternate request and release
commands, then executing a release command could delete zero, one,
or more than one request from the queue.

Anomalous Behavior

Our resource scheduling algorithm ordered the re-
quests according to the total ordering =*. This permits
the following type of "anomalous behavior." Consider a
nationwide system of interconnected computers. Suppose
a person issues a request A on a computer A, and then
telephones a friend in another city to have him issue a
request B on a different computer B. It is quite possible
for request B to receive a lower timestamp and be ordered
before request A. This can happen because the system
has no way of knowing that A actually preceded B, since
that precedence informatiori is based on messages exter-
nal to the system.
Let us examine the source of the problem more
closely. Let O ° be the set of all system events. Let us
introduce a set of events which contains the events in b °
together with all other relevant external events, such as
the phone calls in our example. Let ~ denote the "hap-
pened before" relation for ~. In our example, we had A
B, but A-~ B. It is obvious that no algorithm based
entirely upon events in 0 °, and which does not relate
those events in any way with the other events in~, can
guarantee that request A is ordered before request B.
There are two possible ways to avoid such anomalous
behavior. The first way is to explicitly introduce into the
system the necessary information about the ordering
--~. In our example, the person issuing request A could
receive the timestamp TA of that request from the system.
When issuing request B, his friend could specify that B
be given a timestamp later than TA. This gives the user
the responsibility for avoiding anomalous behavior.
The second approach is to construct a system of
clocks which satisfies the following condition.

Strong Clock Condition. For any events a, b in O°:
ifa --~ b then C(a} < C(b).

This is stronger than the ordinary Clock Condition be-
cause ~ is a stronger relation than ---~. It is not in general
satisfied by our logical clocks.
Let us identify ~ with some set of "real" events in
physical space-time, and let ~ be the partial ordering of
events defined by special relativity. One of the mysteries
of the universe is that it is possible to construct a system
of physical clocks which, running quite independently of
one another, will satisfy the Strong Clock Condition. We
can therefore use physical clocks to eliminate anomalous
behavior. We now turn our attention to such clocks.

Physical Clocks

Let us introduce a physical time coordinate into our
space-time picture, and let Ci(t) denote the reading of
the clock Ci at physical time t. 8 For mathematical con-
We will assume a Newtonian space-time. If the relative motion
of the clocks or gravitational effects are not negligible, then CM) must
be deduced from the actual clock reading by transforming from proper
time to the arbitrarily chosen time coordinate.

562 Communications July 1978
of Volume 2 l
the ACM Number 7

## Page 6

venience, we assume that the clocks run continuously
rather than in discrete "ticks." (A discrete clock can be
thought of as a continuous one in which there is an error
of up to ½ "tick" in reading it.) More precisely, we
assume that Ci(t) is a continuous, differentiable function
of t except for isolated jump discontinuities where the
clock is reset. Then dCg(t)/dt represents the rate at which
the clock is running at time t.
In order for the clock Cg to be a true physical clock,
it must run at approximately the correct rate. That is, we
must have dCi(t)/dt -~ 1 for all t. More precisely, we will
assume that the following condition is satisfied:

PCI. There exists a constant x << 1
such that for all i: [dCg(t)/dt - 1 [ < x.

For typical crystal controlled clocks, x _< 10 -(~.
It is not enough for the clocks individually to run at
approximately the correct rate. They must be synchro-
nized so that Cg(t) = C/(t) for all i,j, and t. More precisely,
there must be a sufficiently small constant e so that the
following condition holds:

PC2. For all i, j: [ Ci(t) - Cy(t)[ < •.

If we consider vertical distance in Figure 2 to represent
physical time, then PC2 states that the variation in height
of a single tick line is less than E.
Since two different clocks will never run at exactly
the same rate, they will tend to drift further and further
apart. We must therefore devise an algorithm to insure
that PC2 always holds. First, however, let us examine
how small x and • must be to prevent anomalous behav-
ior. We must insure that the system 5e of relevant physical
events satisfies the Strong Clock Condition. We assume
that our clocks satisfy the ordinary Clock Condition, so
we need only require that the Strong Clock Condition
holds when a and b are events in 0 ° with a 4-> b.
Hence, we need only consider events occurring in differ-
ent processes.
Let # be a number such that if event a occurs at
physical time t and event b in another process satisfies
a ~ b, then b occurs later than physical time t + bt. In
other words,/~ is less than the shortest transmission time
for interprocess messages. We can always choose # equal
to the shortest distance between processes divided by the
speed of light. However, depending upon how messages
in ~ are transmitted, # could be significantly larger.
To avoid anomalous behavior, we must make sure
that for any i, j, and t: Ci(t + #) - CAt) > 0. Combining
this with PC I and 2 allows us to relate the required
smallness of x and ~ to the value of # as follows. We
assume that when a clock is reset, it is always set forward
and never back. (Setting it back could cause C I to be
violated.) PCI then implies that Cg(t + #) - Cg(t) > (1
- x)#. Using PC2, it is then easy to deduce that Cg(t +
\#) - C/(t) > 0 if the following inequality holds:
E/(I - ~) _< ~.
This inequality together with PC 1 and PC2 implies that
anomalous behavior is impossible.

563

We now describe our algorithm for insuring that PC2
holds. Let m be a message which is sent at physical time
t and received at time t'. We define l, m ~- t t -- I to be the
total delay of the message m. This delay will, of course,
not be known to the process which receives m. However,
we assume that the receiving process knows some mini-
mum delay tzm >_ 0 such that ~£m ~ Pro. We call ~,, = I,m
-- #m the unpredictable delay of the message.
We now specialize rules IRI and 2 for our physical
clocks as follows:
IR 1'. For each i, if Pi does not receive a message at
physical time t, then C/is differentiable at t and dCg(t)/dt
\>0.
IR2'. (a) If Pg sends a message m at physical time t,
then m contains a timestamp Tm= C/(t). (b) Upon
receiving a message m at time t', process P/ sets C/(t')
equal to maximum (Cj(t' - 0), Tm + /Zm). 9
Although the rules are formally specified in terms of
the physical time parameter, a process only needs to
know its own clock reading and the timestamps of mes-
sages it receives. For mathematical convenience, we are
assuming that each event occurs at a precise instant of
physical time, and different events in the same process
occur at different times. These rules are then specializa-
tions of rules IR1 and IR2, so our system of clocks
satisfies the Clock Condition. The fact that real events
have a finite duration causes no difficulty in implement-
ing the algorithm. The only real concern in the imple-
mentation is making sure that the discrete clock ticks are
frequent enough so C 1 is maintained.
We now show that this clock synchronizing algorithm
can be used to satisfy condition PC2. We assume that
the system of processes is described by a directed graph
in which an arc from process Pi to process P/represents
a communication line over which messages are sent
directly from Pi to P/. We say that a message is sent over
this arc every T seconds if for any t, Pi sends at least one
message to P/between physical times t and t + -r. The
diameter of the directed graph is the smallest number d
such that for any pair of distinct processes P/, Pk, there
is a path from P/to P~ having at most d arcs.
In addition to establishing PC2, the following theo-
rem bounds the length of time it can take the clocks to
become synchronized when the system is first started.
THEOREM. Assume a strongly connected graph of
processes with diameter d which always obeys rules IR 1'
and IR2'. Assume that for any message m, #m --< # for
some constant g, and that for all t > to: (a) PC 1 holds.
(b) There are constants ~" and ~ such that every ~- seconds
a message with an unpredictable delay less than ~ is sent
over every arc. Then PC2 is satisfied with • = d(2x~- +
~) for all t > to + Td, where the approximations assume
\# + ~<< z.
The proof of this theorem is surprisingly difficult,
and is given in the Appendix. There has been a great
deal of work done on the problem of synchronizing
physical clocks. We refer the reader to [4] for an intro-
:) C/(t' - 0) = lim C,(t' - 181).

Communications July 1978
of Volume 21
the ACM Number 7

## Page 7

duction to the subject. The methods described in the
literature are useful for estimating the message delays
ktm and for adjusting the clock frequencies dCi/dt (for
clocks which permit such an adjustment). However, the
requirement that clocks are never set backwards seems
to distinguish our situation from ones previously studied,
and we believe this theorem to be a new result.

Conclusion

We have seen that the concept of "happening before"
defines an invariant partial ordering of the events in a
distributed multiprocess system. We described an algo-
rithm for extending that partial ordering to a somewhat
arbitrary total ordering, and showed how this total or-
dering can be used to solve a simple synchronization
problem. A future paper will show how this approach
can be extended to solve any synchronization problem.
The total ordering defined by the algorithm is some-
what arbitrary. It can produce anomalous behavior if it
disagrees with the ordering perceived by the system's
users. This can be prevented by the use of properly
synchronized physical clocks. Our theorem showed how
closely the clocks can be synchronized.
In a distributed system, it is important to realize that
the order in which events occur is only a partial ordering.
We believe that this idea is useful in understanding any
multiprocess system. It should help one to understand
the basic problems of multiprocessing independently of
the mechanisms used to solve them.

Appendix

Proof of the Theorem
For any i and t, let us define C~ t to be a clock which
is set equal to C~ at time t and runs at the same rate as
Ci, but is never reset. In other words,

Cit(t ') = Ci(t) + [dCz(t)/dtldt (1)

for all t' >_ t. Note that

Ci(t') >_ Cit(t ') for all t' >__ t. (2)

Suppose process P~ at time tl sends a message to
process Pz which is received at time t2 with an unpre-
dictable delay _< ~, where to <- ta _< t2. Then for all t ___ t2
we have:

C~(t) >_ C~(t2) + (1 - x)(t - t2) [by (1) and PCI]
\> Cfftl) +/~m + (1 -- x)(t -- t2) [by IR2' (b)]
= Cl(tl) + (1 - x)(t - tl) - [(t2 - tO - ~m] + x(t2 - t,)
\>-- Cl(tl) + (1 - x)(t - tl) - 4.

Hence, with these assumptions, for all t >_ t2 we have:
C~(t) _> Cl(tl) + (1 - x)(t -/1)
-- 4" (3)
NOW suppose that for i = 1, ..., n we have t, _< t ~, <

ti+l, to <-- t~, and that at time t[ process Pi sends a message
to process Pi+l which is received at time ti+l with an
unpredictable delay less than 4. Then repeated applica-
tion of the inequality (3) yields the following result for
t >_ tn+l.
Ct~t(t) --> Cl(tl') + (1 - ~)(t - tl') - n~. (4)

From PC1, IRI' and 2' we deduce that

Cl(/l') >" Cl(tl) + (1 -- K)(tl' -- /1).

Combining this with (4) and using (2), we get

Cn+a(t) > C~(tl) + (1 - x)(t - t~) - n~ (5)

for t > tn+l.
For any two processes P and P', we can find a
sequence of processes P -- Po, P~ ..... Pn+~ = P', n _< d,
with communication arcs from each Pi to Pi+~. By hy-
pothesis (b) we can find times ti, t[ with t[ - ti <- T and
ti+l - t" <_ v, where v = # + 4. Hence, an inequality of
the form (5) holds with n <_ d whenever t >_ t~ + d('r +
v). For any i, j and any t, tl with tl > to and t ___ t~ + d(z
+ v) we therefore have:

Ci(t) _> Cj(ta) + (1 - x)(t - tx) - d~. (6)

Now let m be any message timestamped Tin, and
suppose it is sent at time t and received at time t'. We
pretend that m has a clock Cm which runs at a constant
rate such that C,~(t) = tm and Cm(t') = tm +/~m. Then
\#0, ___ t' - t implies that dCm/dt _< 1. Rule IR2' (b) simply
sets Cj(t') to maximum (Cj(t' - 0), Cm(t')). Hence, clocks
are reset only by setting them equal to other clocks.
For any time tx >-- to +/~/(1 - ~), let Cx be the clock
having the largest value at time t~. Since all clocks run
at a rate less than 1 + x, we have for all i and all t >_ tx:

Ci(t) _< Cx(tx) + (1 + x)(t - tx). (7)

We now consider the following two cases: (i) Cx is the
clock Cq of process Pq. (ii) Cx is the clock Cm of a
message sent at time ta by process Pq. In case (i), (7)
simply becomes
El(t) -< Cq(tx) + (1 + x)(t - tx). (8i)

In case (ii), since Cm(tx) = Cq(tl) and dCm/dt _ 1, we
have

Cx(tx) <_ Cq(tl) + (tx - tO.

Hence, (7) yields

Ci(t) <-~ Cq(/1) + (1 + K)(t -- /1).

Since tx

Cq(tx --

(8ii)

\>-- to + ~t/(1 -- X), we get

~/(1 -- K)) <_ Cq(tx) - ~ [by PCI]
___ Cm(tx) -/z [by choice of m]
-< Cm(tx) - (tx - tl)#m/Vm [tXm <-- r.t, tx - tl <_ v,,]
=Tm [by definition of Cm]
= Cq(tl) [by IR2'(a)].

Hence, Cq(tx -/L/(1 - x)) ___ Cq(tl), so tx - tl <-/x/(l -
~) and thus ll ~ to.

564 Communications July 1978
of Volume 21
the ACM Number 7

## Page 8

Letting tl = tx in case (i)~ we can combine (8i) and
(8ii) to deduce that for any t, tx with t _> tx _> to + #/
(1 - x) there is a process Pq and a time tl with tx - #/
(1 - i¢) ~ tl "< tx such that for all i:

Ci(t) _< Cq(t~) + (1 + x)(t - h). (9)

Choosing t and tx with t _ tx + d(r + ~,), we can combine
(6) and (9) to conclude that there exists a tl and a process
Pq such that for all i:

Cq(t 0 + (1 - x)(t - tl) - d~ -< Ci(t) (10)
<: Cq(tl) + (1 + tc)(t - t])

Letting t = t~ + d(r + u), we get

d(r+p)__.t-tl_<d(r+v)+#/(1-x).

Combining this with (10), we get

Cq(tl) + (t - tl) - xd(r + p) - d~ -< Ci(t) _< Cq(tl) (11)
+ (t - tl) + x[d(r + u) +/z/(1 - x)]

Using the hypotheses that x << 1 and/t _< ~, << r, we can
rewrite (11) as the following approximate inequality.

Cq(tl) + (t - tl) - d(tg'r + ~) ~.< Ci(t) (12)

~' Cq(/1) + (l -- tl) -I- dKr.

Since this holds for all i, we get

[Ci(t) - CAt)[ ~< d(2xr + O,

and this holds for all t > to + dr. []
Note that relation (11) of the proof yields an exact
upper bound for [Ci(t) - Cj(t) l in case the assumption
/~ + ~ << r is invalid. An examination of the proof
suggests a simple method for rapidly initializing the
clocks, or resynchronizing them if they should go out of
synchrony for any reason. Each process sends a message
which is relayed to every other process. The procedure
can be initiated by any process, and requires less than
2d~ + ~) seconds to effect the synchronization, assuming
each of the messages has an unpredictable delay less
than ~.

Programming J.J. Homing
Languages Editor

Shallow Binding in

Lisp 1.5

Henry G. Baker, Jr.
Massachusetts Institute of Technology

Shallow binding is a scheme which allows the value
of a variable to be accessed in a bounded amount of
computation. An elegant model for shallow binding in
Lisp 1.5 is presented in which context-switching is an
environment tree transformation called rerooting.
Rerooting is completely general and reversible, and is
optional in the sense that a Lisp 1.5 interpreter will
operate correctly whether or not rerooting is in-
voked on every context change. Since rerooting leaves
assoc [ v, a] invariant, for all variables v and all
environments a, the programmer can have access to a
rerooting primitive, shallow[], which gives him dynamic
control over whether accesses are shallow or deep, and
which affects only the speed of execution of a program,
not its semantics. In addition, multiple processes can be
active in the same environment structure, so long as
rerooting is an indivisible operation. Finally, the
concept of rerooting is shown to combine the concept of
shallow binding in Lisp with Dijkstra's display for Algol
and hence is a general model for shallow binding.
Key Words and Phrases: Lisp 1.5, environment
trees, FUNARG'S, shallow binding, deep binding,
multiprogramming, Algol display
CR Categories: 4.13, 4.22, 4.32

Acknowledgment. The use of timestamps to order
operations, and the concept of anomalous behavior are
due to Paul Johnson and Robert Thomas.

Received March 1976; revised October 1977

References
1. Schwartz, J.T. Relativity in lllustrations. New York U. Press, New
York, 1962.
2. Taylor, E.F., and Wheeler, J.A. Space-Time Physics, W.H.
Freeman, San Francisco, 1966.
3. Lamport, L. The implementation of reliable distributed
multiprocess systems. To appear in Computer Networks.
4. Ellingson, C, and Kulpinski, R.J. Dissemination of system-time.
1EEE Trans. Comm. Com-23, 5 (May 1973), 605-624.

565

General permission to make fair use in teaching or research of all
or part of this material is granted to individual readers and to nonprofit
libraries acting for them provided that ACM's copyright notice is given
and that reference is made to the publication, to its date of issue, and
to the fact that reprinting privileges were granted by permission of the
Association for Computing Machinery. To otherwise reprint a figure,
table, other substantial excerpt, or the entire work requires specific
permission as does republication, or systematic or multiple reproduc-
tion.
This research was supported by the Advanced Research Projects
Agency of the Department of Defense and was monitored by the Office
of Naval Research under contract number N00014-75-C-0522.
Author's present address: Computer Science Department, Univer-
sity of Rochester, Rochester, NY 14627.
© 1978 ACM 0001-0782/78/0700-0565 $00.75

Communications July 1978
of Volume 21
the ACM Number 7


<!-- sllmwiki:original-source -->
## Original source

- URL: <https://lamport.azurewebsites.net/pubs/time-clocks.pdf>
- License: proprietary
