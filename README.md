# Git Bump
Version numbers are an amazing thing. They make it trivial to figure out whether
or not a program is at the most recent version, and there are even some 
relatively sane standards such as [semantic versioning](semver.org) which the 
Clojure community has adopted as the standard format for version numbers.

As a [git flow](http://nvie.com/posts/a-successful-git-branching-model/) adherent, while my branching strategy may be elegant I often 
find that I neglect my version number bumping. This repo represents one lazy 
evening's solution thereto: a VERSION file bump script in Python and a pair of 
git-hooks scripts which leverage that file to generate auto-commits that bump 
the version file so you don't have to.

## Behavior
This setup uses two hooks - the `post-commit` hook and the `post-merge` hook.

  - post-commit - "patch" bump (+ 0.0.1)
  - post-merge  - "minor" bump (+ 0.1.0) when merging to non-master branches
  - post-merge  - "major" bump (+ 1.0.0) when merging to "master"

The idea clearly being that, when combined with git flow, the result will be a 
sane and strictly increasing version number indicative of the state of the 
project.

## Installation
The file `bump.py` must be located on your path.. I use `~/bin`. The files 
`post-merge` and `post-commit` must be added to the `.git/hooks` folder of any 
and all repos which use this strategy.

## Drawbacks
First of all, this tool will at present generate a boatload of auto-commits as 
for every commit it will add a subsequent `[auto]` commit that increments the 
patch number. Second of all, the version number is a naive counter in the 
extreme and is quite likely to encounter conflicts and collisions between banches.

This will be mitigated by the fact that merges will generate an `[auto]` with a 
minor version bump so under git-flow the MASTER branch will step by 1.0.0 once 
conflicts with minor and patch version numbers are resolved. The DEVELOP branch 
will step by 0.1.0, again because conflicts with the patch number will be 
squashed when merging into the development branch, and finally the individual 
FEATURE branches will walk by 0.0.1 blithely independant of one-another.

So basically it may fill your repo with junk, but it'll be useful junk.

## License 
Copyright Reid McKenzie 2012, made available under the 
[WTFPBL](http://sam.zoy.org/wtfpl/) for your enjoyment.
