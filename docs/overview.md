---
sidebar_position: 2
---

# Overview

This PSFFPP system is built on top of the [Cash Stack](https://cashstack.info). The first implementation uses the Bitcoin Cash (BCH) blockchain, but there may be future implementations on other blockchains. There are two 'halves' of the PSFFPP system.

- The **User Interface** is on the left side of the diagram below. It's takes files from the user, makes the file available to the IPFS network for pinning, and generates a [Pin Claim](https://github.com/Permissionless-Software-Foundation/specifications/blob/master/ps008-claims.md) on the blockchain or each file.

- The **File Pinning Service** is on the right side of the diagram below. There are many instances of the service on the internet, and they all work together to redundantly store the file and make it available for download. It detects new Pin Claims added to the blockchain, downloads the file over IPFS, and pins the file to their local node.

![Docusaurus logo](/img/system-diagram.png)

## Payment

The native curency of the PSFFPP is the [PSF token](https://token.fullstack.cash/?tokenid=38e97c5d7d3585a2cbf3f9580c82ca33985f9cb0845d4dcce220cb709f9538b0). PSF tokens can be purchased at [PSFoundation.cash](https://psfoundation.cash).

In order to use the User Interface at [upload.psfoundation.info](https://upload.psfoundation.info), you will need to load the wallet with a few cents of BCH and PSF tokens. $1 USD worth of BCH and PSF tokens is enough for pinning several megabytes worth of data. [This video](https://youtu.be/d9AGMTRM3Ws?si=0hk38E6B0KoaTqfN) shows how to load the wallet and use it for file upload.

The protocol requires the burning of PSF tokens, to prove payment, in order to pin files across the network. The amount of PSF tokens required to be burned is proportional to the size of the files being uploaded, with a target of $0.01 per megabyte. During that process, a couple cents of BCH is required to pay transaction fees for interacting with the blockchain.

Files over 100MB in size will be rejected by the network. Files of any size less than 100MB can be uploaded, but the minimum payment is for 1MB. For example, if a file of 100KB is uploaded, the payment required is still for the equivalent of 1MB.

## User Interface

The User Interface is composed of two code repositories:

- [p2wdb-image-upload](https://github.com/Permissionless-Software-Foundation/p2wdb-image-upload) is a React web app that contains the cryptocurrency wallet and user interface for uploading files. This is the same app published at [upload.psfoundation.cash](https://upload.psfoundation.cash).

- [p2wdb-image-upload-backend](https://github.com/Permissionless-Software-Foundation/p2wdb-image-upload-backend) is server-side software that does the heavily lifting. Eventually this software will be phased out and all the functionallity will take place in the React app directly.

The above pieces of software are somewhat mis-named as they no longer interact with the [Pay-to-Write Database (P2WDB)](https://p2wdb.com). They will eventually be renamed. The P2WDB was an early prototype for the PSFFPP, but is no longer used for file upload.


## File Pinning Service

The software that does the file pinning is composed of two pieces of software. This software is run on many redundant servers distributed over the world. They form a mesh network. Every node on the network independently verifies the proof-of-burn (**PoB**) of PSF tokens and pins a copy of the file if it successfully validates the PoB.

- [psf-slp-indexer (pin-ipfs branch)](https://github.com/Permissionless-Software-Foundation/psf-slp-indexer/tree/pin-ipfs) is a blockchain indexer that tracks [SLP tokens](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md). A [pin-ipfs branch](https://github.com/Permissionless-Software-Foundation/psf-slp-indexer/tree/pin-ipfs) includes additional logic for tracking [Pin Claims](https://github.com/Permissionless-Software-Foundation/specifications/blob/master/ps008-claims.md) used to pin files with the PSFFPP. This indexer requires a direct connection to a full node. When a new Pin Claim is detected, it send the information to the ipfs-file-pin-service via a web hook.

- [ipfs-file-pin-service](https://github.com/Permissionless-Software-Foundation/ipfs-file-pin-service) is activated by the psf-slp-indexer via webhook, when a new Pin Claim is detected. The software completes these steps for each Pin Claim:
  - It validates the Pin Claim.
  - It downloads the file over IPFS.
  - It validates the PoB burned enough PSF tokens, proportional to the size of the file.
  - It then pins the file to its local IPFS node.

Both pieces of software are written in node.js JavaScript, and are based on this [ipfs-service-provider boilerplate](https://github.com/Permissionless-Software-Foundation/ipfs-service-provider). Anyone can run these pieces of software on their own computer, in order to get direct access to the collection of files stored on the network.
