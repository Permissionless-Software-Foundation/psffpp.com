---
sidebar_position: 3
---

# Roadmap

Below is the latest roadmap for the PSF File Pinning Protocol (PSFFPP) project.

## Last Updated

This document was last updated 2/24/24.

## Milestones

Below is a short bullet-point list of milestones on the PSFFPP roadmap.

### Documentation

- Specifications - The File Pinning Protocol needs to be formally documented. This will be done in tandem with a JS library that will implement the protocol.
  - Renewal Claims - The protocol needs to be expanded to include renewals of pinned data past the initial year of hosting. This would be expressed as a modified Pin Claim, most likely a different Lokad ID.

- Minting Council - A document needs to be created that explains how the PSF Minting Council sets the cost of writing data to the PSFFPP network.

- READMEs - The README files of the major repos needs to be updated to include instructions for running the Docker container and explaining the different configuration options.

### Code

- JS Library - A JS library needs to be created that implements the File Pinning Protocol. Putting the logic into a library will make it easy to include in future projects, so that they can leverage the protocol.

- Blacklist - An optional library for blacklisting content will be developed. This will be a voluntary plugin to the ipfs-file-pin-service. It will be a location to note content that contains illegal content. Nodes that voluntarily run this plugin will not pin the content in the blacklist.

- Test Coverage - Each software component aspires to maintain 100% unit test coverage.

### Expand Infrastructure

- Network Monitoring - An app will be developed that monitors the nodes on the network and tests them for file availability. This will track uptime and honesty of nodes, and will be the basis upon which future bounties will be paid.

- Bounties - A bounty system will be developed to incentivize members of the PSF community to operate infrastructure and provide network services to the community.
