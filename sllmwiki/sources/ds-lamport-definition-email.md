Received: by jumbo.dec.com (5.54.3/4.7.34)
	id AA09105; Thu, 28 May 87 12:23:29 PDT
Date: Thu, 28 May 87 12:23:29 PDT
From: lamport (Leslie Lamport)
Message-Id: <8705281923.AA09105@jumbo.dec.com>
To: src-t
Subject: distribution


There has been considerable debate over the years about what
constitutes a distributed system.  It would appear that the following
definition has been adopted at SRC:

  A distributed system is one in which the failure of a computer
  you didn't even know existed can render your own computer
  unusable.

The current electrical problem in the machine room is not the
culprit--it just highlights a situation that has been getting
progressively worse.  It seems that each new version of the nub makes
my FF more dependent upon programs that run elsewhere.

Having to wait a few seconds for a program to be swapped in is a lot
less annoying than having to wait an hour or two for someone to reboot
the servers.  I therefore propose a development project to make our
system more robust.  I am not proposing any particular approach
(enabling stand-alone operation is just one possibility).

I will begin the effort by volunteering to gather some data on the
problem.  If you know of any instance of user's FF becoming inoperative
through no fault of its own, please send me a message indicating the
user, the time, and the cause (if known).

Leslie



<!-- sllmwiki:original-source -->
## Original source

- URL: <https://lamport.azurewebsites.net/pubs/distributed-system.txt>
- License: unknown
