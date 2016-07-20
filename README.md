# Using iceberg as a git remote

I recommend that you set up SSH (secure shell) so that you have
a key pair and you use that key pair to login to `iceberg`. It
means you will be able to login to `iceberg` without using a
password.

If you already have an SSH key pair, you can skip this
instruction to create a pair (skip to ∎).
If you're not sure if you have a key pair,
you can check by running the command

   ls -l ~/.ssh/id_rsa

If it shows the details of the file, then you have a key pair,
skip to ∎.
If you get an error like

    ls: cannot access /home/drj/.ssh/id_rsa: No such file or directory

then you do not have a key pair.

## Generating a key pair

```
ssh-keygen
```

The key is stored in a file,
and you are prompted for a location for this file.
The default is fine so accept that (it's `~/.ssh/id_rsa`).

This actually creates a pair of keys in a pair of files:
A _private_ key and a _public_ key.
They are usually in the two files `~/.ssh/id_rsa` (private) and
`~/.ssh/id_rsa.pub` (public).

The private key should not be revealed to anyone.

The public key is, as its name suggests, public.
It can and should be distributed to systems that you wish to use.
If a server has your public key then you can use your private
key to login using SSH without a password.

∎

## Distributing your public key

Now you have an SSH key pair
(check with `ls -l ~/.ssh/id_rsa` if you like).

The public part, `~/.ssh/id_rsa.pub`,
needs to be put on `iceberg`.
This is surprisingly tricky.

You need to ssh into `iceberg` to create a `.ssh` directory;

    ssh username@iceberg.shef.ac.uk

You'll need to replace `username` with your `iceberg` username.
Mine is `md1xdrj`, yours will follow a similar pattern
(department, number, maybe some initials);
it will certainly be nothing to do with your email address,
and won't be found on your Ucard.

You may be prompted to accept the fingerprint of
`iceberg.shef.ac.uk` after being told: `The authenticity of host
'iceberg.shef.ac.uk (143.167.3.27)' can't be established.`

This is a really crucial step in ensuring the connection between
you and iceberg is secure.
Without having some secure means of verifying the fingerprint,
you will not be able to trust the security of the connection.
In the interests of simplicitly, getting things done, and
honouring stupid traditions, I'm telling you to accept the
offered fingerprint.

Once you're logged in:

    mkdir ~/.ssh



[[ Do once ever

Create an SSH key

Put public part of key on `iceberg:.ssh/authorized_keys`

]]

[[ Do once per fresh repo

Login to iceberg and: `git init --bare repo-name`

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


