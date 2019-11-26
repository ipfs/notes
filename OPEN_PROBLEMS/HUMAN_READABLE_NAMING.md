# Human Readable Naming

## Short Description
> In one sentence or paragraph.

## Long Description

Memorizing Content Addresses is hard, No one should have to do that. We need a solution that can provide Human Readable Names to users without making them to rely on a central provider, an unauthenticated host, on unauthenticated data or without leaking their intent to access some piece of content that is referenced by that name.

## State of the Art

> This survey on the State of the Art is not by any means complete, however, it should provide a good entry point to learn what are the existing work. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

### Within the IPFS Ecosystem
> Existing attempts and strategies

##### DNSlink

DNSlink combines IPFS and DNS to use a user familiar addressing scheme (domains) to point to CIDs indirectly. An IPFS node will receive a request coming from domain-name.com, resolve it to see if it can find a TXT record with a CID and if found, resolve that for the user. This solution is convient but it does rely on DNS itself to work.

- https://docs.ipfs.io/guides/concepts/dnslink

##### ENS with IPFS

- Talk: "ENS + IPFS: Using ENS as a naming system for IPFS", Makoto Inoue https://github.com/ipfs/camp/blob/master/LIGHTNING_TALKS/ipfscamp2019-lightningtalk-ensipfs.pdf
- https://youtu.be/3AS2BD22DZg

##### mnemonic base

Multibase now has support for a [mnemonic base](https://github.com/multiformats/js-multibase/compare/36d60d3f9379ea005327fe3a375f03ba286d0ecc...tableflip:feat/mnemonic-base) and given that CIDs are encoded with multibase so that they can be represented as strings, you can get your own mnemonic base encoded content address. This makes a name pronunceable, but given the amount of information in a CID, it is a long mnemonic.

### Within the broad Research Ecosystem
> How do people try to solve this problem?

- [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) - One of the first widely deployed solutions for Human readable names. 
- 

### Known shortcommins of existing solutions
> What are the limitations on those solutions?

The existing solutions do not offer all the guarantees we want to achieve for Web 3.0 apps because:
-

## Solving this Open Problem

### What is the impact

Make the Content Addressing schemes more approachable by everyone.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

The constraints are:
- 

## Other

### Existing Conversations/Threads

- [Human Friendly Names](https://github.com/ipfs/notes/issues/286)
- [JRFC 28 - note on DNS and the Web](https://github.com/jbenet/random-ideas/issues/28)

### Extra notes

- [https://decentralized.blog/ten-terrible-attempts-to-make-ipfs-human-friendly.html](https://decentralized.blog/ten-terrible-attempts-to-make-ipfs-human-friendly.html)
