## *National Computational Infrastructure* optimisation for **[Canu](Canu.md)**

The workflow management system of canu relies on the use of array jobs
to process sub-tasks within each stage of processing. To reduce the
load on the PBSPro scheduler and allow canu to run effectively at NCI,
a script has been created that consolidates these array tasks into a
single batch job. This 'jobwrapper' script replaces the `qsub` command
and works by translating the canu resource request into a new batch
queue script that is submitted to the queue. At present, the *normal*
queue on gadi is targeted; some tasks that require higher amounts of
memory are automatically sent to the *hugemem* queue. The table at the
bottom of this document gives an idea of how the jobwrapper worked on
a test assembly.

To use the jobwrapper script instead of the qsub command the following
option needs to be set when canu is started:

    gridEngineSubmitCommand="jobwrapper.sh -j oe"

Additional options of relevance to running at NCI are:

- `gridEngine=pbspro`;
- `gridEngineResourceOption="-lncpus=THREADS,mem=MEMORY"` formats
  resource requests correctly for NCI job submission;
- `stageDirectory=\$PBS_JOBFS` tells canu the correct location for
  on-node scrath space;
- `gridEngineStageOption="-ljobfs=DISKSPACEGB"` formats requests for
  on-node scratch space correctly;
- `gridEngineArrayMaxJobs=500` indicates the maximum number of tasks
  in array jobs;
- `gridOptionsExecutive="-lwalltime=4:00:00"` indicates the duration
  of time for the management tasks that canu runs to manage the
  assembly process;
- `gridOptions="-q normal -lwd -P <projectid>
  -lstorage=<storage-options>"` indicates the NCI projectid and any
  storage options that should be used (see
  [here](https://opus.nci.org.au/display/Help/Gadi+Frequently+Asked+Questions#GadiFrequentlyAskedQuestions-Iaddedthenew`-lstorage=`linetomyjobsubmissionscript.Whyitdoesn'twork?)).

### Exemplar: e.g. *Acacia pycnantha Benth. (golden wattle)* reference genome assembly

### Tutorial specific to infrastructure

### How to access *Compute infrastructure name* 

Includes straightforward signing in to *Compute infrastructure name*

### Accessing canu

Canu (version 2.0) has been installed as a system module and can be
accessed with the command:

    module load canu/2.0

Included in the system module is the jobwrapper script described
above. Canu uses the minimap2 assembler which has also been installed
as a system module and is available through

    module load minimap2/2.17

### Test existing or new installation

### Optimisation required at infrastructure

### Infrastructure usage metrics

### Freeform section

