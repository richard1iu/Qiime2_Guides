# Qiime2_SingleEnd

### 1.manifest
**direction：forward**

### 2.demux
```
# import sequence
qiime tools import \
  --type 'SampleData[SequencesWithQuality]' \
  --input-path yf-manifest \
  --output-path single-end-demux.qza \
  --input-format SingleEndFastqManifestPhred33

# demux summarize 
qiime demux summarize \
  --i-data paired-end-demux.qza \
  --o-visualization paired-end-demux.qzv
```

### 3.deblur
```
qiime quality-filter q-score \
 --i-demux demux.qza \
 --o-filtered-sequences demux-filtered.qza \
 --o-filter-stats demux-filter-stats.qza
 
 qiime deblur denoise-16S \
  --i-demultiplexed-seqs demux-filtered.qza \
  --p-trim-length 120 \
  --o-representative-sequences rep-seqs-deblur.qza \
  --o-table table-deblur.qza \
  --p-sample-stats \
  --o-stats deblur-stats.qza
```
