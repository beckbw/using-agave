## Sharing Data with Other Users

Using Agave storage systems, such as the CyVerse central data store (data.iplantcollaborative.org), it is possible to share data with others users.
As a data sharing demonstration, we will use the `seqence12.fasta` file uploaded previously in this tutorial.
If you do not have that file, either [upload it now](managing_data.md), or work with a file of your own.
To list the permissions on an existing file on the remote storage system, issue:

```files-pems-list username/sequence12.fasta```

The output should look similar to:

```
dooley READ WRITE EXECUTE 
ipcservices READ WRITE 
rodsadmin READ WRITE 
username READ WRITE EXECUTE 
```

Where `username` refers to your CyVerse username.
The Agave CLI has a nice set of tools that can be used to find other users.
View your own user profile by issuing:

```profiles-list -v me```

You can search for your colleagues either by name (`-N`), e-mail address (`-E`), or username (`-U`), provided they have credentials with the same tenant:

```
profiles-list -v -N "My Collaborator"
profiles-list -v -E "my_collaborator@agave.com"
profiles-list -v -U my_collaborator
```

Once the username of the target person is known, permissions can be updated by performing the following:

```
files-pems-update -U my_collaborator -P ALL username/sequence12.fasta
files-pems-list username/sequence12.fasta
```
```
dooley READ WRITE EXECUTE 
ipcservices READ WRITE 
rodsadmin READ WRITE 
my_collaborator READ WRITE EXECUTE
username READ WRITE EXECUTE 
```

Now, a user with CyVerse username `my_collaborator` has permissions to access the file.
Valid values for setting permission with the `-P` flag are READ, WRITE, EXECUTE, READ_WRITE, READ_EXECUTE, WRITE_EXECUTE, ALL, and NONE.
This same action can be performed recursively on directories using the `-R` flag.

[Back to: README](../README.md) | [Next: Automating Tasks](automating_tasks.md)
