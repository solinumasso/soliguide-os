# Our workflow

<aside>
🎯 Describe the git workflow we use to contribute to the Soliguide project.

</aside>

# Introduction

## a) The environments

As for now, we currently use 3 + 1 distinct environment for the Soliguide:

- The production one (prod) which is linked to [soliguide.fr](https://soliguide.fr/);
- The preproduction one (preprod) which is linked to [soliguide.site](https://soliguide.site/). It is used to test our releases;
- The test one which is linked to [prosoliguide.fr](https://prosoliguide.fr/). It is used to test our features;
- The local dev environment.

## b) The git repository

We use a mono repo to store the frontend and the backend in the same place. For more information on how you can install it, you can read the README on [the repository page](https://github.com/solinumasso/soliguide).

# A. The different needs

For Soliguide development, we have specific needs caused by our business model and the use of our product in a strong social environment.

- [x] The production deployment should be done regularly with stable and tested releases.
- [x] We should be able to deploy hotfixes and test features without interfering with each other.
- [x] We should be able to deploy to production multiple features in a bulk while developing and testing other features.
- [x] We should be able to review code easily before merging a feature.
- [x] We should be able to add comments to the code when it’s not clear enough.

# B. The workflow

## a) Branches

There are 2 main branches:

- **master** : this is the production branch. All the code in this branch should be stable and tested.
- **develop** : this is the testing and preproduction branch. It is used when a feature is tested and validated to stabilize the branch before merging.

On top of these 2 branches, there are other branches corresponding to issues and features. Their name should be in the form of _number-type-name_. _number_ is the number of the issue. _type_ should be one of : _bug_, _feat_, _tech_, _data_, _hotfix_ and _test_ depending on the type of issue this feature is responding. _name_ is a comprehensible 2-3 words sentence summarizing the object of the issue.

⚠️ Bugs and hotfixes should absolutely be linked to issues. If a developer finds a bug s⋅he should create an issue and link its code and pull request (PR) to this issue.

If the developers works on a big features split in multiple parts, every part should have its own branch and go through the PR and review stage.

## b) Merges et rebase

<aside>
⚠️ **WARNING** : the golden rule of rebase → NEVER and when I say never, I mean NEVER rebase a branch when different developer push code (like **master**, **develop** or a big feature).

</aside>

We can rebase FROM these branches, but never TO them. We’ll have conflicts from hell, delete local branches to pull remote ones, fiddle with git and the code and nobody wants that ^^’. These branch should only get code with merge and/or rebase→push. Don’t forget to use the command `git push --force` after a rebase.

Every merge or rebase→push should be done after a PR on our [Github repository](https://github.com/solinumasso/soliguide) whoever produced the code.

**master** : this branch should never be merged to another branch. It should only be the other way around, **develop** being merged to **master** and in EXTREME cases a hotfix branch.

**develop** : this branch should only be merged to **master**. Features branches should be merged to this one when they are tested and reviewed.

The other branches should be rebased from the destination branch before being merged to it. If a feature needs to be split into multiple branches, all sub-branches should be rebased and push to the main branch, thus, there will be no merge commit and the git history should be cleaner.

For the same reason, if you made a lot of testing commits, do not hesitate to squash those commits into one, so only the final version remains in master.

## c) The environments

The codebase should be deployed into the **prod** environment only by an admin of the repository. The deployment should be done manually after 8pm to avoid disturbing the operational staff during their work. Only the **master** branch should be deployed in production.

The codebase should be deployed into the **preprod** to stabilize the **develop** branch before merging it to master. The deployment will be done manually for now.

The **test** environment will be used by someone else than a developer to test one or more features branches. The deployment will be done manually for now.

## d) The commits

The commits should be standardized according to the [Convention Commits specifications](https://www.conventionalcommits.org/en/v1.0.0/).

```xml
<type>([optional scope])[optional !]: <description>[optional body][optional footer(s)]
```

The **type** can be one of the following :

- **build** : Modifies the build system or external dependencies.
- **ci** : Modifies configuration files and scripts for the continuous integration.
- **docs** : Modifies only the documentation.
- **feat** : Updates to create a new feature.
- **fix** : Fixes a bug.
- **perf** : Improves the product performances.
- **refactor** : Improves the code quality without creating features or fixing a bug.
- **style** : Updates to improve visual clarity of the code (space, formatting, missing semicolon, …)
- **test** : Adds missing tests or improves the current ones.

The **scope** is optional and describe the content targetted by the commit.

The **!** is used for breaking changes.

The **description** simply explains what the commit is accomplishing. We should understand what this commit is doing with the description only. It should be written with a feature point of view.

The **body** details what is included in the commit, if necessary. It should be used especially if the commit is composed of multiple commits squashed together or if a lot of files were modified. It should be written with a dev/code point of view.

The **footer(s)** should be one of :

- _BREAKING CHANGE_: details the breaking change
- _Reviewed-by_: github-username
- _Refs_ #githubIssueNumber

## e) Labels

Labels are automatically generated by _lerna_ when a branch is merged to **master**. The label is chosen according to the commit added during the merge. The labels have the form _vA.b.c_, with A increasing with a breaking change commit, b is increasing when a there is at least a commit with a **feat** type and no breaking changes and c is increasing when there is no breaking changes and no **feat** commit.

# C. How can I collaborate to the project?

## a) For Solinum internal developers and product owners

- I have direct access to the repository.
- I can manage the issues.
- I can create, review and merge pull requests.
- I can push my commit directly to the origin remote repository.

## b) For external developers

- I have to fork the project to be able to contribute.
- I can create pull request and link them to issues.
- If I am very active on the development of the project, I can have the same rights as the internal developers.

# Special contributions

The **translation** is done using a specific tool : [BabelEdit](https://www.codeandweb.com/babeledit)

# Other

## Specs versions

**mongo** : v5.0.10

**node** : v16.16.0

**angular** : v13.3.11

---

Sources :

Conventional commit: [https://www.conventionalcommits.org/fr/v1.0.0/](https://www.conventionalcommits.org/fr/v1.0.0/)

Git with agile method: [https://www.atlassian.com/agile/software-development/git](https://www.atlassian.com/agile/software-development/git)

Atlassian git workflow: [https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow)

OVH UX workflow (in French): [https://medium.com/@OVHUXLabs/la-puissance-des-workflows-git-12e195cafe44](https://medium.com/@OVHUXLabs/la-puissance-des-workflows-git-12e195cafe44)

Merge and rebases management: [https://delicious-insights.com/en/posts/getting-solid-at-git-rebase-vs-merge/](https://delicious-insights.com/en/posts/getting-solid-at-git-rebase-vs-merge/)

How to contribute to an open-source project: [http://blog.davidecoppola.com/2016/11/howto-contribute-to-open-source-project-on-github/](http://blog.davidecoppola.com/2016/11/howto-contribute-to-open-source-project-on-github/)

`git rebase` and `git merge` uses : [https://www.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
