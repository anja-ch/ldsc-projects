# ldsc-projects

1. make the `ldsc` folder on your *Desktop* and within that make the folders `myfiles` `sumstats` and `results`
2. Put the sumstats files that you have downloaded into the `sumstats` folder
3. open terminal and navigate to the ldsc folder on your desktop
`cd Desktop/ldsc`
4. download the ldsc files
`git clone https://github.com/bulik/ldsc`
5. Put the reference files in the new ldsc folder that has been created. You will have a folder called 'ldsc' on your desktop now, and within that folder is another folder called 'ldsc'. Put the files in the second folder.  
6. install anaconda from this website: https://www.anaconda.com/download
You will need to choose the correct installer for your type of mac - either intel or M1/M2
You can find out which you have by clicking the apple in the top left of your screen and clicking 'about this mac' - there you can see which type of chip you have by whether it says M1, M2 or intel
Once you have installed Anaconda, it will open. You can just close that straight away
7. Install python2.7 from this website: - https://www.python.org/downloads/release/python-2718/
click `macOS 64-bit installer`
open the installer when it downloads and click through to install (press cintinue, install and agree)
8. close the terminal and open it again. Right-click on the terminal icon at the bottom of the screen and click quit - dont just close and open it.
9. Navigate to the ldsc folder again
`cd Desktop/ldsc`
10. type the following commands. after each, press enter and wait for it to finish installing. you will know it has finished installing when there is a line at the bottom of the terminal window that ends in a `%` and a rectangle indicating that you can type a new command`
`python2 -m pip install virtualenv`
`python2 -m virtualenv myenv`
`source myenv/bin/activate`
`pip install numpy`
`pip install bitarray`
`pip install pandas`
`pip install scipy`
`mv sumstats ./ldsc`
`mv myfiles ./ldsc`
`mv results ./ldsc`
11. You can't do the next step if your sumstats aren't in a text file format with the right headings. You can check this by going into the file explorer and right clicking on your sumstats file, clicking 'open with' and 'TextEdit'. You will need the following headings:
- A unique identifier (e.g., the rs number) -> call it SNPid
This may be called SNP, variant, RSID, rs etc. 
- Allele 1 (effect allele) -> call it Allele1
- Allele 2 (non-effect allele) -> call it Allele2
- Sample size (which often varies from SNP to SNP) -> call it N
You may have two sample sizes if your study is a case/control design. This would be the case if the phenotype that youre looking at is binary - you either have it or you dont (e.g. alzheimers, bipolar). You will have one sample size if your phenotype is measured continuously along a scale (e.g. height - you can't have/not have a height!)
If your study is case/control you will have a column for the case N and a column for the control N. These should be called cases_N and controls_N respectively. 
- A P-value -> call it P-value
- A signed summary statistic (beta, OR, log odds, Z-score, etc) -> call it beta for our GEMS continuous phenotypes, or check in the paper to which statistics the effect refers to beta, OR, Z-score and call it accordingly)
Similar to the above sample size stipulation, if your study is case/control, you will have an `OR` column. if you're looking at a continuous trait, you will have a `beta` column.
11. Munge the sumstats. You will need to edit the command below with the name of your file and the name that you want your output file to have
`python munge_sumstats.py --sumstats ./sumstats/pheno1_gwas.txt --chunksize 50000 --out ./myfiles/munged_pheno1 --merge-alleles w_hm3.snplist`
where `pheno_1_gwas.txt` is the name of the sumstats file and `munged_pheno1` is the name that you want your munged sumstats to have
Do this command for both of the phenotypes that you are working with, replacing the file names. 
12. Compute the SNP heritability
`python ldsc.py --h2 ./myfiles/munged_pheno1.sumstats.gz --ref-ld-chr eur_w_ld_chr/ --w-ld-chr eur_w_ld_chr/ --out ./results/pheno1_SNPh2`
Where `munged_pheno1` is replaced with whatever you put after the --out flag in the munge step and `pheno1_SNPh2` is what you want your output file to be
13. compute the genetic correlation between the two phenotypes
`python ldsc.py --rg ./myfiles/munged_pheno1.sumstats.gz,./myfiles/munged_pheno2.sumstats.gz --ref-ld-chr eur_w_ld_chr/ --w-ld-chr eur_w_ld_chr/ --samp-prev [add sample prevalence, i.e. number of cases devided by number of controls, derived from paper if the trait is not continuous, for continuous traits add: nan] --pop-prev [add population prevalence derived from paper if the trait is not continuous, for continuous traits add: nan] --out ./results/pheno1_pheno2_corr`





