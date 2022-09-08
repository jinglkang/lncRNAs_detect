# Long non-coding RNAs detection
Use **lncRNAs_detection** after mapping to reference genome (with the bam files)   
- [Requirements](https://github.com/jinglkang/lncRNAs_detect#Requirements)
- [Input files](https://github.com/jinglkang/lncRNAs_detect#input-files)
- [Output files](https://github.com/jinglkang/lncRNAs_detect#input-files)
-------------------------------
## Requirements
The following software must be installed on your machine:   
- **gffread**: extract the nucleotide sequences of cds for each transcript   
- **StringTie**: obtain the merged gtf with transcripts in all individuals   
- **FEELnc**: detect lncRNAs, alignment-free   
- **CPAT**: detect lncRNAs, alignment-free   
- **CPC2**: detect lncRNAs, alignment-dependent   
- **prebuilt_model**: download zebrafish_Hexamer.tsv and Zebrafish_logitModel.RData from https://sourceforge.net/projects/rna-cpat/files/v1.2.2/prebuilt_model/ for CPAT   
scripts in bin/ should also be put in the current working directory    
-------------------------------
## Input files
Basically, **lncRNAs_detection** should have the following minimal input files:   
1. reference genome fasta file   
2. gtf file for reference genome   
3. the directory of bam file   
The number of thread to use: 20 for the default, or use "--cpu" to set the number    
if the reference genome fasta and gtf are not in the current directory, add the path before the two files   
```bash
chmod +x lncRNAs_detection
./lncRNAs_detection --genome ./genome.fasta --gtf ./genome.gtf --bamdir ./ --cpu 20
```
-------------------------------
## Output
Four directory will be in the current directory in the order of runing lncRNAs_detection:    
### In the current working directory   
**./**:   
lncRNAs_detection.sh: the commands during the running of lncRNAs_detection    
Common_lncRNA_genes.txt: lncRNA genes whose all transcripts were deemed as non-coding by at least two of FEElnc, CPAT, CPC   
common_lncRNA_.gtf: gtf file contain lncRNA genes whose all transcripts were deemed as non-coding by at least two of FEElnc, CPAT, CPC    
merged_cds.fa: cds nucleotide sequences of all transcripts in the merged gtf resulted from stringtie    

### 1_stringtie
**./1_stringtie/**:   
gtf per individual, which contain the assembled transcripts;    
**merged.gtf**: contain the assembled transcripts of all individuals   
### 2_feelnc
**./2_feelnc**:   
1_candidate_lncRNA.gtf: the retained transcripts after FEELnc_filter.pl   
**./2_feelnc/codpot/**:   
the candidate lncRNAs after FEELnc_codpot.pl   
2_codpot_RF_summary.txt: here can obtain the cutoff of coding potential score (CPS) for the lncRNAs candidates   
2_codpot_RF.txt: CPS of transcripts in 1_candidate_lncRNA.gtf   
FEELnc_all_trans_nc.txt: lncRNA genes whose all transcripts are deemed as non-coding   
### 3_CPAT
**./3_CPAT**:   
CPAT_cps.txt: CPS of all transcripts in merged gtf file    
CPAT_all_trans_nc.txt: lncRNA genes whose all transcripts are deemed as non-coding by **CPAT**   
### 4_CPC
**./4_CPC**:   
CPC_cps.txt: CPS of all transcripts in merged gtf file    
CPC_all_trans_nc.txt: lncRNA genes whose all transcripts are deemed as non-coding by **CPC**   
### 5_classify (by FEELnc_classifier.pl)
classify new lncRNAs with reference to to the localisation and the direction of transcription of proximal RNA transcripts
**./5_classify**:   
common_lncRNA_classify.txt: the classify of lncRNA genes detected by at least two of FEElnc, CPAT, CPC   
