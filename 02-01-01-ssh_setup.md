---
title: 'SSH Setup'
teaching: 15
exercises: 0
---
::::::::::::::::::::::::::::::::::::::: objectives

- Understand how to setup SSH keys.
- Setup SSH keys for GitHub.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do SSH keys work?

::::::::::::::::::::::::::::::::::::::::::::::::::


## 1\. SSH Background and Setup

Before we can connect to a remote repository, we needs to set up a way for our computer to authenticate with GitHub so it knows it's us trying to connect to our remote repository.

We are going to set up the method that is commonly used by many different services to authenticate access on the command line. This method is called Secure Shell Protocol (SSH). SSH is a cryptographic network protocol that allows secure communication between computers using an otherwise insecure network.

SSH uses what is called a key pair. This is two keys that work together to validate access. One key is publicly known and called the public key, and the other key called the private key is kept private. Very descriptive names.

You can think of the public key as a padlock, and only you have the key (the private key) to open it. You use the public key where you want a secure method of communication, such as your GitHub account. You give this padlock, or public key, to GitHub and say "lock the communications to my account with this so that only computers that have my private key can unlock communications and send Git commands as my GitHub account."

What we will do now is the minimum required to set up the SSH keys and add the public key to a GitHub account.

The first thing we are going to do is check if this has already been done on the computer you're on. Because generally speaking, this setup only needs to happen once and then you can forget about it.

:::::::::::::::::::::::::::::::::::::::::  callout

## Keeping your keys secure

You shouldn't really forget about your SSH keys, since they keep your account secure. It's good
practice to check your SSH keys every so often to ensure they are still secure, up to date,
and that there are no unauthorised keys that could compromise your account.
This is especially important if you are using multiple computers to access your account.


::::::::::::::::::::::::::::::::::::::::::::::::::

We will run the list command to check what key pairs already exist on your computer.

```bash
$ ls -al ~/.ssh
```

Your output is going to look a little different depending on whether or not SSH has ever been set up on the computer you are using.

Alfredo has not set up SSH on his computer, so his output is:

```output
ls: cannot access '/c/Users/Alfredo/.ssh': No such file or directory
```

If SSH has been set up on the computer you're using, the public and private key pairs will be listed. The file names are either `id_ed25519`/`id_ed25519.pub` or `id_rsa`/`id_rsa.pub` depending on how the key pairs were set up.
Since they don't exist on Alfredo's computer, he uses this command to create them.

:::::::::::::::::::::::::::::::::::::::::  spoiler

## Resolving SSH Key Conflicts: Custom Names and Paths

If you need to create an SSH key pair with a custom name or store it in a non-default location (e.g., because a default-named key like id_ed25519 already exists), Git may not automatically use it when pushing or pulling from GitHub.

One solution is to add a GitHub entry to your SSH config.

Open or create the SSH config file:

```bash

$ nano ~/.ssh/config
```

Add an entry for GitHub, replacing the path with your key’s actual name and location:

```
Host github.com
	HostName github.com
	User git
	IdentityFile ~/<full_path_to_SSH_key_file>/<key_name>
	IdentitiesOnly yes
```

Save and exit. Now Git will use the correct key when pushing to GitHub.

::::::::::::::::::::::::::::::::::::::::::::::::::


### 1\.1 Create an SSH key pair

To create an SSH key pair Alfredo uses this command, where the `-t` option specifies which type of algorithm to use and `-C` attaches a comment to the key (here, Alfredo's email):

```bash
$ ssh-keygen -t ed25519 -C "a.linguini@ratatouille.fr"
```
In the command above, the `-C` flag is used to label the SSH key with the owner's email address. It's good practice to use the same email associated to your GitHub account, but it does not have to be, and will not be used to validate the access. You can also decide not to add any label.

If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
`$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

```output
Generating public/private ed25519 key pair.
Enter file in which to save the key (/c/Users/Alfredo/.ssh/id_ed25519):
```

We want to use the default file, so just press <kbd>Enter</kbd>.

```output
Created directory '/c/Users/Alfredo/.ssh'.
Enter passphrase (empty for no passphrase):
```

Now, it is prompting Alfredo for a passphrase. Since he is using his kitchen's laptop that other people sometimes have access to, he wants to create a passphrase. Be sure to use something memorable or save your passphrase somewhere, as there is no "reset my password" option.
Note that, when typing a passphrase on a terminal, there won't be any visual feedback of your typing.
This is normal: your passphrase will be recorded even if you see nothing changing on your screen.

```output
Enter same passphrase again:
```

After entering the same passphrase a second time, we receive the confirmation:

```output
Your identification has been saved in /c/Users/Alfredo/.ssh/id_ed25519
Your public key has been saved in /c/Users/Alfredo/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:SMSPIStNyA00KPxuYu94KpZgRAYjgt9g4BA4kFy3g1o a.linguini@ratatouille.fr
The key's randomart image is:
+--[ED25519 256]--+
|^B== o.          |
|%*=.*.+          |
|+=.E =.+         |
| .=.+.o..        |
|....  . S        |
|.+ o             |
|+ =              |
|.o.o             |
|oo+.             |
+----[SHA256]-----+
```

The "identification" is actually the private key. You should never share it. The public key is appropriately named. The "key fingerprint"
is a shorter version of a public key.

Now that we have generated the SSH keys, we will find the SSH files when we check.

```bash
ls -al ~/.ssh
```

```output
drwxr-xr-x 1 Alfredo   197121   0 Jul 16 14:48 ./
drwxr-xr-x 1 Alfredo   197121   0 Jul 16 14:48 ../
-rw-r--r-- 1 Alfredo   197121 419 Jul 16 14:48 id_ed25519
-rw-r--r-- 1 Alfredo   197121 106 Jul 16 14:48 id_ed25519.pub
```

### 1\.2 Copy the public key to GitHub

Now we have a SSH key pair and we can run this command to check if GitHub can read our authentication.

```bash
ssh -T git@github.com
```

```output
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

Right, we forgot that we need to give GitHub our public key!

First, we need to copy the public key. Be sure to include the `.pub` at the end, otherwise you're looking at the private key.

```bash
cat ~/.ssh/id_ed25519.pub
```

```output
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI a.linguini@ratatouille.fr
```

Now, going to GitHub.com, click on your profile icon in the top right corner to get the drop-down menu. Click "Settings", then on the
settings page, click "SSH and GPG keys", on the left side "Access" menu. Click the "New SSH key" button on the right side. Now,
you can add the title (Alfredo uses the title "Alfredo's Kitchen Laptop" so he can remember where the original key pair
files are located), paste your SSH key into the field, and click the "Add SSH key" to complete the setup.

Now that we've set that up, let's check our authentication again from the command line.

```bash
$ ssh -T git@github.com
```

```output
Hi Alfredo! You've successfully authenticated, but GitHub does not provide shell access.
```

Good! This output confirms that the SSH key works as intended.

:::::::::::::::::::::::::::::::::::::::: keypoints

- SSH keys allow you to authenticate yourself.
- SSH keys should be handled carefully to maintain security.


::::::::::::::::::::::::::::::::::::::::::::::::::
