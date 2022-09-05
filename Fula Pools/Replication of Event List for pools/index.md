# Short Title of the Architectural Decision

[based on https://schubmat.github.io/DecisionCapture/templates/captureTemplate_simple.html]: #

**Context**: [Issue/Document](https://github.com/filecoin-project/gitops-root/issues/)

Decision document for implementation of the replication mechanism for the Event List of the pool

## Options Considered

### Modifying IPFS Cluster

### Writting from scratch using Bitswap


## Conclusion

Chosen Option: **TBD**


# ADR Description

* Status: accepted/denied
* Deciders: Mehdi
* Date: 

Technical Story: 
* We have a DAG tree (CAR file) that we need replicated to all nodes connected together using Libp2p (or in an IPFS-Cluster)

## Context and Problem Statement
What is the challenge?
<Description>

Why is it a challenge?
<Description>


## What options are we considering 
* We can use IPFS-Cluster off the shelf and just use the features we need from it
* We can create our own protocol basedon Bitswap from the ground up
 
## Decision Outcome
<Describe the decision that was choosen>

## Pros and Cons of each Options

### Option title
<Describe in depth each options>

<List pros and cons>
- Good, because...
- Bad, because...