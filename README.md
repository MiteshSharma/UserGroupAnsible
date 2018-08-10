# Users, Groups handling using Ansible

When you are adminstrator of a linux machine that has multiple users, you need to have more control on their creation, deletion, permissions etc. 
Giving user access and revoking access on a new employee joining or old employee leaving you company should be simple. 
I have used ansible to acheive this task of creating user, giving ssh access, adding this user to different groups and deleting user when not needed.

File strucutre: 

playbook.yml: Main playbook which has all needed tasks to create groups, users and delete users

data.yml: This file has all needed data which are used in our playbook

Example

    all_groups:
      - name: 'develper'
        gid: 1001
      - name: 'admin'
        gid: 1000
    create_per_user_group: true
    all_users:
      - userName: 'userNameToLogin'
        uid: 4000
        name: 'User Name'
        create_home_dir: yes
        keyFilePath: 
          - public_ssh_key_path_of_user
          - another_public_ssh_key_path_of_user_if_present
    delete_users:
      - userName: 'userNameToDelete'
      
all_groups: This section has all groups which we need to create with their gids. In this example we are creating two groups: 'developer' with gid 1001 and 'admin' with gid 1000.

create_per_user_group: This property is used to create per user group with same name

all_users: This section has all users with their userName (used to login using ssh) with uid. keyFilePath is ssh public key file path, must be provided for user we want to create.

delete_users: This section has all users with their userName whom we want to delete from servers.

inventory.yml: This file contains static ip address list of all servers. We have currently kept it to be 127.0.0.1 but you can update it with your own server ip address.

Instance from where we are running this ansible playbook need to have ssh access to remote server provided in inventory file.

Ansible command: 

    ansible-playbook -i inventory playbook.yml -u userNameToAccessRemoteServer -K
    
userNameToAccessRemoteServer: user name to access remote server. This is ansible user.

You can add -vvv for verbose mode which help debug.

Instructions to run this code:

Step 1: Clone this code using git

Step 2: Give ssh access to your controller server (from where we are running this) to all remote servers where we want to run this code.

Step 3: Install ansible

Step 4: Run ansible-playbook command and check debug logs

Things to confirm:

1. Your controller system has ssh access to all remote instances
2. Check if data.yml file has correct information
3. Check if you have provided remote server ip addresses in inventoy file.
