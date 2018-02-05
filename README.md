# RNA-seq
Pipeline for analysis of RNA-seq data 


I. STAR – RNA-seq read alignment 


   1. Generating and indexing a genome

      STAR --runThreadN 8 --runMode genomeGenerate --genomeDir /mbio1/royce/STAR-master/genomes/ 
      --genomeFastaFiles /mbio1/royce/STAR-master/GRCm38.primary_assembly.genome.fa --sjdbGTFfile 
      /mbio1/royce/STAR-master/annotations/gencode.vM11.primary_assembly.annotation.gtf

      #Here we indexed an mm10 genome to use for read alignment using the primary assembly and the annotations listed below
      
      #--genomeDir specifies where the index to be written 
      #--genomeFastaFiles specifies the primary assembly to be indexed 
      #--sjdbGTFfile specifies the mm10 annotations to be used in the indexing 

      #mm10 primary assembly used: 
      ftp://ftp.sanger.ac.uk/pub/gencode/Gencode_mouse/release_M11/GRCm38.primary_assembly.genome.fa.gz

      #mm10 annotation used: 
      ftp://ftp.sanger.ac.uk/pub/gencode/Gencode_mouse/release_M11/gencode.vM11.primary_assembly.annotation.gtf.gz

   2. Aligning to the indexed genome 
   
      STAR --runThreadN 8 --genomeDir /mbio1/royce/STAR-master/genomes/ --readFilesIn DRR091671_1.fastq DRR091671_2.fastq 
      --sjdbGTFfile /mbio1/royce/STAR-master/annotations/gencode.vM11.primary_assembly.annotation.gtf 
      --twopassMode Basic --outFileNamePrefix colon_wt --outSAMtype BAM SortedByCoordinate
      
      #--genomeDir specifies where the index is located 
      #--readFilesIn specifies the fastq files to be aligned. In this case it is two paired-end files 
      #--sjdbGTFfile specifies the mm10 annotations
      #--twopassMode Basic specifies the use of two-pass mapping to better map reads around splice junctions 
      #--outSAMtype BAM SortedByCoordinate specifies the output file as a sorted BAM 
 
