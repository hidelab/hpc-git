# a git remote on sharc


## For every repo that you want on sharc


Login to `sharc` and: `git init --bare REPO-NAME`


on your laptop: `git add remote ice md1xdrj@iceberg.sheffield.ac.uk:repo-name`

Now you can push: `git push ice HEAD`
]]

or clone

```
git clone ssh://md1xdrj@iceberg/~/solr1
```

(this results in the respo on iceberg being the `origin` remote)

If we're going to have two remotes (one on iceberg, one on
github) we might need to rename a remote:

(refer to https://help.github.com/articles/renaming-a-remote/)


```
git remote rename origin iceberg
```


