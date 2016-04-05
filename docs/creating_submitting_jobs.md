## Creating and Submitting a Job

Continuing from the previous example of Clustalw, we know that the input that is required is a single input fasta file:

```"(non-aligned) FASTA sequences to be aligned"```

A sample non-aligned FASTA file has been included with this repository.
Upload that sample file to the CyVerse data store by first navigating to the `using-agave/` directory, then by issuing:

```
cd /path/to/using-agave
files-upload -F src/sequence12.fasta wallen/
files-list -L wallen/
```

This example fasta file is courtesy of the ClustalW2 developers, and can be found [here](http://www.ebi.ac.uk/Tools/msa/clustalw2/help/faq.html#11).

The `jobs-template` command is used to help assemble a job script:

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

The important line to edit here is the line pointing to the location of the fasta file.
Use the `agave://` prefix to give a full description of the location of your staged data:

```
  "inputs": {
    "inputFasta": "agave://data.iplantcollaborative.org/wallen/sequence12.fasta"
  },
```

Finally, submit the job by issuing:

```jobs-submit -F Clustalw-job.json```

You can monitor the progress of the jobs-list command:

```jobs-list ```

Once complete, get the output:

```jobs-output-list ```
```jobs-output-get ```

[Back to: README](../README.md)
