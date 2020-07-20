![Imputationserver Logo](logo.png)


[![Build Status](https://api.travis-ci.com/statgen/imputationserver.svg?branch=topmed-imputationserver)](https://travis-ci.com/statgen/imputationserver)
[![Coverage Status](https://coveralls.io/repos/github/genepi/imputationserver/badge.svg)](https://coveralls.io/github/genepi/imputationserver?branch=master)
[![Publication](https://img.shields.io/badge/Published-Nature%20Genetics-26af64.svg?colorB=26af64&style=popout)](https://www.nature.com/articles/ng.3656)
[![Build Status](https://travis-ci.org/genepi/imputationserver.svg?branch=master)](https://travis-ci.org/genepi/imputationserver)
[![codecov](https://codecov.io/gh/genepi/imputationserver/branch/master/graph/badge.svg)](https://codecov.io/gh/genepi/imputationserver)
 <a href="https://twitter.com/intent/follow?screen_name=umimpute"> <img src="https://img.shields.io/twitter/follow/umimpute.svg?style=social" alt="follow on Twitter"></a>

This repository includes the complete source code for the [Michigan Imputation Server](https://imputationserver.sph.umich.edu) workflow based on Minimac4.
The workflow itself is executed with the [Cloudgene workflow system](https://github.com/genepi/cloudgene) for Hadoop MapReduce. The code has been modified to support the [TOPMed Imputation Server](https://imputation.biodatacatalyst.nhlbi.nih.gov)

Michigan Imputation Server consists of several parallelized pipeline steps:

- Quality Control
- QC Report
- Phasing and Imputation
- Compression and Encryption

## Documentation
The documentation is available at http://topmedimpute.readthedocs.io.

## Releases
The release cycle is described [here](RELEASE.md).

## Citation
Please cite this paper if you use Michigan Imputation Server:

> Das S, Forer L, Schönherr S, Sidore C, Locke AE, Kwong A, Vrieze S, Chew EY, Levy S, McGue M, Schlessinger D, Stambolian D, Loh PR, Iacono WG, Swaroop A, Scott LJ, Cucca F, Kronenberg F, Boehnke M, Abecasis GR, Fuchsberger C. [Next-generation genotype imputation service and methods](https://www.ncbi.nlm.nih.gov/pubmed/27571263). Nature Genetics 48, 1284–1287 (2016).


## Contact

Feel free to contact [us](https://imputation.biodatacatalyst.nhlbi.nih.gov/index.html#!pages/contact) in case of any problems.
