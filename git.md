# a git remote on sharc

These instructions explain how to use `sharc` as a remote for git.

They assume you can already SSH into `sharc`
without using a username or password.
I've written instructions for doing that in [ssh.md](ssh.md).

If you haven't followed those fine instructions
and you still need your username to login to `sharc`,
then that's okay,
you can still use these instructions,
but the remote location will be `USERNAME@sharc:REPO-NAME`
instead of `sharc:REPO-NAME`.
And you'll have to type your password for every `push` or `fetch`.


## For every repo that you want on sharc

Login to `sharc` and: `git init --bare REPO-NAME`

## You have an existing repo on your laptop?

on your laptop:

    git remote add sharc sharc:REPO-NAME

Now you can push:

    git push sharc HEAD

## You are creating a fresh repo on your laptop?

You can clone from the (currently blank) repo on `sharc`:

```
git clone sharc:REPO-NAME
```

This results in the repo on `sharc` being the `origin` remote.
Use `git remote -v` to list your remotes.

You can add and commit using the usual git workflow,
then push to `sharc`:

    git push origin HEAD

If we're going to have two remotes
(one on `sharc`, one on github) we might need to rename a remote:

(refer to https://help.github.com/articles/renaming-a-remote/)


    git remote rename origin sharc


