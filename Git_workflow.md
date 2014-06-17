# Git Workflow


### First things first

We have two main branches: **master** and **development**. All development takes place in the development branch. The code in *master* branch always reflects a production-ready state. Each time changes are merged into the *master* branch, we **should** have a new production release. 


### Working on a new feaure

First branch off a new feature branch from the *development* branch:

```
$ git checkout -b newfeature development
```
Do your work and commit the changes. When you're finished, merge it into *development* branch, and delete the feature branch:

```
$ git checkout development
$ git merge --no-ff newfeature
$ git branch -d new feature
$ git push origin development
```

### Creating a release branch

Release branches are only created when a new release is about to happen and the *development* branch includes all the features planned for the up-coming release.

**You cannot add new feature or major fixes to the release branch, only minor bug fixes**

The develoment for the next release can start **AFTER** the release branch for the current release is created.

To create a release branch for version 1.x:

```
$ git checkout -b relase-1.x development
$ // bump version number
$ git commmit -a -m "Bumped version number to 1.x"
```

#### Merging release branch into master

This only happens when *release* branch is production-ready.

```
$ git checkout master
$ git merge --no-ff realease 1.x
$ git tag -a 1.x
```

To apply the bug fixes on the *release* branch into the *development* branch:

```
$ git checkout development
$ git merge --no-ff release-1.x
```

If there are any conflics, fix and commit. Then delete the release branch:

```
$ git branch -d release-1.x
```
### Hotfixes

Critical and urgent bugs that needs to be fixed before the next planned release are dealt with in hotfixes.

Hotfixes are branch off of the *master* branch and are merged back into both *master* and *developoment* branches.

To create a hotfix branch to fix a bug in version 1.x:

```
$ git checkout -b hotfix-1.x.1 master
$ // bump version number
$ git commit -a -m "Bumped version number to 1.x.1"
```

Then fix the bug and commit the fix:

```
$ git commit -m "Fixed ciritical bug"
```

Then, we merge the fix into *master*:

```
$ git checkout master
$ git merge --no-ff hotfix-1.x.1
$ git tag -a 1.x.1
```

And development, if there are no current release branches:

```
$ git checkout development
$ git merge --no-ff hotfix-1.x.1
```

If there is a release branch, the changes will applied there, since it will be merged into the *development* branch eventually.

Finally, we remove the hotfix branch:

```
$ git branch -d hotfix-1.x.1
```

#### More info

This workflow is based on ["A successful Git branching model"](http://nvie.com/posts/a-successful-git-branching-model) by Vincient Driessen.






