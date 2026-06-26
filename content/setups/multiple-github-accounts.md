---
title: GitHub Multi-Login
tags: ["setups","github","dev"]
date: 2025-11-30
---

Reference - [fromDev2Dev](https://www.youtube.com/watch?v=Fyfp0oEWD6w&ab_channel=fromDev2Dev)

### Enable Service

Go to ==services.msc== and enable **OpenSSH Authentication Agent**; Set the Startup as 'Automatic'

### Create Private and Public Keys for Account

>[!info] repeat for all user accounts

```bash
ssh-keygen -t ed25519 -C "test@domain.com"
# provide filename, example id_25519_test
# provide password
```

>[!tip] by default, location of keys will be ~/.ssh, if not found, check under current directory and move the files under ~/.ssh


>[!note] file without extension is private key and with .pub extension is public key

### Add Keys in Github

>[!info] repeat for all user accounts

Go to github account and from settings, go to ==SSH and GPG keys== and add the public key with custom *title*, set *key type* as Authentication Key


###  Create SSH config file

Create ==config== file under ~/.ssh using the following format

```bash
Host github.test // hostname can be customized
    Hostname github.com
    User git
    IdentityFile ~/.ssh/id_25519_test // defines the private key file
    IdentitiesOnly yes
```
### Configure .bashrc 

>[!info] repeat for all user accounts

==.bashrc== file can be found under home directory (~), if not found, create one and add the following configuration so that it looks first openssh directory
```bash
PATH="/c/Windows/System32/OpenSSh:${PATH}"
```
restart the git bash terminal

### Configure SSH

Add the private keys to SSH authentication service

```bash
ssh-add ~/.ssh/id_25519_test
ssh -Tv github.test // use the host used within .config file, T option is to diable terminal session, v - verbose
```
### Add .gitconfig file account

```bash
git config --global --edit

// .gitconfig


[user]
	name = primary username 
	email = primary email address

[includeIf "gitdir:secondary-location"]
	path = .gitconfig-test // secondary .gitconfig

[init]
	defaultBranch = main

[core]
	autocrlf = true
	editor = nvim
	sshCommand = C:/Windows/System32/OpenSSH/ssh.exe
```

Create the .gitconfig-test file under home directory (~)

```bash
[user]
	name = secondary username 
	email = secondary email address
[core]
    sshCommand = "ssh -i ~/.ssh/id_25519_test"
```
### Validate the configs

Go to the gitdir defined within the ~\.gitconfig for the secondary account  
Clone a repository using SSH

```bash
git clone git@host-name:account/repo.git
```
>[!info] repeat for all user accounts

To edit an existing configuration for an already cloned repo

```bash
git remote get-url origin
git remote set-url origin github.test/account/repo.git
```

### Notes

Configure the identity per project. That is, while configuring user.email and user.name, avoid  avoid using ==--global== flag.

```bash
git config user.name "your name"
git config user.email "your email"
```

Create/update the url of remote with PAT or Personal Access Token.

>[!note] while creating PAT, include permissions for content, issues, pull requests; allow read and write acess.  include only required repository within scope.

```bash
git remote set-url remote-name url-with-pat

git remote set-url origin https://PERSONAL-ACCESS-TOKEN@github.com/kaekeshan/garden.git
```
