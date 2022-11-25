# Qiime2_SingleEnd_deblur
![visitors](https://visitor-badge.glitch.me/badge?page_id=richar1iu.Qiime2_SingleEnd&left_color=blue&right_color=gray)

### 1.Manifest
---
+ all sample in the column "direction" are "forward"

### 2.Demux
---

#### Import sequence
```
qiime tools import \
  --type 'SampleData[SequencesWithQuality]' \
  --input-path yf-manifest \
  --output-path single-end-demux.qza \
  --input-format SingleEndFastqManifestPhred33
```

#### Demux summarize 
```
qiime demux summarize \
  --i-data single-end-demux.qza \
  --o-visualization single-end-demux.qzv
```

### 3.Deblur
---

#### *Firstly*, quality filtering process based on quality scores is applied
```
 qiime quality-filter q-score \
  --i-demux demux.qza \
  --o-filtered-sequences demux-filtered.qza \
  --o-filter-stats demux-filter-stats.qza
``` 
+ view summary statistics
```
 qiime metadata tabulate \
  --m-input-file demux-filter-stats.qza \
  --o-visualization demux-filter-stats.qzv
 ```

 
#### *Subsequently*, quality filter
+ the only parameter need to input is "--p-trim-length n", which truncates the sequences at position n determined by
the result of "single-end-demux.qzv"
 ```
 qiime deblur denoise-16S \
  --i-demultiplexed-seqs demux-filtered.qza \
  --p-trim-length 393 \
  --o-representative-sequences rep-seqs-deblur.qza \
  --o-table table-deblur.qza \
  --p-sample-stats \
  --o-stats deblur-stats.qza
 ```
  
+ view summary statistics
 ```
  qiime deblur visualize-stats \
  --i-deblur-stats deblur-stats.qza \
  --o-visualization deblur-stats.qzv
 ``` 
  
#### *Finally*, extract otutab and represent sequence from table-deblur.qza and rep-seqs-deblur.qza,respectively
``` 
  qiime tools export \
  --input-path table-deblur.qza \
  --output-path feature-table
  
  qiime tools export \
  --input-path rep-seqs-deblur.qza \
  --output-path rep-seq
 ``` 
+ convert biom to txt
```
  biom convert -i feature-table.biom -o otutab.txt --to-tsv
```
