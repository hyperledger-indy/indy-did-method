# Indy DID Method

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/hyperledger/indy-did-method)

This repository contains the source for the Indy DID Method specification. The initital content was transferred from a [collaborative HackMD document](https://hackmd.io/2IKUPROnRXW57Lmal_SGaQ).

The current version of the spec is rendered here: [https://hyperledger.github.io/indy-did-method/](https://hyperledger.github.io/indy-did-method/)

To contribute to the specification, please submit a pull request by:

- forking the repo
- editing the appropriate markdown files in the `/spec` folder

The specification is rendered from the markdown files to the `index.html` specification file in the `/docs` folder
using [Spec-Up](https://github.com/decentralized-identity/spec-up). The full guidance for using SpecUp is in that documentation.
The short version of the instructions for this repository is:

- Install prerequisites: node and npm
- Run `npm install` from the root of the repository
- Run `npm run edit` from the root of the repository to render the document with live updates to the `docs/index.html`. You can see the updates as you make as you edit the markdown files. Open the rendered file in a browser and refresh to see the updates as you work.
- You can also run `npm run render` to just generate the specification file once.

When you create a PR to update the specification, please **DO NOT** include `docs/index.html`. A GitHub Action on merge of PRs automagically
renders for the full specification (`docs/index.html`) to the `gh-pages` branch in the repo and the specification is
published from there. If there is a way to .gitignore the `index.html` in the main branch but not in the `gh-pages` branch, please let us know.

Hint: One way to revert the updated `docs/index.html` before doing a commit, is to run: `git checkout -- docs/index.html`

## Community Links

- [Meeting Agendas and Notes](https://wiki.hyperledger.org/display/indy/Indy+DID+Method+Specification)
- [Hyperledger Rocketchat #indy-did-method](https://chat.hyperledger.org/channel/indy-did-method)
  