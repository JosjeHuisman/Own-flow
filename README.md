# Own-flow
# Do the fastq for all the files 
jmahuisman@ares:~/new_workflow$ fastqc -o results/1_initial_qc --noextract input/*.fastq.gz
TRIMMER="ILLUMINACLIP:${adapter_fasta}:2:30:10  LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15  MINLEN:36"

FILE_PREFIXES=$(basename -a -s _R1_001.fastq.gz fastq/*_R1_001.fastq.gz)

for FP in $FILE_PREFIXES; do
  INFILES=$(echo input/${FP}_{R1,R2}_001.fastq.gz)
  OUTFILES=$(echo results/2_trimmed_output/${FP}_{R1,R2}{,_unpaired}.fastq.gz)
  LOG=results/logs/trimmomatic/$FP.log

  # Create the necessary directories for output files and log files
  mkdir -p $(dirname $LOG)
  mkdir -p results/2_trimmed_output

  # Debug: Print the command to be run
  echo "Running: trimmomatic PE $INFILES $OUTFILES $TRIMMER > $LOG 2>&1"

  # Run trimmomatic
  trimmomatic PE $INFILES $OUTFILES $TRIMMER > $LOG 2>&1
done 


