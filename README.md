# ignore-vendor

Demonstrating how [.gitattributes can be used to hide files in PR diffs](https://docs.github.com/en/repositories/working-with-files/managing-files/customizing-how-changed-files-appear-on-github)

See [Files changed](https://github.com/mozey/ignore-vendor/pull/1/files) in the `example` branch
- `foo.txt` is displayed
- `vendor/bar.txt` is not rendered by default
- `vendor/bar/bat.txt` is not rendered by default


## Alternative approach

With the `.gitattributes` approach above, files changed in `vendor` is still considered to be changes, i.e. the *"Files changed"* tab shows the number 3.

An alternative approach would be to use two branches. First branch [example2/vendor](https://github.com/mozey/ignore-vendor/pull/3/files) from main, and only change `vendor` files. Second, branch [example2/app](https://github.com/mozey/ignore-vendor/pull/4/files) from `example2/vendor`. Non-vendor code, e.g. your application code, can be changed in the second branch.
```sh
git checkout main
git checkout -b example2/vendor
git checkout -b example2/app
```

**NOTE** on creating the PRs
- When opening the PR for `example2/vendor`, set it to merge with `main`
- When opening the PR for `example2/app`, set it to merge with `example2/vendor`, not `main`

### Merge changes from main

First merge `main` with `example2/vendor`
```sh
git checkout main
git pull
git checkout example2/vendor
git merge main
```

Then...

### Merge changes from example2/vendor

Merge `example2/vendor` with `example2/app`
```sh
git checkout example2/vendor
git pull
git checkout example2/app
git merge example2/vendor
```

### Instructions to use alternative approach given an existing branch

Suppose you already have a branch where both *"vendor"* and *"app"* files were changed. 

1. Create the two branch as described above

2. Get a list of files that were changed, [see this](https://www.mozey.co/post/git#see-all-changed-files-on-a-branchhttpstackoverflowcoma6913546639133). Save paths starting with *"vendor/"* to `vendor-path.txt`, and the rest of the paths to `app-path.txt`. One file per line
```sh
git diff --name-only main...your-existing-branch
```

3. Checkout vendor paths into the vendor branch
```sh
git checkout your-existing-branch --pathspec-from-file=vendor-paths.txt
```

4. Merge vendor branch into the app branch as described above

5. Checkout the remaining paths into the app branch, and commit
```sh
git checkout your-existing-branch --pathspec-from-file=app-paths.txt
```

Reference
- [Git tip: How to "merge" specific files from another branch](https://jasonrudolph.com/blog/2009/02/25/git-tip-how-to-merge-specific-files-from-another-branch/)





