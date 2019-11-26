# Mutable Data (Naming, Real-Time, Guarantees)

## Short Description
> In one sentence or paragraph.

Enabling a multitude of different patterns of interactions between users, machines and both. In other words, what are the essential primitives that must be provided for dynamic applications to exist, what are the guarantees they require (consistency, availability, persistancy, authenticity, etc) from the underlying layer in order create powerful and complete applications in the Distributed Web.

## Long Description

Mutable Data in a Permanent Datastore? WAT?! If this is your reaction, don't despair, we've been there too. However, the good news is that achieving mutability over immutable datastores is not a new idea; in fact, if you are checking this document through Git/GitHub, you are experiencing just that.

The trick is to use _pointers_. While each data structure has its own reference (in the case of IPFS, that reference will be immutable and represented by a Content Identifier (CID)), we can achieve mutability by creating a meta data structure, known as a record, that points to the latest version of the data that the user is requesting. This is analogous to a developer updating the master branch to point to the latest commit (e.g. when a pull request is merged).

Mutable Data was an essential component of the Web 2.0 revolution, when websites stopped being simple static documents and became fully fledged applications. Now, achieving scalable and secure ways to mutate data wil enable a whole set of new applications for the Web 3.0. 

These applications will have different types of requirements, from millions of records being updated every second to millions of users interacting and mutating a data source. Creating a sound solution to achieve Mutable Data for the dWeb requires considering the three parts of the problem:

- **Update Propagation** - As noted, different applications will have different requirements, e.g. real-time delivery, delivery at least/most once, confirmation of delivery, conflict resolution, etc.
- **Update Authentication** - How to trust the parties that are propagating the updates, how to trust that they are not hiding any important update
- **Interface/API** - Supporting a database-link experience, including APIs that enable a user to query, update and search over multiple records

All of these problems should be solved in a manner that is tolerant of unreliable connections and offline operation.

## State of the Art

> This survey on the State of the Art is not by any means complete, however, it should provide a good entry point to learn what are the existing work. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

### Within the IPFS Ecosystem
> Existing attempts and strategies

##### IPNS, the InterPlanteary Naming System

IPNS is the solution baked into IPFS Core that handles naming. IPNS takes inspiration from the [Self-Certified FileSystem (SFS)](https://en.wikipedia.org/wiki/Self-certifying_File_System) and uses the concept of signed records and a distributed record store for storing and sharing them. When a user gets a signed record, that user can verify its signature, check the updated pointer and resolve it accordingly. Today IPNS can be used over multiple routers, DHT, PubSub and Cloudfare WebWorkers.

- https://docs.ipfs.io/guides/concepts/ipns

##### dnslink

DNSLink leverages the DNSSystem to store a TXT record that contains a hash. This is equivalent to having an A record that stores an IP, in this case, the TXT record is resolved by an IPFS node, fetching the hash and resolving the content for the user. All IPFS project websites are loaded through this naming system (e.g. https://ipfs.io)

- https://docs.ipfs.io/guides/concepts/dnslink

##### libp2p's floodsub/gossipsub

libp2p currently offers two PubSub implementations, floodsub and gossipsub. 

- https://github.com/libp2p/specs/blob/master/pubsub/README.md
- https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/README.md

##### peer-base's CRDTs implementations

CRDTs are an extraordinary building block for data mutability without requiring central mediation. The field of CRDT is vast and there has been many developments. Below you can find some of the implementations done to work with IPFS.

- https://github.com/ipfs-shipyard/js-delta-crdts
- https://github.com/ipfs-shipyard/peer-crdt 
- https://github.com/ipfs-shipyard/peer-crdt-ipfs
- https://orbitdb.org/

##### Textile's data wallet and caffee 

Textile offers a set of tools for build decentralized applications that also work on mobile platforms. Their documentation is pretty amazing and keeps evolving, to learn more follow the links below:

- https://docs.textile.io/concepts/the-wallet
- https://docs.textile.io/concepts/cafes
- [Tutorial on how to use Textile form IPFS Camp](https://github.com/ipfs/camp/tree/master/CORE_AND_ELECTIVE_COURSES/ELECTIVE_COURSE_D)

### Within the broad Research Ecosystem
> How do people try to solve this problem?

There are multiple decades of Research and Projects to solve this Open Problem. Don't feel discouraged by the sheer amount of articles, it is really high valuable work!

- Data Structures
  - [Conflict-free replicated data types](https://scholar.google.pt/citations?view_op=view_citation&hl=en&user=NAUDTpMAAAAJ&citation_for_view=NAUDTpMAAAAJ:M3ejUd6NZC8C)
  - [A comprehensive study of Convergent and Commutative Replicated Data Types](http://hal.upmc.fr/inria-00555588/document)
  - [Merging OT and CRDT Algorithms](http://dl.acm.org/citation.cfm?id=2596636)
  - [Delta State Replicated Data Types](https://arxiv.org/abs/1603.01529)
  - [CRDTs: Making δ-CRDTs Delta-Based](http://novasys.di.fct.unl.pt/~alinde/publications/a12-van_der_linde.pdf)
  - [Key-CRDT Stores](https://run.unl.pt/bitstream/10362/7802/1/Sousa_2012.pdf)
  - [Compreensive PubSub Reading List](https://ipfs.io/ipfs/QmNinWNHd287finciBwbgovkAqEBQKvnys1W26sY8uupc5/pubsub%20reading%20list.pdf)
  - [A CRDT Primer Part I: Defanging Order Theory](http://jtfmumm.com/blog/2015/11/17/crdt-primer-1-defanging-order-theory/)
  - [A CRDT Primer Part II: Convergent CRDTs](http://jtfmumm.com/blog/2015/11/24/crdt-primer-2-convergent-crdts/)
  - Talks
    - [RedisConf18: CRDTs and Redis—From Sequential to Concurrent Executions by Carlos Baquero](https://www.youtube.com/watch?v=ZoMIzBM0nf4)
    - [QCon London 2018: CRDTs and the Quest for Distributed Consistency by Martin Kleppmann](https://www.infoq.com/presentations/crdt-distributed-consistency)
    - ["CRDTs Illustrated" by Arnout Engelen](https://www.youtube.com/watch?v=9xFfOhasiOE)
    - [Coding CRDT](https://www.youtube.com/playlist?list=PLzUeAPxtWcqxBXjUelmcm5ORVjEpbUlHH)
    - [Dmitry Ivanov & Nami Naserazad - Practical Demystification of CRDT (Lambda Days 2016)](https://www.youtube.com/watch?v=PQzNW8uQ_Y4)
    - [ElixirConf 2015 - CRDT: Datatype for the Apocalypse by Alexander Songe](https://www.youtube.com/watch?v=txD1tfyIIvY)
    - [GOTO 2016 • Conflict Resolution for Eventual Consistency • Martin Kleppmann](https://www.youtube.com/watch?v=yCcWpzY8dIA)
    - [CRDTs in IPFS](https://www.youtube.com/watch?v=2VOF-Z-nLnQ)
    - [Journal Club - 2018 06 13 CRDT JSON Datatype, by Gonçalo Pestana](https://www.youtube.com/watch?v=TRvQzwDyVro)
  - [CRDT Tutorial for Beginners](https://github.com/ljwagerfield/crdt)
  - [Conflict-Free Replicated Data Types (CRDTs), An Offline Camp passion talk](https://medium.com/offline-camp/conflict-free-replicated-data-types-crdts-2c6ae67ab9a4#.duh4g0r9k) 
  - [CRDT Notes by Paul Frazee](https://github.com/pfrazee/crdt_notes)
  - [Towards a unified theory of Operational Transformation and CRDT by Raph Levien](https://medium.com/@raphlinus/towards-a-unified-theory-of-operational-transformation-and-crdt-70485876f72f)
  - [Data Laced with History: Causal Trees & Operational CRDTs](http://archagon.net/blog/2018/03/24/data-laced-with-history/) 
  - [A Conflict-Free Replicated JSON Datatype](https://arxiv.org/pdf/1608.03960.pdf)
  - [Snapdoc: Authenticated snapshots with history privacy in peer-to-peer collaborative editing](https://martin.kleppmann.com/papers/snapdoc-pets19.pdf)
  - [OpSets: Sequential Specifications for Replicated Datatypes](https://arxiv.org/abs/1805.04263)
- Access Control
  - [Access Control for Weakly Consistent Data Stores](http://www.complang.tuwien.ac.at/kps2015/proceedings/KPS_2015_submission_25.pdf)
  - [ACGreGate: A Framework for Practical Access Control for Applications using Weakly Consistent Databases](https://arxiv.org/abs/1801.07005)
  - [TRVE Data: Placing a bit less trust in the cloud](https://www.cl.cam.ac.uk/research/dtg/trve/)
  - [LSEQ: an Adaptive Structure for Sequences in Distributed Collaborative Editing](https://hal.archives-ouvertes.fr/hal-00921633/document)
- Applications & Libraries
  - [A simple approach to building a real-time collaborative text editor](http://digitalfreepen.com/2017/10/06/simple-real-time-collaborative-text-editor.html)
  - http://y-js.org/
  - http://swarmdb.net/
  - http://gun.js.org/
  - http://scuttlebot.io/
  - https://github.com/mafintosh/hyperlog
  - https://github.com/jboner/akka-crdt

### Known shortcomings of existing solutions
> What are the limitations on those solutions?

Given the sheer amount of solutions and experiments to solve the multiple challenges of this Open Problem, one common shortcoming is actually a qualitative and quantitative assessment of what is possible today and how these stack up to existing centralized versions and if they are capable or not to take on the load to fulfil the needs of the multiple use cases.

## Solving this Open Problem

### What is the impact

Mutable Data is a fundamental building block for creating applications. Improving the existing solutions and creating new ones that support a large user set in a distributed context will enable the growing number of Internet users to be part of the dWeb.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

We are led to believe that a complete solution for the Mutable Data Open Problem is not only one, but a set of solutions that are capable of adjusting for the type of use case.

Rach of the solutions proposed should present and repetably demonstrate how it handles the most typical use cases and how they handle different loads under multiple network. And overview of these typical use cases are:

- writers (i.e. publishers) to readers (i.e. consumers)
  - 1 to many - Blog
  - some to many - Code Repository
- writers/readers to writers/readers
  - 1 to 1 - Chat
  - few to many - Collaborative Documents
  - many to many - Social Network, Forums, Chat Rooms

More use cases defined at https://github.com/libp2p/research-pubsub/blob/master/USECASES.md

In addition, a solution should also contemplate answers to the following questions:

- What are the guarantees that users can expect?
- Does the system rely in a centralized compontent for it to work?

Additionally, we leave here a set of questions to spark the thinking:

- What form do Access Control and/or Capabilities take in a context non mediated by a centralized point?
- Should the system allow for these Access Control and/or Capabilities to be mutable, transferable or revocable? 
- What kind of cryptography needs to exist for access and revocation to happen?

## Other

### Existing Conversations/Threads

- [IPNS Improvement Design Exploration](https://github.com/ipfs/notes/issues/260)
- [DNS over IPFS](https://github.com/ipfs/notes/issues/250)
- [IPFS database: pubsub, consistency and persistence](https://github.com/ipfs/notes/issues/244)
- [Implement databases over IPFS using Persistent Balanced Trees](https://github.com/ipfs/notes/issues/161)
- [Petnames for multihashes](https://github.com/ipfs/notes/issues/157)
- [pluggable resolvers](https://github.com/ipfs/notes/issues/127)
- [Optimizing IPNS](https://github.com/ipfs/notes/issues/109)
- [Aggregation --> CRDTs discussion](https://github.com/ipfs/notes/issues/40)

### Extra notes
