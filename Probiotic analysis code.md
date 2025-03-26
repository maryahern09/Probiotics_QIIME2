Activate qiime2 in powershell   
wsl \-d Ubuntu   
source \~/miniconda3/bin/activate   
conda activate qiime2-amplicon-2024.10   
   
import   
qiime tools import **\\**   
  \--type 'SampleData\[PairedEndSequencesWithQuality\]' **\\**   
  \--input-path probiotic\_analysis **\\**   
  \--input-format CasavaOneEightSingleLanePerSampleDirFmt **\\**   
  \--output-path demux-paired-end.qza   
 

view   
qiime demux summarize **\\**   
  \--i-data demux-paired-end.qza **\\**   
  \--o-visualization demux.qzv   
\#denoise trun around 120   
 

denoise   
qiime dada2 denoise-single **\\**   
  \--i-demultiplexed-seqs demux-paired-end.qza **\\**   
  \--p-trim-left 0 **\\**   
  \--p-trunc-len 120 **\\**   
  \--o-representative-sequences rep-seqs-dada2.qza **\\**   
  \--o-table table-dada2.qza **\\**   
  \--o-denoising-stats stats-dada2.qza   
 

qiime metadata tabulate **\\**   
  \--m-input-file stats-dada2.qza **\\**   
  \--o-visualization stats-dada2.qzv   
 

\#phylogenetic tree   
qiime phylogeny align-to-tree-mafft-fasttree \\   
  \--i-sequences rep-seqs-dada2.qza \\   
  \--o-alignment alignedfastq.qza \\   
  \--o-masked-alignment maskedfastq.qza \\   
  \--o-tree unrooted-tree.qza \\   
  \--o-rooted-tree rooted-tree.qza   
 

qiime feature-classifier classify-sklearn **\\**   
  \--i-classifier gg-13-8-99-515-806-nb-classifier.qza **\\**   
  \--i-reads rep-seqs-dada2.qza  **\\**   
  \--o-classification taxonomy.qza   
 

qiime metadata tabulate **\\**   
  \--m-input-file taxonomy.qza **\\**   
  \--o-visualization taxonomy.qzv   
 

qiime taxa barplot **\\**   
  \--i-table table-dada2.qza **\\**   
  \--i-taxonomy taxonomy.qza **\\**   
  \--o-visualization taxa-bar-plots.qzv   
