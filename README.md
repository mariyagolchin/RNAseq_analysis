# RNAseq_analysis

[Reference: sanbomics.com](https://www.youtube.com/watch?v=9D1dToIQqls&amp;list=PLi1VnGoeDGjvHvl83QySD2oAQYFHPRYso&amp;index=2)

=========================================================================

# screen Commands
```bash
-create new screen:
screen -S new_screen_name

-check screen list
screen -ls

-check spesific screen(attach):
screen -r screen_id

-close screen(detach):
ctrl+A ctrl+D

-finish screen after finising run:
ctrl+D or type: exit

-quit screen completely
screen -X -S 3207503.run_star quit
```
<br>
==================================================================
1-download file: 
- wget http://ftp.ensembl.org/pub/release-105/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz
<br>
- wget https://ftp.ensembl.org/pub/release-111/gtf/homo_sapiens/Homo_sapiens.GRCh38.111.gtf.gz
<br>
-download fastq file from: https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA720431&o=acc_s%3Aa
![image](https://github.com/mariyagolchin/RNAseq_analysis/assets/20231107/2e9b4c9e-cf38-4bb8-a286-bc198d0f4173)
<br>
2- move all 8 fastq to server:
<br>
scp D:/PHD/1_Thesis/5_Run_Server_Analysis/0_RNA-Seq_Analysis_Runinggggg/fastq/*.gz golchinpour@172.18.57.208:/home/golchinpour/projects/RNAseq-Analysis/fastq
<br>
3-gunzip *.fastq.gz
<br>
4- Reference genome and annotation with star:
STAR --runMode genomeGenerate --genomeDir ref/ --genomeFastaFiles Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa --sjdbGTFfile Homo_sapiens.GRCh38.105.gtf--runThreadN 16
<br>
5-
#!/bin/bash
<br>
# Directory containing the FASTQ files
fastq_dir="fastq"
<br>
# Output directory for STAR output
output_dir="mapp"
<br>
# Genome directory
genome_dir="ref"
<br>
# Iterate over each FASTQ file in the fastq directory
for file in "${fastq_dir}"/*.fastq; do
    # Extract filename without directory and extension
    filename=$(basename "${file}")
    filename_without_extension="${filename%.fastq}"

    # Run STAR command for each FASTQ file
    STAR --runMode alignReads \
         --genomeDir "${genome_dir}" \
         --outSAMtype BAM SortedByCoordinate \
         --readFilesIn "${file}" \
         --runThreadN 12 \
         --outFileNamePrefix "${output_dir}/${filename_without_extension}"
done







