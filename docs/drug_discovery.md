## Running the Drug Discovery Portal from the Command Line

DrugDiscovery@TACC is a web portal that allows users to upload a protein file and perform a virtual screen with pre-defined ZINC libraries.
Access is controlled through a web login with [TACC credentials](https://portal.tacc.utexas.edu/).
Behind the scenes, the portal stages the relevant data on a TACC storage system, assembles a json-format job file, executes the job, and returns the results all using the Agave platform.
More information can be found on [DrugDiscovery@TACC website](https://drugdiscovery.tacc.utexas.edu/).

The web portal provides a nice graphical interface and facilitates the Agave calls.
A user could, however, also run the virtual screen directly from the command line with the Agave CLI.
It is important to know that the applications, storage systems, and execution systems behind the portal are not registered with the CyVerse tenant (**iplantc.org**).
They are registered with the TACC tenant (**tacc.prod**).

The first step is to switch the current tenant for the TACC tenant, preserving the old configuration:

```tenants-init -b -t tacc.prod```

The `-b` flag will backup the current configuration in the `~/.agave/` directory on your local filesystem.
It is always possible to switch back to the previous configuration by using the `-s` flag:

```tenants-init -s```

Now that you are configured to interact with the TACC tenant, make sure your [TACC credentials](https://portal.tacc.utexas.edu/) are ready, then go through the process of registering a client and authentication tokens as described [here](initializing.md).

Identify the correct public application by issuing one of the following two commands:

```
apps-list -P
apps-search name.like=*vina*
```

The most recent revision of the [AutoDock Vina](http://vina.scripps.edu/) application is called `vina-lonestar-1.1.2u4`.
FInd out more information about the application with:

```apps-list -v vina-lonestar-1.1.2u4```

A close read of the result indicates that, as input, a `proteinFile` is required in format `pdb` or `pdbqt`.
The user can also provide `ligandFiles`, but they are not required (`"required": false`).
Many parameters are required for this application, including `sizeX`, `sizeY`, `sizeZ`, `centerX`, `centerY`, `centerZ`, `paramFile` (optional), and `ligandIndices` (optional). To begin to organize all this data into a json-format job script, use the `jobs-template` command:

```jobs-template -N DDP_Job_from_CLI -A vina-lonestar-1.1.2u4 >> vina-job.json```

The resulting `vina-job.json` file should look like:

```
{
  "name":"DDP_Job_from_CLI",
  "appId": "vina-lonestar-1.1.2u4",
  "batchQueue": "",
  "executionSystem": "docking.exec.lonestar",
  "maxRunTime": "01:00:00",
  "memoryPerNode": "1GB",
  "nodeCount": 1,
  "processorsPerNode": 1,
  "archive": true,
  "archiveSystem": "docking.storage",
  "archivePath": null,
  "inputs": {
    "proteinFile": "/work/02875/docking/apps/vina/1.1.2/lonestar/test/2FOM.pdbqt",
    "ligandFiles": [ 
      "/work/02875/docking/apps/vina/1.1.2/lonestar/test/ZINC00000567.pdbqt",
      "/work/02875/docking/apps/vina/1.1.2/lonestar/test/ZINC00000707.pdbqt"
    ]
  },
  "parameters": {
    "sizeY": 22,
    "ligandIndices": -1,
    "centerZ": -4.0899999999999999,
    "sizeZ": 22,
    "centerX": -0.081000000000000003,
    "sizeX": 22,
    "paramFile": "/scratch/01114/jfonner/DockingPortal/TestSet/paramlist",
    "centerY": 2.2130000000000001
  },
  "notifications": [
    {
      "url":"http://requestbin.agaveapi.co/1gcbadn1?job_id=${JOB_ID}&status=${JOB_STATUS}",
      "event":"*",
      "persistent":true
    },
    {
      "url":"username@email.edu",
      "event":"FINISHED",
      "persistent":false
    },
    {
      "url":"username@email.edu",
      "event":"FAILED",
      "persistent":false
    }
  ]
}

```

There are several terms in this job file that need to be understood and modified.
First, notice that the `executionSystem` is called `docking.exec.lonestar`.
This likely indicates that the job will be run on the Lonestar supercomputer.
(This can quickly be confirmed with `systems-list -v docking.exec.lonestar`.)
Why is this important?
Because the input `proteinFile` and `ligandFiles` are pointing to absolute paths _on the execution machine_.

There are two options to proceed.
First, if you have access to Lonestar, you could upload a `proteinFile` to your $HOME or $WORK space, then provide in this job script the complete path to that file.
Second, you could upload a `proteinFile` to any Agave storage system, and provide the complete Agave URI to that file.
In this tutorial, we will choose the second method.

At this point, you may not have access to a storage system on the **tacc.prod** tenant.
In order to upload a `proteinFile`, we have to quickly switch back to the **iplantc.org** tenant.
A protein file is provided with this tutorial, located in this git bundle at `~using-agave/src/protein.pdbqt`.
Issue the following commands:

```
tenants-init -s					# switched to iplantc.org tenant
auth-check
auth-tokens-refresh				# if necessary
files-upload -F src/protein.pdbqt username/
files-list -L					# confirm the file is there
tenants-init -s					# switch back to tacc.prod tenant
auth-check
```

Now in the `vina-job.json` file, modify the `proteinFile` input to:

```"proteinFile": "agave://data.iplantcollaborative.org/username/protein.pdbqt"```

Delete the two lines that point to ligands, but don't delete the `ligandFiles` key itself:

```"ligandFiles": []```

Now, modify the box size and center parameters as follows:

```
sizeX, sizeY, sizeZ, centerX, centerY, centerZ
```

And finally, use the following paramlist that points to a specific ligand library:

```
Is there any way for the user to know what the paramlist options are?
```

#### Submit the job

#### Download the results
[Back to: README](../README.md) | [Next: Assembly and Genotyping with DNA Subway](dna_subway.md)
