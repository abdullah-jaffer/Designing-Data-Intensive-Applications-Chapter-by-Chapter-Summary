# Chapter 8- The Trouble with Distributed Systems

## Consensus
One of the most important abstractions for distributed systems. It involves getting all the nodes to agree on something.
For example, in a leader based replication scheme if the leader dies, the remaining nodes can use consensus to determine the next in line leader.

## Consistency guarantees
In replication lag, eventual consistency can also be called convergence as eventually values in all nodes will converge. This is a weak guarantee.

## Linearizability
In an eventual consistent system if you look at two replicas at the same time they might have different values.
Linearizability gives us the illusion that we have one consistent snapshot of the data even for multiple users.
In a linearizable system, as soon as one write is made it is visible to all users, maintaining the illusion that we have consistent data.

In short it is a guarantee of recency. 

## Example of non leniarizable system
A score update for a football Mach is made to the leader, it is shown to 1 follower, but it is delayed in the second follower, giving the impression that the match is still on even though one time might have won.

## More examples
If multiple clients are reading and writing x, there might a time where the value of x from 0 to 1, some nodes might be shown 1 and theirs 0. until the 1 write is finalized. This is also not linear. in the case of linearibility, we can use a compare and set algo to see if any read in a node has old value return the latest value update in another node. Until the new value is set in all nodes.

## Things that need strong linearazibility
- Locking and leader election
- Constraints and uniqueness guarantees
- Cross-channel timing dependencies

## Most common approach to implement linearizability
- Single-leader replication (potentially linearizable)
- Consensus algorithms (linearizable)
- Multi-leader replication (not linearizable)
- Leaderless replication (probably not linearizable)


