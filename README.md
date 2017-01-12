# combinedVCFParser

This is a bash/awk script that will parse a combined vcf file and for your input SNP of interest output a table of with Het/Ref homo/Alt homo denotation, sample names/IDs, position in vcf, genotype.

Script is useful when you need to quicky assess which sample is Het/Ref homo/Alt homo for a particular SNP of interest.

<pre>

cat >> script.awk <<EOL

#find a line with CHROM in the vcf file, go through the fields, place in hash h1, key=position, value=field content

/#[CHROM|CHR|chrom|chr]/ { for(i = 1; i <= NF; i++) {h1[i] = \$i}}

#find a line with SNP ID, go through the fields, if field contains 0/1 or 0|1 print het, hash value (sample name), position, content 

/$1/{for(i = 1; i <= NF; i++)  
{if (\$i~/^0[\/\|]1/) printf "Heterozygote\t"h1[i]"\t"i"\t"\$i"\n"}}

#find a line with SNP ID, go through the fields, if field contains 0/0 or 0|0 print ref homo, hash value (sample name), position, content

/$1/{for(i = 1; i <= NF; i++)  
{if (\$i~/^0[\/\|]0/) printf "Reference homozygote\t"h1[i]"\t"i"\t"\$i"\n"};}

#find a line with SNP ID, go through the fields, if field contains 1/1 or 1|1 print ref homo, hash value (sample name), position, content

/$1/{for(i = 1; i <= NF; i++)  
{if (\$i~/^1[\/\|]1/) printf "Alternative homozygote\t"h1[i]"\t"i"\t"\$i"\n"};}
EOL

#run awk script with $2 provided as vcf file

awk -f script.awk $2
</pre>

# Usage

<pre>
cat file.vcf 

##fileformat=VCFv4.2
##filedate=20160604
##source="beagle.03May16.862.jar (version 4.1)"
##INFO=<ID=AF,Number=A,Type=Float,Description="Estimated ALT Allele Frequencies">
##INFO=<ID=AR2,Number=1,Type=Float,Description="Allelic R-Squared: estimated squared correlation between most probable REF dose and true REF dose">
##INFO=<ID=DR2,Number=1,Type=Float,Description="Dosage R-Squared: estimated squared correlation between estimated REF dose [P(RA) + 2*P(RR)] and true REF dose">
##INFO=<ID=IMP,Number=1,Type=Flag,Description="Imputed marker">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=DS,Number=1,Type=Float,Description="estimated ALT dose [P(RA) + P(AA)]">
##FORMAT=<ID=GP,Number=G,Type=Float,Description="Estimated Genotype Probability">
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	1060602	1522	1596	2105	2139	2161	2228	2305	2435	2463	2477	2999	2989	3003	3101801	317155
1	84183	rs570923646	C	G	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
1	86028	rs114608975	T	C	.	PASS	.	GT	0|0	0|0	1|1	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	1|0	0|0	0|1|1	0|0
1	86065	rs116504101	G	C	.	PASS	.	GT	0|0	0|0	1|1	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	1|0	0|0	0|1|1	0|0
1	86331	rs115209712	A	G	.	PASS	.	GT	0|0	1|1	0|0	0|0	1|1	0|0	0|0	0|0	0|0	1|1	0|0	0|1	1|1	0|0|0	0|0
1	87021	rs188486692	T	C	.	PASS	.	GT	1|1	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
1	88236	rs186918018	C	T	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
1	88338	rs55700207	G	A	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
1	566875	rs2185539	C	T	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	1|1	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
1	568451	rs78032279	C	T	.	PASS	.	GT	0|0	0|1	0|0	0|0	0|0	0|0	0|0	1|0	0|0	0|1	0|0	0|0	0|0	1|0|0	0|0
1	705882	rs72631875	G	A	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	1|1	1|0|0	0|0
1	713977	rs74512038	C	T	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|1	0|0	0|0	0|0|0	0|0
1	714002	rs143219077	C	G	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|0	1|0	0|0	0|0	0|0|0	0|0
1	714019	rs114983708	A	G	.	PASS	.	GT	0|0	0|0	0|0	0|0	0|0	0|0	0|0	0|1	0|0	0|0	0|0	0|0	0|0	0|0|0	0|0
</pre>

Run combinedVCFParser.sh with SNP ID and vcf file name as two arguments
<pre>
chmod 755 combinedVCFParser.sh
./combinedVCFParser.sh rs115209712 file.vcf

Heterozygote	2999	21	0|1
Reference homozygote	1060602	10	0|0
Reference homozygote	1596	12	0|0
Reference homozygote	2105	13	0|0
Reference homozygote	2161	15	0|0
Reference homozygote	2228	16	0|0
Reference homozygote	2305	17	0|0
Reference homozygote	2435	18	0|0
Reference homozygote	2477	20	0|0
Reference homozygote	3003	23	0|0|0
Reference homozygote	3101801	24	0|0
Alternative homozygote	1522	11	1|1
Alternative homozygote	2139	14	1|1
Alternative homozygote	2463	19	1|1
Alternative homozygote	2989	22	1|1
</pre>

