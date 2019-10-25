# 火山图

## 依赖

在流程说明文档中，要对运行本流程需要安装的依赖进行说明。

1. R版本: 3.6.1
2. 依赖R包：

    |      名称   |    版本   |
    |------------|----------|
    |   ggplot2  |   3.2.1  |
    | argparser  |     0.4  |
    |  openxlsx  | 4.1.0.1  |
    
## 流程与项目

在生物信息项目中，我们利用代码对数据进行分析，得出所需要的结果。这个过程我们
可以抽象为：代码和数据两个主体。

一个可复用的代码可以操作不同的数据。

为了增加代码的可复用性，在数据分析项目中，常用的做法的是把代码从项目中抽离出来。

为了使代码结构化，我们可以把代码分割为几个部分，每个部分代表一个步骤。

一般情况下，开发流程的研发人员并不参与生产环境中的具体分析。为了使生产人员更好地
使用流程进行分析，研发人员必须要写详尽的流程文档。如果一个步骤是高度同一性的（也就是
说这一步骤对于不同的数据，参数都可以相同，也没有探索性的代码），那个文档可以只具体这一
步骤。如果一个步骤有探索性的代码、对于不同的数据参数有可能不同（以下简称这种代码为探索性代码），
为了减轻生产人员的误操作，也防止生产人员操作过程中误操作，文档要对每一行探索性代码进行说明。
以便生产人员可以选择合适的参数、运行合适的代码。

例如，在使用`limma`进行差异表达分析时，分组为2和大于2差异分析的代码是有一些不同的。这时文档
就必须对分组为2时应该选择哪些代码、分组大于2应该选择哪些代码做出解释。

这些每一步骤的代码、每一步骤的文档一起组成一个流程。

## 项目的目录结构

项目的目录结构参照[BMG191024001-volcano_map_edgerAML](https://github.com/sxropensource/BMG191024001-volcano_map_edgerAML)。

## 流程的目录结构

```
volcano_map
├── README.md
├── data
│   └── edgerAML.tsv
├── doc
│   └── volcano_map.md
├── figs
│   ├── volcano_map.pdf
│   └── volcano_map.png
├── output
└── src
    └── volcano_map.R
```

1. 流程根目录的`README.md`文件对流程进行说明，可以导向`doc`目录下任一步骤的文档。
2. 在流程开发过程中，可以建立项目分析过程中的目录（`data`、`figs`、`output`）

    1. `data`：示例数据
    2. `figs`：示例输出图片
    3. `output`：示例输出
3. 流程开发过程中如果需要输入数据，这些数据应当在data目录中存在。一般情况下不允许使用
   `data`目录不存在的数据。项目分析过程中亦然。
4. 一般情况下，流程开发过程中如果需要输出图片请输出到`figs`目录。项目分析过程中亦然。
5. 一般情况下，流程开发过程中如果需要输出数据请输出到`output`目录。项目分析过程中亦然。
6. 流程开发过程中牵扯到文件，可以有以下几种方法指定路径：

    1. 通过命令行参数指定（推荐使用`argparser`）
    
        例如，流程代码可以这样写：
        
        ```r
        #!/usr/bin/env Rscript
        library(argparser, quietly=TRUE)

        parser <- arg_parser("Volcano Map")
        parser <- add_argument(parser, "--fdr", help="FDR", type="numeric", default=0.01)
        parser <- add_argument(parser, "--logfc", help="logFC", type="numeric", default=2)
        parser <- add_argument(parser, "--data_file", help="data file")
        parser <- add_argument(parser, "--fmt", help="data file format", default="tsv")

        argv <- parse_args(parser)

        data <- read.delim(argv$data_file)
        ```
        
        运行的时候可以这样指定：
        ```
        $ Rscript volcano_map.R --data-file data/data-file.tsv
        ```
    2. 使用相对于项目根目录的相对目录
    
        ```r
        dataFile <- "data/data-file.tsv"
        data <- read.delim(dataFile)
        ```
    3. 先指定项目根目录，然后通过`file.path`合成绝对路径。
    
        ```r
        basedir <- "/home/bmg/Projects/BMG191024001-gse29272_dge"
        dataFile <- file.path(basedir, "data", "data-file.tsv")
        data <- read.delim(dataFile)
        ```
    不包含探索性代码的步骤推荐使用方法1，其次是方法2、方法3。这三种方法的好处是，步骤代码不依赖外部环境，
    生产人员只要建立相应的项目目录结构就可以进行分析。方法3也只需要更改`basedir`。
6. 每个步骤都必须在`doc`目录下建立步骤说明。有必要的情况下还需要更详尽的说明。

## 步骤

1. [画图](doc/volcano_map.md)
