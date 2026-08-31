# comtrait

Calculate measures of trait dispersion within each community, and
compare observed patterns to those expected under a null model.

## Usage

``` r
ph_comtrait(
  sample,
  traits,
  binary = NULL,
  metric = "variance",
  null_model = 0,
  randomizations = 999,
  abundance = TRUE
)
```

## Arguments

- sample:

  (data.frame/character) sample data.frame or path to a sample file

- traits:

  (data.frame/character) traits data.frame or path to a traits file

- binary:

  (logical) a logical vector indicating what columns are to be treated
  as binary characters - all others are treated as continuous

- metric:

  (integer) metric to calculate. One of variance, mpd, mntd, or range
  (converted to phylocom integer format internally)

- null_model:

  (integer) which null model to use. See Details.

- randomizations:

  (numeric) number of randomizations. Default: 999

- abundance:

  (logical) If `TRUE` (default) computed accounting for abundance.
  Otherwise, uses presence-absence.

## Value

data.frame of the form:

- trait - Trait name

- sample - Sample name

- ntaxa - Number of taxa in sample

- mean - Mean value of trait in sample

- metric - Observed metric in sample

- meanrndmetric - Mean value of metric in null models

- sdrndmetric - Standard deviation of metric in null models

- sesmetric - Standardized effect size of metric

- ranklow - Number of randomizations with metric lower than observed

- rankhigh - Number of randomizations with metric higher than observed

- runs - Number of randomizations

## Details

See
[phylocomr-inputs](https://docs.ropensci.org/phylocomr/reference/phylocomr-inputs.md)
for expected input formats

If you give a data.frame to `traits` parameter it expects data.frame
like

- species - the taxon labels matching the sample data to `sample`
  parameter

- col1,col2,col3,etc. - any number of trait columns - column names do
  not matter

When giving a data.frame to `traits` make sure to pass in a binary
vector for what traits are to be treated as binary.

## Null models

- 0 - This null model shuffles trait values across species.

- 1 - Species in each sample become random draws from sample pool. This
  null model maintains the species richness of each sample, but the
  identities of the species occurring in each sample are randomized. For
  each sample, species are drawn without replacement from the list of
  all species actually occurring in at least one sample

- 2 - Species in each sample become random draws from traits data. This
  null model maintains the species richness of each sample, but the
  identities of the species occurring in each sample are randomized. For
  each sample, species are drawn without replacement from the list of
  all species with trait values. This function is redundant since by
  definition the sample and trait species must match, but is included
  for consistency with the comstruct function.

- 3 - Independent swap: Same as for
  [ph_comdist](https://docs.ropensci.org/phylocomr/reference/ph_comdist.md)
  and
  [ph_comstruct](https://docs.ropensci.org/phylocomr/reference/ph_comstruct.md)

## Taxon name case

In the `sample` and `trait` tables, if you're passing in a file, the
names in the third and first columns, respectively, must be all
lowercase; if not, we'll lowercase them for you. If you pass in a
data.frame, we'll lowercase them for your. All phylo tip/node labels are
also lowercased to avoid any casing problems

## Examples

``` r
if (FALSE) { # \dontrun{
sfile <- system.file("examples/sample_comstruct", package = "phylocomr")
tfile <- system.file("examples/traits_aot", package = "phylocomr")

# from files
sample_str <- paste0(readLines(sfile), collapse = "\n")
sfile2 <- tempfile()
cat(sample_str, file = sfile2, sep = '\n')

traits_str <- paste0(readLines(tfile), collapse = "\n")
tfile2 <- tempfile()
cat(traits_str, file = tfile2, sep = '\n')

ph_comtrait(sample = sfile2, traits = tfile2)

# from data.frame
sampledf <- read.table(sfile, header = FALSE,
  stringsAsFactors = FALSE)
traitsdf_file <- system.file("examples/traits_aot_df",
  package = "phylocomr")
traitsdf <- read.table(text = readLines(traitsdf_file), header = TRUE,
  stringsAsFactors = FALSE)
ph_comtrait(sample = sampledf, traits = traitsdf,
  binary = c(FALSE, FALSE, FALSE, TRUE))
} # }
```
