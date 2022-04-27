---
id: use-cases
title: What can i do with Semaphore?
sidebar_position: 1
---

## Zero knowledge primitives

In a nutshell, set membership is the problem of a prover P proving to a verifier V (or a set of verifiers)
that an element x is in a (usually public) set S. The prover computes a proof π and sends it to the verifier,
who should be able to efficiently check its validity (https://eprint.iacr.org/2022/155.pdf, p.7).


(from the Semaphore Overview)

- allows Ethereum users to prove their membership of a set which they had previously joined without revealing their original identity. 
- allows users to signal their endorsement of an arbitrary string.


## Onchain privacy use cases

- private voting
- whistleblowing
- mixers
- anonymous authentication

## Safety, security features

- prevents double-signalling
- prevents double-spending


## Semaphore features

Semaphore provides the following features:

1. Generate offchain identities and add them to a Merkle tree (offchain or onchain).
2. Anonymously broadcast a signal onchain, if and only if the identity of the owner belongs to a
   valid Merkle tree and if the nullifier has not already been used.

### Generate offchain identitiies



### Broadcast an onchain signal

## Use cases

### Private voting

(from https://kohweijie.com/articles/19/voting-whistleblowing.html)


|   Attendee    |   Member  |   Voter   |
|   receives membership (e.g. POAP, social NFT) | uses an identity (that remains secret) to register with the voting app, prove membership, and generate an identity commitment |   generates a proof of registration and broadcasts a signal in response to the question (topic)


So, roles should be:
- Voters:  they will join a Semaphore on-chain group only if they have specific POAPs, and they will send their zk-proofs to a relayer
- Relayer: they will sign and send voters' zk-proof transactions to save anonymous votes on-chain
- Any eth account: they will create a question that voters can answer anonymously


### What is a relayer?

A relayer is a third-party who receives a fee for including relayed transactions in the blockchain. (McMenamin, Daza, and Fitz. https://eprint.iacr.org/2022/155.pdf, p.3)

Ethereum transactions are tied to a specific address, and transactions sent from
this address consume Ether to pay for gas. When a signal is broadcast, the
Ethereum address used for the transaction might be used to break the anonymity
of the broadcaster. Semaphore allows signals to be received from any address,
allowing a relayer to broadcast a signal on behalf of a user. Applications might
provide rewards for relayers and implement front-running prevention mechanisms, such as requiring the signals to include the relayer’s address, binding the
signal to that specific address.(https://semaphore.appliedzkp.org/whitepaper-v1.pdf, p.6)



OneOfUs is an anonymous voting platform which allows verified event attendees to anonymously vote on topics without revealing their real identities. With the generous assistance of the Proof of Attendance Protocol (POAP) team, we built and integrated this app with POAP ERC721 tokens in time for Devcon 5, where we used it during a workshop to educate attendees about Semaphore and zero-knowledge proofs.

The basic idea behind this app is that anyone who owns a POAP token associated with Devcon 5 can register an identity in a Semaphore smart contract. Anyone can post questions, such as “Should Devcon 6 be held in Buenos Aires?”, and the hash of the question acts as an external nullifier in Semaphore. Each user may then broadcast a signal (for instance, “Yes” or “No”) towards said external nullifier by producing a zero-knowledge proof that they are part of the set of registered attendees, without revealing their original Ethereum address or token ID.

We initially built a version of this app in a decentralised fashion, where people who posted questions would have to pay some Ether to the contract, and anyone who broadcasts a signal would receive a small fee. This would reward transaction relayers so that actual users would not have to pay gas to post their answers and thereby lose anonymity.

Although we had completed the smart contract code for this version, we decided to pivot to a centralised version in order to save time and reduce complexity. It was a client-server app which mimicked the smart contract logic and stored data entirely off-chain. This allowed us to conduct a smooth workshop where some attendees were able to register themselves and vote on questions.

### Private whistleblowing

More recently, Lai Ying Tong, Brendan Graetz, Barnabé Monnot, myself, and others built a simple whistleblowing application using Semaphore as an entry to the Yale Open Climate Collaboration. One of the goals of the hackerthon was to explore ways that smart contract technology can lead to better accounting and reporting of climate emissions. Our work built on the concept of using economic incentives to encourage executives of carbon-emitting corporations to blow the whistle if their company submits fraudulent data. A trusted referee would investigate such reports, and if they were to find evidence of incorrect reports, they would confiscate a deposit which the company locked up using a smart contract.

Semaphore allows the whistleblower to remain anonymous and prevent retaliation. The way it works is as such: each executive registers their identity into a smart contract. Every day, the company submits climate data, deposits some funds, and adds a unique identifier of the dataset as an external nullifier in the contract. When a whistleblower submits a report, they broadcast a signal to the external nullifier associated with said dataset. This freezes the company’s deposits, pending the referee’s intervention. The whistleblower also receives a reward.
