# Improved layouts to represent data in hash-linked graphs (using IPLD)

## Short Description
> In one sentence or paragraph.

## Long Description

IPFS offers a unique way to represent any kind of data in a hash-linked graph. What this means is that a file can be chunked in different ways and these chunks can be organized with the goal of improving file seek times, file fetching, reducing time to first byte and more, which creates the room for drastic improvements in the performance of certain applications (e.g. video streaming) and the memory footprint of each dataset.

We believe this to be an evergreen field as the IPFS network improves and gets smarter, new ways to chunk and organize data will emerge for all sorts of usecases. This is possible thanks to the [IPLD](https://ipld.io/) data layer of IPFS.

## State of the Art

> This survey on the State of the Art is not by any means complete, however, it should provide a good entry point to learn what are the existing work. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

### Within the IPFS Ecosystem
> Existing attempts and strategies

##### UnixV1

- layouts
  - TrickleDAG
  - Balanced DAG
  - FlatDAG
- chunking
  - fixed-size
  - rabin

##### UnixV2

- https://github.com/ipfs/unixfs-v2/

### Within the broad Research Ecosystem
> How do people try to solve this problem?

Any kind of research that studies different file formats, compression algorithms and data manipulation.

### Known shortcommins of existing solutions
> What are the limitations on those solutions?

Although the existing solutions work, they are far from optimal and we believe that there is multiple avenues to improve.

## Solving this Open Problem

### What is the impact

Improve the experience of everyone that is using IPFS and IPLD and/or enable new usecases for processing data.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

A solution to this evergreen Open Problem should improve in one of more dimensions comparing to existing solutions. From performance, memory, interface, etc.

More concretely, here are some usecases to consider:

- Accessing and Fetching Large Data Archives
- Streaming Video
- Video Rendering

## Other

### Existing Conversations/Threads

- [Content depending chunker](https://github.com/ipfs/notes/issues/183)
- [IPLD Importers - endeavor notes](https://github.com/ipfs/notes/issues/144)
- [merkledag datastructure DSL](https://github.com/ipfs/notes/issues/22)
- [Small Languages for IPLD things (like IPRS)](https://github.com/ipfs/notes/issues/229)
- [IPLD Selector Thoughts](https://github.com/ipfs/notes/issues/272)
- [Authenticated RDF (or RDF/JSON-ld on IPLD)](https://github.com/ipfs/notes/issues/152)
- [commutative or rolling cryptographic hash](https://github.com/ipfs/notes/issues/83)
- [InterPlanetary language](https://github.com/ipfs/notes/issues/50)
- [Stackstream possibilities](https://github.com/ipfs/notes/issues/25)
- [streaming byte-string Stack-based Assembly draft](https://github.com/ipfs/notes/issues/6)
- [JRFC 20 - Merkle DAG](https://github.com/jbenet/random-ideas/issues/20)

### Extra notes
