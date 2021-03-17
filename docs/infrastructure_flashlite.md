Canu on FlashLite @ UQ-RCC
================

# Accessing tool/workflow

The tool is available after logging into Flashlite with

    ssh <USERNAME>@flashlite.rcc.uq.edu.au

Canu itself is available as a module, which can be loaded with, for example

    module load canu/2.1

This is done within the job submission script.


# Quickstart tutorial

## Steps to download the Canu-relevant Waratah data from Bioplatforms Australia

Use the *Bulkdownload* option to download the Waratah ONT (Oxford
Nanopore Technology) PromethION dataset downloader (within a zip file).

There are 2 datasets available for the Waratah data *GAP ONT PromethION,
Reference genome, 79639*:

1.  <https://data.bioplatforms.com/dataset/bpa-gap-ont-promethion-81651>
    downloads: `bpa-gap-ont-promethion-81651_20201126T2339.zip`
2.  <https://data.bioplatforms.com/dataset/bpa-gap-ont-promethion-81993>
    downloads: `bpa-gap-ont-promethion-81993_20201126T2340.zip`

**Note**: From this point on, it is assumed that there are symbolic
links on the Canu user’s home directory on Flashlite from `~/90days` to
`/90days/<USERNAME>` and `~/30days` to `/30days/<USERNAME>`, where
`<USERNAME>` is the username of the person running Canu.

On **Flashlite**, unzip the downloader files for Waratah (the *81993*
set was used, see above) into `~/90days/`.

    unzip -d ~/90days/tmp/ ~/tmp/bpa-gap-ont-promethion-81993_20201126T2340.zip

Then edit the downloader so that is only downloads the `fastq` *pass*
files **NOT** the `fail` files, plots or (**huge**) `fast5` files.

1.  edit `tmp/*_urls.txt` removing everything except
    `<URL>/79639_PAE71863_GAP_AGRF_ONTPromethION_fastq_pass.tar`
2.  edit `tmp/*_md5sum.txt` removing everything except
    `<checksum> 79639_PAE71863_GAP_AGRF_ONTPromethION_fastq_pass.tar`

To run the bulk downloader

1.  Obtain the API key from
    <https://data.bioplatforms.com/user/USERNAME>, where `USERNAME` is
    your BPA username.
2.  Set the API key, with `export CKAN_API_KEY=<secret>`
3.  Ensure curl version is 7.58 or later `curl –version`
4.  And run the download script `./download.sh`

The downloaded file
`79639_PAE71863_GAP_AGRF_ONTPromethION_fastq_pass.tar` should be 21 GB.

## Steps to Prepare the data for use with Canu

1.  Extract the contents of
    `79639_PAE71863_GAP_AGRF_ONTPromethION_fastq_pass.tar`. This
    contains 18 `fastq.tar` files, each of which contains
    `20200325_GAP_AGRF_PAE71863/20200325_GAP_AGRF_PAE71863/79639_GAP_AGRF_PAE71863_fastq_pass/`
    `PAE71863_pass_subset_00\<NUMBER>.fastq/`
    `PAE71863_pass_8bb8d7e2_<NUMBER>.fastq.gz`
2.  Create an interactive job to extract and concatenate the fastq files
    within the tar files. For example (substitute `qris-jcu` with your
    accounting group) `qsub -I -A qris-jcu -l select=1:ncpus=2:mem=8g -l walltime=6:00:00`
3.  Extract contents of tar files: `for i in *.fastq.tar; do echo "extracting $i"; tar -xvf $i; done`
    and move these into the top level directory: `for d in *; do mv $d/* .; rmdir $d; done`
4.  Create a single fastq file from the numerically ordered files with:
    `cat $(ls -1v) > ../PAE71863_pass_8bb8d7e2.fastq.gz` This creates a
    `fastq.gz` file of size 21 GB (the same size as the original tar
    file).
5.  Move `fastq.gz` file to `~/90days/canu` and rename to
    `waratah_bpa-gap-ont-promethion-79639_PAE71863_pass.fastq.gz`.

## Example Canu Submission Script

A PBS job submission script was created for Canu to run on the Waratah
data using `waratah_bpa-gap-ont-promethion-79639_PAE71863_pass.fastq.gz`
(generated in previous step).

Note that in the following submission script, a genome size of **750
Mb** was used based on a search on [NCBI
genbank](https://www.ncbi.nlm.nih.gov/genome) for related species. The
correct genome size of **0.89 Gb** is given by
<https://github.com/AustralianBioCommons/Canu/blob/master/Canu.md#high-level-resource-usage>

Additionally, disk space saving options were used. This was most likely
unnecessary, and came from an earlier version of the submission script
developed for another data set, where disk space issues had been
reported.

The `flow_cell_type` is PromethION (R9.4.1).

The accounting string passed to PBS with `-A <accounting-group>` should
be set accoring to the insitutional membership of the person submitting
the job.

## `canu2_nogrid.sub`

    #!/bin/bash
    #PBS -l walltime=250:00:00
    #PBS -N canu2.1_nogrid_waratah_PromethION.pass
    ##PBS -j oe
    #PBS -A qris-jcu
    #PBS -l select=1:ncpus=24:mem=480G
    
    
    ##### Define General Settings
    canu_version=2.1
    
    # define where the raw data is stored and where canu will run from
    storage_dir=/90days/${USER}/canu
    run_dir=/30days/${USER}/canu
    
    # stageDirectory is fast local storage for the correction stage
    stage_dir="\\\$TMPDIR" # TMPDIR is job-specific local disk - on dereferencing this will be issued by canu as stageDirectory=\$TMPDIR
    
    ##### Define Genomic Sequence Data Settings
    
    ### Waratah (Telopea speciosissima)
    ### GAP PromethION, Reference genome, 79638
    ### https://data.bioplatforms.com/dataset/bpa-gap-ont-promethion-81993
    
    raw_sample="waratah_bpa-gap-ont-promethion-79639_PAE71863_pass"
    raw_suffix=".fastq.gz"
    
    raw_type="nanopore"
    assembly_prefix="waratah"
    geneSize="750m"
    
    canu_params="correctedErrorRate=0.105"
    
    # params note:
    # - geneSize 750m
    #   this was an estimate - should have used 890m as per following
    #   ref: https://github.com/AustralianBioCommons/Canu/blob/master/Canu.md#high-level-resource-usage
    # - flow_cell_type: PromethION (R9.4)
    # - correctedErrorRate=0.105
    #   recommended parameters for Nanopore flip-flop R9.4 or R10.3
    
    ###
    
    # define the assembly dir from data specifics defined above
    assembly_dir="${assembly_prefix}-${raw_type}"
    
    
    ##### Stage-In Section
    
    # stage the data
    
    [ -d ${run_dir} ] || mkdir -p ${run_dir}
    if [ ! -f ${run_dir}/${raw_sample}${raw_suffix} ]; then
        echo "staging ${raw_sample}${raw_suffix} from ${storage_dir} to ${run_dir} ..."
        cp ${storage_dir}/${raw_sample}${raw_suffix} ${run_dir}
    else
        echo "data found at ${run_dir}/${raw_sample}${raw_suffix}${compressed_suffix}"
    fi
    
    
    ##### Assemble raw data
    
    module load canu/${canu_version}
    
    cd ${run_dir}
    echo running Canu from: $PWD
    
    canu -p ${assembly_prefix} -d ${assembly_dir} genomeSize=${geneSize} -${raw_type}-raw ${raw_sample}${raw_suffix} useGrid=false stageDirectory=${stage_dir} ${canu_params} purgeOverlaps="aggressive" corMhapFilterThreshold=0.0000000002 corMhapOptions="--threshold 0.80 --num-hashes 512 --num-min-matches 3 --ordered-sketch-size 1000 --ordered-kmer-size 14 --min-olap-length 2000 --repeat-idf-scale 50" mhapMemory=60g mhapBlockSize=500 ovlMerDistinct=0.975 || exit 1
    
    # note: Disk Space reduction parameters used:
    #       purgeOverlaps="aggressive" corMhapFilterThreshold=0.0000000002 corMhapOptions="--threshold 0.80 --num-hashes 512 --num-min-matches 3 --ordered-sketch-size 1000 --ordered-kmer-size 14 --min-olap-length 2000 --repeat-idf-scale 50" mhapMemory=60g mhapBlockSize=500 ovlMerDistinct=0.975

## Submitting the Canu Job

Submit the Canu job to the PBS cluster with

    qsub canu2_nogrid.sub

and check that it’s queued with

    qstat -a

## Monitoring Canu’s Progress

Monitor running jobs with:

    watch -n 60 'qstat -a'

Watch Canu progress with:

    tail -F ${run_dir}/${assembly_dir}/${assembly_prefix}.report

or, to observe current Canu stage

    tail ${run_dir}/${assembly_dir}/canu.out

and to see what the current stage is

    ls -l ${run_dir}/${assembly_dir}/canu-scripts

## On Canu Completion

On completion, results will be located in `$PWD/${assembly_dir}`. These
can be copied back to the storage area with:

    cp -a ${run_dir}/${assembly_dir}" ${storage_dir}/

then cleanup run directory with:

    rm -rf ${run_dir}/*

# Optimisation required

Running Canu with its standard Grid option is a trial and error process
to get past each processing stage due to Canu’s highly dataset dependent
job resource requirements for each of these stages.

On Flashlite it is possible to take advantage of large memory nodes and
run Canu as a single node job, thus avoiding the issue outlined above.
This can be done by submitting a PBS job that requests all of the
resources of a single worker node, and within this job, running Canu
with the `useGrid=false` option.

# Infrastructure usage and benchmarking

## Summary

## Exemplar 1: e.g. Waratah (*Telopea speciosissima*) reference genome assembly

The approach described above in the optimisation section was used for
the Waratah (*Telopea speciosissima*) data set obtained from the
Bioplatforms Australia data portal.

Canu ran to completion in a single run with this approach, using a
single node with 24 cores, and requesting 480 GB of RAM. This took 150
hours to complete and used approximately 265 GB of RAM, and 3077:58:33
of CPU time (approximately 128 hours per core).

# Acknowledgements / citations / credits

FlashLite was supported by ARC LIEF grant LE140100061, The University of Queensland, Queensland University of Technology, Griffith University, Monash University, University of Technology Sydney, and The Queensland Cyber Infrastructure Foundation.

