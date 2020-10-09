# Canu

## [Description](https://canu.readthedocs.io/en/latest/quick-start.html)

Canu assemblies are long and computational time consuming involving
multiple separate sub tasks. Canu has its own method of managing this
long running workflow and has provisions for submitting jobs to
multiple different HPC queue systems. The system is restartable and
able to resume assemblies when errors occur.

Each stage of processing typically consists of submitting one or more
worker scripts along with an executive/management script that executes
after all workers are finished. This management script sets up the
next stage. The jobs submitted may consist of array jobs and canu
makes assumptions about the size of the jobs that can a cluster can
execute based on the total resources available within the cluster. The
assumptions may not be correct in a large, multiuser HPC facility.

- [Canu readthedocs](https://canu.readthedocs.io/en/latest/)
- [Celera assembler (not maintained)](http://wgs-assembler.sourceforge.net/wiki/index.php?title=Main_Page)

| Repository / Registry | Available? |
|-------------|:--------:|
| GitHub | [&#9679;]()| 
| bio.tools | [&#9679;]()|
| BioContainers | [&#9679;](https://quay.io/repository/biocontainers/canu)|
| bioconda | [&#9679;]()|

## Required (minimum) inputs / parameters

## Key link(s) & additional information (e.g. input(s), parameter(s), output(s))

### Inputs(s)

- [Canu (the command)](https://canu.readthedocs.io/en/latest/tutorial.html#canu-the-command)
- [Input sequences can be FASTA or FASTQ format](https://canu.readthedocs.io/en/latest/quick-start.html)
- [PacBio CLR or Nanopore](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-pacbio-clr-or-nanopore-data)
- [PacBio HiFi with HiCanu](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-pacbio-hifi-with-hicanu)
- [Multiple sequencing technologies & files](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-with-multiple-technologies-and-multiple-files)

### [Parameter(s)](https://canu.readthedocs.io/en/latest/parameter-reference.html)

### [Output(s)](https://canu.readthedocs.io/en/latest/tutorial.html#outputs)

# [Diagram](https://canu.readthedocs.io/en/latest/pipeline.html)

# Usage

## Summary

| Version | Infrastructure | Scheduler | Workflow manager | Container | Install method |
|---------------|---------|-----------|------------------|-----------|----------------|
|1.9|Gadi (NCI)|PBSpro - see [optimisations](https://github.com/AustralianBioCommons/Canu/blob/master/NCI_optimisation.md#infrastructure-specific-optimisation)|None|None|Module|
|2.0|Gadi (NCI)|PBSpro - see [optimisations](https://github.com/AustralianBioCommons/Canu/blob/master/NCI_optimisation.md#infrastructure-specific-optimisation)|None|None|Module|

## High level resource usage

|Version|Group|Sample name (e.g. organism name)|Genus species|Genome size (GB)|Hours required|Cores|Peak RAM in GB (requested)|Drive (GB)|HPC-HTC|Month-Year|
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|1.9|OMG|Fat-tailed Dunnart|*Sminthopsis crassicaudata*||||||[Gadi](NCI_optimisation.md)||
|1.9|OMG|Marsupial Mole|*Notoryctes typhlops*||||||[Gadi](NCI_optimisation.md)||
|1.9|GAP|Waratah|*Telopea speciosissima*|0.89|136|4-480 depending on job - total 3732 across jobs|1059 (NA)|max 61.69|[Gadi](NCI_optimisation.md)|06-2020|
|1.9|GAP|Golden Wattle|*Acacia pycnantha Benth.*|1|72| | | |[Gadi](NCI_optimisation.md)|05-2020|

## Additional notes

- [Version notes](https://github.com/marbl/canu/releases)

Any comment on major features being introduced, or default/API changes that might result in unexpected behaviours.

# [Install](https://github.com/marbl/canu#install)

# Tutorial(s)

- [Canu readthedocs Tutorial](https://canu.readthedocs.io/en/latest/tutorial.html)

# Help / FAQ / Troubleshooting

- [Canu FAQ (readthedocs)](https://canu.readthedocs.io/en/latest/faq.html)
- [FAQ question about assembly resources](https://canu.readthedocs.io/en/latest/faq.html#what-resources-does-canu-require-for-a-bacterial-genome-assembly-a-mammalian-assembly)
- [Grid engine support](https://canu.readthedocs.io/en/latest/parameter-reference.html#grid-engine-support)
- [FAQ: How do I run Canu on my SLURM / SGE / PBS / LSF / Torque system?](https://canu.readthedocs.io/en/latest/faq.html#how-do-i-run-canu-on-my-slurm-sge-pbs-lsf-torque-system)
- [BioStars Canu search](https://www.biostars.org/local/search/page/?q=canu)
- [Long read assembly workshop (Melbourne Bioinformatics)](https://www.melbournebioinformatics.org.au/tutorials/tutorials/pacbio/)

## How does the BioCommons optimised Canu compare with Canu?

Canu uses a non-deterministic algorithm, meaning that it is normal to expect slightly different assemblies with multiple runs of the same version of Canu, using the exact same inputs and parameters, as [discussed here on Canu's github page](https://github.com/marbl/canu/issues/1013). 

A comparison of assembly metrics obtained for Canu and BioCommons Canu was performed for a Wheat Stem Rust sample and is available on our [NCI optimisation page](https://github.com/AustralianBioCommons/Canu/blob/master/NCI_optimisation.md)

# Licence(s)

- [GPL](https://github.com/marbl/canu/blob/master/README.license.GPL)
- [Other](https://github.com/marbl/canu/blob/master/README.licenses)

# Acknowledgements / citations / credits

- [Citation (README)](https://github.com/marbl/canu#citation)
- [Canu History (readthedocs)](https://canu.readthedocs.io/en/latest/history.html)
