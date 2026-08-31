# rao - Rao's quadratic entropy

A measure of within- and among-community diversity taking species
dissimilarity (phylogenetic dissimilarity) into account

## Usage

``` r
ph_rao(sample, phylo)
```

## Arguments

- sample:

  (data.frame/character) sample data.frame or path to a sample file

- phylo:

  (character/phylo) One of: phylogeny as a newick string (will be
  written to a temp file) - OR path to file with a newick string - OR a
  an ape `phylo` object. required.

## Value

A list of 6 data.frame's: **Diversity components**:

- overall alpha (within-site)

- beta (among-site)

- total diversity

- Fst statistic of differentiation for diversity and phylogenetic
  diversity

**Within-community diversity**:

- Plot - Plot name

- NSpp - Number of species

- NIndiv - Number of individuals

- PropIndiv - Proportion of all individuals found in this plot

- D - Diversity (= Simpson’s diversity)

- Dp - Phylogenetic diversity (= Diversity weighted by interspecific
  phylogenetic distances)

The remaining 4 tables compare each community pairwise:

- among_community_diversity_d - Among-community diversities

- among_community_diversity_h - Among-community diversities excluding
  within-community diversity

- among_community_phylogenetic_diversity_dp - Among-community
  phylogenetic diversities

- among_community_phylogenetic_diversity_hp - Among-community
  phylogenetic diversities excluding within-community diversity

## Details

See
[phylocomr-inputs](https://docs.ropensci.org/phylocomr/reference/phylocomr-inputs.md)
for expected input formats

## Taxon name case

In the `sample` table, if you're passing in a file, the names in the
third column must be all lowercase; if not, we'll lowercase them for
you. If you pass in a data.frame, we'll lowercase them for your. All
phylo tip/node labels are also lowercased to avoid any casing problems

## See also

Other phylogenetic-diversity:
[`ph_pd()`](https://docs.ropensci.org/phylocomr/reference/ph_pd.md)

## Examples

``` r
sfile <- system.file("examples/sample_comstruct", package = "phylocomr")
pfile <- system.file("examples/phylo_comstruct", package = "phylocomr")

# sample from data.frame, phylogeny from a string
sampledf <- read.table(sfile, header = FALSE,
  stringsAsFactors = FALSE)
phylo_str <- readLines(pfile)

ph_rao(sample = sampledf, phylo = phylo_str)
#> $diversity_components
#> # A tibble: 4 × 3
#>   component standard phylogenetic
#>   <chr>        <dbl>        <dbl>
#> 1 Alpha       0.860         3.11 
#> 2 Beta        0.0848        0.753
#> 3 Total       0.945         3.86 
#> 4 Fst         0.0897        0.195
#> 
#> $within_community_diveristy
#> # A tibble: 6 × 6
#>   plot     nspp nindiv propindiv     d    dp
#>   <chr>   <int>  <int>     <dbl> <dbl> <dbl>
#> 1 clump1      8      8     0.118 0.875  2.12
#> 2 clump2a     8     12     0.176 0.861  2.47
#> 3 clump2b     8     12     0.176 0.861  2.92
#> 4 clump4      8     12     0.176 0.861  3.47
#> 5 even        8      8     0.118 0.875  3.88
#> 6 random      8     16     0.235 0.844  3.55
#> 
#> $among_community_diversity_d
#> # A tibble: 6 × 7
#>   .          clump1 clump2a clump2b clump4  even random
#>   <chr>       <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1  8.37e+165   0.958   0.958  0.979 0.969  0.977
#> 2 clump2a 9.58e-  1   0.861   0.972  0.958 0.969  0.984
#> 3 clump2b 9.58e-  1   0.972   0.861  0.931 0.969  0.964
#> 4 clump4  9.79e-  1   0.958   0.931  0.861 0.938  0.964
#> 5 even    9.69e-  1   0.969   0.969  0.938 0.875  0.961
#> 6 random  9.77e-  1   0.984   0.964  0.964 0.961  0.844
#> 
#> $among_community_diversity_h
#> # A tibble: 6 × 7
#>   .          clump1 clump2a clump2b clump4   even random
#>   <chr>       <dbl>   <dbl>   <dbl>  <dbl>  <dbl>  <dbl>
#> 1 clump1  8.37e+165  0.0903  0.0903 0.111  0.0938  0.117
#> 2 clump2a 9.03e-  2  0       0.111  0.0972 0.101   0.132
#> 3 clump2b 9.03e-  2  0.111   0      0.0694 0.101   0.111
#> 4 clump4  1.11e-  1  0.0972  0.0694 0      0.0694  0.111
#> 5 even    9.38e-  2  0.101   0.101  0.0694 0       0.102
#> 6 random  1.17e-  1  0.132   0.111  0.111  0.102   0    
#> 
#> $among_community_phylogenetic_diversity_dp
#> # A tibble: 6 × 7
#>   .       clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    2.12    3.38    4.04   4.35  4.03   4.02
#> 2 clump2a   3.38    2.47    4.36   4.21  4.03   3.91
#> 3 clump2b   4.04    4.36    2.92   3.68  4.03   3.97
#> 4 clump4    4.35    4.21    3.68   3.47  3.94   4.12
#> 5 even      4.03    4.03    4.03   3.94  3.88   4   
#> 6 random    4.02    3.91    3.97   4.12  4      3.55
#> 
#> $among_community_phylogenetic_diversity_hp
#> # A tibble: 6 × 7
#>   .       clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    0      1.08    1.52   1.56  1.03   1.18 
#> 2 clump2a   1.08   0       1.67   1.24  0.858  0.898
#> 3 clump2b   1.52   1.67    0      0.486 0.635  0.738
#> 4 clump4    1.56   1.24    0.486  0     0.264  0.606
#> 5 even      1.03   0.858   0.635  0.264 0      0.285
#> 6 random    1.18   0.898   0.738  0.606 0.285  0    
#> 

# both from files
sample_str <- paste0(readLines(sfile), collapse = "\n")
sfile2 <- tempfile()
cat(sample_str, file = sfile2, sep = '\n')
pfile2 <- tempfile()
phylo_str <- readLines(pfile)
cat(phylo_str, file = pfile2, sep = '\n')

ph_rao(sample = sfile2, phylo = pfile2)
#> $diversity_components
#> # A tibble: 4 × 3
#>   component standard phylogenetic
#>   <chr>        <dbl>        <dbl>
#> 1 Alpha       0.860         3.11 
#> 2 Beta        0.0848        0.753
#> 3 Total       0.945         3.86 
#> 4 Fst         0.0897        0.195
#> 
#> $within_community_diveristy
#> # A tibble: 6 × 6
#>   plot     nspp nindiv propindiv     d    dp
#>   <chr>   <int>  <int>     <dbl> <dbl> <dbl>
#> 1 clump1      8      8     0.118 0.875  2.12
#> 2 clump2a     8     12     0.176 0.861  2.47
#> 3 clump2b     8     12     0.176 0.861  2.92
#> 4 clump4      8     12     0.176 0.861  3.47
#> 5 even        8      8     0.118 0.875  3.88
#> 6 random      8     16     0.235 0.844  3.55
#> 
#> $among_community_diversity_d
#> # A tibble: 6 × 7
#>   .          clump1 clump2a clump2b clump4  even random
#>   <chr>       <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1  8.37e+165   0.958   0.958  0.979 0.969  0.977
#> 2 clump2a 9.58e-  1   0.861   0.972  0.958 0.969  0.984
#> 3 clump2b 9.58e-  1   0.972   0.861  0.931 0.969  0.964
#> 4 clump4  9.79e-  1   0.958   0.931  0.861 0.938  0.964
#> 5 even    9.69e-  1   0.969   0.969  0.938 0.875  0.961
#> 6 random  9.77e-  1   0.984   0.964  0.964 0.961  0.844
#> 
#> $among_community_diversity_h
#> # A tibble: 6 × 7
#>   .          clump1 clump2a clump2b clump4   even random
#>   <chr>       <dbl>   <dbl>   <dbl>  <dbl>  <dbl>  <dbl>
#> 1 clump1  8.37e+165  0.0903  0.0903 0.111  0.0938  0.117
#> 2 clump2a 9.03e-  2  0       0.111  0.0972 0.101   0.132
#> 3 clump2b 9.03e-  2  0.111   0      0.0694 0.101   0.111
#> 4 clump4  1.11e-  1  0.0972  0.0694 0      0.0694  0.111
#> 5 even    9.38e-  2  0.101   0.101  0.0694 0       0.102
#> 6 random  1.17e-  1  0.132   0.111  0.111  0.102   0    
#> 
#> $among_community_phylogenetic_diversity_dp
#> # A tibble: 6 × 7
#>   .       clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    2.12    3.38    4.04   4.35  4.03   4.02
#> 2 clump2a   3.38    2.47    4.36   4.21  4.03   3.91
#> 3 clump2b   4.04    4.36    2.92   3.68  4.03   3.97
#> 4 clump4    4.35    4.21    3.68   3.47  3.94   4.12
#> 5 even      4.03    4.03    4.03   3.94  3.88   4   
#> 6 random    4.02    3.91    3.97   4.12  4      3.55
#> 
#> $among_community_phylogenetic_diversity_hp
#> # A tibble: 6 × 7
#>   .       clump1 clump2a clump2b clump4  even random
#>   <chr>    <dbl>   <dbl>   <dbl>  <dbl> <dbl>  <dbl>
#> 1 clump1    0      1.08    1.52   1.56  1.03   1.18 
#> 2 clump2a   1.08   0       1.67   1.24  0.858  0.898
#> 3 clump2b   1.52   1.67    0      0.486 0.635  0.738
#> 4 clump4    1.56   1.24    0.486  0     0.264  0.606
#> 5 even      1.03   0.858   0.635  0.264 0      0.285
#> 6 random    1.18   0.898   0.738  0.606 0.285  0    
#> 
```
