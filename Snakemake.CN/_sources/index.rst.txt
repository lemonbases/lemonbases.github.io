.. _manual-main:

=========
Snakemake
=========

.. image:: https://img.shields.io/conda/dn/bioconda/snakemake.svg?label=Bioconda
    :target: https://bioconda.github.io/recipes/snakemake/README.html

.. image:: https://img.shields.io/pypi/pyversions/snakemake.svg
    :target: https://www.python.org

.. image:: https://img.shields.io/pypi/v/snakemake.svg
    :target: https://pypi.python.org/pypi/snakemake

.. image:: https://quay.io/repository/snakemake/snakemake/status
       :target: https://quay.io/repository/snakemake/snakemake

.. image:: https://circleci.com/bb/snakemake/snakemake/tree/master.svg?style=shield
    :target: https://circleci.com/bb/snakemake/snakemake/tree/master

.. image:: https://img.shields.io/badge/stack-overflow-orange.svg
    :target: http://stackoverflow.com/questions/tagged/snakemake

.. image:: https://img.shields.io/twitter/follow/johanneskoester.svg?style=social&label=Follow
    :target: https://twitter.com/search?l=&q=%23snakemake%20from%3Ajohanneskoester

Snakemake工作流程管理系统是为创建 **可重复和可扩展的** 数据分析的工具。工作流程使用人类可读的基于Python的语言描述。 它们可以无缝扩展到服务器，集群，网格和云环境，而无需修改工作流程定义。 最后，Snakemake工作流程可能包含所需软件的说明，这些软件将自动部署到任何执行环境。

.. _manual-quick_example:

-------------
快速上手
-------------

Snakemake工作流程本质上是由定义 **规则** 的声明性代码扩展而来的Python脚本。规则描述了如何从 **输入文件** 创建 **输出文件** 。

.. code-block:: python

    rule targets:
        input:
            "plots/dataset1.pdf",
            "plots/dataset2.pdf"

    rule plot:
        input:
            "raw/{dataset}.csv"
        output:
            "plots/{dataset}.pdf"
        shell:
            "somecommand {input} {output}"


* 与GNU Make类似，可以根据顶部的伪规则指定目标;
* 对于每个目标文件和中间文件，可以创建规则定义如何从输入文件创建它们;
* Snakemake通过匹配文件名来确定规则依赖;
* 输入和输出文件可以包含多个命名通配符。
* 规则可以使用shell命令，Python代码或外部Python或R脚本，从输入文件创建输出文件;
* Snakemake工作流程可以在 **工作站**, **集群**, **网格** 和 **云环境** 而无需修改。作业调度可以限制任意资源，例如，可用的CPU内核，内存或GPU;
* Snakemake可以使用 `Conda <https://conda.io>`_ 或 `Singularity <http://singularity.lbl.gov/>`_ 自动部署工作流城所需的软件依赖;
* Snakemake可以使用Amazon S3，Google Storage，Dropbox，FTP，WebDAV，SFTP和iRODS访问输入或输出文件，也可通过HTTP和HTTPS访问输入文件。

.. _main-getting-started:

---------------
新手入门
---------------

获得关于Snakemake初步印象，可参阅 `简介幻灯片 <https://slides.com/johanneskoester/snakemake-short>`_ 或观看 `演示视频 <https://youtu.be/hPrXcUUp70Y>`_ 。
有关Snakemake的新闻通过 `Twitter <https://twitter.com/search?l=&q=%23snakemake%20from%3Ajohanneskoester>`_ 发布。
学习Snakemake，请参照 :ref:`Tutorial` ，然后参阅 :ref:`FAQ <project_info-faq>` 。

.. _main-support:

-------
支持
-------

* 有关版本，请参阅 :ref:`Changelog <changelog>` 。
* 查看 :ref:`常见问题解答（FAQ）<project_info-faq>` 。
* 如有疑问，请发贴在 `stack overflow <http://stackoverflow.com/questions/tagged/snakemake>`_ 。
* 使用 `mailing list <https://groups.google.com/forum/#!forum/snakemake>`_ 与其他Snakemake用户讨论。 **请不要在那里发布问题。使用stack overflow提问题。**
* 对于错误和新功能请求，请使用 `issue tracker <https://bitbucket.org/snakemake/snakemake/issues>`_ 。
* 有关贡献，请访问 `bitbucket <https://bitbucket.org/snakemake/snakemake>`_，并阅读 :ref:`guidelines <project_info-contributions>` 。

--------
引用
--------

`Köster, Johannes and Rahmann, Sven. "Snakemake - A scalable bioinformatics workflow engine". Bioinformatics 2012. <http://bioinformatics.oxfordjournals.org/content/28/19/2520>`_

查看 :doc:`引用 <project_info/citations>` 获取更多信息.

---------
相关资源
---------

`Snakemake Wrappers Repository <https://snakemake-wrappers.readthedocs.org>`_
    Snakemake Wrapper Repository是一个可重复使用的包装器集合，可以快速使用Snakemake规则和工作流程中的流行工具。

`Snakemake Workflows Project <https://github.com/snakemake-workflows/docs>`_
    该项目提供了一系列高质量的模块化和可重复使用的工作流程。
    提供的代码还应作为如何使用Snakemake构建生产工作流程的最佳实践。
    邀请每位用户contribute。

`Snakemake Profiles Project <https://github.com/snakemake-profiles/doc>`_
    该项目为各种执行环境提供Snakemake配置文件。
    如果找不到，请考虑contribute。

`Bioconda <https://bioconda.github.io/>`_
    通过定义使用的软件版本和提供二进制文件，Snakemake可以使用Bioconda创建完全可重现的工作流程。


.. project_info-publications_using:

----------------------------
使用Snakemake的文献
----------------------------

下文是使用Snakemake进行分析的 **不完整列表**。请考虑添加自己的相应文献。

* Doris et al. 2018. `Spt6 is required for the fidelity of promoter selection <https://doi.org/10.1016/j.molcel.2018.09.005>`_. Molecular Cell.
* Karlsson et al. 2018. `Four evolutionary trajectories underlie genetic intratumoral variation in childhood cancer <https://www.nature.com/articles/s41588-018-0131-y>`_. Nature Genetics.
* Planchard et al. 2018. `The translational landscape of Arabidopsis mitochondria <https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gky489/5033161>`_. Nucleic acids research.
* Schult et al. 2018. `Effect of UV irradiation on Sulfolobus acidocaldarius and involvement of the general transcription factor TFB3 in the early UV response <https://academic.oup.com/nar/article/46/14/7179/5047281>`_. Nucleic acids research.
* Goormaghtigh et al. 2018. `Reassessing the Role of Type II Toxin-Antitoxin Systems in Formation of Escherichia coli Type II Persister Cells <https://mbio.asm.org/content/mbio/9/3/e00640-18.full.pdf>`_. mBio.
* Ramirez et al. 2018. `Detecting macroecological patterns in bacterial communities across independent studies of global soils <https://www.nature.com/articles/s41564-017-0062-x>`_. Nature microbiology.
* Amato et al. 2018. `Evolutionary trends in host physiology outweigh dietary niche in structuring primate gut microbiomes <https://www.nature.com/articles/s41396-018-0175-0>`_. The ISME journal.
* Uhlitz et al. 2017. `An immediate–late gene expression module decodes ERK signal duration <http://msb.embopress.org/content/13/5/928>`_. Molecular Systems Biology.
* Akkouche et al. 2017. `Piwi Is Required during Drosophila Embryogenesis to License Dual-Strand piRNA Clusters for Transposon Repression in Adult Ovaries <http://www.sciencedirect.com/science/article/pii/S1097276517302071>`_. Molecular Cell.
* Beatty et al. 2017. `Giardia duodenalis induces pathogenic dysbiosis of human intestinal microbiota biofilms <https://www.ncbi.nlm.nih.gov/pubmed/28237889>`_. International Journal for Parasitology.
* Meyer et al. 2017. `Differential Gene Expression in the Human Brain Is Associated with Conserved, but Not Accelerated, Noncoding Sequences <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5400397/>`_. Molecular Biology and Evolution.
* Lonardo et al. 2017. `Priming of soil organic matter: Chemical structure of added compounds is more important than the energy content <http://www.sciencedirect.com/science/article/pii/S0038071716304539>`_. Soil Biology and Biochemistry.
* Beisser et al. 2017. `Comprehensive transcriptome analysis provides new insights into nutritional strategies and phylogenetic relationships of chrysophytes <https://peerj.com/articles/2832/>`_. PeerJ.
* Piro et al 2017. `MetaMeta: integrating metagenome analysis tools to improve taxonomic profiling <https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-017-0318-y>`_. Microbiome.
* Dimitrov et al 2017. `Successive DNA extractions improve characterization of soil microbial communities <https://peerj.com/articles/2915/>`_. PeerJ.
* de Bourcy et al. 2016. `Phylogenetic analysis of the human antibody repertoire reveals quantitative signatures of immune senescence and aging <http://www.pnas.org/content/114/5/1105.short>`_. PNAS.
* Bray et al. 2016. `Near-optimal probabilistic RNA-seq quantification <http://www.nature.com/nbt/journal/v34/n5/abs/nbt.3519.html>`_. Nature Biotechnology.
* Etournay et al. 2016. `TissueMiner: a multiscale analysis toolkit to quantify how cellular processes create tissue dynamics <https://elifesciences.org/content/5/e14334>`_. eLife Sciences.
* Townsend et al. 2016. `The Public Repository of Xenografts Enables Discovery and Randomized Phase II-like Trials in Mice <http://www.cell.com/cancer-cell/abstract/S1535-6108%2816%2930090-3>`_. Cancer Cell.
* Burrows et al. 2016. `Genetic Variation, Not Cell Type of Origin, Underlies the Majority of Identifiable Regulatory Differences in iPSCs <http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1005793>`_. PLOS Genetics.
* Ziller et al. 2015. `Coverage recommendations for methylation analysis by whole-genome bisulfite sequencing <http://www.nature.com/nmeth/journal/v12/n3/full/nmeth.3152.html>`_. Nature Methods.
* Li et al. 2015. `Quality control, modeling, and visualization of CRISPR screens with MAGeCK-VISPR <https://genomebiology.biomedcentral.com/articles/10.1186/s13059-015-0843-6>`_. Genome Biology.
* Schmied et al. 2015. `An automated workflow for parallel processing of large multiview SPIM recordings <http://bioinformatics.oxfordjournals.org/content/32/7/1112>`_. Bioinformatics.
* Chung et al. 2015. `Whole-Genome Sequencing and Integrative Genomic Analysis Approach on Two 22q11.2 Deletion Syndrome Family Trios for Genotype to Phenotype Correlations <http://onlinelibrary.wiley.com/doi/10.1002/humu.22814/full>`_. Human Mutation.
* Kim et al. 2015. `TUT7 controls the fate of precursor microRNAs by using three different uridylation mechanisms <http://emboj.embopress.org/content/34/13/1801.long>`_. The EMBO Journal.
* Park et al. 2015. `Ebola Virus Epidemiology, Transmission, and Evolution during Seven Months in Sierra Leone <http://doi.org/10.1016/j.cell.2015.06.007>`_. Cell.
* Břinda et al. 2015. `RNF: a general framework to evaluate NGS read mappers <http://bioinformatics.oxfordjournals.org/content/early/2015/09/30/bioinformatics.btv524>`_. Bioinformatics.
* Břinda et al. 2015. `Spaced seeds improve k-mer-based metagenomic classification <http://bioinformatics.oxfordjournals.org/content/early/2015/08/10/bioinformatics.btv419>`_. Bioinformatics.
* Spjuth et al. 2015. `Experiences with workflows for automating data-intensive bioinformatics <http://www.biologydirect.com/content/10/1/43>`_. Biology Direct.
* Schramm et al. 2015. `Mutational dynamics between primary and relapse neuroblastomas <http://www.nature.com/ng/journal/v47/n8/full/ng.3349.html>`_. Nature Genetics.
* Berulava et al. 2015. `N6-Adenosine Methylation in MiRNAs <http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0118438>`_. PLOS ONE.
* The Genome of the Netherlands Consortium 2014. `Whole-genome sequence variation, population structure and demographic history of the Dutch population <http://www.nature.com/ng/journal/v46/n8/full/ng.3021.html>`_. Nature Genetics.
*  Patterson et al. 2014. `WhatsHap: Haplotype Assembly for Future-Generation Sequencing Reads <http://online.liebertpub.com/doi/10.1089/cmb.2014.0157>`_. Journal of Computational Biology.
* Fernández et al. 2014. `H3K4me1 marks DNA regions hypomethylated during aging in human stem and differentiated cells <http://genome.cshlp.org/content/25/1/27.long>`_. Genome Research.
* Köster et al. 2014. `Massively parallel read mapping on GPUs with the q-group index and PEANUT <https://peerj.com/articles/606/>`_. PeerJ.
* Chang et al. 2014. `TAIL-seq: Genome-wide Determination of Poly(A) Tail Length and 3′ End Modifications <http://www.cell.com/molecular-cell/abstract/S1097-2765(14)00121-X>`_. Molecular Cell.
* Althoff et al. 2013. `MiR-137 functions as a tumor suppressor in neuroblastoma by downregulating KDM1A <http://onlinelibrary.wiley.com/doi/10.1002/ijc.28091/abstract;jsessionid=33613A834E2A2FDCCA49246C23DF777E.f04t02>`_. International Journal of Cancer.
* Marschall et al. 2013. `MATE-CLEVER: Mendelian-Inheritance-Aware Discovery and Genotyping of Midsize and Long Indels <http://bioinformatics.oxfordjournals.org/content/29/24/3143.long>`_. Bioinformatics.
* Rahmann et al. 2013. `Identifying transcriptional miRNA biomarkers by integrating high-throughput sequencing and real-time PCR data <http://www.sciencedirect.com/science/article/pii/S1046202312002605>`_. Methods.
* Martin et al. 2013. `Exome sequencing identifies recurrent somatic mutations in EIF1AX and SF3B1 in uveal melanoma with disomy 3 <http://www.nature.com/ng/journal/v45/n8/full/ng.2674.html>`_. Nature Genetics.
* Czeschik et al. 2013. `Clinical and mutation data in 12 patients with the clinical diagnosis of Nager syndrome <http://link.springer.com/article/10.1007%2Fs00439-013-1295-2>`_. Human Genetics.
* Marschall et al. 2012. `CLEVER: Clique-Enumerating Variant Finder <http://bioinformatics.oxfordjournals.org/content/28/22/2875.long>`_. Bioinformatics.


.. toctree::
   :caption: Getting started
   :name: getting_started
   :hidden:
   :maxdepth: 1

   getting_started/installation
   getting_started/examples
   tutorial/tutorial
   tutorial/short


.. toctree::
  :caption: Executing workflows
  :name: execution
  :hidden:
  :maxdepth: 1

  executable

.. toctree::
    :caption: Defining workflows
    :name: snakefiles
    :hidden:
    :maxdepth: 1

    snakefiles/writing_snakefiles
    snakefiles/rules
    snakefiles/configuration
    snakefiles/modularization
    snakefiles/remote_files
    snakefiles/utils
    snakefiles/deployment
    snakefiles/reporting


.. toctree::
    :caption: API Reference
    :name: api-reference
    :hidden:
    :maxdepth: 1

    api_reference/snakemake
    api_reference/snakemake_utils
    api_reference/internal/modules


.. toctree::
    :caption: Project Info
    :name: project-info
    :hidden:
    :maxdepth: 1

    project_info/citations
    project_info/more_resources
    project_info/faq
    project_info/contributing
    project_info/authors
    project_info/history
    project_info/license
