smoke-alarm-risk
================

This repository contains code and documentation for generating scores that help indicate whether or not the residents of a census block group have a high risk for its residents not having smoke alarms. You can read more about the genesis of this project [here](http://blog.enigma.io/open-data-and-public-safety/). This analysis is made possible by mapping common variables in the American Housing Survery and the American Community Survey. You can see details on how these mappings are done [in this repository](https://github.com/enigma-io/ahs-acs).

## Getting Started.

### Installation
First clone the repository and navigate to the project's root directory:

```bash
git clone https://github.com/enigma-io/smoke-alarm-risk.git
cd smoke-alarm-risk
```

This project is written in [`R`](http://www.r-project.org/) and depends on the following packages:
    
- `bit64`
- `plyr`
- `ggplot2`
- `data.table`
- `knitr`
- `reshape2`
- `scales`
- `bigrf`

You can install these packages by running the following command in the project's root directory:

```bash
$ make init
```

### Get the data

This project also requires five csv files (two of which - the ACS and the AHS, are generated by [this project](https://github.com/enigma-io/ahs-acs)). You can grab these files from the web by running the following command:

```bash
$ make fetch_data
```

*WARNING*: This may take a while. The ACS file is ~ 2 GB.

Once this is finished, you should see five files in [`data/`](data/):

- `acs-bg-at-risk-population.csv` - percent of population under the age of 5 and over the age of 65 per block group.
- `acs-bg-population.csv` - total population per block group.
- `msa80-bg.csv` - A lookup of 1980 MSA IDs to 2010 Block Group IDs.
- `acs.csv` - an export of the ACS with variables mapped to the AHS. ([see this repo](https://github.com/enigma-io/ahs-acs)) 
- `ahs.csv` - an export of the AHS with variables mapped to the ACS. ([see this repo](https://github.com/enigma-io/ahs-acs)) 

Once you've run got these files, you should be all set to generate risk scores.

### Generate the risk scores.

```bash
$ make model
```

Under the hood, this command executes [`index.Rmd`](index.Rmd), which is a [RMarkdown](http://rmarkdown.rstudio.com/) file. It contains notes on each step of our process and generates plots which visualize our results. You can see the finalized output of the modeling process on [RPubs](http://rpubs.com/enigma/smoke-alarm-risk). You can view a local version of the output by typing this command:

```bash
$ make view
```

If you open a web browser and navigate to [http://localhost:8000/](http://localhost:8000/) you should see the report on the modeling process.

### Get the output.

When the modeling script has finished executing, the risk scores per block group will be output to [`data/smoke-alarm-risk-scores.csv`](data/smoke-alarm-risk-scores.csv). These also include total population and at-risk population (< 5 years old, > 65 years old) per block group.


### Known Issues

`bigrf` seems to have a memory leak when executed within RStudio. This can be avoided by simply using the `make model` command. SEE: https://github.com/aloysius-lim/bigrf/issues/16.

