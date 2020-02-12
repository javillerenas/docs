# How to Setup Multiple SSH Accoutns For CodeCommit
> Feb 12 2020

## Why?
Imagine you're a developer working on multiple teams, each with a different AWS account.
Each team provides you with a __user__ within their accounts.
You need to setup SSH credentials to __CodeCommit__ to work with both teams' repos.

## Solution
It's all about the `~/.ssh/config` file. It should look like this:
```
Host <NAME_FOR_CREDS_1>
   Hostname git-codecommit.us-east-1.amazonaws.com
   User <SSH_ID_1>
   IdentityFile ~/.ssh/<NAME_FOR_SSH_ID_1>

Host <NAME_FOR_CREDS_2>
   Hostname git-codecommit.us-east-1.amazonaws.com
   User <SSH_ID_2>
   IdentityFile ~/.ssh/<NAME_FOR_SSH_ID_2>
```

Where:
* `NAME_FOR_CREDS_X` is a random name to refer to those credentials
* `SSH_ID_1` is the `SSH ID` given to you when you add an SSH public key to the AWS IAM User.
* `NAME_FOR_SSH_ID_1` is the name for your PRIVATE SSH key


## Cloning A Repo
Now, when you clone the repo, you must clone it in the following way
```
git clone ssh://<NAME_FOR_CREDS_1>/v1/repos/my-repo
```

## Steps
We will assume you already have an IAM user.
1. Create ssh credentials
You will need to issue the `ssh-keygen` command and follow the instructions (leave everything default EXCEPT for the name of the file)
The name you give to the file will become the `NAME_FOR_SSH_ID_X` shown above.

2. Copy the Public Key
Issue the following:
```
pbcopy < ~/.ssh/<NAME_FOR_YOUR_SSH_ID>.pub
```

3. Add the Public Key to your IAM User.
When you do so, make sure you COPY the `SSH_KEY_ID` since it will be the value for the `User` key above.

4. Git the `SSH_KEY_ID` and the `NAME_FOR_SSH_ID_X` open `~/.ssh/config` and setup as shown above.

5. Repeat for the other account.

# Additional Information
* https://gist.github.com/justinpawela/3a7056cd592d688425e59de2ef6f1da0
* https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html