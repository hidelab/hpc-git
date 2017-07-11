# Setting up sharc for passwordless logins

or

## How to avoid typing in your sharc password all the time

git uses SSH (secure shell, used as both a noun and a verb)
to access the remote host;
it's better if we set it up so that you don't have to type your
password in all the time.

If you can already SSH into `sharc` without needing a password,
then you already have this set up.
You can skip to ∎.

If you're not sure, try `ssh sharc.sheffield.ac.uk`.
If you log in without needing your password then you can
skip to ∎.

To login without a password, we need to:

- create a _key pair_; and,
- copy the public part of the key pair onto `sharc`.

If you already have an SSH key pair, you can skip this
instruction to create a pair (skip to •).
If you're not sure if you have a key pair,
you can check by running the command

    ls -l ~/.ssh/id_rsa

If it shows the details of the file, then you have a key pair,
skip to •.
If you get an error like

    ls: cannot access /home/drj/.ssh/id_rsa: No such file or directory

then you do not have a key pair.
Let's generate one!

### Generating a key pair

Run this command in a terminal.

```
ssh-keygen
```

The key pair is stored in two files,
and you are prompted for a location for the private key file.
The default is fine so accept that (it's `~/.ssh/id_rsa`).

The key pair consists of a pair of files:
A _private_ key and a _public_ key.
They are usually in the two files `~/.ssh/id_rsa` (private) and
`~/.ssh/id_rsa.pub` (public).

The private key should not be revealed to anyone.

The public key is, as its name suggests, public.
It can and should be distributed to systems that you wish to use.
If a server has your public key then you can use your private
key to login using SSH without a password.

• carry on here if you already have a key pair.

### Copying your public key to sharc

Now you have an SSH key pair
(check with `ls -l ~/.ssh/id_rsa` if you like).

The public part, `~/.ssh/id_rsa.pub`,
needs to be put on `sharc`.
This is surprisingly tricky.

You need to ssh into `iceberg` to create a `.ssh` directory,
and copy your `id_rsa.pub` file to `.ssh/authorized_keys`.

There are various ways to do this,
I'm going to suggest the one-liner:

    cat ~/.ssh/id_rsa.pub |
    ssh USERNAME@sharc.sheffield.ac.uk 'mkdir -p ~/.ssh ; cat >> ~/.ssh/authorized_keys'

You'll need to replace `USERNAME` with your `sharc` username.
Mine is `md1xdrj`, yours will follow a similar pattern
(department, number, maybe some initials);
it will certainly be nothing to do with your email address,
and won't be found on your Ucard.

You'll be prompted for your password.

You may be prompted to accept the fingerprint of
`sharc.sheffield.ac.uk` after being told:

```
The authenticity of host 'sharc.sheffield.ac.uk (143.167.3.47)' can't be established.
ECDSA key fingerprint is 2c:d0:f5:f0:a9:fc:c4:3e:da:81:e7:de:6c:5a:f8:b7.
```

(because stuff, the IP address and the fingerprint might change)

This is a really crucial step in
ensuring the connection between you and `sharc` is secure.
Without having some secure means of verifying the fingerprint,
you will not be able to trust the security of the connection.
In the interests of simplicitly, getting things done,
and honouring stupid traditions,
I'm telling you to accept the offered fingerprint.

∎ skip here if you can already `ssh sharc.sheffield.ac.uk`

Try it now.
This time when you go

    ssh USERNAME@sharc.sheffield.ac.uk

you should be logged straight in,
without having to type in your password.

How cool is that?

## Stretch goal: avoid explicit username

As it stands right now, you still have to type in your USERNAME in

    ssh USERNAME@sharc.sheffield.ac.uk

We're going to make it so that you can just type:

    ssh sharc

The secret lies in your `~/.ssh/config` file.
(See `man ssh_config` for the gory details)

In my `~/.ssh/config` file, I have the following lines:

    Host iceberg sharc
    User md1xdrj

That means that every time I go `ssh HOST` and `HOST`
matches either `iceberg` or `sharc` (exactly),
`ssh` will supply the username as `md1xdrj`.

As it happens, within The University of Sheffield eduroam network,
we can already shorten `sharc.sheffield.ac.uk` to `sharc`
(the rest of the domain name is supplied implicitly).

All of that now means that I can just go

    ssh sharc

to get logged into `sharc` with no uername and no password.


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


