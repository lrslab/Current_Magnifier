# Welcome to the nanoCEM's documentation !


 <center>![logo](logo.png "nanoCEM") </center>


<center><span style="color:#0084A9;">nanoCEM (nanopore Current Events Magnifier) </span></center>

A Python command-line tool designed to  visualize and analyse multiple statistical features of current events from nanopore sequencing data.

## Installation
<a href="https://pypi.python.org/pypi/nanoCEM" rel="pypi">![PyPI](https://img.shields.io/pypi/v/nanoCEM?color=green) </a>

Before pip install, make sure you have installed the `samtools`(>=1.16) , `f5c`(>=1.4), `slow5tools`(>=1.1.0) and `minimap2`(>=2.17),

    conda install samtools=1.16 minimap2 f5c=1.4 slow5tools -c conda-forge -c bioconda 

To install the latest nanoCEM

    pip install nanoCEM

And install from the resource

    git clone https://github.com/lrslab/nanoCEM.git
    cd nanoCEM/
    pip install .
To install nanoCEM from docker,

    docker pull zhihaguo/nanocem_env
    
To check the version of nanoCEM, run:

    pip list | grep nanoCEM


## Content

* [Quick start](tutorials.md)
* [Output files description](output_format.md)
* [Data preparation from raw reads](preparation.md)
* [Command line arguments](argument.md)
* [Description of statistical features](statistics.md)
* [Adaption for preprocessing methods](adaption.md)
* [An example for application](example.md)
* [Contacts](contact.md)
