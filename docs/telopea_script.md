```
ONTREADS=/scratch/vw17/sc4781/Telopea-Aug19/data/2020-01-15.TspeNanoporeTrimmed/tspe-porechop-filt.fastq
OUTDIR=/scratch/vw17/sc4781/Telopea-Aug19/assembly/2020-05-22.TspeCanu/

/scratch/wz54/software/canu/Linux-amd64/bin/canu -p TSPE -d $OUTDIR genomeSize=0.89g -nanopore-raw $ONTREADS \
'corMhapOptions=--threshold 0.8 --ordered-sketch-size 1000 --ordered-kmer-size 14' correctedErrorRate=0.105 \
useGrid=true gridEngine=pbspro \
gridEngineSubmitCommand="/scratch/wz54/software/jobwrapper/jobwrapper.sh -j oe" \
gridEngineResourceOption="-lncpus=THREADS,mem=MEMORY" \
gridEngineNameToJobIDCommand="qstat -f | grep -F -B 1 WAIT_TAG | grep Id: | cut -c 9-" \
gridEngineStageOption="-ljobfs=DISK_SPACEGB" \
gridEngineArrayMaxJobs=500 \
gridOptionsExecutive="-lwalltime=4:00:00" \
gridOptions="-q normal -lwd -P wz54 -lstorage=scratch/vw17+scratch/wz54" \
stageDirectory=\$PBS_JOBFS
```
