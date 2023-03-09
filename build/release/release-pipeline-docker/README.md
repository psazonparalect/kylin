## Background

These scripts and docker image are used to provide an **easy and standard way** for release manager to complete [apache release process](https://www.apache.org/legal/release-policy.html) .

Some source code are copied from [apache spark release guide](https://github.com/apache/spark/tree/master/dev/create-release).

## How to release

### What you need to prepare

| Item                                                                     | Used for                                                                   | Reference                                                                                             |
|--------------------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Apache Account<br/>(Should belongs to PMC member) <br> (id and password) | 1. Write access to ASF's Gitbox service and SVN service <br> 2. Send email | https://id.apache.org                                                                                 |
| GPG Key <br> (key files and GPG_PASSPHRASE)                              | Sign your released files(binary and compressed source files)               | https://infra.apache.org/release-signing.html <br> https://infra.apache.org/release-distribution.html |
| Laptop which installed Docker                                            | The place you run release scripts                                          | N/A                                                                                                   |

### Step 1 : Configure Basic Info and Copy GPG Private Key

-  Start docker container

```bash
# you may use custom name other than 'rm-xxyu'
docker run --name rm-xxyu -i -t apachekylin/release-machine:5.0  bash
# docker ps -f name=rm-xxyu
```

- Copy GPG Private Key from your laptop into container

```bash
# ~/xxyu-release-manager.private.key is your private key
docker cp ~/xxyu-release-manager.private.key rm-xxyu:/root
```

### Step 2 : Configure setenv.sh

- Set correct values for all variables in `/root/scripts/setenv.sh`, such as **ASF_PASSWORD** and **GPG_PASSPHRASE**.

#### Variables in setenv.sh

| Name            | Comment                                                                                                                                                                          |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ASF_USERNAME    | ID of Apache Account                                                                                                                                                             |
| ASF_PASSWORD    | (**Never leak this**)Password of Apache Account                                                                                                                                  |
| GPG_PASSPHRASE  | (**Never leak this**)PASSPHRASE of GPG Key                                                                                                                                       |
| GIT_BRANCH      | Branch which you used to release, default is **kylin5**                                                                                                                          |
| RELEASE_VERSION | Which version you want to release, default is **kylin5.0.0-alpha**                                                                                                               |
| NEXT_VERSION    | Next version you want to use after released, default is **kylin5.0.0-beta**                                                                                                      |
| RELEASE_STEP    | (default is **publish-rc**)<br/>Legal values are <br/> publish-rc : upload binary to release candidate folder <br> publish : publish release binary officially after vote passed |


- After you set correct value in `/root/scripts/setenv.sh`, yu should active these variables.

```bash
source scripts/setenv.sh
```

### Step 3 : Install GPG Private Key

```bash
gpg --import xxyu-release-manager.private.key
```

```bash
# you may use the name in private key
gpg --list-sigs Xiaoxiang Yu
```

### Step 4 : Publish Release Candidate

```bash
export RELEASE_STEP=publish-rc
bash release-publish.sh
```

### Step 5 : Vote for Release Candidate

- Prepare vote template for voting

### Step 6 : Publish Release Candidate

```bash
export RELEASE_STEP=publish
bash release-publish.sh
```

- Prepare vote template for announcement
- Close maven repository

### Step 7 : Remove Docker container

```bash
docker rm rm-xxyu
```