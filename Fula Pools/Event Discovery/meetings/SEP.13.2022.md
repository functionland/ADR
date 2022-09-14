# Meeting notes

Meeting notes for the meeting on September.13.2022

## Attendees

### Masih
### Mehdi
### Ehsan

## Discussed topics

- How to add to Events List
- Design system for pools

# Meeting notes

The Events list creation is done and seems like a good initial version to start with. Now we need to add events to teh list of events and announce it to others.

## Adding a new event

- We create a gossipsub with the topic(pool-(x)-eventupdate)
- All peers in a cluster subscribe to the topic
- Everytime a node adds a head to the list of events the CID is broadcasted. 
- Then the subscribers get the content if it is new to them, check againt the signature, from the head up to the last place they have in their own list to ensure it matches, and if so add it to their list of events and create CAR file

We use CAR file just as the last snapshot of the list of events and use the IPFS-Cluster to replicate it across all nodes. 

Also Gossipsub just announces the CID of the new head and not the content.  


### Keywords: IPFS Cluster, Gossipsub, provider, announcer



# Action Items

## Fill out the ADR for choosing IPFS-Cluster

### Assignee: Mehdi


## Run IPFS-Cluster with Fula

### Assignee: Mehdi

Considerations:

- For now we createa secret-less cluster and replicate the list of events. Sine even if we use a secret, someone can share it with others once he gets it.
- We need to see if replication can happen based on CID or CAR file.


## Create the Gossipsub

### Assignee: Masih