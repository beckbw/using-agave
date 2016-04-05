## Running the Drug Discovery Portal from the Command Line

DrugDiscovery@TACC is a web portal that allows users to upload a protein file and perform a virtual screen with pre-defined ZINC libraries.
Access is controlled through a web login with [TACC credentials](https://portal.tacc.utexas.edu/).
Behind the scenes, the portal stages the relevant data on a TACC storage system, assembles a json-format job file, executes the job, and returns the results using the Agave platform.
More information can be found on [DrugDiscovery@TACC website](https://drugdiscovery.tacc.utexas.edu/).

The web portal provides a nice graphic interface and facilitates the Agave calls.
A user could, however, also run the portal directly from the command line with the Agave CLI.
The applicationss, storage systems, and execution systems behind the portal are not registered with the CyVerse tenant (iplantc.org).
They are registered with the TACC tenant (tacc.prod).

The first step is to swith the current tenant for the TACC tenant, preserving the old configuration:

```tenants-init -b -t tacc.prod```

The `-b` flag will backup the current configuration in the `~/.agave/` directory on your local filesystem. It is always possible to switch back to the previous configuration by using the `-s` flag:

```tenants-init -s```

Now that you are configured to interact with the TACC tenant, make sure your [TACC credentials](https://portal.tacc.utexas.edu/) are ready, then go through the process of registering a client and authentication tokens as described [earlier](initializing.md).

Identify the correct public application by issuing one of the following two commands:

```
apps-list -P
apps-search name.like=*vina*
```

The most recent revision of the [AutoDock Vina](http://vina.scripps.edu/) application is called `vina-lonestar-1.1.2u4`.
FInd out more information about the application with:

```apps-list -v vina-lonestar-1.1.2u4```

A close read of this information indicates that, as input, a `proteinFile` is required in format `pdb` or `pdbqt`.
The user can also provide `ligandFiles`, but they are not required: `"required": false`.
Many parameters are required for this application, including `sizeX`, `sizeY`, `sizeZ`, `centerX`, `centerY`, `centerZ`, `paramFile` (optional), and`ligandIndices` (optional). To begin to organize all this data into a json-format job script, use the `jobs-template` command:

```jobs-template -N DDP_Job_from_CLI -A vina-lonestar-1.1.2u4 >> vina-job.json```

The resulting `vina-job.json` file should look like:




[Back to: README](../README.md) | [Next: Assembly and Genotyping with DNA Subway](dna_subway.md)
