<!-- readme.md -->
# How to add verified signature into github
### Installing Packages

**Install gnupg**

```brew install gpg```

Install passphrase entry dialogs

```brew install pinentry-mac```

**Generate a key**

Follow instructions, select default, Enter if unsure.

```gpg --full-generate-key
Identify your key:
gpg --list-secret-keys --keyid-format=long
```
Your <key> is in "sec" part after slash, eg: sec ed25519/HERE 2024-05-07 [SC]

    ### NOTE:Freshly installed brew
Check where brew is located itself:

```which brew```

Substitute all ```/usr/local/bin``` locations in the paths below to: ```/opt/homebrew/bin```.

### Set git settings
```
git config --global user.signingkey <key>
git config --global commit.gpgsign true
git config --global gpg.program /usr/local/bin/gpg
```
Additional config
```
if [ -r ~/.zshrc ]; then echo 'export GPG_TTY=$(tty)' >> ~/.zshrc; \
  else echo 'export GPG_TTY=$(tty)' >> ~/.zprofile; fi
echo "pinentry-program /usr/local/bin/pinentry-mac" > ~/.gnupg/gpg-agent.conf
```
### Restart gpg service

```gpgconf --kill gpg-agent```

### Add your key to GitHub
#### Output your public key:
```gpg --armor --export <key>```

#### Add it to GitHub here:
https://github.com/settings/gpg/new

##### Adding into Second Machine
If you are moving your gpg folder ```(~/.gnupg)``` to another machine, make sure to correct permissions afterwards:

```
chown -R $(whoami) ~/.gnupg/
chmod 600 ~/.gnupg/*
chmod 700 ~/.gnupg
```

Notes: 

Feshly installed brew
Some tools may be hardcoded to look only in one location, make a symlink:

```sudo ln -s /opt/homebrew/bin/gpg /usr/local/bin```



Sometimes, GPG needs to know the tty device to prompt for the passphrase. Ensure the environment variable GPG_TTY is set correctly in your shell's profile (like .bashrc or .bash_profile):
```export GPG_TTY=$(tty)```

After adding this to your profile, source the file:
```source ~/.bashrc```


@shoaibswe


<sub>  ref hackmd & github</sub>