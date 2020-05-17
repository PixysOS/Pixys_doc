# Configuring gerrit for PixysOS

## Generating SSH Key

```bash
$ cd ~/
$ ssh-keygen -t rsa
```

After that it will ask for the key location, for default press enter
Then it will ask to add passphrase, for blank pass press enter
[Note: if u want to any pass u can do it, so while pushing commits everytime u need to enter ur pass]

## Setting up gerrit account

Open `http://gerrit.pixysos.com` and Sign-up with ur github. After successfull sign-up go to `http://gerrit.pixysos.com/#/settings/contact` and register ur email, and then go to `http://gerrit.pixysos.com/#/settings`, make sure there every info should be filled up there.

```
1. Username       xxxxxx
2. Full Name      xxxxxx
3. Email Address  xxxxxx@xxxx.com
4. Registered     xxxxx xx:xx
5. Account ID     100xxxx
```

## Adding SSH Key to gerrit

Open your terminal and type: 

```bash
cat .ssh/id_rsa.pub
```

Copy the SSH key and paste it on `http://gerrit.pixysos.com/#/settings/ssh-keys` and save it.

## Creating config file

First type:

`$ nano .ssh/config`

And add these details there: 

```    
Host gerrit.pixysos.com
HostName gerrit.pixysos.com
Port 29418
User <YourGerritUsername>
IdentityFile ~/.ssh/id_rsa
```

Save and exit the file after doing so.

## Granting required permissions

Type the following commands:

```bash
$ chmod 600 .ssh/config
$ chmod 700 .ssh/
$ chmod 600 .ssh/id_rsa
```

## Checking everything

To check everything is fine, do a connection Test by typing -> `ssh gerrit.pixysos.com` [Type yes if they ask for it]

```
               Welcome to Gerrit Code Review

Hi <YourGerritUserName>, you have successfully connected over SSH.

Unfortunately, interactive shells are disabled.
To clone a hosted Git repository, use:

git clone ssh://<YourGerritUserName>@gerrit.pixysos.com:29418/REPOSITORY_NAME.git

Connection to gerrit.pixysos.com closed.
```

[If you get the above output message, your gerrit is configured]

## Pushing changes to gerrit

Commit all required changes you need to push and use the below command to add hook to it:

`$ gitdir=$(git rev-parse --git-dir); scp -p -P 29418 <YourGerritUsername>@gerrit.pixysos.com:hooks/commit-msg ${gitdir}/hooks/`

Then use the following command to push:

`$ git push ssh://<YourGerritUserName>@gerrit.pixysos.com:29418/PixysOS/<REPOSITORY_NAME> HEAD:refs/for/<branchname>`
