## Documentation of Project1-Linux Basics/ Fundamentals for a suuceesful DevOps Engineer


[Linux Basic Commands](https://www.guru99.com/linux-commands-cheat-sheet.html#1)

`Samples`

![linux commands](./images/Linux%20commands-01.png)

Aux Project 1 (Shell Scripting)
In this project, we will need to onboard 20 new Linux users onto a server. Create a shell script that reads a csv file that contains the first name of the users to be onboarded.

Create the project folder called Shell

/home/ubuntu/projects/Shell

Create a csv file name - names.csv

Touch names.csv

Output:

2

Next step:Open the names.csv file and insert some random names

Testing with 2 users first:

step2

Next, I created the onboarding_users script file and gave it the appropriate permissions:

Touch onboarding_users

Chmod +x onboarding_users

script file created

Below is the script content:

#!/bin/bash

Automating the creation of new users on linux server
#setting of csv_file

CSV_FILE=names.csv

GROUP_NAME=developers

#setting name of ssh directory for skel

SSH_SKEL=/etc/skel/.ssh

variable for public key
AUTH=authorized_keys

password variable
PASSWORD=password

check if group exists if not create group
if [ $(getent group developers) ]; then echo group already exists else sudo groupadd $GROUP_NAME echo group successfuly created fi

add ssh folder to skel directory
if [ -d "$SSH_SKEL" ]; then echo "$SSH_SKEL already exists" else sudo mkdir -p $SSH_SKEL sudo bash -c "cat $AUTH >> $SSH_SKEL/authorized_keys" fi

create each user on the server
while IFS= read USERNAME do

     # check if theusername already exists
     
     if [ $(getent passwd $USERNAME) ];
     
     then
         echo $USERNAME already exists
    else
        sudo useradd -m -G $GROUP_NAME -s /bin/bash $USERNAME
        
        sudo echo -e "$PASSWORD\n$PASSWORD" | sudo passwd "${USERNAME}"
        
        sudo passwd -x 5 ${USERNAME}
        
        sudo chmod 700 /home/$USERNAME/.ssh
        
        sudo chmod 644 /home/$USERNAME/.ssh/authorized_keys
        
        echo "$USERNAME successfully created"
    fi
done < $CSV_FILE
exit 0

script

Next Step- I ran the the script and it was successful. However, I forgot to take a screen shot. But below is the re-confirmation when I ran the script again:

TodayUserScript

Checked all users are in the home directory:

user in Home Directory 8

Checked users are in the developers group:

All users added to Group 7

Tested with user MARYBETH and hurray!! She is able to connect to the server.

Output:

User MaryBteth logged in
