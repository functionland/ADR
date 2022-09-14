# Short Title of the Architectural Decision

[based on https://schubmat.github.io/DecisionCapture/templates/captureTemplate_simple.html]: #

**Context**: [Issue/Document](https://github.com/filecoin-project/gitops-root/issues/)

Decision document for implementation of the messaging system for events

## Options Considered

### Using Gossipsub

### Other mechanisms like telling every other node about the update


## Conclusion

Chosen Option: Gossipsub


# ADR Description
- We need it to be scalable and telling each node is not scalable
- We need it to be tested and reliable, and Gossipub provides it
- We better reuse already existing modues for the sake of speed of implementation
* Status: accepted/denied
* Deciders: Mehdi
* Date: 

Technical Story: Once we have a list of events, we need a way to announce new events to peers
* We have a DAG tree (CAR file) that we need replicated to all nodes connected together using Libp2p (or in an IPFS-Cluster)

## Context and Problem Statement
What is the challenge?
Announcing the new event to all other peers

Why is it a challenge?
Since we need to reliably let other peers receive the new event and decide if it is a legit event or not.


## What options are we considering 
* We can use a pubsub mechanism like Gossipsub that is suported by Libp2p
* We can create our own protocol from the ground up
 
## Decision Outcome
We chose to use Gossipsub as it provides us with all features we need.

## Pros and Cons of each Options

### Option title

Gosssipsub:
- Good, because it is reliable and fast
- Bad, because not all messages might be received by all peers in each transfer and if it drops below a threshold, consensus might not be reachable

Our own protocol:
- Good, because we have total control over its features
- Bad, because considering our current team size, we might not be abe to implement anything better that Gossipsub

