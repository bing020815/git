# GIT - SSH Keys
SSH key feature is used as the access permission linked to the certain Github Account. In that case, there is no need to type in the username and password every time you push to or pull the repository from Github. It is a convenient and secure way to access your account. Each computer need a SSH key in order to link the Github Account.


## Table of contents:  
* [Create an SSH Key](#create-an-ssh-key)
* [Link SSH Key](#link-ssh-key) 

<br>

## Create an SSH Key

Generate ssh key with an email:
```
$ ssh-keygen -t rsa -b 4096 -C "youremail@email.domain"
```

* Press `Enter` to accept the default file location
* Enter a secure passphrase and Press `Enter`

Display your public key
```
$ cat .ssh/id_rsa.pub
```

Now, copy the ssh key. It will be used in the next step.

## Link SSH Key

By having the public key generated from your computer, follow the steps below to link your computer to the Github account:

1. Go to [Github](https://github.com/) and login your account.

2. Go to `Settings` and select `SSH and GPG keys`

3. Click on `New SSH key`

4. Enter a title in the field and paste the key content in the `Key` field

5. Then, click on `Add SSH key`. 

6. You are done !
<br>

### Reference:  
* [How to Add SSH Keys to Your GitHub Account](https://www.inmotionhosting.com/support/website/ssh/how-to-add-ssh-keys-to-your-github-account/)  
