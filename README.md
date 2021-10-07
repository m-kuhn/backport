Backport is a [JavaScript GitHub Action](https://help.github.com/en/articles/about-actions#javascript-actions) to backport a pull request by simply adding a label to it.

It can backport [rebased and merged](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-request-merges#rebase-and-merge-your-pull-request-commits) pull requests with a single commit and [squashed and merged](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-request-merges#squash-and-merge-your-pull-request-commits) pull requests.
It thus integrates well with [Autosquash](https://github.com/marketplace/actions/autosquash).

# Usage

1.  :electric_plug: Add this [.github/workflows/backport.yml](.github/workflows/backport.yml) to your repository.
2.  :speech_balloon: Let's say you want to backport a pull request on a branch named `production`.
    Then label it with `backport production`. (See [how to create labels](https://help.github.com/articles/creating-a-label/).)
3.  :sparkles: That's it! When the pull request gets merged, it will be backported to the `production` branch.
    If the pull request cannot be backported, a comment explaining why will automatically be posted.

_Note:_ multiple backport labels can be added.
For example, if a pull request has the labels `backport staging` and `backport production` it will be backported to both branches: `staging` and `production`.

## Forks not supported

Currently this does not work with PRs created by forks. This fails with the following error:

```
Run m-kuhn/backport@v1.2.3
Backporting #1
/usr/bin/git clone ***<repository>.git
Cloning into 'backport-test-base'...
/usr/bin/git config --global user.email github-actions[bot]@users.noreply.github.com
/usr/bin/git config --global user.name github-actions[bot]
Backporting to old on backport-1-to-old
  /usr/bin/git fetch origin pull/1/head
  From https://github.com/<repository>
   * branch            refs/pull/1/head -> FETCH_HEAD
  /usr/bin/git switch old
  Switched to a new branch 'old'
  Branch 'old' set up to track remote branch 'old' from 'origin'.
  /usr/bin/git switch --create backport-1-to-old
  Switched to a new branch 'backport-1-to-old'
  /usr/bin/git show 5c2ae37484359679013eb302d773896b966133cc^2
  commit b7394c12937947d1ba14f62036ea98d056adb7c4
  Author: <author>
  Date:   <date>
  
      <title>
  
  diff --git a/<file> b/<file>
  index 2546125..d487e86 100644
  ...
  /usr/bin/git cherry-pick 5c2ae37484359679013eb302d773896b966133cc^..5c2ae37484359679013eb302d773896b966133cc^2
  [backport-1-to-old 0fdb0f4] <title>
   Author: <author>
   Date: <date>
   1 file changed, 1 insertion(+), 1 deletion(-)
  /usr/bin/git push --set-upstream origin backport-1-to-old
  remote: Permission to <repository>.git denied to github-actions[bot].
  fatal: unable to access 'https://github.com/<repository>.git/': The requested URL returned error: 403
  Error: Error: The process '/usr/bin/git' failed with exit code 128
Error: HttpError: Resource not accessible by integration
```
