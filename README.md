# RNAseq_analysis

[Reference: sanbomics.com](https://www.youtube.com/watch?v=9D1dToIQqls&amp;list=PLi1VnGoeDGjvHvl83QySD2oAQYFHPRYso&amp;index=2)


## RNAseq tutorial - part 1 - building STAR genome index

1-download file: 

```bash
wget http://ftp.ensembl.org/pub/release-105/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz
wget https://ftp.ensembl.org/pub/release-111/gtf/homo_sapiens/Homo_sapiens.GRCh38.111.gtf.gz
-download fastq file from:
 https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA720431&o=acc_s%3Aa
```
<br>

2- move all 8 fastq to server:

```bash
scp D:/PHD/1_Thesis/5_Run_Server_Analysis/0_RNA-Seq_Analysis_Runinggggg/fastq/*.gz golchinpour@172.18.57.208:/home/golchinpour/projects/RNAseq-Analysis/fastq
```

<br>
3-gunzip *.fastq.gz

<br>

4- Reference genome and annotation with star:

```bash
STAR --runMode genomeGenerate --genomeDir ref/ --genomeFastaFiles Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa --sjdbGTFfile Homo_sapiens.GRCh38.105.gtf--runThreadN 16
```

<br>

## RNAseq tutorial - part 2 - Aligning reads with STAR

5- run star for all fastq files

```bash
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
```

## RNAseq tutorial - part 3 - generating count table


```bash
~/projects/RNAseq-Analysis$ ls bams/
SRR14168768Aligned.sortedByCoord.out.bam  SRR14168775Aligned.sortedByCoord.out.bam  SRR14168780Aligned.sortedByCoord.out.bam
SRR14168771Aligned.sortedByCoord.out.bam  SRR14168776Aligned.sortedByCoord.out.bam  SRR14168783Aligned.sortedByCoord.out.bam
SRR14168772Aligned.sortedByCoord.out.bam  SRR14168779Aligned.sortedByCoord.out.bam
```

```bash
conda install -c bioconda subread

ls
# bams  fastq  genomeDir  Homo_sapiens.GRCh38.111.gtf  Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa  mapped  ref  run_mapp_for_all_fastq.sh
```

```bash
 featureCounts -a Homo_sapiens.GRCh38.111.gtf -o count.out -T 8 bams/*.bam
```

```bash
 ls
#bams       count.out.summary  genomeDir                    Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa  ref
#count.out  fastq              Homo_sapiens.GRCh38.111.gtf  mapped
```

copy count.out from server to my pc:

```bash
scp golchinpour@172.18.57.208:/home/golchinpour/projects/RNAseq-Analysis/count.out D:\PHD\1_Thesis\5_Run_Server_Analysis\0_RNA-Seq_Analysis_Runinggggg
```

# RNAseq tutorial – part 4 – Differential expression analysis with Deseq2




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






