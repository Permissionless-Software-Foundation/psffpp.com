---
sidebar_position: 2
---

# Overview

This PSFFPP system is built on top of the [Cash Stack](https://cashstack.info). The first implementation uses the Bitcoin Cash (BCH) blockchain, but there may be future implementations on other blockchains. There are two 'halves' to the PSFFPP system.

- The **User Interface** is on the left side of the diagram below. It's takes files from the user, makes the file available to the IPFS network for pinning, and generates a [Pin Claim](https://github.com/Permissionless-Software-Foundation/specifications/blob/master/ps008-claims.md) on the blockchain or each file.

- The **File Pinning Service** is on the right side of the diagram below. There are many instances (nodes) of the service on the internet, and they all work together to redundantly store the file and make it available for download. Each node detects new [Pin Claims](https://github.com/Permissionless-Software-Foundation/specifications/blob/master/ps010-file-pinning-protocol.md#pin-claim) added to the blockchain, downloads the file over IPFS, and pins the file to their local node.

![PSFFPP blockchain interface](/img/blockchain-interface.png)

## Payment

The native curency of the PSFFPP is the [PSF token](https://slp-token.fullstack.cash/?tokenid=38e97c5d7d3585a2cbf3f9580c82ca33985f9cb0845d4dcce220cb709f9538b0). PSF tokens can be purchased at [PSFoundation.cash](https://psfoundation.cash).

In order to use the User Interface at [explorer.psffpp.com](https://explorer.psffpp.com), you will need to load the wallet with a few cents of BCH and PSF tokens. $1 USD worth of BCH and PSF tokens is enough for pinning several megabytes worth of data.

The protocol requires the burning of PSF tokens, to prove payment, in order to pin files across the network. The amount of PSF tokens required to be burned is proportional to the size of the files being uploaded, with a target of $0.01 per megabyte. During that process, a couple cents of BCH is required to pay transaction fees for interacting with the blockchain.

Files over 100MB in size will be rejected by the network. Files of any size less than 100MB can be uploaded, but the minimum payment is for 1MB. For example, if a file of 100KB is uploaded, the payment required is still for the equivalent of 1MB.

## User Interface

There is a growing list of user interfaces:

- [explorer.psffpp.com](https://explorer.psffpp.com) is analogous to a block explorer. It allows users to browse the files pinned on the decentralized network. It also has a web interface for uploading files and generating a Pin Claim.

- [TokenTiger.com](https://tokentiger.com) is an easy to use app for tokenizing data. With a few clicks you can upload an image, PDF, or zip file and generate a token to link the file to the blockchain. The files are hosted on the PSFFPP network.

- [psf-msg-wallet](https://github.com/Permissionless-Software-Foundation/psf-msg-wallet) is a command line interface (**CLI**) for JavaScript developers. It can stage files on the IPFS network and generate Pin Claims, and the source code provides a good example for integrating the PSF File Pinning Protocol into your own applications.


## File Pinning Service

The software that does the file pinning is composed of two pieces of software. This software is run on many redundant servers distributed around the world. They form a mesh network, modeled after the Bitcoin network. Every node on the network independently verifies the proof-of-burn (**PoB**) of PSF tokens and pins a copy of the file if it successfully validates the PoB.

- [psf-slp-indexer (pin-ipfs branch)](https://github.com/Permissionless-Software-Foundation/psf-slp-indexer/tree/pin-ipfs) is a blockchain indexer that tracks [SLP tokens](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md). The [pin-ipfs branch](https://github.com/Permissionless-Software-Foundation/psf-slp-indexer/tree/pin-ipfs) includes additional logic for tracking [Pin Claims](https://github.com/Permissionless-Software-Foundation/specifications/blob/master/ps008-claims.md) used to pin files with the PSFFPP. This indexer requires a direct connection to a full node. When a new Pin Claim is detected, it sends the information to the ipfs-file-pin-service via a web hook.

- [ipfs-file-pin-service](https://github.com/Permissionless-Software-Foundation/ipfs-file-pin-service) is activated by the psf-slp-indexer via webhook, when a new Pin Claim is detected. The software completes these steps for each Pin Claim:
  - It validates the Pin Claim.
  - It downloads the file over IPFS.
  - It validates the PoB burned enough PSF tokens, proportional to the size of the file.
  - It then pins the file to its local IPFS node.

Both pieces of software are written in node.js JavaScript, and are based on this [ipfs-service-provider boilerplate](https://github.com/Permissionless-Software-Foundation/ipfs-service-provider). Anyone can run these pieces of software on their own computer, in order to get direct access to the collection of files stored on the network.

## More Information

Below is a very early video of the development of the idea. Many things presented in it are outdated.

<iframe width="600" height="400" src="https://www.youtube.com/embed/flaEm4RFzYA" title="PSF File Pin Service - Technical Introduction" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
