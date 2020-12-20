# Paxos Notes

## 1.Problem
A number of processes can propose value, but only one value can be valid. therefore there are several natural safety requirements (basically making sure it is well-defined and unique)

We can abstract the agents to be three types: proposers, acceptors and learners. To run in a truely distributed sense, we have to assume the following:
1. Agents operate at different speed and may fail or restart, therefore we need to store some information from an agent that has failed and restarted.
2. messages can take arbitrarily long to be delivered/duplicated/lost, but can not be corrupted.

## 2. Choosing a value
Simplest solution is to choose the first value seen, but it would rely on the acceptor not failing, otherwise, we can not go any further beyond this failure. For the failure tolerance, we need multiple acceptors and generate acceptance consensus after a number of acceptors accept the same value. In this case, we need a majority of acceptors to agree.

If acceptor only accept the first value it sees, it would also create problem. (When half of the acceptors accept one value and the other half accept another). Therefore, acceptor needs to accept mulitple values (with index.) We therefore only generate consensus, when an index with associated value is accepted by the majority of the acceptors. 

(Because everything is done in a non-blocking and async sense, we have to allow for multiple proposal to be chosen.) However, we need to make sure all chosen proposal to have the same value. Obviously this can only be guaranteed if we put certain restriction on the proposer. In this case, we need proposer to only inssue higher-numbered/indexed proposal with value v if a proposal with value v is chosen. How can this be done though? 

This can be done by using the following algorithm on the proposer:
* choose a new proposal number/index n, and sends a request to a subset of the accpetors asking it to never again accept a proposal number less than n, and the proposal with the highest number less than n that it has accepted if any. (prepare request step)
* receives response from a majority of the acceptors and issue a propsoal with number n and value v, where v is the highest-numbered proposal among response (or any value if no accepted proposal reported.) Then send to another subset of accpetors (accepte request step.)

The accept then would only need to keep it promise if and only if it has promised to a prepare request.
( There are some weird asymmetic topology that makes this algorithm fail but in probabilitic sense this algorithm would guarantee convergence of consensus.)

The acceptor would only need to keep track of highest number of accepted proposal and highest number of prepare request (remember these even after restart.)

(If there are only two proposer, A and B, both has the same index strategy and proposes different value at first instance. on top of that if half of the acceptors always get propsal from A first, the other half B. If A and B always prefer the value it proposes in the last accept request when index collides. Is it possible that this algorithm never converge?? This is a very bad implementation though.)

## 3. Learning a chosen value
acceptor talks to one specific or a set of learners. The learner then informs other learners.

## 4. Progess
To ensure progress, one distinguished prosper must be selected as the only one to try issuing proposals. This issuer needs to be chosen randomly. (This partly solves my question at the end of section 2.)

## 5. implementation
We need to also make sure no two prososal are ever issued with the same number (see my question at the end of section 2.) We could assigned disjoint set of index/numbers to different proposers. 

## 