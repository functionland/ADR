# Authorization and Privacy in the Fula Drive

[based on https://schubmat.github.io/DecisionCapture/templates/captureTemplate_simple.html]: #

**Context**: 
- [Issue/Document](https://github.com/filecoin-project/gitops-root/issues/)

This document demonstrates the desicion process for choosing a DAG structure that allows us to store encrypted data structures in a Fula Drive. 
The DAG structure MUST allow for privacy (enrcyption), shared read access (decryptable by multiple users). The DAG structure SHOULD allow for directory structure encryption (directory level read access), shared write access.

## Options Considered

### Using a sidetree for storing cryptographic keys and structures


### Implementing Cryptree for Fula Drive


## Conclusion

Chosen Option: **TBD**


# ADR Description

* Status: In Progress
* Deciders: Mehdi, Jamshid
* Date: 22 Sep 2022

Technical Story: 
* We are storing a user's files in a DAG created using `go-unixfs`. We want users being able to store encrypted (private) data inside their drives. Also, the users should be able to share their files and directories with each other.
* The first implementation adds a new file type to `go-unixfs` called `EncFile` which has an additional field `jwe`. The file gets encrypted with a randomly generated symmetric key using the AES algorithm on the client side. The symmetric key is then encrypted with the user's public key creating a JWE object. The generated JWE is then stored on the drive inside the `EncFile`.

## Context and Problem Statement
What is the challenge?

- **We are not using deduplication despite using content addressing**. In current implementation, if a user wants to share an already uploaded encrypted file with a friend, she has to store a new `EncFile` which has a different JWE and in result a different CID. So we are changing the CID of the file every time the access list changes. This will lead to big problems in future when dealing with data sharding.

- **The directory structure is not encrypted**

- **We have no write access control**

Why is it a challenge?
<Description>


## What options are we considering 
* Create a side-tree and store JWE objects in a tree structure.
* Implement the CrypTree
 
## Decision Outcome
<Describe the decision that was choosen>

## Pros and Cons of each Option

### Side-tree
Instead of storing a JWE object along side with the encrypted file inside the Fula Drive DAG, We will store the JWE objects in a side-tree. This side-tree will have these characteristics:

- It wil have a single root node. This root node will be added to the Fula Drive
- The tree represents a mapping between a file's path and its corresponding JWE object(s)
- Every encrypted file in the Fula Drive's private space, will have a unique entry inside this map

### WNFS
WNFS uses same consept of CryptoTree. 
- Build on top of UnixFS. 
- Encrypting every folder/directory and files.
- Unblock node/parent and see all belongs/childs
- WNFS is E2EE. It has WebCrypto API. 
- WNFS dose not handling any access management process but UCAN is additional layer for access management. UCAN has access revoke API with signatures.
- Puting all together we can not revoke read access. Once audiance has symetric key we can not revoke read access. But we can change file version and change key rotation. Which means, we have to re-encrypt again.
- All documentation related to WNFS showing that symmetric keys are stored in node/folder. But it worries me a bit.

What does WNFS mainly have?
- structured and unstructured directory solution
- Encryption

Conclusion:
We alredy have a solution for encryption.
- Basically we need to solve the directory structure problem as mentioned above.
- When files are updated, we should update them and be able to pin them. If Audiences have public access to some folder, I would suggest adding pub-sub. In oreder to notify they have got some updates.
- We have E2EE encryption, but we can also add hard encryption. When data stored in full-blox can use the 2nd layer encryption method.