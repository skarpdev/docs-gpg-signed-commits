# Ubuntu guide

Get the required tools:

```bash
sudo apt-get install gpa seahorse
```

If you do not already have a GPG key, generate a new one and follow the on-screen instructions.
Please make sure that the email you configure in the key is the SAME as the email that you are usually committing code with:

```bash
gpg --gen-key
```

When the key-generator finishes list your keys

```bash
$ gpg --list-keys

/home/{USERNAME}/.gnupg/pubring.kbx
------------------------------
pub   rsa3072 2018-04-12 [SC] [expires: 2020-04-11]
      KEY_ID

uid                 [ultimate] Your Name <your@email.dk>
sub   rsa3072/SUB_ID 2018-04-12 [E] [expires: 2020-04-11]
```

Take note of the value where `KEY_ID` is written above, and export your public key to a file:

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
```

Now when you commit, git will ask for your passphrase to unlock your GPG vault and sign.

### Extending the GPG session timeout

To avoid entering the passphrase each time you want to commit you can extend the session lifetime by editing the `~/.gnupg/gpg-agent.conf` file:

```config
default-cache-ttl 28800
max-cache-ttl 28800
```

Which will cache the password for 8 hours.
Start the agent in case it's not running:

```bash
gpg-agent --daemon
```