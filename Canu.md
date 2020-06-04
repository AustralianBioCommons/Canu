
## Description

Introduction of tool, including:

- Its input(s)/output(s)
- List of available shell commands (useful when building interfaces/wrappers for containers)

Or:

- Link(s) to the available information / documentation

Canu assemblys are long and computational time consuming involving
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

### Inputs(s)

### Parameter(s)

### Output(s)

## Diagram

Logical visual description of processing steps for tool, e.g. for pipelines shipped as packages (Falcon, Cellranger, ..)

## Version notes

Any comment on major features being introduced, or default/API changes that might result in unexpected behaviours.

### Containerised version(s)

Which public registry (Biocontainers, Docker, ..)

Any notes on tagging conventions

## Third party tools links

## Tutorials

## Tool infrastructure requirements

### Scheduler

### Hardware

## Tool Install

## *Compute infrastructure name* optimisation (**LINK**)

## Help / FAQ / Troubleshooting

## Licence

## Acknowledgements / citations / credits
