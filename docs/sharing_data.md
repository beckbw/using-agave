## Sharing Data with Other Users

Using the CyVerse central data store (data.iplantcollaborative.org), it is possible to share data with others users.
To demonstrate sharing data, we will use the `seqence12.fasta` file uploaded previously in this tutorial.
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
To update permissions, perform the following:

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
