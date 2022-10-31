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
- When opening the PR for `example2/app`, set it to merge with `example2/vendor`, not `main`. Then merge this branch first

If required, merge changes from main directly into each branch
```sh
git checkout main
git pull
git checkout example2/vendor
git merge main
git checkout example2/app
git merge main
```


## Alternative approach given an existing branch

Suppose you already have a branch where both *"vendor"* and *"app"* files were changed. 

- On the existing branch, get a list of files that were changed, [see this](https://www.mozey.co/post/git/#list-changes-in-branch). Save paths starting with *"vendor/"* to `vendor-paths.txt`, and the rest of the paths to `app-paths.txt`. One file per line
```sh
git diff --name-only main...your-existing-branch
```

- From main, create a branch for the *"vendor"* files. Checkout the vendor paths into this branch, and commit
```sh
git checkout main
git checkout -b example3/vendor
git checkout your-existing-branch --pathspec-from-file=vendor-paths.txt
git commit -m "Vendor changes from existing branch"
```

- From *"example3/vendor"*, create a branch for the *"app"* files. Checkout the app paths into this branch, and commit
```sh
git checkout example3/vendor
git checkout -b example3/app
git checkout your-existing-branch --pathspec-from-file=app-paths.txt
git commit -m "App changes from existing branch"
```

- Make sure you've committed the changes into the new branches using `git diff`. Then delete *"your-existing-branch"*, and work on the two branches created above

- Create the PRs, and merge changes as explained above

Reference
- [Git tip: How to "merge" specific files from another branch](https://jasonrudolph.com/blog/2009/02/25/git-tip-how-to-merge-specific-files-from-another-branch/)





