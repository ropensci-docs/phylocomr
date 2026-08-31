# aot

AOT conducts univariate and bivariate tests of phylogenetic signal and
trait correlations, respectively, and node-level analyses of trait means
and diversification.

## Usage

``` r
ph_aot(
  traits,
  phylo,
  randomizations = 999,
  trait_contrasts = 1,
  ebl_unstconst = FALSE
)
```

## Arguments

- traits:

  (data.frame/character) trait data.frame or path to traits file.
  required. See Details.

- phylo:

  (character/phylo) One of: phylogeny as a newick string (will be
  written to a temp file) - OR path to file with a newick string - OR a
  an ape `phylo` object. required.

- randomizations:

  (numeric) number of randomizations. Default: 999

- trait_contrasts:

  (numeric) Specify which trait should be used as 'x' variable for
  contrasts. Default: 1

- ebl_unstconst:

  (logical) Use equal branch lengths and unstandardized contrasts.
  Default: `FALSE`

## Value

a list of data.frames

## Details

See
[phylocomr-inputs](https://docs.ropensci.org/phylocomr/reference/phylocomr-inputs.md)
for expected input formats

## Taxon name case

In the `traits` table, if you're passing in a file, the names in the
first column must be all lowercase; if not, we'll lowercase them for
you. If you pass in a data.frame, we'll lowercase them for your. All
phylo tip/node labels are also lowercased to avoid any casing problems

## Examples

``` r
if (FALSE) { # \dontrun{
traits_file <- system.file("examples/traits_aot", package = "phylocomr")
phylo_file <- system.file("examples/phylo_aot", package = "phylocomr")

# from data.frame
traitsdf_file <- system.file("examples/traits_aot_df",
  package = "phylocomr")
traits <- read.table(text = readLines(traitsdf_file), header = TRUE,
  stringsAsFactors = FALSE)
phylo_str <- readLines(phylo_file)
(res <- ph_aot(traits, phylo = phylo_str))

# from files
traits_str <- paste0(readLines(traits_file), collapse = "\n")
traits_file2 <- tempfile()
cat(traits_str, file = traits_file2, sep = '\n')
phylo_file2 <- tempfile()
cat(phylo_str, file = phylo_file2, sep = '\n')
(res <- ph_aot(traits_file2, phylo_file2))
} # }
```
