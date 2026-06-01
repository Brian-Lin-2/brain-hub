# User

A user in Linux is just an identity that defines who is interacting with the system, what files they own, and what actions they are allowed to perform

## Username vs UID (User ID)

Every username is mapped to a unique UID. Linux only cares about numbers, so while the username is meant for you the UID is what Linux actually uses to distinguish who you are

`0` - This is always reserved for the root user
`1-999` - This is reserved for System Users (background services and applications). For security these users are given a non-interactive shell `/sbin/nologin` to ensure that if the application gets compromised, the hacker can't manipulate anything
`1000+` - This is assigned to Human Users. Their starting location is always at `/home/username`. This is where they have full control of the filesystem and can do whatever they want (assuming they have full permissions). An interactive shell is also automatically launched when the user logs in to give them an easy way to manipulate this starting location

> All users are tracked at `/etc/passwd` which is a giant file containing all users within the Linux machine

## Primary Group (GID)

Every user must belong to at least one primary group. This allows admins to easily grant the same set of permissions to multiple users at once
