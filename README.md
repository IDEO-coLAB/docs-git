# `docs-git`

These are the CoLab's documents for how we handle version control, branch strategies, and code merges.


<br/>
## Table of Contents

- [Version Control: Git](#git)
- [Source Hosting: GitHub](#github)
- [Git Strategy: Git Flow](#gitflow)
- [Merging Code: Pull Requests](#pulls)
- [Questions](#questions)


<br/>
## <a name="git"></a>Version Control: Git

We currently use Git for version control in the CoLab.

### Resources

If you're unfamiliar with Git, it is an *extremely useful* tool, and one that we use daily. You'll need to familiarie yourself with the basics in order to work with, and contribute to, CoLab engineering projects.

- Overall Introductions:
  * [Great overall explanation](https://betterexplained.com/articles/aha-moments-when-learning-git/)
  * [In-browser interactive tutorial](https://try.github.io/levels/1/challenges/1) (from GitHub)
  * [Git's official 'get started' docs](https://git-scm.com/documentation) (surprisingly good!)

- Relevant Specific Deeper Dives:
  * [Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
  * [Remote Branches](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches) 


<br/>
## <a name="github"></a>Source Hosting: GitHub

We host all CoLab repos under the [CoLab organization](https://github.com/IDEO-coLAB) on [GitHub](https://github.com/).


<br/>
## <a name="gitflow"></a>Git Strategy: Git Flow

We use our own version of the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/) strategy for handling branches and releases. We follow this strictly to avoid odd bugs and unknown state in our production code.

### The Branch Strategy

The Git Flow development model is inspired by existing models that have been proven under various conditions. 

#### Two Main Branches

The CoLab origin's central repo holds two main branches with an infinite lifetime: `origin/master` and `origin/develop`.

#### Master Branch: `master`

We consider `master` to be the main branch where the source code of `HEAD` **always reflects a production-ready state**. 

Changes to `master` should always come through a pull request.

#### Develop Branch: `develop`

We consider `develop` to be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release. Some would call this the “integration branch”. This is the branch from which all `feature` and `hotfix` branches are forked, and into which all `feature` and `hotfix` branches are merged.

Changes to `develop` should always come through a pull request.

- Naming convention: `develop`
- Branches from: `master`
- Must merge back into: `master`

#### Feature Branches: `feature-*`

Feature branches are used to develop new features for some upcoming release. The essence of a feature branch is that it exists as long as the feature is in development, but will eventually be merged back into develop (to definitely add the new feature to the upcoming release) or discarded (in case of a disappointing experiment). Basically, all feature branches are eventually pruned.

Feature branches should not exist in `origin` for very long. They should be merged into `develop`, via pull request, as quickly as possible and then cleaned up.

- Naming convention: `feature-*`
- Branches from: `develop`
- Must merge back into: `develop`

##### Creation: 

Here's an example of creating a new `feature-headerbar` branch that tracks the branch `origin/develop`:

```sh
> git checkout -b feature-headerbar origin/develop
Switched to a new branch "feature-headerbar"
```

##### Merging: 

Push your feature branch (`feature-headerbar`) up to Github.

```sh
> git push origin feature-headerbar
```

Then open a pull request on GitHub, attempting to merge your `feature` branch into `develop`. **Note: Feature branches always merge into `develop`.**

Once successfully merged into `develop`, you can safely delete the feature branch on GitHub as well as from your local branches.

```sh
> git branch -D feature-headerbar
Deleted branch feature-headerbar (was 05e9557).
```

#### Hotfix Branches: `hotfix-*`

Hotfix branches arise from the necessity to act immediately upon an undesired state of a live production version. When a critical bug in a production version must be resolved immediately, a `hotfix` branch must be branched from the current `master`.

- Naming convention: `hotfix-*`
- Branches from: `master`
- Must merge back into: first `master`, then `develop` immediately after (to ensure consistency)

##### Creation:

Here's an example of creating a new `hotfix-button_bug` branch that tracks the branch `origin/master`:

```sh
> git checkout -b hotfix-button_bug origin/master
Switched to a new branch "hotfix-button_bug"
```

##### Merging:

Push your feature branch (`hotfix-button_bug`) up to Github.

```sh
> git push origin hotfix-button_bug
```

Then open a pull request on GitHub, attempting to merge your hotfix branch (`hotfix-button_bug`) into `master`.

Once your pull request is accepted into `master`, immediately open up a new pull request to merge it into `develop` to keep things in sync.

Once successfully merged into **both `master` and `develop`**, you can safely delete your `hotfix-button_bug` branch on GitHub as well as from your local branches.


<br/>
## <a name="pulls"></a>Merging Code: Pull Requests

All merges from `feature` and `hotfix` branches into the `develop` and `master` branches need to be made through pull requests. Doing this accomplishes several important goals:

- Information dissemination across a larger swath of the team.
- Increased familiarity with the expanding codebase.
- Ensuring code quality / consistency.

For these reasons we ask that all merges be made by opening a new pull request and tagging someone for review. Thanks in advance!


<br/>
## <a name="questions"></a>Questions

If you have questions about any of the above, please reach out to [Reid](https://github.com/ReidWilliams/) or [Gavin](https://github.com/gavinmcdermott) in the CoLab.
