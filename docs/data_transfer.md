## Uploading and Downloading Data

Before running an application, it is important to understand how data flows in and out of CyVerse with the Agave CLI.
As a CyVerse user, you are able to store data in the central data store called **data.iplantcollaborative.org**.
To see more information, use the following command:

```systems-list -V data.iplantcollaborative.org```

A "system" in Agave is a server or collection of servers associated with a single hostname.
As you may see in the output from the above command, the iPlant data store is a cloud-based repository from which data is accessed by all of iPlant's (CyVerse's) technologies._
To make this your default data repository when using the Agave CLI, issue the command:

```systems-setdefault data.iplantcollaborative.org```

Now, when using the Agave CLI "files" commands, you will automatically be configured to interact with this data storage system.
With the Agave CLI "files" commands, you can list available files and directories, upload or download data, copy, move, or delete data, import data from another source, and change permissions on existing data.
To see a Linux-style long listing of what is currently available in your path issue:

```files-list -L username/```


Where `username` is replaced with your CyVerse username.
If this is your first time interacting with the iPlant data store, then your home directory may still be empty.
You can create a new folder, then list the files in the folder by typing:

```
files-mkdir -N new-folder username/
files-list -L username/
files-list -L username/new-folder/
```

Create an example file on your local machine, and upload it to the data store:

```
touch new-file.txt
echo "some example text" >> new-file.txt
files-upload -F new-file.txt username/
files-list -L username/
```

Many common file-related commands have an analogous command in the Agave CLI:

```
files-copy -D wallen/new-file-copy.txt wallen/new-file.txt
files-move -D wallen/new-folder/new-file-copy.txt wallen/new-file-copy.txt
files-list -L username/
files-list -L username/new-folder/
```

Files and folders can be deleted, but be cautious because there is no way to recover:

```
files-delete wallen/new-folder/new-file-copy.txt
files-delete wallen/new-folder/
files-list -L wallen/
```

First delete the local copy of `new-file.txt`, then download the remote copy with the `files-get` command:

```
rm new-file.txt
files-get username/new-file.txt
cat new-file.txt
```

Finally, it may be useful to import data to your home directory on the iPlant data store from another data store.
For example, one of the other public data stores is the 1000 genome project:

```
files-import
```

This concludes an overview of how to put data on the data storage system. At any time, you can issue an Agave command with the `-h` flag to find more information on the function and usage of the command.

[Back to: README](../README.md) | [Next: Searching for an Application](searching_apps.md)
