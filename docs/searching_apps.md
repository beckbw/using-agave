## Searching for an Application

The CyVerse catalog has hundreds of public applications that are available to run right now.
Each public app is already registered with an execution system, so there is no need to go through the process of creating your own execution system.
(Please see [this tutorial](https://github.com/iPlantCollaborativeOpenSource/cyverse-sdk) if you are interested in creating your own personal copies of apps and execution systems.)

At this stage, presumably you have already formulated a scientific question independent of CyVerse and Agave.
Now, you are looking for an application in the CyVerse library that can be used to address that question.
Perhaps, for example, you are looking for a tool to manipulate fastq files:

apps-search name.like=*fast*

FaST-LMM-hpc-2.07u1
FasttreeDispatcher-2.1.4u2
dnasubway-fastx-stampede-0.0.13.2.1u1
FasttreeDispatcher-2.1.4u1
fast-lmm-stampede-2.07u2
fastStructure-0.0.1u2
dnasubway-fastqc-stampede-0.11.2.1u1
dnasubway-fastx-lonestar-0.0.13.2.0u3
dnasubway-fastqc-lonestar-0.11.2.0u2
dnasubway-fastx-lonestar-0.0.13.2.0u1
dnasubway-fastqc-lonestar-0.11.2.0u1


apps-list -v dnasubway-fastqc-stampede-0.11.2.1u1


[Back to: README](../README.md) | [Next: Creating and Submitting a Job](creating_submitting_jobs.md)
