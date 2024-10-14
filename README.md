# Own-flow
# Do the fastq for all the files 
jmahuisman@ares:~/new_workflow$ fastqc -o results/1_initial_qc --noextract input/*.fastq.gz


FILE_PREFIXES=$(basename -a -s _R1_001.fastq.gz input/*_R1_001.fastq.gz)

adapter_fasta=~/miniconda/share/trimmomatic-0.39-2/adapters/TruSeq3-PE-2.fa
TRIMMER="ILLUMINACLIP:${adapter_fasta}:2:30:10  LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15  MINLEN:36"

for FP in $FILE_PREFIXES; do   echo "Processing file prefix: $FP";    INFILES="input/${FP}_R1_001.fastq.gz input/${FP}_R2_001.fastq.gz";   echo "Input files: $INFILES";    OUTFILES="results/2_trimmed_output/${FP}_R1.fastq.gz results/2_trimmed_output/${FP}_R2.fastq.gz results/2_trimmed_output/${FP}_R1_unpaired.fastq.gz results/2_trimmed_output/${FP}_R2_unpaired.fastq.gz";   echo "Output files: $OUTFILES";    LOG="results/logs/trimmomatic/$FP.log";   echo "Log file: $LOG";    echo "Running Trimmomatic...";   trimmomatic PE -phred33 $INFILES $OUTFILES ILLUMINACLIP:$adapter_fasta:2:30:10 SLIDINGWINDOW:4:20 MINLEN:36 > $LOG 2>&1;  done
