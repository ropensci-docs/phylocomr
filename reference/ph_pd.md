# pd - Faith's index of phylogenetic diversity

Calculates Faith’s (1992) index of phylogenetic diversity (PD) for each
sample in the phylo.

## Usage

``` r
ph_pd(sample, phylo)
```

## Arguments

- sample:

  (data.frame/character) sample data.frame or path to a sample file.
  required

- phylo:

  (character/phylo) One of: phylogeny as a newick string (will be
  written to a temp file) - OR path to file with a newick string - OR a
  an ape `phylo` object. required.

## Value

A single data.frame, with the colums:

- sample - community name/label

- ntaxa - number of taxa

- pd - Faith's phylogenetic diversity

- treebl - tree BL

- proptreebl - proportion tree BL

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
[`ph_rao()`](https://docs.ropensci.org/phylocomr/reference/ph_rao.md)

## Examples

``` r
sfile <- system.file("examples/sample_comstruct", package = "phylocomr")
pfile <- system.file("examples/phylo_comstruct", package = "phylocomr")

# from data.frame
sampledf <- read.table(sfile, header = FALSE,
  stringsAsFactors = FALSE)
phylo_str <- readLines(pfile)
ph_pd(sample = sampledf, phylo = phylo_str)
#> # A tibble: 6 × 5
#>   sample  ntaxa    pd treebl proptreebl
#>   <chr>   <int> <dbl>  <dbl>      <dbl>
#> 1 clump1      8    16     62      0.258
#> 2 clump2a     8    17     62      0.274
#> 3 clump2b     8    18     62      0.29 
#> 4 clump4      8    22     62      0.355
#> 5 even        8    30     62      0.484
#> 6 random      8    27     62      0.435

# from files
sample_str <- paste0(readLines(sfile), collapse = "\n")
sfile2 <- tempfile()
cat(sample_str, file = sfile2, sep = '\n')
pfile2 <- tempfile()
phylo_str <- readLines(pfile)
cat(phylo_str, file = pfile2, sep = '\n')

ph_pd(sample = sfile2, phylo = pfile2)
#> # A tibble: 6 × 5
#>   sample  ntaxa    pd treebl proptreebl
#>   <chr>   <int> <dbl>  <dbl>      <dbl>
#> 1 clump1      8    16     62      0.258
#> 2 clump2a     8    17     62      0.274
#> 3 clump2b     8    18     62      0.29 
#> 4 clump4      8    22     62      0.355
#> 5 even        8    30     62      0.484
#> 6 random      8    27     62      0.435
```
