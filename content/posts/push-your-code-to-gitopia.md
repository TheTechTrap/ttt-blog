---
title: "Push Your Code To Gitopia"
date: 2020-11-04T19:30:00+05:30
draft: false
tags: ["Gitopia"]
categories: ["Technology"]
author: The Tech Trap
---

Recent events [(YouTube-dl)](https://news.ycombinator.com/item?id=24872911) show us that censorship can also happen in open-source code. Github is no utopia, and is not the most suitable place to store open-source code.
Hence we are building [_Gitopia_](https://gitopia.org).
We harness the versioning power of git and the data permanence of Arweave to provide a true utopia for your git repositories, which makes your code resistant to Censorship. Then, it cannot be removed by anyone. We take advantage of `git-remote-gitopia` git-remote-helper, which helps git client understand the gitopia remote URL `gitopia://.../...`.

#### Mirror your GitHub repositories

Change happens slowly and to reduce friction, we have released [Gitopia Mirror Action](https://github.com/marketplace/actions/gitopia-mirror-action) in Github Marketplace.
This enables you to mirror your existing GitHub repositories automatically to Gitopia.

#### Requirements

To use the Gitopia mirror action:

- You need to have a repository on Github
- You need to have an Arweave wallet file; if you don't, grab one from the faucet that Arweave generously provides [Get Wallet](https://www.arweave.org/wallet)

#### Step 1

Go to [Gitopia.org](https://gitopia.org)

![](/images/gitopia.png)

#### Step 2

Click on the login button on the top right corner.
This will show a pop-up, as shown in the image below:

![](/images/login.png)

Here you need to select/drop the Arweave wallet file you want to use.
A successful login will take you to your profile page, as shown below:

#### Step 3

![](/images/profile.png)

Now to create a new repository,
Click Create Repository `+` sign.

![](/images/create-repo.png)

#### Step 4

All repository names must be unique for each user. The form will show an error if a repo with the same name exists.

Enter the repository name and an optional description.

![](/images/repo-filled.png)

#### Step 5

Click Create Repository.
This would post a transaction with the necessary tags to Arweave. This would cost a nominal transaction fee; Gitopia doesn't charge anything here. So you would need some amount of Arweave in your wallet to pay the transaction fee.
The transaction takes some time to confirm on the blockchain. Still, since the action is performed client-side, we cache the transaction id and show it to you in the notifications that can be accessed by clicking the address or notification icon in the Navbar.

![](/images/pending-notification.png)

#### Step 6

Once successful, you would be redirected to the new repository page:

![](/images/new-repo-page.png)

Copy the remote URL from the repo page, as suggested in the above image `gitopia://VC4NJ3nlVJJPgyJ4DScpeXx3-UnXmiasfrxiIFpJwb0/gitopia-mirror-sample`.

#### Step 7

Gitopia mirror action is available on the [marketplace](https://github.com/marketplace/actions/gitopia-mirror-action)
In your GitHub repo that you want to mirror, create a file called `gitopia-mirror.yml` in the `.github/workflows/` directory:

```
name: Gitopia Mirror

on:
  push:
    branches: [master]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - name: Gitopia Mirror Action
        uses: gitopia/gitopia-mirror-action@v0.1.1
        with:
          gitopiaWallet: "${{ secrets.GITOPIA_WALLET }}"
          branch: "master"
          remoteUrl: "gitopia://VC4NJ3nlVJJPgyJ4DScpeXx3-UnXmiasfrxiIFpJwb0/gitopia-mirror-sample"
```

#### Step 9

```
on:
  push:
    branches: [master]
```

In the above snippet, push to the `master` branch will trigger the action. This is the branch that will be used in the mirror action and will be pushed to Gitopia.

You need to update 3 custom parameters that lie under `with`:

- `gitopiaWallet`: This is set as secrets.GIOPIA_WALLET in the above example. For this, you need to create a Github Secret (repo or organization level) naming it `GITOPIA_WALLET` and paste the contents of your Arweave wallet file into it.
- `branch`: This is the Gitopia repo branch you want to push to
- `remoteUrl`: This is the remote URL that we copied in step 6

The checkout action that we use above, by default, checks out a shallow copy of your repo without history.
`fetch-depth: 0` configures it to checkout the branch with history.

#### Step 10

Push your Repository to Github. Pushing your master branch to GitHub will trigger the Github Action as described in the above .yml.

![](/images/action.png)

As you can see in the image, the code has been pushed to Gitopia. Now we wait for the bundled transaction to confirm, and voila -
you can see your repository directory and files on [Gitopia](https://gitopia.org):

![](/images/repo-clone.png)

#### Step 11

We have created a cool badge for the community which can be used to showcase the repositories that are mirrored on Gitopia.

![Gitopia](https://img.shields.io/endpoint?style=&url=https://gitopia.org/mirror-badge.json)

You can use it in your `README.md` by adding:

```
[![Gitopia](https://img.shields.io/endpoint?style=&url=https://gitopia.org/mirror-badge.json)](gitopia-repo)
```

where `gitopia-repo` can be `https://gitopia.org/address/repo-name`

#### Conclusion

The first push to mirror the repo contains all the historical objects; hence it can take upwards of 30minutes for large repositories. All the subsequent pushes will take negligible time.
We charge a small PST fee of 0.01 AR or 10% of the transaction fee (Repository Push), whichever is greater on every push.
This is then distributed to the LORE (Gitopia PSC Token) holders.
Cloning from Gitopia is free and does not require any wallet. You need to install the git-remote-gitopia helper `npm install -g @gitopia/git-remote-gitopia` so that your git command line can understand `gitopia://` transport.
Once the transaction is confirmed, you can use `git clone gitopia://VC4NJ3nlVJJPgyJ4DScpeXx3-UnXmiasfrxiIFpJwb0/gitopia-mirror-sample` to clone the repo to your local filesystem.

For the next 3 months, we will be distributing 250,000 LORE tokens (0.25% of the total supply) to the early adopters.
To stay updated, follow us on [Twitter](https://twitter.com/gitopiaOrg) and join us on [Discord](https://discord.gg/mVpQVW3vKE).
