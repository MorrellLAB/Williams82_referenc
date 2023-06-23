# Gmax_275_v2.0 is the primary reference for the USDA BRAG 120 soybean line experiment

# Preparing BED file for filtering of structural variant and SNP calls
* Identify positions where the reference genome has an 'N' and thus single nucleotide or structural variants should not be called


## Creating a bed file of positions with missing data in the reference
* Use the [find_Ns_in_assembly.py](https://github.com/MorrellLAB/morex_reference/blob/master/morex_v3/find_Ns_in_assembly.py) script from Chaochih's [Morex repository](https://github.com/MorrellLAB/morex_reference)

```bash
module load python3/3.8.3_anaconda2020.07_mamba
module avail pigz
cd /panfs/jay/groups/9/morrellp/pmorrell/Apps/CLiu/morex_reference/morex_v3


[//]: Identify strings of Ns in the assembly and write to a BED file
python3 find_Ns_in_assembly.py /panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/Gmax_275_v2.0.fa >/panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/Gmax_275_v2.0_Ns.bed &

[//]: Find softmasked regions of the soybean genome
python3 find_softmasked_in_assembly.py /panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/Gmax_275_v2.0.fa >/scratch.global/pmorrell/test.bed

[//]: Soft and hard masked versions of the Gmax_275_v2.0 genome already exist, but need to decompress
[//]: Create bed files of hard and soft masked positions so they can be excluded from 
cd /panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/
cp -aR Gmax_275_v2.0.hardmasked.fa.gz /scratch.global/pmorrell/ &
cp -aR Gmax_275_v2.0.softmasked.fa.gz /scratch.global/pmorrell/ &
pigz -d /scratch.global/pmorrell/Gmax_275_v2.0.hardmasked.fa.gz
pigz -d /scratch.global/pmorrell/Gmax_275_v2.0.softmasked.fa.gz
python3 find_Ns_in_assembly.py /scratch.global/pmorrell/Gmax_275_v2.0.hardmasked.fa >/panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/Gmax_275_v2.0.hardmasked.bed &
python3 find_softmasked_in_assembly.py /scratch.global/pmorrell/Gmax_275_v2.0.softmasked.fa >/panfs/jay/groups/9/morrellp/shared/References/Reference_Sequences/Soybean/PhytozomeV11/Gmax/assembly/Gmax_275_v2.0.softmasked.bed &
```
