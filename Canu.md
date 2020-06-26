# [Canu](https://github.com/marbl/canu)

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


### Link(s)

- [Canu GitHub](https://github.com/marbl/canu)
- [Canu readthedocs](https://canu.readthedocs.io/en/latest/)
- [Celera assembler (not maintained)](http://wgs-assembler.sourceforge.net/wiki/index.php?title=Main_Page)

### Inputs(s)

- [Canu (the command)](https://canu.readthedocs.io/en/latest/tutorial.html#canu-the-command)
- [Input sequences can be FASTA or FASTQ format](https://canu.readthedocs.io/en/latest/quick-start.html)
- [PacBio CLR or Nanopore](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-pacbio-clr-or-nanopore-data)
- [PacBio HiFi with HiCanu](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-pacbio-hifi-with-hicanu)
- [Multiple sequencing technologies & files](https://canu.readthedocs.io/en/latest/quick-start.html#assembling-with-multiple-technologies-and-multiple-files)

### [Parameter(s)](https://canu.readthedocs.io/en/latest/parameter-reference.html)

### [Output(s)](https://canu.readthedocs.io/en/latest/tutorial.html#outputs)

## [Diagram](https://canu.readthedocs.io/en/latest/pipeline.html)

## [Version notes](https://github.com/marbl/canu/releases)

Any comment on major features being introduced, or default/API changes that might result in unexpected behaviours.

### [Containerised version(s)](https://quay.io/repository/biocontainers/canu)

Biocontainers.

Any notes on tagging conventions?

## Third party tools links

## [Tutorial(s)](https://canu.readthedocs.io/en/latest/tutorial.html)

## Tool infrastructure requirements

### Scheduler

- [Grid engine support](https://canu.readthedocs.io/en/latest/parameter-reference.html#grid-engine-support)
- [FAQ: How do I run Canu on my SLURM / SGE / PBS / LSF / Torque system?](https://canu.readthedocs.io/en/latest/faq.html#how-do-i-run-canu-on-my-slurm-sge-pbs-lsf-torque-system)

### Hardware

- [FAQ question about assembly resources](https://canu.readthedocs.io/en/latest/faq.html#what-resources-does-canu-require-for-a-bacterial-genome-assembly-a-mammalian-assembly)

## [Tool Install](https://github.com/marbl/canu#install)

## [*National Computational Infrastructure* optimisation](NCI_optimisation.md)

## Help / FAQ / Troubleshooting

- [Canu FAQ (readthedocs)](https://canu.readthedocs.io/en/latest/faq.html)
- [BioStars Canu search](https://www.biostars.org/local/search/page/?q=canu)
- [Long read assembly workshop (Melbourne Bioinformatics)](https://www.melbournebioinformatics.org.au/tutorials/tutorials/pacbio/)

## Licence(s)

- [GPL](https://github.com/marbl/canu/blob/master/README.license.GPL)
- [Other](https://github.com/marbl/canu/blob/master/README.licenses)

## Acknowledgements / citations / credits

- [Citation (README)](https://github.com/marbl/canu#citation)
- [Canu History (readthedocs)](https://canu.readthedocs.io/en/latest/history.html)
