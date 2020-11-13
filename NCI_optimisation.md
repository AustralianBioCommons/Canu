# **[Canu](Canu.md)** on Gadi @ the *National Computational Infrastructure* 

## Accessing canu

Canu (version 1.9) has been installed as a system module and can be
accessed with the command:

    module load canu/1.9

Included in the system module is the jobwrapper script described
below. Canu uses the minimap2 assembler which has also been installed
as a system module and is available through

    module load minimap2/2.17
    
## Quick start tutorial specific to Gadi @ NCI

An example PBSPro job submission script for Canu at NCI is shown below. 

**NOTE** you will need to replace: 

`#PBS -N canu-run` with the name of your run.

`#PBS -P wz54` with your project name. 

`genomeSize=100m` with the genome size (known or predicted) for your organism.

`-nanopore-raw my_raw_nanopore_reads.fasta.gz` with your read data. Note that this option will need to be changed to `-pacbio-raw my_pacbio.fastq.gz` if you are using pacbio data. 

`gridOptions="-q normal -lwd -P wz54 -lstorage=scratch/wz54"` with your NCI project and storage options.

<br/>

    #!/bin/bash
    #PBS -lncpus=1
    #PBS -lmem=4G
    #PBS -lwalltime=24:00:00
    #PBS -lwd
    #PBS -P wz54
    #PBS -N my-canu-run
    #PBS -q normal

    module load canu/1.9

    canu -correct-trim-assemble \
        -p canu_assembly \
        -d canu_assembly \
        genomeSize=100m \
        useGrid=true \
        gridEngineSubmitCommand="${CANU_BASE}/Linux-amd64/bin/jobwrapper.sh -j oe" \
        gridEngine=pbspro \
        gridEngineResourceOption="-lncpus=THREADS,mem=MEMORY" \
        stageDirectory=\$PBS_JOBFS/\$PBS_ARRAY_INDEX \
        gridEngineStageOption="-ljobfs=DISK_SPACEGB" \
        gridEngineArrayMaxJobs=500 \
        gridOptionsExecutive="-lwalltime=4:00:00" \
        gridOptions="-q normal -lwd -P wz54 -lstorage=scratch/wz54" \
        -nanopore-raw my_raw_nanopore_reads.fasta.gz


The `-p` option given to Canu sets the prefix of the assembly
filenames, while the `-d` option indicates the directory in which the
assembly is run.

## Optimisation required

The workflow management system of Canu relies on the use of array jobs
to process sub-tasks within each stage of processing. To reduce the
load on the PBSPro scheduler and allow Canu to run effectively at NCI,
a script has been created that consolidates these array tasks into a
single batch job. This 'jobwrapper' script replaces the `qsub` command
and works by translating the Canu resource request into a new batch
queue script that is submitted to the queue. At present, the *normal*
queue on Gadi is targeted; some tasks that require higher amounts of
memory are automatically sent to the *hugemem* queue. The table at the
bottom of this document gives an idea of how the jobwrapper worked on
a test assembly.

To use the jobwrapper script instead of the qsub command the following
option needs to be set when Canu is started:

    gridEngineSubmitCommand="${CANU_BASE}/Linux-amd64/bin/jobwrapper.sh -j oe"

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
  -lstorage=<storage-options>"` specifies the NCI project and any
  storage options that should be used (see
  [here](https://opus.nci.org.au/display/Help/Gadi+Frequently+Asked+Questions#GadiFrequentlyAskedQuestions-Iaddedthenew`-lstorage=`linetomyjobsubmissionscript.Whyitdoesn'twork?) for information on the storage option).

## Infrastructure usage and benchmarking

You can collect usage metrics from your Canu run using the NCI Gadi optimised workflow using scripts available on the [Sydney Informatics Hub, University of Sydney GitHub repository](https://github.com/Sydney-Informatics-Hub/BioCommons-Canu-Metrics).

### Summary

### Exemplar: **Fat-tailed Dunnart** (*Sminthopsis crassicaudata*)

Available soon!

### Exemplar: **Marsupial Mole** (*Notoryctes typhlops*)

Available soon!

### Exemplar: **Waratah** (*Telopea speciosissima*)

- Waratah assembly used all three phases of the Canu pipeline: 'correct', 'trim', and 'assemble'.
- See script used [here](docs/telopea_script.sh).

|Field|Canu|Flye|Necat|
|-------|:-----:|:-----:|:-----:|
|Total number of sequences| 3,983 | 2,524 | 365 |    
|Total length of sequences (bp)| 981,953,849 | 855,765,535 | 842,143,239 |  
|Min. length of sequences (bp)| 1,138 | 496 | 563 |  
|Max. length of sequences (bp)| 12,971,499 | 12,592,574 | 37,347,493 |    
|Mean length of sequences (bp)| 246,536 | 339,051 | 2,307,242 |  
|Median length of sequences (bp)| 58,209 | 22,950 | 155,267 |
|N50 length of sequences (bp)| 1,848,137 | 2,588,707 | 10,701,597 |
|L50 count of sequences| 132 | 92 | 24 |
|GC content (%)| 39.95 | 40.47 | 40.15 |
|BUSCO (n=1440)| C:78.4%[S:68.6%,D:9.8%], F:5.2%, M:16.4% | C:80.6%[S:74.2%,D:6.4%], F:4.2%, M:15.2% | C:81.2%[S:75.2%,D:6.0%], F:4.3%, M:14.5% |

### Exemplar: **Golden Wattle** (*Acacia pycnantha Benth.*)

The table below indicates the resources used by a test assembly run at
NCI during 2020Q2 using Canu version 1.9 and the jobwrapper script. The
#tasks, cores/task and memory/task fields indicate the resources
requested by Canu and the queue, #cores, and memory fields indicate
the submitted job.

Canu jobs submitted to Gadi for Golden Wattle. **NOTE**: only the 'trim' and 'assemble' phases of the Canu pipeline are included in the metrics below. The first phase, 'correction', was carried out prior to assembly at NCI. 

|directory|task|#tasks|cores/task|memory/task (GB)|queue|#cores|memory (GB)|walltime (hh:mm:ss)|SU|
|---------|----|------|----------|----------------|-----|------|-----------|-------------------|--|
|trimming/0-mercounts  |meryl-count.sh|6|8|18     |normal|48   |192        |0:51:35           |83|
|trimming/0-mercounts  |meryl-process.sh|1|8|32   |normal|8    |32         |0:33:12           |9|
|trimming/1-overlapper |overlap.sh|158|16|16      |normal|480  |1920       |12:28:19          |11973|
|unitigging/0-mercounts|meryl-count.sh|4|8|26     |normal|32   |128        |0:47:30           |51|
|unitigging/0-mercounts|meryl-process.sh|1|8|32   |normal|8    |32         |0:09:35           |3|
|unitigging/1-overlapper|overlap.sh|149|16|16     |normal|480  |1920       |17:49:19          |17109|
|unitigging/canu_assembly.ovlStore|1-bucketize.sh|2|1|4|normal|2|8         |0:02:38           ||
|unitigging/canu_assembly.ovlStore|2-sort.sh|2|1|27|hugemem|2|54           |0:06:34           |1|
|unitigging/3-overlapErrorAdjustment|red.sh|126|8|32|normal|480|1920       |0:30:15           |484|
|unitigging/3-overlapErrorAdjustment|oea.sh|102|1|10|hugemem|96|1020       |1:58:50           |570|
|unitigging/4-unitigger|unitigger.sh|1|16|256     |hugemem|16  |256        |                  ||
|unitigging/5-consensus|consensus.sh|181|8|7      |normal|480  |1920       |0:28:00           |448|
|unitigging/4-unitigger|alignGFA.sh|1|16|32       |normal|16   |32         |00:26:39          |14|

### Infrastructure benchmarking

The table below are assembly metrics for barley leaf rust samples (Plant Breeding Institute, University of Sydney) obtained from Canu and BioCommons Canu runs performed on NCI Gadi. The assembly metrics are similar for both runs. 

|Statistic|Canu|BioCommons Canu|
|---------|----|------|
|Genome Size	|309.91 MB	|301.68 MB|
|Total Contigs	|2133	|1549|
|N/L50	|268/280.00 KB	|139/493.54 KB|
|N/L90	|1302/59.30 KB	|771/72.08 KB|
|Max contig length|	2.38 MB|	3.32 MB|
|% contigs > 50 KB|	93.57%|	94.32%|
|GC Content	|41.64%|	41.57%|
|Complete BUSCOs|	87.50%|	87.10%|
|Complete and single-copy BUSCOs|	12.60%|	9.60%|
|Complete and duplicated BUSCOs|	74.90%|	77.50%|
|Fragmented BUSCOs|	4.90%|	4.80%|
|Missing BUSCOs|	7.60%|	8.10%|

## Acknowledgements / citations / credits

### Genomics for Australian Plants

We would like to acknowledge the contribution of the [Genomics for Australian Plants Framework Initiative consortium](https://www.genomicsforaustralianplants.com/consortium/) in the generation of data used in this workflow. The Initiative is supported by funding from Bioplatforms Australia (enabled by NCRIS), the Ian Potter Foundation, Royal Botanic Gardens Victoria, the Royal Botanic Gardens and Domain Trust, CSIRO, Centre for Australian National Biodiversity Research and the Department of Biodiversity, Conservation and Attractions, Western Australia.
