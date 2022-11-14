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

### Editing the spec in the cloud with Gitpod

An alternative way to contribute to the specification, without any local installation, is to use [Gitpod](https://www.gitpod.io/).
- Fork the repo. 
- Open `https://gitpod.io/#https://github.com/YOUR_FORK_USER/indy-did-method`
- Register with Gitpod using your GitHub Account and provide `public_repo` permissions in order to commit to your fork from Gitpod.
- You'll see a prepared VSCode workspace with two windows. To the left, you can edit the markdown files in the `/spec` folder. To the right, you'll see the rendered spec.
- If the window is frozen try to reload the page.
- You can create a new branch and commit and push your changes using a terminal or the source control plugin to the left.
- Then, you can create a pull request from Github.

## Community Links

- [Meeting Agendas and Notes](https://wiki.hyperledger.org/display/indy/Indy+DID+Method+Specification)
- [Hyperledger Discord](https://discord.gg/hyperledger)
  
