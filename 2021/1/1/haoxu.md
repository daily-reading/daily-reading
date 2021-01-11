# How Google Spanner Assigns Commit Timestamps - The Secret Sauce of Its Strong Consistency

* Author: Eileen Pangu
* Link: https://levelup.gitconnected.com/how-google-spanner-assigns-commit-timestamps-the-secret-sauce-of-its-strong-consistency-8bc143614f26

WHY
They bring order to the chaotic distributed systems which can benefit too many other operations.
Instead of using local machines' clock, SPANNER uses an API for accurate timestamp.

WHAT
Definition:
*commit timestamp*
commit timestamp is a timestamp assigned to a transaction after it has acquired all the locks and before it releases any lock.

*The TrueTime API*
an interval [earliest, latest]

HOW
paxos:
lol.
"In fact, interestingly, research has shown that learning Paxos prematurely actually hurt your chance of comprehending distributed consensus"
2 notes
1. wait until the next tt.now().earliest > C_i, this guarantees C_(I+1) > C_I
2. yes, this is causing an intentional delay, but Spanner will do other things while waiting, so the delay can be ignored.

in partitions mode, using the overall C from all participants. 
Notes: participants are communicating though channels.

Q: I am actually interested in how TT.now() returns the interval, how earliest and latest are calculated (through different geo locations?)