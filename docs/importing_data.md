## Importing Data from Other Systems or the Web

This tutorial previously covered [managing data](managing_data.md) between a local machine and the CyVerse central data store (data.iplantcollaborative.org).
It also may be useful to import data from other storage systems, or even from the web.
The `files-import` command can be used for that purpose.
First, use `systems-list` to discover publicly available storage systems:

```systems-list -PS```
```
us-west-1.data.iplantcollaborative.org
data.tacc.utexas.edu
austin.data.iplantcollaborative.org
ncbi-sra-ftp
ncbi-refseq
ncbi-genbank
ncbi-genbank-genomes
**ncbi-1000genomes**
s3-demo-03.iplantc.org
rodeo.storage.demo
data.iplantcollaborative.org
```

The `-P` flag lists public systems only, and the `-S` flag lists storage systems only.
One of the available storage systems is called `ncbi-1000genomes`.
Find out more about that systems by listing it verbosely:

```systems-list -v ncbi-1000genomes```

A subset of the output includes:

```
{
    "description": "NCBI 1000 Genomes data catalog", 
    "id": "ncbi-1000genomes", 
    "lastModified": "2016-01-10T16:21:24.000-06:00", 
    "name": "NCBI 1000 Genomes FTP", 
    "public": true, 
    "revision": 2, 
    "site": "ncbi.nih.gov", 
    "status": "UP", 
    "storage": {
        "auth": {
            "type": "ANONYMOUS"
        }, 
        "homeDir": "/", 
        "host": "ftp.ncbi.nih.gov", 
        "mirror": false, 
        "port": 21, 
        "protocol": "FTP", 
        "proxy": null, 
        "publicAppsDir": null, 
        "rootDir": "/1000genomes/ftp/"
    }, 
    "type": "STORAGE", 
    "uuid": "7184991354511430117-e0bd34dffff8de6-0001-006"
}
```

This storage system is registered to the `ftp.ncbi.nih.gov` host.
Currently, your environment may be configured so that `data.iplantcollaborative.org` is your default storage system.
It is still possible to list files that are available on other storage systems as long the identity of the system is explicitly stated.
For example:

```files-list -S ncbi-1000genomes```


```files-import ```

[Back to: README](../README.md) | [Next: Sharing Data with Other Users](sharing_data.md)
