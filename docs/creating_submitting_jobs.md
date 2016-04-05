## Creating and Submitting a Job

Continuing from the previous example of Clustalw, we know that the only input required is a single fasta file:

```"(non-aligned) FASTA sequences to be aligned"```

An example, non-aligned FASTA file, courtesy of the [ClustalW2 developers](http://www.ebi.ac.uk/Tools/msa/clustalw2/help/faq.html#11),  has been included with this repository.
Upload that sample file to the CyVerse data store by first navigating to the `using-agave/` directory, then by issuing:

```
cd /path/to/using-agave
files-upload -F src/sequence12.fasta username/
files-list -L username/
```

(Please replace `username` with your CyVerse username).
The only other thing needed is a job script that describes the application you intend to run, the system on which the job will be executed, the location of the input data, and any other necessary parameters.
The `jobs-template` command is used to help assemble a job script specific to an application:

```jobs-template -A Clustalw-2.1.0u2 >> Clustalw-job.json```

The `-A` flag indicates to use all fields in the json job template file.
Open up the resulting file, `Clustalw-job.json`, in a Linux text editor such as VIM.

```vim Clustalw-job.json```

```
{
  "name":"Clustalw test-1459832693",
  "appId": "Clustalw-2.1.0u2",
  "batchQueue": "normal",
  "executionSystem": "stampede.tacc.utexas.edu",
  "maxRunTime": "23:56:00",
  "memoryPerNode": "32GB",
  "nodeCount": 1,
  "processorsPerNode": 16,
  "archive": true,
  "archiveSystem": "data.iplantcollaborative.org",
  "archivePath": null,
  "inputs": {
    "inputFasta": ""
  },
  "parameters": {
    "format": "CLUSTAL",
    "T": "DNA",
    "outname": "clustalw2.aln",
    "arguments": ""
  },
  "notifications": [
    {
      "url":"http://requestbin.agaveapi.co/1i1th851?job_id=${JOB_ID}&status=${JOB_STATUS}",
      "event":"*",
      "persistent":true
    },
    {
      "url":"username@tacc.utexas.edu",
      "event":"FINISHED",
      "persistent":false
    },
    {
      "url":"username@tacc.utexas.edu",
      "event":"FAILED",
      "persistent":false
    }
  ]
}
```

The important part to edit here is lines 13-15, which point to the location of the fasta file.
Use the `agave://` prefix to give a full description of the location of your staged data:

```
  "inputs": {
    "inputFasta": "agave://data.iplantcollaborative.org/username/sequence12.fasta"
  },
```

(Please replace `username` with your CyVerse username). Finally, submit the job by issuing:

```jobs-submit -F Clustalw-job.json```

Hopefully you will see a success message upon submission.
This indicates that the application, data, and any other instructions you provide have been bundled and sent to the execution system for processing.
You can monitor the progress of the jobs-list command, with or without the job id:

```
jobs-list
jobs-list -V 658585977227923941-242ac114-0001-007
```

Once complete, you can see what output is available, then download it to your local system:

```
jobs-output-list -L 658585977227923941-242ac114-0001-007
jobs-output-get -r 658585977227923941-242ac114-0001-007 
```

[Back to: README](../README.md)
