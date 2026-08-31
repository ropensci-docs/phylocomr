# comstruct

Calculates mean phylogenetic distance (MPD) and mean nearest
phylogenetic taxon distance (MNTD; aka MNND) for each sample, and
compares them to MPD/MNTD values for randomly generated samples (null
communities) or phylogenies.

## Usage

``` r
ph_comstruct(
  sample,
  phylo,
  null_model = 0,
  randomizations = 999,
  swaps = 1000,
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

- null_model:

  (integer) which null model to use. See Details.

- randomizations:

  (numeric) number of randomizations. Default: 999

- swaps:

  (numeric) number of swaps. Default: 1000

- abundance:

  (logical) If `TRUE` (default) computed accounting for abundance.
  Otherwise, uses presence-absence.

## Value

data.frame

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
(res <- ph_comstruct(sample = sampledf, phylo = phylo_str))
#> # A tibble: 6 × 15
#>   plot   ntaxa   mpd mpd.rnd mpd.sd    nri mpd.ranklow mpd.rankhi  mntd mntd.rnd
#>   <chr>  <int> <dbl>   <dbl>  <dbl>  <dbl>       <int>      <int> <dbl>    <dbl>
#> 1 clump1     8  4.25    7.26  0.309  9.75          999          0  2        4.70
#> 2 clump…     8  4.94    7.15  0.316  6.97          999          0  2        4.67
#> 3 clump…     8  5.83    7.16  0.319  4.18          997          4  2        4.70
#> 4 clump4     8  6.94    7.17  0.312  0.738         829        199  2        4.74
#> 5 even       8  7.75    7.29  0.279 -1.64           12        999  6        4.70
#> 6 random     8  7.11    7.01  0.352 -0.272         497        513  4.88     4.72
#> # ℹ 5 more variables: mntd.sd <dbl>, nti <dbl>, mntd.ranklo <int>,
#> #   mntd.rankhi <int>, runs <int>

# from files
sample_str <- paste0(readLines(sfile), collapse = "\n")
sfile2 <- tempfile()
cat(sample_str, file = sfile2, sep = '\n')
pfile2 <- tempfile()
cat(phylo_str, file = pfile2, sep = '\n')
(res <- ph_comstruct(sample = sfile2, phylo = pfile2))
#> # A tibble: 6 × 15
#>   plot   ntaxa   mpd mpd.rnd mpd.sd    nri mpd.ranklow mpd.rankhi  mntd mntd.rnd
#>   <chr>  <int> <dbl>   <dbl>  <dbl>  <dbl>       <int>      <int> <dbl>    <dbl>
#> 1 clump1     8  4.25    7.27  0.293 10.3           999          0  2        4.72
#> 2 clump…     8  4.94    7.16  0.319  6.94          999          0  2        4.70
#> 3 clump…     8  5.83    7.18  0.305  4.41          997          2  2        4.69
#> 4 clump4     8  6.94    7.17  0.322  0.688         808        212  2        4.72
#> 5 even       8  7.75    7.29  0.276 -1.68            5        999  6        4.71
#> 6 random     8  7.11    7.02  0.353 -0.265         478        535  4.88     4.70
#> # ℹ 5 more variables: mntd.sd <dbl>, nti <dbl>, mntd.ranklo <int>,
#> #   mntd.rankhi <int>, runs <int>

# different null models
ph_comstruct(sample = sfile2, phylo = pfile2, null_model = 0)
#> # A tibble: 6 × 15
#>   plot   ntaxa   mpd mpd.rnd mpd.sd    nri mpd.ranklow mpd.rankhi  mntd mntd.rnd
#>   <chr>  <int> <dbl>   <dbl>  <dbl>  <dbl>       <int>      <int> <dbl>    <dbl>
#> 1 clump1     8  4.25    7.27  0.293 10.3           999          0  2        4.72
#> 2 clump…     8  4.94    7.16  0.319  6.94          999          0  2        4.70
#> 3 clump…     8  5.83    7.18  0.305  4.41          997          2  2        4.69
#> 4 clump4     8  6.94    7.17  0.322  0.688         808        212  2        4.72
#> 5 even       8  7.75    7.29  0.276 -1.68            5        999  6        4.71
#> 6 random     8  7.11    7.02  0.353 -0.265         478        535  4.88     4.70
#> # ℹ 5 more variables: mntd.sd <dbl>, nti <dbl>, mntd.ranklo <int>,
#> #   mntd.rankhi <int>, runs <int>
ph_comstruct(sample = sfile2, phylo = pfile2, null_model = 1)
#> # A tibble: 6 × 15
#>   plot   ntaxa   mpd mpd.rnd mpd.sd    nri mpd.ranklow mpd.rankhi  mntd mntd.rnd
#>   <chr>  <int> <dbl>   <dbl>  <dbl>  <dbl>       <int>      <int> <dbl>    <dbl>
#> 1 clump1     8  4.25    7.07  0.400  7.04          999          0  2        4.51
#> 2 clump…     8  4.94    7.09  0.368  5.82          999          0  2        4.63
#> 3 clump…     8  5.83    7.10  0.361  3.50          992          9  2        4.62
#> 4 clump4     8  6.94    7.10  0.332  0.457         747        281  2        4.60
#> 5 even       8  7.75    7.20  0.330 -1.65            5        999  6        4.64
#> 6 random     8  7.11    6.94  0.401 -0.430         409        611  4.88     4.61
#> # ℹ 5 more variables: mntd.sd <dbl>, nti <dbl>, mntd.ranklo <int>,
#> #   mntd.rankhi <int>, runs <int>
ph_comstruct(sample = sfile2, phylo = pfile2, null_model = 2)
#> # A tibble: 6 × 15
#>   plot   ntaxa   mpd mpd.rnd mpd.sd    nri mpd.ranklow mpd.rankhi  mntd mntd.rnd
#>   <chr>  <int> <dbl>   <dbl>  <dbl>  <dbl>       <int>      <int> <dbl>    <dbl>
#> 1 clump1     8  4.25    7.29  0.272 11.2           999          0  2        4.72
#> 2 clump…     8  4.94    7.16  0.327  6.77          999          0  2        4.69
#> 3 clump…     8  5.83    7.18  0.299  4.51          996          4  2        4.70
#> 4 clump4     8  6.94    7.17  0.327  0.688         824        200  2        4.74
#> 5 even       8  7.75    7.27  0.297 -1.62            7        999  6        4.68
#> 6 random     8  7.11    7.02  0.336 -0.272         482        528  4.88     4.72
#> # ℹ 5 more variables: mntd.sd <dbl>, nti <dbl>, mntd.ranklo <int>,
#> #   mntd.rankhi <int>, runs <int>
# ph_comstruct(sample = sfile2, phylo = pfile2, null_model = 3)
```
