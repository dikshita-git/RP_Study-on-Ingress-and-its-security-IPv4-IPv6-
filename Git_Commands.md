K100f51f4ddc4d653d1afb32ee6531ba69267853f7cc811fe302d32011b39e2ccf5::server:6b30b7a5a9a95fb4dd9435e10eac5d2e

Here the SSH keys and GPG keys are established and synchronised between my proxmox server and the github account. The GIT commands used thereby are:

#### 1. git init
#### 2. git config --global user.name "John Doe"
#### 3. git config --global user.email "John Doe@gmail.com"


### GIT PUSH:
  1. Navigate inside the folder (could be the cloned repo too)  which has the new updates to be pushed to GIT 
  2.  git add . -->in case of adding the whole folder or git add <Folder_Name>
  3.  git status ---> to check the updates
  4.  git commit -m "Message"
  5.  git remote set-url origin <SSH_Link_of Git_repo>
  6.  git push
### GIT PULL:
  1. mkdir 
  2. git clone <ssh_repo_link>
 
PS: OpenSSh server could be installed on the machine beforehand because the latest GIT operations hold right only either via SSH or Authentication apps like Twilio Authy etc.
