


When you execute the “useradd” command to add a new user on the server, in the background, there are several files updated and created. This post lists out the exact steps carried out in the background when you execute the below command.

# useradd gregory

1. A new line for user john is created in /etc/passwd.

The new line in /etc/passwd file will look similar to the line shown below.

john:x:1001:1001::/home/john:/bin/bash

The new line in /etc/passwd file has the following fields:

    It begins with the username john.
    There is an x for the password field indicating that the system is using shadow passwords.
    A UID at or above 1000 is created. (Under Red Hat Enterprise Linux, UIDs and GIDs below 1000 are reserved for system use.)
    A GID at or above 1000 is created.
    You can have the comment for the user in the next field. For this example it is kept blank.
    The home directory for john is set to /home/john.
    The default shell is set to /bin/bash.

2. A new line for john is created in /etc/shadow.

Below is a sample line from /etc/shadow.

john:!!:17530:0:99999:7:::

The line created in /etc/group has the following characteristics:

    It begins with the group name john. The default grou name is same as the username is you do not provide the username explicitly.
    Two exclamation points (!!) appear in the password field of the /etc/shadow file, which locks the account.
    The number of days (since January 1, 1970) since the password was last changed i.e. 17530 days
    The number of days before password may be changed (0 indicates it may be changed at any time)
    The number of days after which password must be changed (99999 indicates user can keep his or her password unchanged for many, many years)
    The number of days to warn user of an expiring password (7 for a full week)
    The number of days after password expires that account is disabled.
    The number of days since January 1, 1970 that an account has been disabled.
    A reserved field for possible future use.

3. A new line for a group named john is created in /etc/group.

A group with the same name as a user is called a user private group. When you do not provide a group name explicitly a private group name with the same name as that of the username is created. The fields in /etc/group file are as shown below.

john:x:1001:john

    It begins with the group name john.
    The x in the next field indicates that the tgroup password is encrypted. If this field is empty, no password is needed.
    The next field is the numeric group ID.
    The last field is a list of the usernames that are members of this group, separated by commas.

4. A new line for a group named juan is created in /etc/gshadow.

/etc/gshadow contains the shadowed information for group accounts. The line created in the /etc/gshadow file has the following characteristics:

john:!::john

    The first field starts with the group name which john in our case.
    An exclamation point appears in the password field of the /etc/gshadow file, which locks the group.
    Comma-separated list of administrator user names. These users can access the group without being prompted for a password.
    Comma-separated list of member user names. Members can access the group without being prompted for a password.

5. User’s home directory is created and files under /etc/skel directory are copied.

A directory for user john is created in the /home directory. This directory is owned by user john and group john. However, it has read, write, and execute privileges only for the user john. All other permissions are denied.

The files within the /etc/skel directory (which contain default user settings) are copied into the new /home/john directory.

Here is a summary flowchart of the steps carried out in the background when a user executes the “useradd” command on a Linux server.

useradd command background steps performed on the server

