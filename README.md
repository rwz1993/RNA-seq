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
 

II. Salmon - Quantification of aligned RNA-seq reads from STAR 


Salmon has two modes: 1) Quasi-mapping-based mode, where it aligns reads to an indexed genome similar to STAR followed by quantification, and 2) alignment-based mode, where previously aligned reads with a different program (such as STAR) are then quantified against a user defined list of cDNA transcripts. We are using the alignment-based mode for our purposes. 

   1. Quantifying previously aligned reads using alignment-based mode 
   
      salmon quant -p 8 -t Mus_musculus.GRCm38.cdna.all.fa -l A -a mouse_colon/colon_wtAligned.sortedByCoord.out.bam 
      -o colon_wt_quant
   
      #-t specifies the transcript cDNA we are quantifying against 
      #-l A specifies the library type (paired, unpaired, etc). The option A allows Salmon to peak inside the bam files and
      decide for itself what library type it is and proceed accordingly. 
      #-a specifies the bam file to be quantified. This is the output of STAR from the previous part 
   
      #The cDNA transcript reference was downloaded from here: 
   
      ftp://ftp.sanger.ac.uk/pub/gencode/Gencode_mouse/release_M11/gencode.vM11.transcripts.fa.gz
   
      Here we got the quant.sf file as an output of Salmon that was very short. 
   
