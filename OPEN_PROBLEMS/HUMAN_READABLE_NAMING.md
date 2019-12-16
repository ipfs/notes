# Human Readable Naming

## Short Description
> In one sentence or paragraph.

You can only have two of three properties for a name: Human-meaningful, Secure and/or Decentralized. This is [Zooko's Trilemma](https://en.wikipedia.org/wiki/Zooko%27s_triangle). Can we have all 3, or even more? Can context related to some data help solve this problem?

## Long Description

Content naming is an integral part of any content-oriented network architecture. In contrast to today’s approach of securing the channel and authenticating the end-host when receiving some content, in content-centric networks, it is the content itself that has to be secured and authenticated. Doing so enables the very important property of location-independent caching of content - that is, clients don’t need to trust the machine that provides the content, as long as one can verify that the content is what it claims to be. The claim is verified through the content name.

That said, the naming scheme needs to integrate some properties to guarantee security and some others to enable non-security-related functionality.

There are generally three types of names that have so far been investigated and implemented in the area of content-oriented networks: flat, normally hash-based names (as seen in IPFS and [NetInf](https://core.ac.uk/download/pdf/11433344.pdf)), hierarchical, as seen in [NDN](https://named-data.net) and [tag-based](http://conferences2.sigcomm.org/acm-icn/2014/papers/p17.pdf).

We start by dividing between two types of properties for content names, that is, those that a name _must_ have and those that a name _should_ have.

In a content-oriented architecture (such as IPFS) the name **has to:**

- **provide security** guarantees, which further splits into two sub-properties
  - be *persistent & unique:* the name needs to stay the same even if the storage location or the owner of some content changes. Unique names enable the very important property of being able to cache content in arbitrary points in the network. Even if a tiny portion of the content changes, the name identifier changes too. In IPFS, this is achieved with hashing. Name-persistency makes it difficult to address dynamic content - see discussion on *“mutable content”* further below.
  - be *self-certifying:* the main purpose of having self-certifying names is to prove _data authenticity_. This is an important complementary part of name persistency to enable location-agnostic caching. Self-certifying names are normally non-human readable. 
Traditionally, self-certifying names have been considered to be those that are not human-readable and most commonly have been based on the hash of the content itself. An alternative way to authenticate content is through the hash of the owner’s key. This helps in case of dynamic content and is used in IPNS and NDN, but it might lead to limited decentralisation properties.

The second set of properties are those that **would be good to have** or need to be supported due to other core infrastructure requirements. These are:


- *human readable:* memorizing Content Addresses is hard. No one should have to do that. We need a solution that can provide Human Readable Names to users without making them rely on either a central provider, an unauthenticated host, or on unauthenticated data, but without leaking their intent to access some piece of content that is referenced by that name.
Another element that comes into play in self-certifying names is to ensure that the user gets hold of the correct content ID.  For this to happen, there needs to be an external entity (e.g., search engine, or reputation algorithm) that provides the right self-certifying ID to the user. Therefore, decentralised search engines are expected to be an important component of human-readable names - see [this thread](https://discuss.ipfs.io/t/file-discovery-in-ipfs/1320) for a discussion on this topic.

- *hierarchical:* adding some hierarchy in the naming structure can enable aggregation, which can be very important in terms of scalability of the routing fabric. This is especially the case with network-layer names, that is, names used by routers to forward packets (such as in Named Data Networking). Hierarchical names are usually linked with human-meaningful names, mainly due to their structural relation to URLs, but they do not have to be.

- *mutable:* content-derived hashes make it difficult to identify dynamic content, as the hash changes every time the content is altered. However, being able to support mutable content is key in any content addressable architecture. An approach often used is to address mutable content through the hash of the owner’s/publisher’s key. [IPNS](https://docs.ipfs.io/guides/concepts/ipns) is following a very similar approach, having been inspired by the [Self-Certified File System (SFS)](https://en.wikipedia.org/wiki/Self-certifying_File_System). IPLD is then used to address subgraphs of DAGs.

- *decentralised:* being able to resolve and verify content integrity/authenticity without relying on a (centralised) trusted third party, e.g., a Certificate Authority, is a central part of solutions in the Web3 space. Decentralisation also links to anonymity, that is, being able to publish content without being linked back to the publication of the content. Authenticating content through the hash of the owners’ key might reduce anonymity properties (see study of the NetInf project for solution proposals).

We conjecture that not all of the second group of properties can be natively supported by the naming scheme itself and will therefore have to be supported by external infrastructure components.

[Zooko's Trilemma](https://en.wikipedia.org/wiki/Zooko%27s_triangle) suggests that it is impossible to get a name that is Human-meaningful, Secure and Decentralized. However, what resides as a question is the possibility to take into account the context for the resolution of the name (locality: Decentralized, but local / namespaced). We believe there is an avenue here to explore. Furthermore, recent proposals including [Blockstack](https://en.wikipedia.org/wiki/Blockstack) and [Namecoin](https://en.wikipedia.org/wiki/Namecoin) have gone into significant depth to overcome Zooko's conjecture. None of these systems, however, have been tested at scale, or analytically proven.

**Summary of open problem:** The focus of this Open Problem is to formally define which of the *“would be good to have”* properties can we integrate in a naming scheme, while in all cases guaranteeing the first set of requirements is in place. We would like to formally investigate the Zooko’s trilema in a wider scope and assuming security and decentralisation are core parts of a naming scheme explore how and whether human-readability, mutability and name hierarchy can be integrated natively in a naming scheme, or alternatively supported by external architecture elements.


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

##### IPFS link-shortening service provided by textile.photos cafes

Full explanation at https://blog.textile.io/ipfs-experiments-creating-ipfs-links-that-you-can-delete/

### Within the broad Research Ecosystem
> How do people try to solve this problem?

##### [DNS](https://en.wikipedia.org/wiki/Domain_Name_System)

One of the first widely deployed solutions for Human readable names.

##### ICN/NDN

In Named Data Networking, names are used to do routing in the network (i.e., natively at the network layer). The NDN (formerly CCNx) project started with hierarchical, human-readable name structure, but it later became apparent that having only human-readable names is not quite an option.

It's impossible to guarantee that no two users will give the same name to some content. This translates to eventually needing to introduce a (logically) centralised entity (DNS-like) to keep track of these. Furthermore, most such ICN architectures assume existence of ASes and ISPs, as well as some entity similar to ICANN.

You can learn more about this at the following references:

- [Secure Naming for a Network of Information](https://core.ac.uk/download/pdf/11433344.pdf)
- [A Survey of Naming and Routing in Information-Centric Networks](https://www.researchgate.net/publication/260670278_A_Survey_of_Naming_and_Routing_in_Information-Centric_Networks)
- [Where is in a Name: A Survey of Mobility in ICN](http://www.eecs.qmul.ac.uk/~tysong/files/CACM13.pdf)

### Known shortcommins of existing solutions
> What are the limitations on those solutions?

To the best of our knowledge, there isn't an existing solution for Web3 that offers a naming solution that is Human-meaningful, Secure and Decentralized. More generally, and considering the two sets of requirements earlier in this document, there has been no formal proof that the listed content name properties can or cannot co-exist, as well as what are the reasons affecting the tradeoffs that arise. 

## Solving this Open Problem

### What is the impact

It will unblock the onboarding path from existing Web 2.0 users to Web 3.0 by reducing the friction of adoption.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

We would like to see approaches that take into account different applications as use cases (blogs, forums, social networks) and offer solutions that can hit 3 out of 3 of Zooko's triangle.

Assuming security and decentralisation needs to be inherent parts of the naming scheme (at least in the IPFS ecosystem), we would like to: i) prove that the Zooko’s trilema holds (or prove that it does not hold), and ii) extend the Zooko’s trilema to explore the “would be good to have” properties above.

## Other

### Existing Conversations/Threads

- [Human Friendly Names](https://github.com/ipfs/notes/issues/286)
- [JRFC 28 - note on DNS and the Web](https://github.com/jbenet/random-ideas/issues/28)

### Extra notes

- [Ten terrible attempts to make IPFS human friendly](https://decentralized.blog/ten-terrible-attempts-to-make-ipfs-human-friendly.html)
