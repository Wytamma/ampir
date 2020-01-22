
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Introduction to ampir

The **ampir** (short for **a**nti**m**icrobial **p**eptide prediction
**i**n **r** ) package was designed to be a fast and user-friendly
method to predict AMPs (antimicrobial peptides) from large protein
dataset. **ampir** uses a *supervised statistical machine learning*
approach to predict AMPs. It incorporates a support vector machine
classification model that has been trained on publicly available
antimicrobial peptide data.

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/Legana/ampir.svg?branch=master)](https://travis-ci.org/Legana/ampir)
[![codecov](https://codecov.io/gh/Legana/ampir/branch/master/graph/badge.svg)](https://codecov.io/gh/Legana/ampir)
[![License: GPL
v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
[![CRAN\_Release\_Badge](http://www.r-pkg.org/badges/version-ago/ampir)](https://CRAN.R-project.org/package=ampir?color=yellow)
![CRAN\_Download\_Badge](http://cranlogs.r-pkg.org/badges/grand-total/ampir?color=red)
<!-- badges: end -->

## Installation

Install from CRAN:

``` r
install.packages("ampir")
```

Or install the development version from GitHub:

``` r
# install.packages("devtools")
devtools::install_github("Legana/ampir")
```

``` r
library(ampir)
```

# Background

The **ampir** (short for **a**nti**m**icrobial **p**eptide prediction
**i**n **r** ) package was designed to be a fast and user-friendly
method to predict antimicrobial peptides (AMPs) from any given size
protein dataset. **ampir** uses a *supervised statistical machine
learning* approach to predict AMPs. It incorporates a support vector
machine classification model that has been trained on publicly available
antimicrobial peptide data.

## Usage

Standard input to **ampir** is a `data.frame` with sequence names in the
first column and protein sequences in the second column.

Read in a FASTA formatted file as a `data.frame` with
`read_faa()`

``` r
my_protein_df <- read_faa(system.file("extdata/bat_protein.fasta", package = "ampir"))
```

| seq\_name     | seq\_aa                                        |
| :------------ | :--------------------------------------------- |
| G1P6H5\_MYOLU | MALTVRIQAACLLLLLLASLTSYSLLLSQTTQLADLQTQDTAGAT… |

Calculate the probability that each protein is an antimicrobial peptide
with `predict_amps()`

*Note that amino acid sequences that are shorter than five amino acids
long and/or contain anything other than the standard 20 amino acids are
not evaluated and will contain an `NA` as their `prob_AMP`
value.*

``` r
my_prediction <- predict_amps(my_protein_df)
```

| seq\_name     | seq\_aa                                        | prob\_AMP |
| :------------ | :--------------------------------------------- | --------: |
| G1P6H5\_MYOLU | MALTVRIQAACLLLLLLASLTSYSLLLSQTTQLADLQTQDTAGAT… |     0.934 |

Predicted proteins with a specified predicted probability value could
then be extracted and written to a FASTA file:

``` r
my_predicted_amps <- my_protein_df[my_prediction[,3] >= 0.9,]
```

| seq\_name     | seq\_aa                                        |
| :------------ | :--------------------------------------------- |
| G1P6H5\_MYOLU | MALTVRIQAACLLLLLLASLTSYSLLLSQTTQLADLQTQDTAGAT… |

Write the `data.frame` with sequence names in the first column and
protein sequences in the second column to a FASTA formatted file with
`df_to_faa()`

``` r
df_to_faa(my_predicted_amps, "my_predicted_amps.fasta")
```
