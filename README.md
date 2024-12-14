In this repo, I would like to mark the steps I take to obtain bedgraph files from inputting a bam file in modkit. 
This is the command line to run cpg
modkit pileup -t 50 --ignore h --cpg --ref /private/groups/migalab/dan/reference/hg002v1.0.1.fasta --combine-strands --mod-threshold m:0.80 --only-tabs 06_11_24_R1041_UL_DiMeLo_CENPAold_1_5mC_6mA_winnowmap_sorted.bam 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up.bed

This is the command line to run mA
modkit pileup -t 30 --mod-threshold m:0.95 --ignore h --motif A 0 --ref /pr
ivate/groups/migalab/dan/reference/hg002v1.0.1.fasta --only-tabs --log-filepath mA_modkit.log --force-allow-implicit 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD.bam  01_09_24_R1041_DiMeLoAdaptive_CENPA_5mC_6mA_pileup.bed

I am only keeping the first, second, third, and 11th column in the bed file output 
awk '{print $1, $2, $3, $11}' 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up.bed > 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up.bedgraph

Trading  spaces with tabs 
tr ' ' '\t' < 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up.bedgraph > 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up_tap.bedgraph

We need to further shrink the bed file. I am removing any line in the second column with a number larging than 6605244, which is furthest CDR boundary
awk -F '\t' '$2 > 6605244' 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up_tab.bedgraph > 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_cpg_pile_up_filtered.bedgraph

We need to further shrink the bed file. I am removing any line in the second column with a number larging than 6605244, which is furthest CDR boundary
awk -F '\t' '$3 < 128587150' 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_pile_up_filtered_tab.bedgraph > 06_11_24_R1041_UL_DiMeLo_CENPAyoung_1_5mA_6mC_winnowmap_MD_pile_up_filtered_filtered.bedgraph
