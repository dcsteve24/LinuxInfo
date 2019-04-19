# Cloning Down a Git Repo

1. Install Git Onto the Machine:
  - **yum install git**
  - **apt install git**

2. Change directories to where you'll want the git repos.

3. Clone the repo. Repo paths can be found on the main page of the repo -- green button that says "clone or download":
  - **git clone /copied/repo/path**

## Ansible Sync
For ansible plays/roles you can easily sync the downloaded clone with your normal operating destination. I keep mine in the defaults so my commands will reflect such.
  - **cd /path/to/cloned/repo/**
  - **rsync -va --delete-after roles/* /etc/ansible/roles/**
  - **rsync -va playbooks/* /etc/ansible/playbooks**
