# Human Readable Naming

## Short Description
> In one sentence or paragraph.

You can only have two of three properties for a name: Human-meaningful, Secure and/or Decentralized. This is [Zooko's Trilemma](https://en.wikipedia.org/wiki/Zooko%27s_triangle). Can we have all 3? Does context matter to solve this problem?

## Long Description

Memorizing Content Addresses is hard, No one should have to do that. We need a solution that can provide Human Readable Names to users without making them to rely on either/or a central provider, an unauthenticated host, on unauthenticated data and without leaking their intent to access some piece of content that is referenced by that name.

[Zooko's Trilemma](https://en.wikipedia.org/wiki/Zooko%27s_triangle) suggests that it is impossible to get a name that is Human-meaningful, Secure and Decentralized. However, what is resides as a question is the possibility to take into account the context for the resolution of the name (locality: Decentralized, but local / namespaced). We believe there is an avenue here to explore.

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

##### textile.photos short URLs

TODO

### Within the broad Research Ecosystem
> How do people try to solve this problem?

##### [DNS](https://en.wikipedia.org/wiki/Domain_Name_System)

One of the first widely deployed solutions for Human readable names.

##### ICN/NDN

In ICN/NDN names are used to do routing in the network (i.e., involving network routers). The NDN (formerly CCNx) project started with hierarchical, human-readable name structure, but it later became apparent that having only human-readable names is not quite an option.

It's impossible to guarantee that no two users will give the same name to some content. This translates to eventually needing to introduce a (logically) centralised entity (DNS-like) to keep track of these. Furthermore, most such ICN architectures assume existence of ASes and ISPs, as well as some entity similar to ICANN.

You can learn more about this at the following references:

- [Secure Naming for a Network of Information](https://core.ac.uk/download/pdf/11433344.pdf)
- [A Survey of Naming and Routing in Information-Centric Networks](https://www.researchgate.net/publication/260670278_A_Survey_of_Naming_and_Routing_in_Information-Centric_Networks)
- [Where is in a Name: A Survey of Mobility in ICN](http://www.eecs.qmul.ac.uk/~tysong/files/CACM13.pdf)

### Known shortcommins of existing solutions
> What are the limitations on those solutions?

To the best of our knowledge, there isn't an existing solution for Web3 that offers a naming solution that is Human-meaningful, Secure and/or Decentralized.

## Solving this Open Problem

### What is the impact

It will unblock the onboarding path from existing Web 2.0 users to Web 3.0 by reducing the friction of adoption.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

We would like to see approaches that take into account different applications as use cases (blogs, forums, social networks) and offer solutions that can hit 3 out of 3 of Zooko's triangle.

## Other

### Existing Conversations/Threads

- [Human Friendly Names](https://github.com/ipfs/notes/issues/286)
- [JRFC 28 - note on DNS and the Web](https://github.com/jbenet/random-ideas/issues/28)

### Extra notes

- [Ten terrible attempts to make IPFS human friendly](https://decentralized.blog/ten-terrible-attempts-to-make-ipfs-human-friendly.html)
