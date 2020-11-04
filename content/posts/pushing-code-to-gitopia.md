---
title: "Push your Code Repositories to Gitopia"
date: 2020-10-29T17:52:04+05:30
draft: true
tags: ["Gitopia"]
categories: ["Technology"]
author: The Tech Trap
---

### Introduction

Recent events (Cite) shows us that cencorship can happen in open source. We cannot consider Github to be a utopia for Our Open Source Code.

Hence we are building [_Gitopia_](https://gitopia.org).
We harness the versioning power of git and the data permanence of arweave to provide a true Utopia for your git repos which makes your code resistant to Censorship. Then, it cannot be removed by anyone. We take advatage of `git-remote-gitopia` git-remote-helper, which helps git cli understand the gitopia remote url `gitopia://.../...`.

### Mirror your github repos

Change happens slowly and to reduce friction we have released Gitopia Mirror Action on Github Action Market place (link).
This enables you to mirror your repos to Utopia.

### How it works

### Requirements

- You need to have a repository on Github
- You need to have an arweave wallet file, If you dont, grab one from the faucet that arweave generously provides (link)

#### Step 1

Go to [Gitopia.org](https://gitopia.org)

![](/images/gitopia.png)

#### Step 2

Click on the login button on the top right corner
This will show a pop up as shown on the image below.

![](/images/login.png)

Here you need to drop/select your arweave wallet file you want to use.
A successful login will take you to your profile page as shown below

#### Step 3

![](/images/profile.png)

Now to create a new Repository.
Click Create Repository `+` sign

![](/images/create-repo.png)

#### Step 4

All repositories names must be unique for each user. The form would show an error if a repo with the same name exists.

Enter repo name and description(optional).

![](/images/repo-filled.png)

#### Step 5

Click Create Repository.
This would trigger a transaction with the application necessary tags to arweave. This would cost a nominal tx fee, Gitopia deosn't charge anything here. So you would need some amount of arweave in your wallet to pay the tx fee.
The transaction takes some time to confirm on the blockchain, but since the action is performed client side, we cache the txid and show it to you in the notifications which can be accessed by clicking the address or notifiction icon in the header.

![](/images/pending-notification.png)

#### Step 6

Once successful, you would be redirected to the Blank repository page

![](/images/new-repo-page.png)

Copy the remote url from the repo page as suggested in the above image `gitopia://VC4NJ3nlVJJPgyJ4DScpeXx3-UnXmiasfrxiIFpJwb0/gitopia-mirror-sample`

#### Step 7

Gitopia mirror action is available on the [marketplace](https://github.com/marketplace/actions/gitopia-mirror-action)
In your github repo thatyou want to mirror, create a file called `gitopia-mirror.yml` in `.github/workflows/` directory

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
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - name: Gitopia Mirror Action
        uses: gitopia/gitopia-mirror-action@v0.1.0
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

The above snippets the src branch which will trigger the action. This is the branch that will be used in the action and will be pushed to Gitopis

You need to update 3 custom parameters that lie under `with`

- `gitopiaWallet` : This is set as secrets.GIOPIA_WALLET in the above example. For this you need to create a Github Secret (repo or organization level) naming it `GITOPIA_WALLET` and paste the contents of your arweave wallet file into it.
- `branch` : This is the Gitopia repo branch you want to push to
- `remoteurl`: This is the remote url that we copied in step 6

#### Step 10

Push your repo to github. Pushing your master branch to github as described in the above yml will trigger the Github Action.

![](/images/action.png)

As you can see in the image, the code has been pushed to Gitopia. Now we just wait for the bundled transaction to confirm and voila.
You can see your repository deirectory and files on [Gitopia](https://gitopia.org)

![](/images/repo-clone.png)

P.S. Cloning from Gitopia does not require any wallet. You just need to install the git-remote-gitopia helper `npm install -g @gitopia/git-remote-gitpia` so that your git command line can understand gitopia transport.
and then `git clone gitopia://VC4NJ3nlVJJPgyJ4DScpeXx3-UnXmiasfrxiIFpJwb0/gitopia-mirror-sample`, this would clone the repo to your local filesystem.

### Conclusion
