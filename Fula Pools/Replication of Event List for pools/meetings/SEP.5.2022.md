# Meeting notes

Meeting notes for the meeting on September.9.2022

## Attendees

### Masih
### Mehdi
### Ehsan

## Discussed topics

- Bussiness requirements for pools
- Design system for pools

# Meeting notes

We break down the creation of pools into 3 sub-projects:
## Create a list of events

The list should be: 
- ordered (serially applied)
- immutable
- contain enough information

From the above requirements, IPLD seems the onvious choice. DAGs provide the ordered and immutablity and we can put as much information as we need in each node.
We use IPLD schema and APIs to create instances of each IPLD link system. To ensure the parent nodes does not change when we add more information, instead of having link to next node, we link each node to previous one.
We do not need to know what is exactly inside the event list at the begining. We start and as requirements come up we mature it.
If we put a version in each event, then as new versionscome up, we know if the event were created based on an older version or not and can act accordingly for backward compatability.

## Replicate List of events (ignore the content)
we can use something like go-ipld-prime and extract IPLD info and package into CAR files. Since IPFS-Cluster natively understand s the CAR files, it seems a good choice for replication.
Bascially, we just use IPFS-Cluster (off-the-shelf) to share the DAGs. The goal is replication of state so that every node has access to the state.
We need to decide whether to use IPFS-Cluster or create a Bitswap server for exmaple using Bitswap from scratch.

If someone changes the state that is not allowed, every other node has access to the state to see if it is a legit change or not. Events are accumulatively replicated and we apply information when majority approves. This lead us to use RAFT mechanism. 
We can either build on top of RAFT directly ourselves, or use Etcd which provides Strong sonsistent key-value store and uses RAFT.

## Recosnnstruct the state (Fula APIs)
the goal here is provide a set of protocols that each node can understand what is the expected state at each point, by reading the list of events and can rebuild the state.
Basically, each node can do the right thing based on events. We have two ways to implement:
1- Whenever an event happens we apply it to the state: this can be fast but create some security problems.
2- There is no already-created state and each node makes decisions on the fly. Here we need to read the whole event list and it can take a long time but there are cases where we might not have speed issues. Here the processor is stateless.
<<We do not need it stateful. Conseptually recosntruct.>>

# Action Items

## Check if it is easier to do step 2 using IPFS Cluster or build from scratch

### Assignee: Mehdi

Considerations:
1- We can build from scratch even at a alter stage
2- For now we want to see the functioning system and test

## Create a list of events

###Assignee: Masih