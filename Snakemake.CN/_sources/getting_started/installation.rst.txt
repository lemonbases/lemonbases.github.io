.. _getting_started-installation:

============
安装
============

Snakemake可以通过PyPi以及Bioconda安装，也可以从源代码安装。
您可以使用以下方法安装Snakemake。

.. _conda-install:

Conda安装
======================

这是安装Snakemake的推荐方法，因为它还使Snakemake能够 :ref:`处理工作流程的软件依赖` 。

首先，安装Miniconda Python3发行版。请参阅 `此处 <https://conda.io/docs/install/quick.html>`_ 获取安装说明。确保

* 安装 **Python3** 版本你的Miniconda;
* 将conda加入环境变量

然后，安装Snakemake

.. code-block:: console

    $ conda install -c bioconda -c conda-forge snakemake

来自 `Bioconda <https://bioconda.github.io>`_ channel。可以安装最小版本的Snakemake

.. code-block:: console

    $ conda install -c bioconda -c conda-forge snakemake-minimal

Note that Snakemake is available via Bioconda for historical, reproducibility, and continuity reasons.
However, it is easy to combine Snakemake installation with other channels, e.g., by prefixing the package name with ``::bioconda``, i.e.,

.. code-block:: console

    $ conda install -c conda-forge bioconda::snakemake bioconda::snakemake-minimal

全局安装
===================

With a working Python ``>=3.5`` setup, installation of Snakemake can be performed by issuing

.. code-block:: console

    $ easy_install3 snakemake

or

.. code-block:: console

    $ pip3 install snakemake

in your terminal.


虚拟环境安装
========================

To create an installation in a virtual environment, use the following commands:

.. code-block:: console

    $ virtualenv -p python3 .venv
    $ source .venv/bin/activate
    $ pip install snakemake


源码安装
======================

We recommend installing Snakemake into a virtualenv instead of globally.
Use the following commands to create a virtualenv and install Snakemake.
Note that this will install the development version and as you are installing from the source code, we trust that you know what you are doing and how to checkout individual versions/tags.

.. code-block:: console

    $ git clone https://bitbucket.org/snakemake/snakemake.git
    $ cd snakemake
    $ virtualenv -p python3 .venv
    $ source .venv/bin/activate
    $ python setup.py install

You can also use ``python setup.py develop`` to create a "development installation" in which no files are copied but a link is created and changes in the source code are immediately visible in your ``snakemake`` commands.
