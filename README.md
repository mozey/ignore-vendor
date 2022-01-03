# ignore-vendor

Demonstrating how [.gitattributes can be used to hide files in PR diffs](https://docs.github.com/en/repositories/working-with-files/managing-files/customizing-how-changed-files-appear-on-github)

See [Files changed](https://github.com/mozey/ignore-vendor/pull/1/files) in the `example` branch
- `foo.txt` is displayed
- `vendor/bar.txt` is not rendered by default
- `vendor/bar/bat.txt` is not rendered by default


## Alternative approach

With the `.gitattributes` approach above, files changed in `vendor` is still considered to be changes, i.e. the *"Files changed"* tab shows the number 3.

An alternative approach would be to use two branches. First branch [example2/vendor]() from main, and only change `vendor` files. Second, branch [example2]() from `example2/vendor`. Non-vendor code, e.g. your application code, can be changed in the second branch.
```sh
git checkout main
git checkout -b example2/vendor
git checkout -b example2
```

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

Merge `example2/vendor` with `example2`
```sh
git checkout example2/vendor
git pull
git checkout example2
git merge example2/vendor
```





