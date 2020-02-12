# Preserve user privacy when providing and fetching content

## Short Description
> In one sentence or paragraph.

How to ensure that users of the IPFS network can retrieve and provide (whether as original publisher or as a cache provider) information while maintaining full anonymity. In other words, we need to find and/or build tools, techniques and protocols that <strong>decouple actions from the entities that perform them</strong>.

## Description

The Web 2.0 implements a default centralized infrastructure design that fails to protect users' privacy. Some of the common patterns that we see making users vulnerable are: when data is not encrypted, both at rest and in transit, users' interactions with services leaks their intent, which creates the opportunity for a pattern analysis attack. In a content-addressable network, unencrypted *content names* also allow for identification of what users are requesting and matching of requests to users.

In the Web 3.0, the dWeb, users get the ability to share data with other peers without using an intermediary. However, a complete solution is still missing that can prevent users leaking what data they are serving and fetching through side channels/pattern analysis (e.g. when searches are made, either through a search engine or simply by searching for the blocks in a Distributed Hash Table).

Some solutions exist to mitigate this problem (see the State of the Art section below). However, none is yet complete as in "it is always 100% private, period", requiring users to adapt and adopt certain strategies to anonymize the content, depending on the type of interaction they have with other users. This Open Problem is beyond a data encryption or wire encryption problem; a complete solution will have to provide a way to grant and revoke other users' access to content (Authorization), provide users with the ability to know when a piece of content they have published is being accessed (Accounting), and guarantee that the users with whom the content is being shared are who they claim to be (Authentication), while not letting a third party understand what is being shared, how and with whom.

This happens to be one of the toughest problems to solve in order to provide a complete and human rights preserving fabric for knowledge.

### Background

One of the strongest advantages of a content-addressable, or content-centric network is the fact that (if natively deployed as a network-layer architecture) it can successfully hide the identity of requesting nodes. In contrast to the standard situation where every request is carrying the requestor's IP address, a native network-layer request for explicitly named content only carries with it the name of the content and *not* where the request is coming from (i.e., source IP address). In such an environment, only immediate neighbour observers can (potentially) identify which node a request is originating from, since after the first-hop requests get "blended" together making it impossible to identify the source of the request. This is in stark contrast to the IP approach, where the source address of any request or data packet is carried permanently in the packet itself on its way to the source of the content.

Assuming that Data Authenticity can be supported by default through hash-based names and always-signed content, we identify the following categories that need immediate attention in this area:

- <strong>Node privacy:</strong> IPFS is working on content hashes, but libp2p is using the IP address of requesting and serving nodes. This has implications to anonymity of both requesting and serving nodes. We should find a way to hide or obfuscate the source IP address, as together with the content hash (at the IPFS layer), they are revealing what content each requesting node is consuming.
- <strong>Name privacy:</strong> Content addressing comes with the invaluable advantage of enabling the network to know what content it is actually transmitting. This is in contrast to current IP address-based communication, where the network is forwarding packets based on IP addresses and only Deep-Packet Inspection (DPI) techniques can reveal what are the contents inside the packet. Although this feature of content addressable networks brings huge advantages in terms of performance and security as mentioned elsewhere, it comes with some privacy concerns. In particular, *the content that the network is transferring is now "in the clear" and anyone can see what is being transmitted, even if the content itself is encrypted.* Ephemeral signatures and even ephemeral names have been proposed in the past to deal with the issue, but there is no widely adopted solution to date.
- <strong>Cache privacy:</strong> Similarly to being able to see what the network is transferring through content names, seeing the contents of a cache is another feature that is becoming possible with explicitly named content. This can be tremendously beneficial in terms of performance (i.e., find content closer and faster), but it can also reveal what content a node has consumed in the recent past. This is especially so in the case of IPFS where node caches hold content that the user has either explicitly agreed to store, or content that it has recently consumed and has not expired from the cache yet.
- <strong>Producer or signature privacy:</strong> Content authenticity in content-addressable networks is guaranteed through signatures. This is a very powerful feature, but at the same time it always reveals the identity of the signing authority or producer. Although techniques such as ephemeral signatures and anonymous credentials have been proposed, there isn't yet a widely accepted and tested solution.


## State of the Art

> This survey on the State of the Art is not by any means complete, however, it should provide a good entry point to learn what are the past and existing efforts in the area. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

### Within the IPFS Ecosystem
> Existing attempts and strategies

##### Wire Encryption

Thanks to libp2p, IPFS ensures that the communication between any two IPFS nodes is always encrypted. This is achieved through one of the many Crypto Channels that libp2p supports such as: SECIO, TLS 1.3, DTLS (in WebRTC) and NOISE.

##### Data Encryption

By encrypting the data at rest and only decrypting it when it needs to be used, we can ensure that only someone with access to the decryption key can indeed access that data. Some solutions using this technique are:

- [ipfs-senc](https://github.com/jbenet/ipfs-senc) - Encrypts the data with a symmetric key that is shared to the receiver through a sidechannel

##### Capability Systems / Cryptographic ACLs

- [Cryptree](https://book.peergos.org/security/cryptree.html) - This is the core of [Peergos](https://github.com/peergos/peergos) where it allows granting and revoking read or write access to individual files or entire directories in a global filesystem. Peergos' variation on the [original Cryptree](https://github.com/Peergos/Peergos/raw/master/papers/wuala-cryptree.pdf) invented by Wuala, has the added benefit of being post-quantum secure already because it only inherently relies on symmetric encryption, not asymmetric. 
- [peer-base cryptographic ACLs](https://github.com/peer-base/peer-base) - These are used by [PeerPad](https://peerpad.net). For each user, a Public/Private key pair is generated. Every time a user wants to make a modification, the user signs that modification and encrypts it with a symmetric room key so that only owners of the symmetric key can change and only changes from valid peers are accepted.

##### Private/Disjoint Networks

Creating a separate IPFS Network will ensure that only member nodes can access the content within that network. 

- [libp2p-pnet](https://github.com/libp2p/specs/blob/master/pnet/Private-Networks-PSK-V1.md) takes that one step forward and creates a protection using a pre-shared key. This means that only the owners of that key can join this network (to prevent from mistakenly joining two networks and making all data accessible).
  - See a demo of it at https://youtu.be/fObld4alGag?t=47

##### Onion Routing 

Onion routing is a technique for anonymous communication over a computer network. In an onion network, messages are encapsulated in layers of encryption, analogous to layers of an onion. The encrypted data is transmitted through a series of network nodes called onion routers, each of which "peels" away a single layer, uncovering the data's next destination.

- [libp2p-onion](https://github.com/OpenBazaar/go-onion-transport)
- [p3lib-sphinx](https://github.com/hashmatter/p3lib/tree/master/sphinx). Implements the sphinx onion packet format along with logic for packet construction and relaying. This protocol is used in lightning network and in Nym and  there is also [a demo of p3lib-sphinx working with libp2p](https://github.com/hashmatter/libp2p-onion-routing), [and a video](https://www.youtube.com/watch?v=j64C5CTb8J8). (Basically it allows libp2p nodes to delegate DHT requests using other libp2p nodes as relayers. This is pretty cool since you don't have to plug in/depend on any other overlay (i2p/Tor) and provide similar unlikeability and privacy properties (providing that you have enough entropy in the network and do not leak the circuit path).

### Within the broad Research Ecosystem
> How do people try to solve this problem?

A general and rather simple approach and rule of thumb is to use an anonymising proxy which is placed between a sender and a receiver of traffic. This inevitably means that there is trust placed on this proxy to behave as desired. Decentralised approaches (that don't necessarily have to trust proxies or proxy operators) rely on layered encryption and multiple extra hops of routing so that any message cannot be linked to its originator/source.

Research has generally fallen into one of two areas: 

- i) those solutions that target non-delay-sensitive applications (e.g., email or FTP)
- ii) those that do (e.g., VoIP, web). Traditionally, those classes have different requirements and therefore, different tradeoffs in terms of performance-anonymity requirements.

Solutions to the first category include mix-network techniques, where, for instance, there is extra (spurious) traffic inserted in the network to obfuscate the environment and make it difficult to correlate traffic with senders and receivers. Artificially adding extra delay on the path from source to destination has also been a popular technique in this area.

On the other end of the spectrum, anonymisation approaches that target delay-sensitive applications mainly consist of choosing random next hops to add an extra indirection (and encryption) on the path from source to destination.

Past research has indicated several approaches to anonymisation and privacy, but very little has seen adoption to date (e.g., Tor, I2P). We believe that with the renewed interest in P2P, decentralised networks a fresh look is needed in order to either identify or re-design techniques that fit present-day challenges.

- **ICN-related Literature**
  - On preserving privacy in content-oriented networks (ACM ICN 2011)
  - ANDaNA: Anonymous Named Data Networking Application (NDSS 2011)
  - Content-Centric and Named-Data Networking Security: The Good, The Bad and The Rest (IEEE LANMAN 2018)
- **Non Delay Sensitive Applications**
  - Mixing email with Babel (NDSS 1996)
  - Mixmaster Protocol -- IETF Internet Draft 2003
  - Maxminion: Design of type III anonymous remailer protocol (IEEE S&P 2003)
- **Delay-Sensitive traffic**
  - Anonymity for web transactions (ACM Transactions on Information and System Security 1998)
  - Morphmix: peer-to-peer based anonymous internet usage with collusion detection, 2002
  - Tarzan: A peer-to-peer anonymising network layer, (ACM CCS 2002)
  - Passive attack analysis for connection-based anonymity systems, 2003
- **Authorization, Authentication, Accounting**
  - [2017 TahoeLAFS Capability System](https://en.wikipedia.org/wiki/Tahoe-LAFS) - The peer-base Cryptographic ACLs were inspired by TahoeLAFS Capability System. 
- **Data Encryption**
  - [2016 DoubleRatchet](https://signal.org/docs/specifications/doubleratchet/) / [Matrix.org Olm/MegaOLM](https://gitlab.matrix.org/matrix-org/olm/blob/master/docs/olm.md#olm-a-cryptographic-ratchet)
- **M2M Private Communication**
  - [2014 Ricochet](https://github.com/ricochet-im/ricochet/blob/master/doc/protocol.md)
  - [2015 Scuttlebutt Secret Handshare](https://dominictarr.github.io/secret-handshake-paper/shs.pdf)
  - [2015 SoK: Secure Messaging](https://ieeexplore.ieee.org/document/7163029)
  - [2016 Talek: a Private Publish-Subscribe Protocol](https://raymondcheng.net/download/papers/talek-tr.pdf)
  - [2018 HOPR - privacy-preserving messaging protocol ](https://github.com/validitylabs/hopr)
- **Content Routing**
  - [Define privacy threat model for DHTs (and other overlay P2P networks)](https://github.com/gpestana/notes/issues/3)
  - [2010 Using Sphinx to Improve Onion Routing Circuit Construction](https://www.cypherpunks.ca/~iang/pubs/SphinxOR.pdf)
  - [2012 Octopus: A Secure and Anonymous DHT Lookup](https://ieeexplore.ieee.org/document/6258005)
  - [2019 Encrypted Distributed Hash Tables](https://eprint.iacr.org/2019/1126)
- **Traffic Analysis resistant**
  - [2009 ShadowWalker: peer-to-peer anonymous communication using redundant structured topologies](https://dl.acm.org/citation.cfm?id=1653683)
  - [2015 Vuvuzela: Scalable Private Messaging Resistant to Traffic Analysis](https://davidlazar.org/papers/vuvuzela.pdf)
- **Private Information Retrieval**
  - [XPIR : Private Information Retrieval for Everyone](https://eprint.iacr.org/2014/1025.pdf)
  - [PIR with compressed queries and amortized query processing](https://eprint.iacr.org/2017/1142.pdf)
- **Other**
  - [2018 Cwtch: Privacy Preserving Infrastructure for Asynchronous,Decentralized, Multi-Party and Metadata Resistant Applications](https://cwtch.im/cwtch.pdf)

### Known shortcomings of existing solutions
> What are the limitations on those solutions?

Some of the known shortcomings of existing solutions are:

- They don't offer protection against network analysis (it is possible to infer what the user is doing by analysing network traffic)
- Some of the solutions (e.g. OctopusDHT) rely on centralised certificate authorities for reputation management
- Solutions that are more resistant (not fully resistant) typically trade off bandwidth + memory for creating that protection (e.g. creating noise in the network to make it hard to distinguish valid from dummy traffic) 
- Lack of data encryption at rest
- Lack of complete authorization + revocation
- Lack of solutions to ensure content name privacy

## Solving this Open Problem

### What is the impact

Finding a complete solution to this Open Problem will grant any human in the planet the ability to privately access or share information without leaking their intent to non-local observers other than the provider of that content. Solutions may also be able to hide said intent from the content provider.

It will lead to a generation of safer applications, both consumer level and business critical, that require full control of one's data (e.g. Health Data).

Such solutions will bring P2P, decentralised storage and delivery networks to a different level and will establish related platforms (such as IPFS) as the de facto standard for anonymous communication.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

Valid solutions should improve the current state of the art or offer definitive solutions for:

- Two users can transfer a piece of information without any other user knowing or being able to predict: what it is, why it is being transferred, when and between whom.
- No single central authority are required to mediate the communication
- A provider has a way to grant and revoke access to information.
- A user can discover public information without leaking information about their interest. An adversary should not be able to link two or more discovery requests either.

As additional constraints

- Mechanism to prevent data exfiltration (e.g. when a user goes rogue)

Also, contributions towards solving this Open Problem can be in the form of answers to the following questions:

- How to measure privacy?

## Other

### Existing Conversations/Threads

- [Censorship resistance on IPFS](https://github.com/ipfs/notes/issues/281)
- [Content Encryption](https://github.com/ipfs/notes/issues/270)
- [Shared Secret constructions for Private Networks](https://github.com/ipfs/notes/issues/177)
- [Search over encrypted data](https://github.com/ipfs/notes/issues/128)
- [Plausible deniability for blocks](https://github.com/ipfs/notes/issues/21)
- [Alternative BitSwap strategies](https://github.com/ipfs/notes/issues/20)
- [Anonymous IPFS](https://github.com/ipfs/go-ipfs/issues/6430)
- [Privacy preserving DHTs](https://github.com/gpestana/notes/issues/8)
- [DHT improvement ideas](https://github.com/libp2p/research-dht/issues/6)
- [Tor onion integration](https://github.com/ipfs/notes/issues/37)
- [Identity RFC](https://github.com/ipfs-shipyard/peer-star/pull/15)
- [A Taxonomy of Privacy, 2006](https://www.law.upenn.edu/journals/lawreview/articles/volume154/issue3/Solove154U.Pa.L.Rev.477(2006).pdf)

### Extra notes
