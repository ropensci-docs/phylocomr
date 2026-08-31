# comdist

Outputs the phylogenetic distance between samples, based on phylogenetic
distances of taxa in one sample to the taxa in the other

## Usage

``` r
ph_comdist(
  sample,
  phylo,
  rand_test = FALSE,
  null_model = 0,
  randomizations = 999,
  abundance = TRUE
)

ph_comdistnt(
  sample,
  phylo,
  rand_test = FALSE,
  null_model = 0,
  randomizations = 999,
  abundance = TRUE
)
```

## Arguments

- sample:

  (data.frame/character) sample data.frame or path to a sample file

- phylo:

  (character/phylo) One of: phylogeny as a newick string (will be
  written to a temp file) - OR path to file with a newick string - OR a
  an ape `phylo` object. required.

- rand_test:

  (logical) do you want to use null models? Default: `FALSE`

- null_model:

  (integer) which null model to use. See Details.

- randomizations:

  (numeric) number of randomizations. Default: 999

- abundance:

  (logical) If `TRUE` (default) computed accounting for abundance.
  Otherwise, uses presence-absence.

## Value

data.frame or a list of data.frame's if use null models

## Details

See
[phylocomr-inputs](https://docs.ropensci.org/phylocomr/reference/phylocomr-inputs.md)
for expected input formats

## Null models

- 0 - Phylogeny shuffle: This null model shuffles species labels across
  the entire phylogeny. This randomizes phylogenetic relationships among
  species.

- 1 - Species in each sample become random draws from sample pool: This
  null model maintains the species richness of each sample, but the
  identities of the species occurring in each sample are randomized. For
  each sample, species are drawn without replacement from the list of
  all species actually occurring in at least one sample. Thus, species
  in the phylogeny that are not actually observed to occur in a sample
  will not be included in the null communities

- 2 - Species in each sample become random draws from phylogeny pool:
  This null model maintains the species richness of each sample, but the
  identities of the species occurring in each sample are randomized. For
  each sample, species are drawn without replacement from the list of
  all species in the phylogeny pool. All species in the phylogeny will
  have equal probability of being included in the null communities. By
  changing the phylogeny, different species pools can be simulated. For
  example, the phylogeny could include the species present in some
  larger region.

- 3 - Independent swap: The independent swap algorithm (Gotelli and
  Entsminger, 2003); also known as ‘SIM9’ (Gotelli, 2000) creates
  swapped versions of the sample/species matrix.

## Taxon name case

In the `sample` table, if you're passing in a file, the names in the
third column must be all lowercase; if not, we'll lowercase them for
you. If you pass in a data.frame, we'll lowercase them for your. All
phylo tip/node labels are also lowercased to avoid any casing problems

## Examples

``` r
sfile <- system.file("examples/sample_comstruct", package = "phylocomr")
pfile <- system.file("examples/phylo_comstruct", package = "phylocomr")

# from data.frame
sampledf <- read.table(sfile, header = FALSE,
  stringsAsFactors = FALSE)
phylo_str <- readLines(pfile)
ph_comdist(sample = sampledf, phylo = phylo_str)
#> # A tibble: 6 × 7
#>   name    clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    4.25    6.75    8.08   8.71  8.06   8.05
#> 2 clump2a   6.75    4.94    8.72   8.42  8.06   7.82
#> 3 clump2b   8.08    8.72    5.83   7.36  8.06   7.95
#> 4 clump4    8.71    8.42    7.36   6.94  7.88   8.24
#> 5 even      8.06    8.06    8.06   7.87  7.75   8   
#> 6 random    8.05    7.82    7.95   8.24  8      7.11
ph_comdistnt(sample = sampledf, phylo = phylo_str)
#> # A tibble: 6 × 7
#>   name    clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    2       4.17    4.83   6     4.75   4.88
#> 2 clump2a   4.17    2       6      4.33  4.5    4.62
#> 3 clump2b   4.83    6       2      3     4      3.94
#> 4 clump4    6       4.33    3      2     2      4.10
#> 5 even      4.75    4.5     4      2     6      2.62
#> 6 random    4.88    4.62    3.94   4.10  2.62   4.88
ph_comdist(sample = sampledf, phylo = phylo_str, rand_test = TRUE)
#> $obs
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 4.250000 6.749999 8.083335 8.708335 8.062500 8.046875
#> 2 clump2a 6.750001 4.944444 8.722219 8.416661 8.062505 7.822916
#> 3 clump2b 8.083340 8.722218 5.833330 7.361108 8.062505 7.947916
#> 4  clump4 8.708339 8.416662 7.361108 6.944441 7.875004 8.239582
#> 5    even 8.062500 8.062500 8.062496 7.874995 7.750000 8.000000
#> 6  random 8.046875 7.822917 7.947919 8.239586 8.000000 7.109375
#> 
#> $null_mean
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 7.259947 7.966545 7.958874 8.136598 8.057464 8.120824
#> 2 clump2a 7.966545 7.151288 8.093084 7.974978 8.063355 8.188970
#> 3 clump2b 7.958874 8.093084 7.164248 7.751027 8.065817 8.018214
#> 4  clump4 8.136598 7.974978 7.751027 7.174673 7.808661 8.024700
#> 5    even 8.057464 8.063355 8.065817 7.808661 7.292105 8.002033
#> 6  random 8.120824 8.188970 8.018214 8.024700 8.002033 7.013686
#> 
#> $null_sd
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 0.308642 0.231095 0.232979 0.224982 0.200867 0.246050
#> 2 clump2a 0.231095 0.316470 0.237203 0.230441 0.210234 0.263354
#> 3 clump2b 0.232979 0.237203 0.318708 0.223451 0.213991 0.259107
#> 4  clump4 0.224982 0.230441 0.223451 0.312039 0.221995 0.246711
#> 5    even 0.200867 0.210234 0.213991 0.221995 0.279212 0.224483
#> 6  random 0.246050 0.263354 0.259107 0.246711 0.224483 0.351534
#> 
#> $NRI_or_NTI
#>      name    clump1   clump2a   clump2b    clump4      even    random
#> 1  clump1  9.752243  5.264268 -0.534216 -2.541253 -0.025073  0.300544
#> 2 clump2a  5.264256  6.973313 -2.652308 -1.916682  0.004046  1.389967
#> 3 clump2b -0.534236 -2.652304  4.175976  1.744983  0.015478  0.271312
#> 4  clump4 -2.541270 -1.916686  1.744980  0.737830 -0.298850 -0.870986
#> 5    even -0.025073  0.004069  0.015518 -0.298809 -1.639957  0.009057
#> 6  random  0.300544  1.389966  0.271299 -0.871002  0.009057 -0.272205
#> 
ph_comdistnt(sample = sampledf, phylo = phylo_str, rand_test = TRUE)
#> $obs
#>      name   clump1  clump2a  clump2b   clump4  even   random
#> 1  clump1 2.000000 4.166667 4.833334 6.000000 4.750 4.875000
#> 2 clump2a 4.166667 2.000000 6.000000 4.333333 4.500 4.625000
#> 3 clump2b 4.833333 6.000000 2.000000 3.000000 4.000 3.937500
#> 4  clump4 6.000000 4.333334 3.000000 2.000000 2.000 4.104167
#> 5    even 4.750000 4.500000 4.000000 2.000000 6.000 2.625000
#> 6  random 4.875000 4.625000 3.937500 4.104167 2.625 4.875000
#> 
#> $null_mean
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 4.704454 2.688589 2.661761 3.537436 3.349349 3.527274
#> 2 clump2a 2.688589 4.671337 2.994495 2.603606 3.358058 3.678034
#> 3 clump2b 2.661761 2.994495 4.704869 2.239824 3.345645 3.352637
#> 4  clump4 3.537436 2.603606 2.239824 4.744907 2.238040 3.347064
#> 5    even 3.349349 3.358058 3.345645 2.238040 4.700951 3.162746
#> 6  random 3.527274 3.678034 3.352637 3.347064 3.162746 4.716717
#> 
#> $null_sd
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 0.658822 0.478569 0.490387 0.586351 0.542716 0.616054
#> 2 clump2a 0.478569 0.678444 0.521096 0.447428 0.525495 0.616569
#> 3 clump2b 0.490387 0.521096 0.686749 0.395568 0.577551 0.566924
#> 4  clump4 0.586351 0.447428 0.395568 0.684971 0.381189 0.548798
#> 5    even 0.542716 0.525495 0.577551 0.381189 0.694216 0.567362
#> 6  random 0.616054 0.616569 0.566924 0.548798 0.567362 0.738316
#> 
#> $NRI_or_NTI
#>      name    clump1   clump2a   clump2b    clump4      even    random
#> 1  clump1  4.104987 -3.088535 -4.428280 -4.199813 -2.580816 -2.187674
#> 2 clump2a -3.088535  3.937446 -5.767663 -3.865936 -2.173079 -1.535865
#> 3 clump2b -4.428279 -5.767661  3.938657 -1.921735 -1.132983 -1.031642
#> 4  clump4 -4.199813 -3.865937 -1.921735  4.007335  0.624466 -1.379567
#> 5    even -2.580816 -2.173080 -1.132983  0.624466 -1.871247  0.947800
#> 6  random -2.187674 -1.535865 -1.031642 -1.379567  0.947800 -0.214384
#> 

# from files
sample_str <- paste0(readLines(sfile), collapse = "\n")
sfile2 <- tempfile()
cat(sample_str, file = sfile2, sep = '\n')
pfile2 <- tempfile()
cat(phylo_str, file = pfile2, sep = '\n')
ph_comdist(sample = sfile2, phylo = pfile2)
#> # A tibble: 6 × 7
#>   name    clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    4.25    6.75    8.08   8.71  8.06   8.05
#> 2 clump2a   6.75    4.94    8.72   8.42  8.06   7.82
#> 3 clump2b   8.08    8.72    5.83   7.36  8.06   7.95
#> 4 clump4    8.71    8.42    7.36   6.94  7.88   8.24
#> 5 even      8.06    8.06    8.06   7.87  7.75   8   
#> 6 random    8.05    7.82    7.95   8.24  8      7.11
ph_comdistnt(sample = sfile2, phylo = pfile2)
#> # A tibble: 6 × 7
#>   name    clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    2       4.17    4.83   6     4.75   4.88
#> 2 clump2a   4.17    2       6      4.33  4.5    4.62
#> 3 clump2b   4.83    6       2      3     4      3.94
#> 4 clump4    6       4.33    3      2     2      4.10
#> 5 even      4.75    4.5     4      2     6      2.62
#> 6 random    4.88    4.62    3.94   4.10  2.62   4.88
ph_comdist(sample = sfile2, phylo = pfile2, rand_test = TRUE)
#> $obs
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 4.250000 6.749999 8.083335 8.708335 8.062500 8.046875
#> 2 clump2a 6.750001 4.944444 8.722219 8.416661 8.062505 7.822916
#> 3 clump2b 8.083340 8.722218 5.833330 7.361108 8.062505 7.947916
#> 4  clump4 8.708339 8.416662 7.361108 6.944441 7.875004 8.239582
#> 5    even 8.062500 8.062500 8.062496 7.874995 7.750000 8.000000
#> 6  random 8.046875 7.822917 7.947919 8.239586 8.000000 7.109375
#> 
#> $null_mean
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 7.259947 7.966545 7.958874 8.136598 8.057464 8.120824
#> 2 clump2a 7.966545 7.151288 8.093084 7.974978 8.063355 8.188970
#> 3 clump2b 7.958874 8.093084 7.164248 7.751027 8.065817 8.018214
#> 4  clump4 8.136598 7.974978 7.751027 7.174673 7.808661 8.024700
#> 5    even 8.057464 8.063355 8.065817 7.808661 7.292105 8.002033
#> 6  random 8.120824 8.188970 8.018214 8.024700 8.002033 7.013686
#> 
#> $null_sd
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 0.308642 0.231095 0.232979 0.224982 0.200867 0.246050
#> 2 clump2a 0.231095 0.316470 0.237203 0.230441 0.210234 0.263354
#> 3 clump2b 0.232979 0.237203 0.318708 0.223451 0.213991 0.259107
#> 4  clump4 0.224982 0.230441 0.223451 0.312039 0.221995 0.246711
#> 5    even 0.200867 0.210234 0.213991 0.221995 0.279212 0.224483
#> 6  random 0.246050 0.263354 0.259107 0.246711 0.224483 0.351534
#> 
#> $NRI_or_NTI
#>      name    clump1   clump2a   clump2b    clump4      even    random
#> 1  clump1  9.752243  5.264268 -0.534216 -2.541253 -0.025073  0.300544
#> 2 clump2a  5.264256  6.973313 -2.652308 -1.916682  0.004046  1.389967
#> 3 clump2b -0.534236 -2.652304  4.175976  1.744983  0.015478  0.271312
#> 4  clump4 -2.541270 -1.916686  1.744980  0.737830 -0.298850 -0.870986
#> 5    even -0.025073  0.004069  0.015518 -0.298809 -1.639957  0.009057
#> 6  random  0.300544  1.389966  0.271299 -0.871002  0.009057 -0.272205
#> 
ph_comdistnt(sample = sfile2, phylo = pfile2, rand_test = TRUE)
#> $obs
#>      name   clump1  clump2a  clump2b   clump4  even   random
#> 1  clump1 2.000000 4.166667 4.833334 6.000000 4.750 4.875000
#> 2 clump2a 4.166667 2.000000 6.000000 4.333333 4.500 4.625000
#> 3 clump2b 4.833333 6.000000 2.000000 3.000000 4.000 3.937500
#> 4  clump4 6.000000 4.333334 3.000000 2.000000 2.000 4.104167
#> 5    even 4.750000 4.500000 4.000000 2.000000 6.000 2.625000
#> 6  random 4.875000 4.625000 3.937500 4.104167 2.625 4.875000
#> 
#> $null_mean
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 4.704454 2.688589 2.661761 3.537436 3.349349 3.527274
#> 2 clump2a 2.688589 4.671337 2.994495 2.603606 3.358058 3.678034
#> 3 clump2b 2.661761 2.994495 4.704869 2.239824 3.345645 3.352637
#> 4  clump4 3.537436 2.603606 2.239824 4.744907 2.238040 3.347064
#> 5    even 3.349349 3.358058 3.345645 2.238040 4.700951 3.162746
#> 6  random 3.527274 3.678034 3.352637 3.347064 3.162746 4.716717
#> 
#> $null_sd
#>      name   clump1  clump2a  clump2b   clump4     even   random
#> 1  clump1 0.658822 0.478569 0.490387 0.586351 0.542716 0.616054
#> 2 clump2a 0.478569 0.678444 0.521096 0.447428 0.525495 0.616569
#> 3 clump2b 0.490387 0.521096 0.686749 0.395568 0.577551 0.566924
#> 4  clump4 0.586351 0.447428 0.395568 0.684971 0.381189 0.548798
#> 5    even 0.542716 0.525495 0.577551 0.381189 0.694216 0.567362
#> 6  random 0.616054 0.616569 0.566924 0.548798 0.567362 0.738316
#> 
#> $NRI_or_NTI
#>      name    clump1   clump2a   clump2b    clump4      even    random
#> 1  clump1  4.104987 -3.088535 -4.428280 -4.199813 -2.580816 -2.187674
#> 2 clump2a -3.088535  3.937446 -5.767663 -3.865936 -2.173079 -1.535865
#> 3 clump2b -4.428279 -5.767661  3.938657 -1.921735 -1.132983 -1.031642
#> 4  clump4 -4.199813 -3.865937 -1.921735  4.007335  0.624466 -1.379567
#> 5    even -2.580816 -2.173080 -1.132983  0.624466 -1.871247  0.947800
#> 6  random -2.187674 -1.535865 -1.031642 -1.379567  0.947800 -0.214384
#> 
```
