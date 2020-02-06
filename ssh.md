# Setting up ShARC for passwordless logins

or

## How to avoid typing in your ShARC password all the time

`git` uses SSH (secure shell, used as both a noun and a verb)
to access the remote host;
it's better if we set it up so that you don't have to type your
password in all the time.

If you can already SSH into `sharc` without needing a password every time,
then you already have this set up.
You can skip to ∎.

If you're not sure, try `ssh USERNAME@sharc.sheffield.ac.uk`.
You'll need to replace `USERNAME` with your `sharc` username.
Mine is `md1xdrj`, yours will follow a similar pattern
(department, number, maybe some initials);
it will certainly be nothing to do with your email address,
and won't be found on your Ucard.

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

    ssh-keygen

The key pair is stored in two files,
and you are prompted for a location for the private key file.
The default is fine so accept that (it's `~/.ssh/id_rsa`).

Enter a new 'passphrase' (password) when prompted.
This shouldn't be a password you use for any other service.

The key pair consists of a pair of files:
A _private_ key and a _public_ key.
They are usually in the two files `~/.ssh/id_rsa` (private) and
`~/.ssh/id_rsa.pub` (public).

The private key should not be revealed to anyone.
The passphrase we just entered is used to encrypt the private key file,
which prevents malicious programs on our machine from being able to use it
without that passphrase.

But now we have another password to remember/manage!
Don't worry, there's a way of ensuring you only rarely need to type it.

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

The following appends your public key to a file `.ssh/authorized_keys`
in your home directory on ShARC:

    ssh-copy-id ~/.ssh/id_rsa.pub USERNAME@sharc.sheffield.ac.uk

You'll be prompted for your password (_not_ the private key passphrase)

You may be prompted to accept the fingerprint of
`sharc.sheffield.ac.uk` after being told something like:

```
The authenticity of host 'sharc.sheffield.ac.uk (143.167.3.47)' can't be established.
ECDSA key fingerprint is SHA256:WJYHPbMKrWud4flwhIbrfTB1SR4pprGhx4Vu88LhP58.
```

This is a really crucial step in
ensuring the connection between you and `sharc` is secure.
Without having some secure means of verifying the fingerprint,
you will not be able to trust the security of the connection.

You should compare the _fingerprint_ value shown in your terminal
with [those published here](https://docs.hpc.shef.ac.uk/en/latest/troubleshooting.html)
and only proceed if you see a match.

∎ skip here if you can already `ssh sharc.sheffield.ac.uk`

Next, we're going to tell a program called `ssh-agent` to remember our passphrase
so it can be decrypted when needed:

    eval $(ssh-agent -s)
    ssh-add ~/.ssh/id_rsa

This time when you go

    ssh USERNAME@sharc.sheffield.ac.uk

you should be logged straight in,
without having to type in your password.

How cool is that?

Note that you'll need to re-run the `eval` and `ssh-add` lines for each new terminal you open.

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

