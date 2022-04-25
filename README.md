# vcf-annotation
### 对原始vcf文件进行压缩
```bash
工具：bgzip
bgzip -i clinvar.vcf_
# -i, --index                compress and create BGZF index
```
clinvar.vcf_ ===>  clinvar.vcf_.gz + gnomad.vcf_.gz.gzi

### 建立索引
```bash
工具：tabix
tabix -p vcf clinvar.vcf_.gz
-p, --preset STR           gff, bed, sam, vcf
```
生成gnomad.vcf_.gz.tbi文件

### 利用slivar对vcf文件进行注释
下载注释文件

+ gnomad for hg37 with AF popmax, numhomalts (total and controls only) [here](https://s3.amazonaws.com/slivar/gnomad.hg37.zip)

+ gnomad for hg38 (v3) genomes [here](https://slivar.s3.amazonaws.com/gnomad.hg38.genomes.v3.fix.zip)

+ lifted gnomad exomes+genomes for hg38 with AF popmax, numhomalts (updated in release v0.1.2) [here](https://s3.amazonaws.com/slivar/gnomad.hg38.v2.zip)

+ spliceai scores (maximum value of the 4 scores in spliceai) [here](https://s3.amazonaws.com/slivar/spliceai.hg37.zip)

+ [topmed allele frequencies (via dbsnp)](https://slivar.s3.amazonaws.com/topmed.hg38.dbsnp.151.zip) these can be used with ```INFO.topmed_af```. Useful when analyzing data in hg38 because some variants in hg38 are not visible in GRCh37

```bash
slivar expr \
  --gnotate 压缩的注释文件 \
  -o clinvar.annotated.vcf \
  -v clinvar.vcf_.gz
```
最终生成的clinvar.annotated.vcf文件中，会根据注释文件的不同，在文件INFO的末尾添加参数

+ [也可以通过自己生成注释文件来选择添加特定的参数](https://github.com/brentp/slivar/wiki/make-gnotate)

```bash
slivar make-gnotate \
    --field AF_POPMAX:gnomad_popmax_af \
    --field Hom:gnomad_nhomalt \
    --prefix gnomad.hg38 \
     gnomad.exomes.r2.0.1.sites.GRCh38.noVEP.norm.bcf \
     gnomad.genomes.r2.0.1.sites.GRCh38.noVEP.norm.bcf
```


