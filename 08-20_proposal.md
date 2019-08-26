---
title: A Thesis Proposal
category: entry
date: 2019-08-20
---

Coming from Computer Science, research workflows in the Digital Humanities (DH) seem innately different when regarding the employment of digital tools. After all, the computer scientist's field of study concerns actual computers and their theoretical and practical capabilities. However, researchers in the Digital Humanities often bring a pragmatic approach to computers, given that their main concern, at least in the Classics, isn't the computer itself.

This leads to interesting ways of collaboration, which are of special importance for multi-disciplinary DH research workflows ([Hunyadi, 2016](https://www.taylorfrancis.com/books/e/9781315572659/chapters/10.4324/9781315572659-10)). DH research has established a presence on code versioning platforms such as [GitHub](https://github.com/topics/digital-humanities), where, for instance, large corpora consisting of TEI XMLs are easily exchanged while being sustainably authored and versioned. Platforms that offer access to increasingly abstract data, like [Perseus' Scaife Viewer](https://scaife.perseus.org/), [Pelagios' Recogito](https://recogito.pelagios.org/), and the [Mirador IIIF viewer](https://projectmirador.org/). These platforms are built upon open standards like [TEI](https://tei-c.org/), [IIIF](https://iiif.io/), and [W3C Linked Data](https://www.w3.org/standards/semanticweb/data), and are quickly gaining popularity among researchers.

But relying on platforms to manage one's data isn't sufficient---they may raise questions in regard to censorship, privacy, data ownership, open access, among many others. Recently, [GitHub blocked users from Iran and Syria](https://techcrunch.com/2019/07/29/github-ban-sanctioned-countries/), to give an idea of some of the implication of increasing reliance on platforms.


## Peer-to-Peer Systems

One solution to the platform *situation* could be peer-to-peer (P2P) systems: Distributing data directly among researchers---or, more generally, peers---without making additional stops at centralized servers. This suits well oncerning collaboration itself: If I want to collaborate with someone, I simply connect to their computer and ask for the respective data. 

As DH workflows to some degree originate from the notion of Hypertext and Hypermedia ([Nelson, 1965](https://dl.acm.org/citation.cfm?id=806036)), there are two P2P projects one could concern: [IPFS](https://ipfs.io/) and [Dat](https://dat.foundation/) both are P2P hypermedia protocols with each an ecosystem of implementations and applications. They allow for peer-to-peer sharing of arbitrary data as well as whole folders of files, while enabling discovery of peers without the need of a centralized server infrastructure. While IPFS offers [an implementation in Go](https://github.com/ipfs/go-ipfs), Dat has a reference implementation in JavaScript built upon [hypercore](https://github.com/mafintosh/hypercore) ([Ogden et al., 2017](https://github.com/datprotocol/whitepaper/blob/master/dat-paper.pdf)).


## The Thesis Proposal

Considering the above concerns on platforms---ownership, privacy, open access---, I aim to evaluate the utilization of a P2P hypermedia protocol for sharing annotation data among researchers in the Digital Humanities with a novel P2P collaboration system. Additional to creating a reference implementation for that system, I will analyze the usability of the employed P2P functionality with user studies.

The thesis will be conducted at the Leipzig University's Digital Humanities chair, supervised by Dr. Thomas Koentges and Gregory Crane of Tufts University.
