## 画图

### 数据说明

数据文件包含6列，分别为：

1. gene_id
2. logFC
3. logCPM
4. PValue
5. FDR

例如：

```
gene_id   logFC   logCPM  PValue  FDR
ENSG0000183762    15.671  8.85    0   0
ENSG0000183763    14.561  7.74    0   0
ENSG0000183764    13.451  6.63    0   0
```

### 参数说明


```
usage: src/volcano_map.R [--] [--help] [--opts OPTS] [--fdr FDR] [--logfc LOGFC] [--alpha ALPHA] [--size SIZE] [--dclr DCLR] [--nclr NCLR] [--uclr UCLR] [--fmt FMT] [--png PNG] [--pdf PDF] data_file

Volcano Map

positional arguments:
  data_file                     data file

flags:
  -h, --help                    show this help message and exit

optional arguments:
  -x, --opts OPTS                       RDS file containing argument values
  -f, --fdr FDR                 FDR [default: 0.01]
  -l, --logfc LOGFC                     logFC [default: 2]
  -a, --alpha ALPHA                     alpha [default: 0.4]
  -s, --size SIZE                       size [default: 3.5]
  -d, --dclr DCLR                       down color [default: blue]
  -n, --nclr NCLR                       no significant color [default: grey]
  -u, --uclr UCLR                       up color [default: red]
  --fmt FMT                     data file format [default: tsv]
  -p, --png PNG                 output png file
  --pdf PDF                     output pdf file
```

### 示例

```
$ src/volcano_map.R --png figs/volcano_map.png --pdf figs/volcano_map.pdf data/edgerAML.tsv
```