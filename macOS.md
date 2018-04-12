# macOS Guide

This guide was written on macOS High Sierra 13.3 and assumes that you have homebrew installed. 

Get the tools:

```bash
brew cask install gpg-suite
brew install pinentry-mac
```

Generate a new GPG key if you do not already have one, and follow the onscreen instructions. Please make sure that the email you configure in the key is the SAME as the email that you are usually committing code with:

```bash
gpg --gen-key
```

When the key-generator finishes list your keys

```bash
$ gpg --list-keys
/Users/{USERNAME}/.gnupg/pubring.kbx
-------------------------------
pub   rsa4096 2016-08-05 [SC] [expires: 2020-08-05]
      KEY_ID
uid           [ultimate] Your Username <you@email.dk>
sub   rsa4096 2016-08-05 [E] [expires: 2020-08-05]
```

Take note of the value where KEY_ID is written above, and export your public key to a file:

```bash
gpg --armor --export KEY_ID > gpg-key.txt
```

## Configure GitHub / GitLab

With the file `pgp-key.txt` in hand you can now go to your profile settings on GitHub and GitLab and upload the key, in order for their system to start knowing your signatures and thereby "verify" your commits.

## Configure your local git client

The last step is configuring your local git client to actually sign the commits you make.

```bash
git config --global user.signingKey KEY_ID
git config --global commit.gpgsign true
git config --global gpg.program gpg
```
