---
title: Collaborative Transcription with Local-First Research Data
category: entry
date: 2019-10-25
---
 
For our ongoing efforts with [Mediating Hafiz](/mediating-hafiz), we recently aimed at developing an application for collaborative real-time transcription of Persian-language texts. This meant we'd need to find a storage for the 7000-verse-strong corpus and user-provided transcription data. Our first attempt included using [Google's Firestore](https://firebase.google.com/products/firestore/), but we became frustrated: Setting it up was easy, but Firestore was slow for us---we had [issues with querying](https://firebase.google.com/docs/reference/js/firebase.firestore.Query.html) our data, and felt like we lacked control.

Kleppmann et al. made a point for [local-first software](https://www.inkandswitch.com/local-first.html), and their pledge also applies to research---why have your data tucked away in data silos, prone to platforms’ authority, when you can have it fully available on your local machine? Even with today’s limited browser APIs, storage backends such as [IndexedDB](https://www.w3.org/TR/IndexedDB/) and [upcoming proprietary APIs](https://web.dev/native-file-system/) allow for persisting data locally.

## Going Full-P2P

From my [thesis work](https://kassel.works/thesis) I knew about Mathias Buus’ [hyperdb](https://firebase.google.com/products/firestore/), a distributed key-value database built on top of [hypercore](https://firebase.google.com/products/firestore/). hyperdb entails many of hypercore’s prospects: Sparse replication allows to just exchange parts of the database. Continuous replication enables to exchange data in real-time. And hypercore's public-key cryptography makes selective write authorization possible---among even more functionality.

We gave it a shot and migrated our whole document-based database into a string-based key-value format... and it worked, sort of. These are our learnings from building an architecture with hyperdb:

* hyperdb does sparse replication, but we didn’t really get into _what exactly_ is being replicated, and _when_. Is it the node you’re watching or the node you already downloaded? In the limited timeframe we had, we weren’t able to figure that out.
* [`db.list`](https://github.com/mafintosh/hyperdb#dblistprefix-options-callback) operations take a long time and are generally inefficient, but (as it seemed to us) the only documented way of fetching new data after `db.watch` announced a change.
* When developing for the browser, WebRTC is your friend for realizing real-time experiences. It’s not completely serverless, though, and relies on exchanging signal messages---which happens via servers. [`webrtc-swarm`](https://github.com/mafintosh/webrtc-swarm) provides such a system, but ultimately relies on a [signalhub server](https://github.com/mafintosh/signalhub) for signal exchange. Frustratingly, signalhub [has issues with Firefox](https://github.com/mafintosh/signalhub/issues/32) and hasn’t been updated since October 2017. We settled with GEUT’s [`discovery-swarm-webrtc`](https://github.com/geut/discovery-swarm-webrtc).
* You’d still need an always-online replication peer that ‘pins’ the database for availability if there are no other peers (i.e., users) online. We didn’t find any pre-existing solution for that, and with the uncertainty about sparse replication mentioned above, we didn’t attempt to build something ourselves.
* There is no official support for JSON-based values---`JSON.stringify` is what we used. This could have lead to really bad merge conflicts and wasn't good practice.

In the end, our experiment did work out, somehow, but we’d rather build upon a technology stack we’re trusting. hypercore is great, and so is hyperdb, but it’s not batteries-included database software, especially when you're working with JSON-based documents. [hypermerge](https://github.com/automerge/hypermerge) could have been an alternative, but recent versions of the hypercore-based CRDT don’t support browsers anymore.

## Multi-Master Appeasement

In [our prior work](https://dl.acm.org/citation.cfm?id=3343667), we’ve worked with a CouchDB backend---a document-based NoSQL database that includes a Rest API out-of-the-box. More interestingly, CouchDB follows a multi-master approach: Opposed to master-slave architectures, there is no hierarchy when replicating to or from multiple CouchDB instances. They’re all available as master nodes on their own.

[PouchDB](https://github.com/pouchdb/pouchdb) is a CouchDB-compatible database that works in the browser, using a local storage backend. By simply calling [`db.sync`](https://pouchdb.com/api.html#sync), you’re able to continuously replicate two PouchDBs---or, as in our case, a PouchDB and a CouchDB---in real-time. Fortunately for us, {C,P}ouchDB replication covers the whole database, and isn’t sparse as with hyperdb.

Migrating (another time) to CouchDB worked flawlessly for us, as did replicating with PouchDB clients. Once the initial replication has succeeded, subsequent replications happen very quickly. You’ll need to create your indexes manually with the `couchdb-find` plugin for [running Mango queries](https://pouchdb.com/guides/mango-queries.html), though, but once set up, running queries on our data worked out fine.

## Next Steps

While our application itself isn’t local per se, our data is: all collaborators have the entire database replicated to their machines---it works even on mobile devices. We consider that a win towards independence for collaborating individuals and researchers who aren’t trained to query a CouchDB by hand.

In the future, we aim to enhance our teams’ data workflows even more by bringing many features directly into the browser, establish more standards to our data models, and integrating more powerful tools such as [Jupyter notebooks](https://jupyter.org/) or [R Markdown](https://rmarkdown.rstudio.com/) directly into our architecture to allow for powerful data pipelines.
