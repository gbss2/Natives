==========================================
 #LDGH Project
=========================================

 (/u0254) Copyleft 2015, by LDGH and Contributors.

----------------------------------------
<b>Natives Genetic Diversity Project</b>
----------------------------------------
 GNU GPL 2015, by LDGH and Contributors.

<b>Original Author:</b> Giordano Bruno Soares-Souza

<b>Contributor(s):</b> To be added <...>

<b>Updated by:</b> Giordano Bruno Soares-Souza


<b> Databases: </b>

1) (<b>OPA-N</b>)	OPA - LDGH Natives (HGDP, OPA-Natives, SNP500Cancer-excluded)

2) (<b>OPA-Li</b>)	OPA Innate Immunity, OPA Non-Hodgkin Linfoma, Li et al

3) (<b>MaxPops</b>)	HGDP, HAPMAP, Natives25M

4) (<b>MaxSNPs</b>)	1000Genomes, Natives25M

5) (<b>HAP25N</b>)	HAPMAP, Natives25M

6) (<b>NAT2</b>) Native dataset 2 (to be defined)

<b> Populations: </b>

1) OPA-Natives (Cayapa, San Martin, Quechua, Matsiguenga)

2) Natives25M (Quechua, Aymara, Ashaninka, Shimaa)	/home/ldgh_projects/2.5_natives/data/Inputs_analysis/Peruvians/Shimaa_Ashanincas_Peruvians_comuns.ped

3) SNP500Cancer (CEU, Pacific Rim, Hispanic, Afro-american)

4) OPA Innate Immunity/Non-Hodgkin Linfoma (HGDP-CEPH)

5) HAPMAP

6) HGDP

7) 1000Genomes (1kG) /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_25M/1000Genomes_banco_completo_snpscomuns_25.ped

8) Native dataset INS (Quechua, Aymara, Ashaninka, Shimaa, Chopcas, Matzes, Moche, Nahua, Qeros, Uros)
	
<b> Databases Statiscs </b>

<i>Sample Size</i>

1) <b>OPA-N</b> - 1021

2) <b>OPA-Li</b> - 966

3) <b>MaxPops</b> - 1095

4) <b>MaxSNPs</b> - 1222

5) <b>HAP25N</b> - 1053

6) <b>NAT2</b> - 1683
 	
<i>Number of loci</i>

1) <b>OPA-N</b> - 1247

2) <b>OPA-Li</b> - 647151

3) <b>MaxPops</b> - 358389

4) <b>MaxSNPs</b> - 2000155

5) <b>HAP25N</b> - 966000

6) <b>NAT2</b> - 1921864
 	
<b> Database files <\b>

1) <b>OPA-N</b> - /home/ldgh_projects/genes_natives/data/HGDP_LDGH_EX_NAT_***.sdat.gz 

2) <b>OPA-Li</b> - /home/ldgh_projects/genes_natives/data/HGDP_IINHL600K_XYMfreeEX_NAT_***.sdat.gz

3) <b>MaxPops</b> - /home/ldgh_projects/genes_natives/data/EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_***_comuns.sdat.gz

4) <b>MaxSNPs</b> - /home/ldgh_projects/genes_natives/data/EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita_names_NAT_***.sdat.gz

5) <b>HAP25N</b> - /home/ldgh_projects/genes_natives/data/EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_***_comuns.sdat.gz

6) <b>NAT2</b> - /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc
 	(*** -> EAS, EUR, WAFR)
 	
<b> Results folder </b>

/home/ldgh_projects/genes_natives/results/

<b>Analysis Pipeline</b>

1) Select snps from databases

    awk '{print $2}' /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_Pops/NativosLDGH_HGDP_HAPMAP.map > snsps_HGDP_HAPMAP_maxPops.txt
    
    awk '{print $2}' /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_SNPs/Nativos_Hapmap_Quechuas.map > snps_HAPMAP_maxSnps2.txt

    awk '{print $2}' /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_25M/1000Genomes_banco_completo_snpscomuns_25.map > /home/giordano/lista_snps_1000G
    sed -i '1iSNPs' /home/giordano/lista_snps_1000G

    awk '{print $2}' /home/ldgh_projects/2.5_natives/data/Inputs_analysis/Peruvians/Shimaa_Ashanincas_Peruvians_comuns.map > /home/giordano/lista_snps_NATIVOS
    sed -i '1iSNPs' /home/giordano/lista_snps_NATIVOS
    
    cut -f2 /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP.bim > /scratch/capeverd/gbss2/temp/Natives/dataset_nativos_max_snp.snps


2) SNPs Union between databases

    comm -1 -2 <(sort snps_1000G.txt) <(sort snps_NATIVES.txt) > snps_comuns_1000G_NAT.txt
   
    perl /home/epigen/equipes/ancestralidade/scripts/snpconsensus.pl SNPs /home/giordano/lista_snps_1000G /home/giordano/lista_snps_NATIVOS

3) SNPs Intersect between databases

3.1) MaxSNPs
  
    comm -3 <(sort snps_1000G.txt) <(sort snps_NATIVES.txt) > snps_excluir_1000G_NAT.txt

3.2) MaxPops

    comm -3 <(sort snsps_HGDP_HAPMAP_maxPops.txt) <(sort snps_NATIVES.txt) > snps_excluir_maxPops_NAT.txt

3.3) HAP25N

    comm -3 <(sort snps_HAPMAP_maxSnps2.txt) <(sort snps_NATIVES.txt) > snps_excluir_maxSnpsHapMap_NAT.txt

4) Filter by SNPs in commom between databases

    plink --noweb --file /home/ldgh_projects/2.5_natives/data/Inputs_analysis/Peruvians/Shimaa_Ashanincas_Peruvians_comuns --extract snps_comuns_1000G_NAT.txt --recode --out EPIGEN_25M_autossomicos_snpscomuns_1000G
    
    plink --noweb --file /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_25M/1000Genomes_banco_completo_snpscomuns_25 --extract snps_comuns_1000G_NAT.txt --recode --out EPIGEN_25M_autossomicos_snpscomuns_nativos
    
    plink --noweb --file /home/ldgh_projects/2.5_natives/data/unphased_data/Quechuas/quechuas_Sem_Snps_Repetidos_KGP_Sexuais_inconsistente_limpo --extract snps_comuns_1000G_NAT.txt --recode --out EPIGEN_25M_autosso

5) Merge databases

5.1) MaxPops

    plink --noweb --file /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_Pops/NativosLDGH_HGDP_HAPMAP --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas.map --recode --out EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT
    awk '{print $2}' EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT.missnp > EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_trocada.txt
    plink --noweb --file EPIGEN_25M_autossomicos_snpscomuns_quechuas --flip EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_trocada.txt --recode --out EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_pops
    plink --noweb --file /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_Pops/NativosLDGH_HGDP_HAPMAP --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_pops.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_pops.map --recode --out EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita

5.2) HAP25N

    plink --noweb --file /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_SNPs/Nativos_Hapmap_Quechuas --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas.map --recode --out EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT
    awk '{print $2}' EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT.missnp > EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_trocada.txt
    plink --noweb --file EPIGEN_25M_autossomicos_snpscomuns_quechuas --flip snps_nativos_fit_fita_trocada.txt --recode --out EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_snps
    plink --noweb --file /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_SNPs/Nativos_Hapmap_Quechuas --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_snps.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas_Max_snps.map --recode --out EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita

5.3) MaxSNPs

    plink --noweb --file EPIGEN_25M_autossomicos_snpscomuns_1000G --merge EPIGEN_25M_autossomicos_snpscomuns_nativos.ped EPIGEN_25M_autossomicos_snpscomuns_nativos.map --recode --out EPIGEN_25M_1000G_Nativos_snpscomuns 
    plink --file EPIGEN_25M_autossomicos_snpscomuns_1000G --exclude snps_25M5M_1000G_NAT_excluir.txt
    plink --file EPIGEN_25M_autossomicos_snpscomuns_nativos_fitas_trocadas --exclude snps_25M5M_1000G_NAT_excluir.txt
    plink --file EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.map --recode --out EPIGEN_25M5M_1000G_Nativos_QT_snpscomuns_fita
    awk '{print $2}' EPIGEN_25M5M_1000G_Nativos_QT_snpscomuns_fita_final.missnp > snps_25M5M_1000G_NAT_QT_excluir.txt
    plink --noweb --file EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita --exclude snps_25M5M_1000G_NAT_QT_excluir.txt
    plink --noweb --file EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita --exclude snps_25M5M_1000G_NAT_QT_excluir.txt
    plink --noweb --file EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.map --recode --out EPIGEN_25M5M_1000G_Nativos_QT_snpscomuns_fita
    awk '{print $2}' EPIGEN_25M5M_1000G_Nativos_QT_snpscomuns_fita.missnp > snps_25M5M_1000G_NAT_QT_excluir_2.txt
    plink --file EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita --exclude snps_25M5M_1000G_NAT_QT_excluir_2.txt
    plink --file EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita --exclude snps_25M5M_1000G_NAT_QT_excluir_2.txt
    plink --noweb --file EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita --merge EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.ped EPIGEN_25M_autossomicos_snpscomuns_quechuas_fita.map --recode --out EPIGEN_25M5M_1000G_Nativos_QT_snpscomuns_fita

5.4) NAT2

    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP --hardy --keep-allele-order --freq --missing --mendel --het --write-snplist --indep-pairwise 50 10 0.1 --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP
    
    gunzip /scratch/capeverd/gbss2/temp/gbss2/Documents/1000Genomes/FullData/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.fam.gz
    gunzip /scratch/capeverd/gbss2/temp/gbss2/Documents/1000Genomes/FullData/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.bim.gz
    gunzip /scratch/capeverd/gbss2/temp/gbss2/Documents/1000Genomes/FullData/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.bed.gz
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/gbss2/Documents/1000Genomes/FullData/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes --extract /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP.prune.in --make-bed --out 
    /scratch/capeverd/gbss2/temp/Natives/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.Dataset_Nativos_max_SNP
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP --bmerge /scratch/capeverd/gbss2/temp/Natives/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.Dataset_Nativos_max_SNP --keep-allele-order --make-bed --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP --flip /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502-merge.missnp  --keep-allele-order --make-bed --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_strand
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_strand --bmerge /scratch/capeverd/gbss2/temp/Natives/ALL.phase3_shapeit2_mvncall_integrated_v5a.20130502.Dataset_Nativos_max_SNP --keep-allele-order --make-bed --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502 --keep-allele-order --mind 0.1 --geno 0.05 --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc 

6) Transform PED -> SDAT

6.1) Natives25M

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/ldgh_projects/2.5_natives/data/Inputs_analysis/Natives_Only/Nativos_only.ped -F sdat -o /home/giordano/Nativos_only.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/ldgh_projects/2.5_natives/data/Inputs_analysis/Peruvians/Shimaa_Ashanincas_Peruvians_comuns.ped -F sdat -o /home/giordano/Shimaa_Ashanincas_Peruvians.sdat

6.2) 1000Genomes

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_25M/1000Genomes_banco_completo_snpscomuns_25.ped -F sdat -o /home/giordano/1000Genomes_banco_completo_snpscomuns_25.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_25M/cabecalho_1KG.ped -F sdat -o cabecalho.sdat

6.3) MaxPops

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_Pops/NativosLDGH_HGDP_HAPMAP.ped -F sdat -o /home/giordano/NativosLDGH_HGDP_HAPMAP.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita.ped -F sdat -o EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita.sdat

6.4) MaxSNPs

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /home/ldgh_projects/2.5_natives/data/Inputs_analysis/MAX_SNPs/Nativos_Hapmap_Quechuas.ped -F sdat -o /home/giordano/Nativos_Hapmap_Quechuas.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita.ped -F sdat -o EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita.sdat

7) Select Inds

7.1) HAP25N

    awk -F '\t' '{print $1}' Nativos_Hapmap_Quechuas.sdat > ind_Hapmap_Quechuas.txt

7.2) MaxPops

    awk -F '\t' '{print $1}' NativosLDGH_HGDP_HAPMAP.sdat > ind_LDGH_HGDP_HAPMAP.txt

7.3) Natives25M

    awk -F '\t' '{print $1}' Nativos_only.sdat > ind_only.txt

8) Info Files

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu ginfo Nativos_Hapmap_Quechuas.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu ginfo Nativos_only.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu ginfo NativosLDGH_HGDP_HAPMAP.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu ginfo /home/epigen/equipes/ancestralidade/dados/public_databases/1000Genomes_files/Omni2.5M-5M_1000genomes_vcf_ped/1000Genomes_snpscomuns_com_EPIGEN_5M/1000Genomes_base_completa_snpscomuns_5M.ped

9) EXCLUDE Loci

9.1) MaxSNPs

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped --excludeloci=/home/giordano/snps_excluir_1000G_NAT.txt 1000Genomes_banco_completo_snpscomuns_25.ped -F sdat -o 1000Genomes_banco_completo_snpscomuns_25.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_1000G_NAT.txt Shimaa_Ashanincas_Peruvians.sdat -F sdat -o Shimaa_Ashanincas_Peruvians_comuns.sdat

9.2) MaxPops

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxPops_NAT.txt EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EUR.sdat -F sdat -o EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EUR_comuns.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxPops_NAT.txt EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EAS.sdat -F sdat -o EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EAS_comuns.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxPops_NAT.txt EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_WAFR.sdat -F sdat -o EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_WAFR_comuns.sdat

9.3) HAP25N

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxSnpsHapMap_NAT.txt EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EAS.sdat -F sdat -o EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EAS_comuns.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxSnpsHapMap_NAT.txt EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EUR.sdat -F sdat -o EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EUR_comuns.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --excludeloci=/home/giordano/snps_excluir_maxSnpsHapMap_NAT.txt EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_WAFR.sdat -F sdat -o EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_WAFR_comuns.sdat
    
9.4) LDGH

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform HGDP_LDGH_NewNames.sdat --excludeloci=ldgh_snp500_snps_duplicate.txt --excludesamples=rosenberg_H971_excluded_individuals.txt -o HGDP_LDGH_NewNamesEXCLUDE.sdat

9.5) II-NHL-Li

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform HGDP_IINHL600K_XYMfree.sdat --excludeloci=list_remove_HGDP650K.txt --excludesamples=rosenberg_H971_excluded_individuals.txt -o HGDP_IINHL600K_XYMfreeEXCLUDE.sdat

10) Rename Inds

10.1) HAP25N

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --renamesamples=rename_ind_Hapmap_Quechuas.txt Nativos_Hapmap_Quechuas.sdat -F sdat -o Nativos_Hapmap_Quechuas_names.sdat

10.2) Natives25M

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --renamesamples=rename_ind_only.txt Nativos_only.sdat -F sdat -o Nativos_only_names.sdat

10.3) MaxPops

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --renamesamples=rename_ind_LDGH_HGDP_HAPMAP.txt NativosLDGH_HGDP_HAPMAP.sdat -F sdat -o NativosLDGH_HGDP_HAPMAP_names.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --renamesamples=rename_ind_LDGH_HGDP_HAPMAP.txt Shimaa_Ashanincas_Peruvians_comuns.sdat -F sdat -o Shimaa_Ashanincas_Peruvians_comuns_names.sdat

11) FREQ continents

11.1)OPAN

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu qc.summary /home/giordano/HGDP_IINHL600K_XYMfreeEXCLUDE.sdat --includesamples=/home/giordano/POPs/EUR/EUR.txt --hwp -s /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EUR_summary.txt -o /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EUR_locus.txt -O /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EUR_sample.txt
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu qc.summary /home/giordano/HGDP_IINHL600K_XYMfreeEXCLUDE.sdat --includesamples=/home/giordano/POPs/EAS/EAS.txt --hwp -s /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EAS_summary.txt -o /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EAS_locus.txt -O /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_EAS_sample.txt
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu qc.summary /home/giordano/HGDP_IINHL600K_XYMfreeEXCLUDE.sdat --includesamples=/home/giordano/POPs/NAT/NAT.txt --hwp -s /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_NAT_summary.txt -o /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_NAT_locus.txt -O /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_NAT_sample.txt
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu qc.summary /home/giordano/HGDP_IINHL600K_XYMfreeEXCLUDE.sdat --includesamples=/home/giordano/POPs/WAFR/WAFR.txt --hwp -s /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_WAFR_summary.txt -o /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_WAFR_locus.txt -O /home/giordano/opa/HGDP_IINHL600K_XYMfreeEX_WAFR_sample.txt

12) SPLIT into continents

12.1) HAP25N

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_WAFR_LD_HGDP_HAPMAP_samples.txt Nativos_Hapmap_Quechuas_names.sdat -F sdat -o Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_EUR_LD_HGDP_HAPMAP_samples.txt Nativos_Hapmap_Quechuas_names.sdat -F sdat -o Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_EAS_LD_HGDP_HAPMAP_samples.txt Nativos_Hapmap_Quechuas_names.sdat -F sdat -o Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat

12.2) MaxPops

    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_WAFR_LD_HGDP_HAPMAP_samples.txt NativosLDGH_HGDP_HAPMAP_names.sdat -F sdat -o NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_EUR_LD_HGDP_HAPMAP_samples.txt NativosLDGH_HGDP_HAPMAP_names.sdat -F sdat -o NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat
    /opt/glu/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f sdat --includesamples=NAT_EAS_LD_HGDP_HAPMAP_samples.txt NativosLDGH_HGDP_HAPMAP_names.sdat -F sdat -o NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat

12.3) NAT2

    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502 --keep-allele-order --mind 0.1 --geno 0.05 --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc 
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc --within /alice/scratch/capeverd/shared/Baependi/ids_all_pops_con_hgdp_1000G_cv_baep_nat_CON_uniq.clust --keep-cluster-names EAFR WAFR EUR EAS NAT --make-bed --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_EAS_EUR_NAT
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_EAS_EUR_NAT --within /alice/scratch/capeverd/shared/Baependi/ids_all_pops_con_hgdp_1000G_cv_baep_nat_CON_uniq.clust --keep-cluster-names EAFR WAFR NAT --recode --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_EAS_EUR_NAT --within /alice/scratch/capeverd/shared/Baependi/ids_all_pops_con_hgdp_1000G_cv_baep_nat_CON_uniq.clust --keep-cluster-names EAS NAT --recode --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT
    
    /scratch/capeverd/gbss2/soft/plink_1.9/plink --bfile /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_EAS_EUR_NAT --within /alice/scratch/capeverd/shared/Baependi/ids_all_pops_con_hgdp_1000G_cv_baep_nat_CON_uniq.clust --keep-cluster-names EUR NAT --recode --out /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT
    
    /scratch/capeverd/gbss2/temp/gbss2/soft/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT.ped -F sdat -o /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT.sdat
    
    /scratch/capeverd/gbss2/temp/gbss2/soft/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT.ped -F sdat -o /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT.sdat
    
    /scratch/capeverd/gbss2/temp/gbss2/soft/glu-1.0b3-prerelease4-Linux_x86-64/glu transform -f ped /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT.ped -F sdat -o /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT.sdat

13) HIERSTAT input file preparation

13.1) HAP25N

    perl tHIERsV2.pl -i Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat
    perl tHIERsV2.pl -i Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat
    perl tHIERsV2.pl -i Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat

13.2) MaxPops

    perl tHIERsV2.pl -i NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat
    perl tHIERsV2.pl -i NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat
    perl tHIERsV2.pl -i NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat -p thiers_pops_HGDP_HAP_LDGH.txt -o HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat
    
13.3) NAT2

    perl tHIERsV2.pl -i /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT.sdat -o /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT.sdat
    
    perl tHIERsV2.pl -i /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT.sdat -o /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT.sdat
    
    perl tHIERsV2.pl -i /scratch/capeverd/gbss2/temp/Natives/Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT.sdat -o /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT.sdat

14) INSERT NA's (tHIERs QC)

    sed -i 's/  /NA/g' HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat
    sed -i 's/  /NA/g' HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat
    sed -i 's/  /NA/g' HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat
    
    sed -i 's/441\t//g' HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat
    sed -i 's/441\t//g' HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat
    sed -i 's/441\t//g' HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat

    sed -i 's/  /NA/g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat
    sed -i 's/  /NA/g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat
    sed -i 's/  /NA/g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat
    
    sed -i 's/441\t//g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat
    sed -i 's/441\t//g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat
    sed -i 's/441\t//g' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat

    R commands
    data1 = read.table(HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat,head=TRUE, sep = "\t", na.string = NA)
    data1 = read.table(HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat,head=TRUE, sep = "\t", na.string = NA)
    data1 = read.table(HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat,head=TRUE, sep = "\t", na.string = NA)

    awk '{ print NF } ' HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat > linecount_HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat
    awk '{ print NF } ' HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat > linecount_HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat
    awk '{ print NF } ' HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat > linecount_HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat

    awk '{ print NF } ' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat > linecount_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat
    awk '{ print NF } ' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat > linecount_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat
    awk '{ print NF } ' HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat > linecount_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat

15) RUN HIERFSTAT

15.1) HAPMAP

    nohup R CMD BATCH --save ./HAPMAP_NAT_WAFR.r &
    nohup R CMD BATCH --save ./HAPMAP_NAT_EUR.r &
    nohup R CMD BATCH --save ./HAPMAP_NAT_EAS.r &

15.2) AD

    nohup R CMD BATCH --save ./HAPMAP_HGDP_NAT_WAFR.r &
    nohup R CMD BATCH --save ./HAPMAP_HGDP_NAT_EUR.r &
    nohup R CMD BATCH --save ./HAPMAP_HGDP_NAT_EAS.r &

15.3) 1000Genomes

    #nohup R CMD BATCH --save ./NAT_1000G_NAT_WAFR.r &
    #nohup R CMD BATCH --save ./NAT_1000G_NAT_EUR.r &
    #nohup R CMD BATCH --save ./NAT_1000G_NAT_EAS.r &

15.4) NAT2

    #nohup R CMD BATCH --save ./amova_script.r /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EAS_NAT.sdat &
    #nohup R CMD BATCH --save ./amova_script.r /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_EUR_NAT.sdat  &
    #nohup R CMD BATCH --save ./amova_script.r /scratch/capeverd/gbss2/temp/Natives/HIER_Dataset_Nativos_max_SNP_ALL.phase3_shapeit2_v5a.20130502_qc_AFR_NAT.sdat  &

16) UPLOAD HIERFSTAT Results

16.1) HAPMAP 600K

    fstat_HAP_NAT_WAFR = read.table('/home/giordano/Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR.sdat', head=T, as.is=TRUE, row.names =1, na.strings=NA)
    fstat_HAP_EUR_NAT = read.table('/home/giordano/Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat', head=T, as.is=TRUE, row.names =1, na.strings=NA)
    fstat_HAP_EAS_NAT = read.table('/home/giordano/Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS.sdat', head=T, as.is=TRUE, row.names =1, na.strings=NA)
    
16.2) HAPMAP-HGDP NATIVE
    
    #fstat_HMHG_NAT_WAFR = read.table("/home/giordano/Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat", as.is=TRUE, head=T, row.names=1,na.strings=NA)
    #fstat_HMHG_EUR_NAT = read.table("/home/giordano/Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat", as.is=TRUE, head=T, row.names=1,na.strings=NA)
    #fstat_HMHG_EAS_NAT = read.table("/home/giordano/Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat", as.is=TRUE, head=T, row.names=1,na.strings=NA)

16.3) OPAN

    fs_OPAN_NAT_EAS = read.table('Results_HIER_HGDP_LDGH_EX_NAT_EAS.sdat', head=T, as.is=T)
    fs_OPAN_NAT_EUR = read.table('Results_HIER_HGDP_LDGH_EX_NAT_EUR.sdat', head=T, as.is=T)
    fs_OPAN_NAT_WAFR = read.table('Results_HIER_HGDP_LDGH_EX_NAT_WAFR.sdat', head=T, as.is=T)
    
    fs_OPAN_NAT_EAS = lapply(fs_OPAN_NAT_EAS, as.numeric)
    fs_OPAN_NAT_EUR = lapply(fs_OPAN_NAT_EUR, as.numeric)
    fs_OPAN_NAT_WAFR = lapply(fs_OPAN_NAT_WAFR, as.numeric)

16.4) OPAIINHL

    fs_OPAIINHL_NAT_EAS = read.table('.sdat', head=T, as.is=T)
    fs_OPAIINHL_NAT_EUR = read.table('.sdat', head=T, as.is=T)
    fs_OPAIINHL_NAT_WAFR = read.table('.sdat', head=T, as.is=T)
    
    table_fct[2,] = c(0.235,0.274,0.465,0.301,0.367,0.587,0.503,0.555,0.770)
    table_fsc[2,] = c(0.000,0.000,0.000,0.004,0.000,0.000,0.013,0.007,0.005)

16.5) OPALi

    fs_OPALi_NAT_EAS = read.table('Results_HIER_HGDP_IINHL600K_XYMfreeEX_NAT_EAS.sdat', head=T, as.is=T)
    fs_OPALi_NAT_EUR = read.table('Results_HIER_HGDP_IINHL600K_XYMfreeEX_NAT_EUR.sdat', head=T, as.is=T)
    fs_OPALi_NAT_WAFR = read.table('Results_HIER_HGDP_IINHL600K_XYMfreeEX_NAT_WAFR.sdat', head=T, as.is=T)

    fs_OPALi_NAT_EAS = lapply(fs_OPALi_NAT_EAS, as.numeric)
    fs_OPALi_NAT_EUR = lapply(fs_OPALi_NAT_EUR, as.numeric)
    fs_OPALi_NAT_WAFR = lapply(fs_OPALi_NAT_WAFR, as.numeric)

16.6) MaxPops

    fs_MaxPops_NAT_EAS = read.table('Results_HIER_t_EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EAS_comuns.txt', head=T, as.is=T)
    fs_MaxPops_NAT_EUR = read.table('Results_HIER_t_EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_EUR_comuns.txt', head=T, as.is=T)
    fs_MaxPops_NAT_WAFR = read.table('Results_HIER_t_EPIGEN_25M5M_Max_pops_HAPMAP_HGDP_NAT_QT_fita_names_NAT_WAFR_comuns.txt', head=T, as.is=T)

16.7) MaxSNPs

    fs_MaxSNPs_NAT_EAS = read.table('Results_HIER_t_EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita_names_NAT_EAS.sdat', head=T, as.is=T)
    fs_MaxSNPs_NAT_EUR = read.table('Results_HIER_t_EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita_names_NAT_EUR.sdat', head=T, as.is=T)
    fs_MaxSNPs_NAT_WAFR = read.table('Results_HIER_t_EPIGEN_25M5M_1000G_Nativos_snpscomuns_fita_names_NAT_WAFR.sdat', head=T, as.is=T)
    fs_MaxSNPs_NAT_EAS = lapply(fs_MaxSNPs_NAT_EAS, as.numeric)
    fs_MaxSNPs_NAT_EUR = lapply(fs_MaxSNPs_NAT_EUR, as.numeric)
    fs_MaxSNPs_NAT_WAFR = lapply(fs_MaxSNPs_NAT_WAFR, as.numeric)

16.8) HAP25N

    fs_HAP25N_NAT_EAS = read.table('Results_HIER_t_EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EAS_comuns.txt', head=T, as.is=T)
    fs_HAP25N_NAT_EUR = read.table('Results_HIER_t_EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_EUR_comuns.txt', head=T, as.is=T)
    fs_HAP25N_NAT_WAFR = read.table('Results_HIER_t_EPIGEN_25M5M_Max_snps2_HAPMAP_NAT_QT_fita_names_NAT_WAFR_comuns.txt', head=T, as.is=T)


19) REPLACE NaN and ERROR

    fs_OPAN_NAT_WAFR = replace(fs_OPAN_NAT_WAFR, fs_OPAN_NAT_WAFR=="ERROR", NA)
    fs_OPAN_NAT_WAFR = replace(fs_OPAN_NAT_WAFR, fs_OPAN_NAT_WAFR=="NaN", NA)
    fs_OPAN_NAT_EUR = replace(fs_OPAN_NAT_EUR, fs_OPAN_NAT_EUR=="ERROR", NA)
    fs_OPAN_NAT_EUR = replace(fs_OPAN_NAT_EUR, fs_OPAN_NAT_EUR=="NaN", NA)
    fs_OPAN_NAT_EAS = replace(fs_OPAN_NAT_EAS, fs_OPAN_NAT_EAS=="NaN", NA)
    fs_OPAN_NAT_EAS = replace(fs_OPAN_NAT_EAS, fs_OPAN_NAT_EAS=="ERROR", NA)

    fs_OPALi_NAT_EAS = replace(fs_OPALi_NAT_EAS, fs_OPALi_NAT_EAS=="ERROR", NA)
    fs_OPALi_NAT_EUR = replace(fs_OPALi_NAT_EUR, fs_OPALi_NAT_EUR=="ERROR", NA)
    fs_OPALi_NAT_WAFR = replace(fs_OPALi_NAT_WAFR, fs_OPALi_NAT_WAFR=="ERROR", NA)
    fs_OPALi_NAT_EAS = replace(fs_OPALi_NAT_EAS, fs_OPALi_NAT_EAS=="NaN", NA)
    fs_OPALi_NAT_EUR = replace(fs_OPALi_NAT_EUR, fs_OPALi_NAT_EUR=="NaN", NA)
    fs_OPALi_NAT_WAFR = replace(fs_OPALi_NAT_WAFR, fs_OPALi_NAT_WAFR=="NaN", NA)

    fs_MaxSNPs_NAT_EAS = replace(fs_MaxSNPs_NAT_EAS, fs_MaxSNPs_NAT_EAS=="ERROR", NA)
    fs_MaxSNPs_NAT_EUR = replace(fs_MaxSNPs_NAT_EUR, fs_MaxSNPs_NAT_EUR=="ERROR", NA)
    fs_MaxSNPs_NAT_WAFR = replace(fs_MaxSNPs_NAT_WAFR, fs_MaxSNPs_NAT_WAFR=="NaN", NA)
    fs_MaxSNPs_NAT_WAFR = replace(fs_MaxSNPs_NAT_WAFR, fs_MaxSNPs_NAT_WAFR=="ERROR", NA)
    fs_MaxSNPs_NAT_EAS = replace(fs_MaxSNPs_NAT_EAS, fs_MaxSNPs_NAT_EAS=="NaN", NA)
    fs_MaxSNPs_NAT_EUR = replace(fs_MaxSNPs_NAT_EUR, fs_MaxSNPs_NAT_EUR=="NaN", NA)

20) FSC Critical Values

20.1) II-NHL-Li 600K

    quantile(as.numeric(fstat_HAP_EUR_NAT$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)
    quantile(as.numeric(fstat_HAP_EAS_NAT$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)
    quantile(as.numeric(fstat_HAP_NAT_WAFR$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)

20.2) LDGH NATIVE

    quantile(as.numeric(fstat_HMHG_EUR_NAT$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)
    quantile(as.numeric(fstat_HMHG_NAT_WAFR$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)
    quantile(as.numeric(fstat_HMHG_EAS_NAT$FSC),probs=c(0.01,0.05,0.1,0.25,0.5), na.rm=T)

20.3) OPAN

    table_fsc[1,c(1,4,7)] = quantile(as.numeric(fs_OPAN_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[1,c(2,5,8)] = quantile(as.numeric(fs_OPAN_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[1,c(3,6,9)] = quantile(as.numeric(fs_OPAN_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)

20.4) OPAIINHL

    table_fsc[2,c(1,4,7)] = quantile(as.numeric(fs_OPAN_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[2,c(2,5,8)] = quantile(as.numeric(fs_OPAN_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[2,c(3,6,9)] = quantile(as.numeric(fs_OPAN_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[2,] = c(0.000,0.000,0.000,0.004,0.000,0.000,0.013,0.007,0.005)

20.5) OPALi

    table_fsc[3,c(1,4,7)] = quantile(as.numeric(fs_OPALi_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[3,c(2,5,8)] = quantile(as.numeric(fs_OPALi_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[3,c(3,6,9)] = quantile(as.numeric(fs_OPALi_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)

20.6) MaxPops

    table_fsc[4,c(1,4,7)] = quantile(as.numeric(fs_MaxPops_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[4,c(2,5,8)] = quantile(as.numeric(fs_MaxPops_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[4,c(3,6,9)] = quantile(as.numeric(fs_MaxPops_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)

20.7) MaxSNPs

    table_fsc[5,c(1,4,7)] = quantile(as.numeric(fs_MaxSNPs_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[5,c(2,5,8)] = quantile(as.numeric(fs_MaxSNPs_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[5,c(3,6,9)] = quantile(as.numeric(fs_MaxSNPs_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)

20.8) HAP25N

    table_fsc[6,c(1,4,7)] = quantile(as.numeric(fs_HAP25N_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[6,c(2,5,8)] = quantile(as.numeric(fs_HAP25N_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fsc[6,c(3,6,9)] = quantile(as.numeric(fs_HAP25N_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)

21) FCT Critical Values

21.1) II-NHL-Li 600K

    #quantile(as.numeric(fstat_HAP_EUR_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    #quantile(as.numeric(fstat_HAP_EAS_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    #quantile(as.numeric(fstat_HAP_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.2) LDGH NATIVE

    #quantile(as.numeric(fstat_HMHG_EUR_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    #quantile(as.numeric(fstat_HMHG_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    #quantile(as.numeric(fstat_HMHG_EAS_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.3) OPAN

    table_fct[1,c(1,4,7)] = quantile(as.numeric(fs_OPAN_NAT_EAS$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[1,c(2,5,8)] = quantile(as.numeric(fs_OPAN_NAT_EUR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[1,c(3,6,9)] = quantile(as.numeric(fs_OPAN_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.4) OPAIINHL

    #table_fsc[2,c(1,4,7)] = quantile(as.numeric(fs_OPAN_NAT_EAS$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    #table_fsc[2,c(2,5,8)] = quantile(as.numeric(fs_OPAN_NAT_EUR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    #table_fsc[2,c(3,6,9)] = quantile(as.numeric(fs_OPAN_NAT_WAFR$FSC),probs=c(0.05,0.10,0.25), na.rm=T)
    table_fct[2,] = c(0.235,0.274,0.465,0.301,0.367,0.587,0.503,0.555,0.770)

21.5) OPALi

    table_fct[3,c(1,4,7)] = quantile(as.numeric(fs_OPALi_NAT_EAS$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[3,c(2,5,8)] = quantile(as.numeric(fs_OPALi_NAT_EUR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[3,c(3,6,9)] = quantile(as.numeric(fs_OPALi_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.6) MaxPops

    table_fct[4,c(1,4,7)] = quantile(as.numeric(fs_MaxPops_NAT_EAS$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[4,c(2,5,8)] = quantile(as.numeric(fs_MaxPops_NAT_EUR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[4,c(3,6,9)] = quantile(as.numeric(fs_MaxPops_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.7) MaxSNPs

    table_fct[5,c(1,4,7)] = quantile(as.numeric(fs_MaxSNPs_NAT_EAS$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[5,c(2,5,8)] = quantile(as.numeric(fs_MaxSNPs_NAT_EUR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[5,c(3,6,9)] = quantile(as.numeric(fs_MaxSNPs_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

21.8) HAP25N

    table_fct[6,c(1,4,7)] = quantile(as.numeric(fs_HAP25N_NAT_EAS$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[6,c(2,5,8)] = quantile(as.numeric(fs_HAP25N_NAT_EUR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)
    table_fct[6,c(3,6,9)] = quantile(as.numeric(fs_HAP25N_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T)

22) PLOT FSC AND FCT Critical Values

    write.table(round(table_fsc,3), "table_fsc.txt", quote=F)
    write.table(round(table_fct,3), "table_fct.txt", quote=F)

22.1) Latex Table

    latex(table_fct, file="", booktabs = TRUE, title = "Valores Criticos de FCT", cgroup = c('0.90','0.95','0.99'), colheads = rep(c('EAS','EUR','WAFR'), 3))
    latex(table_fsc, file="", booktabs = TRUE, title = "Valores Criticos de FSC", cgroup = c('0.90','0.95','0.99'), colheads = rep(c('EAS','EUR','WAFR'), 3))

22.2) FSC VALUES

    pdf("fsc_HAP_HHMG_CritValues.pdf")
    par(mfrow=c(3,1))
    plot(density(fstat_HAP_EUR_NAT$FSC,kernel="gaussian",na.rm=T),col=2, xlim=c(-0.05,0.6), xlab="FSC", main="HAPMAP (EUR-NAT)")
    abline(v=quantile(fstat_HAP_EUR_NAT$FSC,probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_EUR_NAT$FSC),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,0.6), xlab="FSC", main="HGDP-HAPMAP (EUR-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_EUR_NAT$FSC),probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    
    plot(density(as.numeric(fstat_HAP_NAT_WAFR$FSC),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,0.6), xlab="FSC", main="HAPMAP (WAFR-NAT)")
    abline(v=quantile(as.numeric(fstat_HAP_NAT_WAFR$FSC),probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_NAT_WAFR$FSC),kernel="gaussian",na.rm=T),col=2 , xlim=c(-0.05,0.6), xlab="FSC", main="HGDP-HAPMAP (WAFR-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_NAT_WAFR$FSC),probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    
    plot(density(fstat_HAP_EAS_NAT$FSC,kernel="gaussian",na.rm=T),col=2,xlim=c(-0.05,0.6), xlab="FSC", main="HAPMAP (EAS-NAT)")
    abline(v=quantile(fstat_HAP_EAS_NAT$FSC,probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_EAS_NAT$FSC),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,0.6), xlab="FSC", main="HGDP-HAPMAP (EAS-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_EAS_NAT$FSC),probs=c(0.05,0.1,0.2), na.rm=T), col="black")
    dev.off()

22.3) FCT VALUES

    pdf("fct_HAP_HHMG_CritValues.pdf")
    par(mfrow=c(3,1))
    plot(density(as.numeric(fstat_HAP_EUR_NAT$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HAPMAP (EUR-NAT)")
    abline(v=quantile(as.numeric(fstat_HAP_EUR_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_EUR_NAT$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HGDP-HAPMAP (EUR-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_EUR_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    
    plot(density(as.numeric(fstat_HAP_NAT_WAFR$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HAPMAP (WAFR-NAT)")
    abline(v=quantile(as.numeric(fstat_HAP_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_NAT_WAFR$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HGDP-HAPMAP (WAFR-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_NAT_WAFR$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    
    plot(density(as.numeric(fstat_HAP_EAS_NAT$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HAPMAP (EAS-NAT)")
    abline(v=quantile(as.numeric(fstat_HAP_EAS_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    plot(density(as.numeric(fstat_HMHG_EAS_NAT$FCT),kernel="gaussian",na.rm=T),col=2 ,xlim=c(-0.05,1), xlab="FCT", main="HGDP-HAPMAP (EAS-NAT)")
    abline(v=quantile(as.numeric(fstat_HMHG_EAS_NAT$FCT),probs=c(0.9,0.95,0.99), na.rm=T), col="black")
    dev.off()

22.4) FCTxFIS

    tiff(filename = "fctxfis.tiff", width = 17.35, height = 17.35, units = "cm", pointsize = 10, res=100, bg = "white", compression="lzw")
    par(mfrow=c(3,2))
    plot(as.numeric(fstat_HAP_EUR_NAT$FIS),as.numeric(fstat_HAP_EUR_NAT$FCT),pch=20,col=2,xlim=c(-1.05,1.05),xlab="FIS",ylab="FCT", main="HAPMAP Dataset (EUR-NAT)")
    plot(as.numeric(fstat_HMHG_EUR_NAT$FIS),as.numeric(fstat_HMHG_EUR_NAT$FCT),col=2 ,xlim=c(-1.05,1.05),pch=20,xlab="FIS",ylab="FCT", main="HAPMAP-HGDP Dataset (EUR-NAT)")
    plot(as.numeric(fstat_HAP_NAT_WAFR$FIS),as.numeric(fstat_HAP_NAT_WAFR$FCT),col=2,xlim=c(-1.05,1.05),pch=20,xlab="FIS",ylab="FCT", main="HAPMAP Dataset (WAFR-NAT)")
    plot(as.numeric(fstat_HMHG_NAT_WAFR$FIS),as.numeric(fstat_HMHG_NAT_WAFR$FCT),col=2 ,xlim=c(-1.05,1.05),pch=20,xlab="FIS",ylab="FCT", main="HAPMAP-HGDP Dataset (WAFR-NAT)")
    plot(as.numeric(fstat_HAP_EAS_NAT$FIS),as.numeric(fstat_HAP_EAS_NAT$FCT),col=2,xlim=c(-1.05,1.05),pch=20,xlab="FIS",ylab="FCT", main="HAPMAP Dataset (EAS-NAT)")
    plot(as.numeric(fstat_HMHG_EAS_NAT$FIS),as.numeric(fstat_HMHG_EAS_NAT$FCT),col=2 ,xlim=c(-1.05,1.05),pch=20,xlab="FIS",ylab="FCT", main="HAPMAP-HGDP Dataset (EAS-NAT)")
    dev.off()

22.5) FCTxFIC

    tiff(filename = "fctxfic.tiff", width = 17.35, height = 17.35, units = "cm", pointsize = 10, res=100, bg = "white", compression="lzw")
    par(mfrow=c(3,2))
    plot(as.numeric(fstat_HAP_EUR_NAT$FIC),as.numeric(fstat_HAP_EUR_NAT$FCT),pch=20,col=2,xlim=c(-1.05,1.05),xlab="FIC",ylab="FCT", main="HAPMAP Dataset (EUR-NAT)")
    plot(as.numeric(fstat_HMHG_EUR_NAT$FIC),as.numeric(fstat_HMHG_EUR_NAT$FCT),col=2 ,pch=20,xlim=c(-1.05,1.05),xlab="FIC",ylab="FCT", main="HAPMAP-HGDP Dataset (EUR-NAT)")
    plot(as.numeric(fstat_HAP_NAT_WAFR$FIC),as.numeric(fstat_HAP_NAT_WAFR$FCT),col=2,pch=20,xlim=c(-1.05,1.05),xlab="FIC",ylab="FCT", main="HAPMAP Dataset (WAFR-NAT)")
    plot(as.numeric(fstat_HMHG_NAT_WAFR$FIC),as.numeric(fstat_HMHG_NAT_WAFR$FCT),col=2 ,pch=20,xlim=c(-1.05,1.05),xlab="FIC",ylab="FCT", main="HAPMAP-HGDP Dataset (WAFR-NAT)")
    plot(as.numeric(fstat_HAP_EAS_NAT$FIC),as.numeric(fstat_HAP_EAS_NAT$FCT),col=2,xlim=c(-1.05,1.05),pch=20,xlab="FIC",ylab="FCT", main="HAPMAP Dataset (EAS-NAT)")
    plot(as.numeric(fstat_HMHG_EAS_NAT$FIC),as.numeric(fstat_HMHG_EAS_NAT$FCT),col=2 ,xlim=c(-1.05,1.05),pch=20,xlab="FIC",ylab="FCT", main="HAPMAP-HGDP Dataset (EAS-NAT)")
    dev.off()

22.6) FCTxFIS HEXBIN

    tiff("myOut.tiff")
    par(mfrow=c(3,2))
    bin1<-hexbin(as.numeric(fstat_HAP_EUR_NAT$FIS),as.numeric(fstat_LDGH_EUR_NAT$FCT), xbins=50)
    bin2<-hexbin(as.numeric(fstat_HMHG_EUR_NAT$FIS),as.numeric(NAT_WAFR_600K$FCT), xbins=50)
    bin3<-hexbin(as.numeric(fstat_HAP_NAT_WAFR$FIS),as.numeric(fstat_LDGH_NAT_WAFR$FCT), xbins=50)
    bin4<-hexbin(as.numeric(fstat_HMHG_NAT_WAFR$FIS),as.numeric(NAT_WAFR_600K$FCT), xbins=50)
    bin5<-hexbin(as.numeric(fstat_HAP_EAS_NAT$FIS),as.numeric(fstat_LDGH_EAS_NAT$FCT), xbins=50)
    bin6<-hexbin(as.numeric(fstat_HMHG_EAS_NAT$FIS),as.numeric(NAT_WAFR_600K$FCT), xbins=50)
    plot(bin1, main="LDGH Dataset (EUR-NAT)")
    plot(bin2, main="Li-II-NHL Dataset (EUR-NAT)")
    plot(bin3, main="LDGH Dataset (WAFR-NAT)")
    plot(bin4, main="Li-II-NHL Dataset (WAFR-NAT)")
    plot(bin5, main="LDGH Dataset (EAS-NAT)")
    plot(bin6, main="Li-II-NHL Dataset (EAS-NAT)")
    dev.off()

23) SELECT STRUCTRED SNPS

23.1) AD

    str_NAT_EUR_HMHG <- na.omit(subset(fstat_HMHG_EUR_NAT, as.numeric(fstat_HMHG_EUR_NAT$FCT) > 0.373 & as.numeric(fstat_HMHG_EUR_NAT$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))
    str_NAT_EAS_HMHG <- na.omit(subset(fstat_HMHG_EAS_NAT, as.numeric(fstat_HMHG_EAS_NAT$FCT) > 0.297 & as.numeric(fstat_HMHG_EAS_NAT$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))
    str_NAT_WAFR_HMHG <- na.omit(subset(fstat_HMHG_NAT_WAFR, as.numeric(fstat_HMHG_NAT_WAFR$FCT) > 0.538 & as.numeric(fstat_HMHG_NAT_WAFR$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))

23.2) HAPMAP

    str_NAT_EUR_HAP <- na.omit(subset(fstat_HAP_EUR_NAT, as.numeric(fstat_HAP_EUR_NAT$FCT) > 0.410 & as.numeric(fstat_HAP_EUR_NAT$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))
    str_NAT_EAS_HAP <- na.omit(subset(fstat_HAP_EAS_NAT, as.numeric(fstat_HAP_EAS_NAT$FCT) > 0.350 & as.numeric(fstat_HAP_EAS_NAT$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))
    str_NAT_WAFR_HAP <- na.omit(subset(fstat_HAP_NAT_WAFR, as.numeric(fstat_HAP_NAT_WAFR$FCT) > 0.562 & as.numeric(fstat_HAP_NAT_WAFR$FSC) < 0.12,  select=c("FCT","FSC","FST","FIS")))

24) WRITE TABLES

    write.table(str_NAT_EUR_HMHG, file="str_NAT_EUR_HMHG_EP.txt",col.names= F,quote=F)
    write.table(str_NAT_EAS_HMHG, file="str_NAT_EAS_HMHG_EP.txt",col.names= F,quote=F)
    write.table(str_NAT_WAFR_HMHG, file="str_NAT_WAFR_HMHG_EP.txt",col.names= F,quote=F)
    
    write.table(str_NAT_EUR_HAP, file="str_NAT_EUR_HAP_EP.txt",col.names= F,quote=F)
    write.table(str_NAT_EAS_HAP, file="str_NAT_EAS_HAP_EP.txt",col.names= F,quote=F)
    write.table(str_NAT_WAFR_HAP, file="str_NAT_WAFR_HAP_EP.txt",col.names= F,quote=F)


25) AWK way of select structured SNPs

25.1) AD

    awk '$2>0.359 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EUR.sdat > str_NAT_EUR_HMHG_EP.txt
    awk '$2>0.279 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_EAS.sdat > str_NAT_EAS_HMHG_EP.txt
    awk '$2>0.553 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_NativosLDGH_HGDP_HAPMAP_names_NAT_WAFR.sdat > str_NAT_WAFR_HMHG_EP.txt

25.2) HAPMAP

    awk '$2>0.343 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_EUR.sdat > str_NAT_EUR_HAP_EP.txt
    awk '$2>0.227 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_EAS > str_NAT_EAS_HAP_EP.txt
    awk '$2>0.571 && $5<0.12 {print $1, $2, $3, $5, $7}' Results_HIER_Nativos_Hapmap_Quechuas_names_NAT_WAFR > str_NAT_WAFR_HAP_EP.txt

26) CUT rs Id

26.1) AD

    cut -f1 -d" " str_NAT_EAS_HMHG_EP.txt > str_HMHG_NAT_EAS_EP_rs.txt
    cut -f1 -d" " str_NAT_EUR_HMHG_EP.txt > str_HMHG_NAT_EUR_EP_rs.txt
    cut -f1 -d" " str_NAT_WAFR_HMHG_EP.txt > str_HMHG_NAT_WAFR_EP_rs.txt

26.2) HAPMAP

    cut -f1 -d" " str_NAT_EAS_HAP_EP.txt > str_HAP_NAT_EAS_EP_rs.txt
    cut -f1 -d" " str_NAT_EUR_HAP_EP.txt > str_HAP_NAT_EUR_EP_rs.txt
    cut -f1 -d" " str_NAT_WAFR_HAP_EP.txt > str_HAP_NAT_WAFR_EP_rs.txt

27) SELECT UNIQUES RS

27.1) AD

    sort str_HMHG_NAT_EAS_EP_rs.txt | uniq -c | sort -n > str_uniq_HMHG_NAT_EAS_EP_rs_count.txt
    sort str_HMHG_NAT_EUR_EP_rs.txt | uniq -c | sort -n > str_uniq_HMHG_NAT_EUR_EP_rs_count.txt
    sort str_HMHG_NAT_WAFR_EP_rs.txt | uniq -c | sort -n > str_uniq_HMHG_NAT_WAFR_EP_rs_count.txt
    awk '{print $2}' str_uniq_HMHG_NAT_EAS_EP_rs_count.txt > str_uniq_HMHG_NAT_EAS_EP_rs.txt 
    awk '{print $2}' str_uniq_HMHG_NAT_EUR_EP_rs_count.txt > str_uniq_HMHG_NAT_EUR_EP_rs.txt 
    awk '{print $2}' str_uniq_HMHG_NAT_WAFR_EP_rs_count.txt > str_uniq_HMHG_NAT_WAFR_EP_rs.txt 

27.2) HAPMAP

    sort str_HAP_NAT_EAS_EP_rs.txt | uniq -c | sort -n > str_uniq_HAP_NAT_EAS_EP_rs_count.txt
    sort str_HAP_NAT_EUR_EP_rs.txt | uniq -c | sort -n > str_uniq_HAP_NAT_EUR_EP_rs_count.txt
    sort str_HAP_NAT_WAFR_EP_rs.txt | uniq -c | sort -n > str_uniq_HAP_NAT_WAFR_EP_rs_count.txt
    awk '{print $2}' str_uniq_HAP_NAT_EAS_EP_rs_count.txt > str_uniq_HAP_NAT_EAS_EP_rs.txt 
    awk '{print $2}' str_uniq_HAP_NAT_EUR_EP_rs_count.txt > str_uniq_HAP_NAT_EUR_EP_rs.txt 
    awk '{print $2}' str_uniq_HAP_NAT_WAFR_EP_rs_count.txt > str_uniq_HAP_NAT_WAFR_EP_rs.txt 

27.3) ALL datasets

<b> UNIONs </b>

    > HAP-HMHG NAT-EAS
    cat str_uniq_H6_NAT_EAS_EP_rs.txt str_uniq_LD_NAT_EAS_EP_rs.txt | sort | uniq > str_uniq_ALL_NAT_EAS_EP_rs.txt
    > HAP-HMHG NAT-EUR
    cat str_uniq_H6_NAT_EUR_EP_rs.txt str_uniq_LD_NAT_EUR_EP_rs.txt | sort | uniq > str_uniq_ALL_NAT_EUR_EP_rs.txt
    > HAP-HMHG NAT-WAFR
    cat str_uniq_H6_NAT_WAFR_EP_rs.txt str_uniq_LD_NAT_WAFR_EP_rs.txt | sort | uniq > str_uniq_ALL_NAT_WAFR_EP_rs.txt
    > H6 NAT-(EAS-EUR-WAFR)
    cat str_uniq_H6_NAT_EAS_EP_rs.txt str_uniq_H6_NAT_EUR_EP_rs.txt str_uniq_H6_NAT_WAFR_EX_rs.txt | sort | uniq > str_uniq_H6_ALL_EP_rs.txt
    > LD NAT-(EAS-EUR-WAFR)
    cat str_uniq_LD_NAT_EAS_EP_rs.txt str_uniq_LD_NAT_EUR_EP_rs.txt str_uniq_LD_NAT_WAFR_EX_rs.txt | sort | uniq > str_uniq_LD_ALL_EP_rs.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_EP_rs.txt str_uniq_H6_NAT_WAFR_EP_rs.txt | sort | uniq > str_uniq_H6_NAT_EUR_WAFR_EP_rs.txt
    > LD NAT-(EUR-WAFR)
    cat str_uniq_LD_NAT_EUR_EP_rs.txt str_uniq_LD_NAT_WAFR_EP_rs.txt | sort | uniq > str_uniq_LD_NAT_EUR_WAFR_EP_rs.txt
    > HAP-HMHG NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_WAFR_EP_rs.txt str_uniq_LD_NAT_EUR_WAFR_EP_rs.txt | sort | uniq > str_uniq_ALL_NAT_EUR_WAFR_EP_rs.txt
    > HAP-HMHG NAT-(EAS-EUR-WAFR) ALL
    cat str_uniq_H6_ALL_EP_rs.txt str_uniq_LD_ALL_EP_rs.txt | sort | uniq > str_uniq_ALL_ALL_EP_rs.txt

<b> Intersections </b>

    > H6 NAT-(EAS-EUR-WAFR)
    cat str_uniq_H6_NAT_EAS_EP_rs.txt str_uniq_H6_NAT_EUR_EP_rs.txt str_uniq_H6_NAT_WAFR_EP_rs.txt | sort | uniq -c > str_non_uniq_H6_ALL_EP_rs_count.txt
    awk '$1==3 {print $2}' str_non_uniq_H6_ALL_rs_count.txt > str_non_uniq_H6_ALL_rs.txt
    > LD NAT-(EAS-EUR-WAFR)
    cat str_uniq_LD_NAT_EAS_EP_rs.txt str_uniq_LD_NAT_EUR_EP_rs.txt str_uniq_LD_NAT_WAFR_EP_rs.txt | sort | uniq -c > str_non_uniq_LD_ALL_EP_rs_count.txt
    awk '$1==3 {print $2}' str_non_uniq_LD_ALL_EP_rs_count.txt > str_non_uniq_LD_ALL_EP_rs.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_EP_rs.txt str_uniq_H6_NAT_WAFR_EP_rs.txt | sort | uniq -d > str_non_uniq_H6_NAT_EUR_WAFR_EP_rs.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_LD_NAT_EUR_EP_rs.txt str_uniq_LD_NAT_WAFR_EP_rs.txt | sort | uniq -d > str_non_uniq_LD_NAT_EUR_WAFR_EP_rs.txt
    > HAP-HMHG NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_WAFR_EP_rs.txt str_uniq_LD_NAT_EUR_WAFR_EP_rs.txt | sort | uniq -d > str_non_uniq_ALL_NAT_EUR_WAFR_EP_rs.txt
    > HAP-HMHG NAT-(EAS-EUR-WAFR) ALL
    cat str_uniq_H6_ALL_EP_rs.txt str_uniq_LD_ALL_EP_rs.txt | sort | uniq -d > str_non_uniq_ALL_ALL_EP_rs.txt

28) SELECT UNIQUES GENES

    cut -f3 /home/DivergenomEnrich/data/str_uniq_LD_NAT_EAS_EX_rs.txt.annotated > str_LD_NAT_EAS_EX_gene.txt
    cut -f3 /home/DivergenomEnrich/data/str_uniq_LD_NAT_EUR_EX_rs.txt.annotated > str_LD_NAT_EUR_EX_gene.txt
    cut -f3 /home/DivergenomEnrich/data/str_uniq_LD_NAT_WAFR_EX_rs.txt.annotated > str_LD_NAT_WAFR_EX_gene.txt
    cut -f3 /home/DivergenomEnrich/data/str_uniq_H6_NAT_WAFR_EX_rs.txt.annotated > str_H6_NAT_WAFR_EX_gene.txt
    cut -f3 /home/DivergenomEnrich/data/str_uniq_H6_NAT_EUR_EX_rs.txt.annotated > str_H6_NAT_EUR_EX_gene.txt
    cut -f3 /home/DivergenomEnrich/data/str_uniq_H6_NAT_EAS_EX_rs.txt.annotated > str_H6_NAT_EAS_EX_gene.txt

28.1) II-NHL-Li 600K

    sort str_H6_NAT_EAS_EX_gene.txt | uniq -c | sort -n > str_uniq_H6_NAT_EAS_EX_gene_count.txt
    sort str_H6_NAT_EUR_EX_gene.txt | uniq -c | sort -n > str_uniq_H6_NAT_EUR_EX_gene_count.txt
    sort str_H6_NAT_WAFR_EX_gene.txt | uniq -c | sort -n > str_uniq_H6_NAT_WAFR_EX_gene_count.txt
    awk '{print $2}' str_uniq_H6_NAT_EAS_EX_gene_count.txt > str_uniq_H6_NAT_EAS_EX_gene.txt 
    awk '{print $2}' str_uniq_H6_NAT_EUR_EX_gene_count.txt > str_uniq_H6_NAT_EUR_EX_gene.txt 
    awk '{print $2}' str_uniq_H6_NAT_WAFR_EX_gene_count.txt > str_uniq_H6_NAT_WAFR_EX_gene.txt 

28.2) LDGH NATIVE

    sort str_LD_NAT_EAS_EX_gene.txt | uniq -c | sort -n > str_uniq_LD_NAT_EAS_EX_gene_count.txt
    sort str_LD_NAT_EUR_EX_gene.txt | uniq -c | sort -n > str_uniq_LD_NAT_EUR_EX_gene_count.txt
    sort str_LD_NAT_WAFR_EX_gene.txt | uniq -c | sort -n > str_uniq_LD_NAT_WAFR_EX_gene_count.txt
    awk '{print $2}' str_uniq_LD_NAT_EAS_EX_gene_count.txt > str_uniq_LD_NAT_EAS_EX_gene.txt 
    awk '{print $2}' str_uniq_LD_NAT_EUR_EX_gene_count.txt > str_uniq_LD_NAT_EUR_EX_gene.txt 
    awk '{print $2}' str_uniq_LD_NAT_WAFR_EX_gene_count.txt > str_uniq_LD_NAT_WAFR_EX_gene.txt 

28.3) ALL datasets

<b> UNIONs </b>

    > H6-LD NAT-EAS
    cat str_uniq_H6_NAT_EAS_EX_gene.txt str_uniq_LD_NAT_EAS_EX_gene.txt | sort | uniq > str_uniq_ALL_NAT_EAS_EX_gene.txt
    > H6-LD NAT-EUR
    cat str_uniq_H6_NAT_EUR_EX_gene.txt str_uniq_LD_NAT_EUR_EX_gene.txt | sort | uniq > str_uniq_ALL_NAT_EUR_EX_gene.txt
    > H6-LD NAT-WAFR
    cat str_uniq_H6_NAT_WAFR_EX_gene.txt str_uniq_LD_NAT_WAFR_EX_gene.txt | sort | uniq > str_uniq_ALL_NAT_WAFR_EX_gene.txt
    > H6 NAT-(EAS-EUR-WAFR)
    cat str_uniq_H6_NAT_EAS_EX_gene.txt str_uniq_H6_NAT_EUR_EX_gene.txt str_uniq_H6_NAT_WAFR_EX_gene.txt | sort | uniq > str_uniq_H6_ALL_EX_gene.txt
    > LD NAT-(EAS-EUR-WAFR)
    cat str_uniq_LD_NAT_EAS_EX_gene.txt str_uniq_LD_NAT_EUR_EX_gene.txt str_uniq_LD_NAT_WAFR_EX_gene.txt | sort | uniq > str_uniq_LD_ALL_EX_gene.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_EX_gene.txt str_uniq_H6_NAT_WAFR_EX_gene.txt | sort | uniq > str_uniq_H6_NAT_EUR_WAFR_EX_gene.txt
    > LD NAT-(EUR-WAFR)
    cat str_uniq_LD_NAT_EUR_EX_gene.txt str_uniq_LD_NAT_WAFR_EX_gene.txt | sort | uniq > str_uniq_LD_NAT_EUR_WAFR_EX_gene.txt
    > H6-LD NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_WAFR_EX_gene.txt str_uniq_LD_NAT_EUR_WAFR_EX_gene.txt | sort | uniq > str_uniq_ALL_NAT_EUR_WAFR_EX_gene.txt
    > H6-LD NAT-(EAS-EUR-WAFR) ALL
    cat str_uniq_H6_ALL_EX_gene.txt str_uniq_LD_ALL_EX_gene.txt | sort | uniq > str_uniq_ALL_ALL_EX_gene.txt

<b> Intersections </b>

    > H6 NAT-(EAS-EUR-WAFR)
    cat str_uniq_H6_NAT_EAS_EX_gene.txt str_uniq_H6_NAT_EUR_EX_gene.txt str_uniq_H6_NAT_WAFR_EX_gene.txt | sort | uniq -c > str_non_uniq_H6_ALL_EX_gene_count.txt
    awk '$1==3 {print $2}' str_non_uniq_H6_ALL_EX_gene_count.txt > str_non_uniq_H6_ALL_EX_gene.txt
    > LD NAT-(EAS-EUR-WAFR)
    cat str_uniq_LD_NAT_EAS_EX_gene.txt str_uniq_LD_NAT_EUR_EX_gene.txt str_uniq_LD_NAT_WAFR_EX_gene.txt | sort | uniq -c > str_non_uniq_LD_ALL_EX_gene_count.txt
    awk '$1==3 {print $2}' str_non_uniq_LD_ALL_EX_gene_count.txt > str_non_uniq_LD_ALL_EX_gene.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_EX_gene.txt str_uniq_H6_NAT_WAFR_EX_gene.txt | sort | uniq -d > str_non_uniq_H6_NAT_EUR_WAFR_EX_gene.txt
    > H6 NAT-(EUR-WAFR)
    cat str_uniq_LD_NAT_EUR_EX_gene.txt str_uniq_LD_NAT_WAFR_EX_gene.txt | sort | uniq -d > str_non_uniq_LD_NAT_EUR_WAFR_EX_gene.txt
    > H6-LD NAT-(EUR-WAFR)
    cat str_uniq_H6_NAT_EUR_WAFR_EX_gene.txt str_uniq_LD_NAT_EUR_WAFR_EX_gene.txt | sort | uniq -d > str_non_uniq_ALL_NAT_EUR_WAFR_EX_gene.txt
    > H6-LD NAT-(EAS-EUR-WAFR) ALL
    cat str_uniq_H6_ALL_EX_gene.txt str_uniq_LD_ALL_EX_gene.txt | sort | uniq -d > str_non_uniq_ALL_ALL_EX_gene.txt

