## RP_Study-on-Ingress-and-its-security-IPv4-IPv6-
This is an academic Research project carried out for intensive learning of Ingress across diffrent platforms and determines the possible measures to secure it both using IPv4 and IPv6.

Here the SSH keys and GPG keys are established and synchronised with my machine. The GIT commands used thereby are:

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

### PORTRAINER: (installed as)
  1. root@ubuntu:/home/dk/Desktop# docker volume create portainer_data
  2. root@ubuntu:/home/dk/Desktop# docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
  
      ***Check http://localhost:9000/#/home***
      
---------------------------------------------------------------------------------------------------------------------------------------------------------------

# <u>My GIT Roadmap:</u>


Here is a roadmap to the files in this repository:
 ## Wiki  : 
 It consists of all the documentations of ideas gathered throughout:
 
1. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Daily_MoM">Weekly meeting table</a>
        
2. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Docker-%7C-Its-Environments">Docker</a>

3. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Traefik</a>
 
 ## Codes  : 
        Folder with all the code files
