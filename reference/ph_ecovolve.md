# ecovolve

Ecovolve generates a phylogeny via a random birth and death process,
generates a traits file with five randomly evolving, in-dependent
traits, and a sample file with a single sample unit (‘alive’) containing
all extant members of the phylogeny.

## Usage

``` r
ph_ecovolve(
  speciation = 0.05,
  extinction = 0.01,
  time_units = 100,
  out_mode = 3,
  prob_env = "3211000000",
  extant_lineages = FALSE,
  only_extant = FALSE,
  taper_change = NULL,
  competition = FALSE
)
```

## Arguments

- speciation:

  (numeric) Probability of speciation per unit time. Default: 0.05

- extinction:

  (numeric) Probability of extinction per unit time. Default: 0.01

- time_units:

  (integer) Time units to simulate over. Default: 100

- out_mode:

  (integer) Output mode (2 = LTT; 3 = newick). Default: 3

- prob_env:

  (character) Probability envelope for character change. must be a
  string of 10 integers. Default: 3211000000

- extant_lineages:

  (logical) Stop simulation after this number of extant lineages.
  Default: `FALSE`

- only_extant:

  (logical) Output phylogeny pruned only for extant taxa. Default:
  `FALSE`

- taper_change:

  (numeric/integer) Taper character change by `e^(-time/F)`. This
  produces more conservatism in traits (see Kraft et al., 2007).
  Default: `NULL`, not passed

- competition:

  (logical) Simulate competition, with trait proximity increasing
  extinction. Default: `FALSE`

## Value

a list with three elements:

- phylogeny - a phylogeny as a newick string. In the case of
  `out_mode = 2` gives a Lineage Through Time data.frame instead of a
  newick phylogeny

- sample - a data.frame with three columns, "sample" (all "alive"),
  "abundance" (all 1's), "name" (the species code). In the case of
  `out_mode = 2` gives an empty data.frame

- traits - a data.frame with first column with spcies code ("name"),
  then 5 randomly evolved and independent traits. In the case of
  `out_mode = 2` gives an empty data.frame

## Clean up

Two files, "ecovolve.sample" and "ecovolve.traits" are written to the
current working directory when this function runs - we read these files
in, then delete the files via
[unlink](https://rdrr.io/r/base/unlink.html)

## Failure behavior

Function occasionally fails with error "call to 'ecovolve' failed with
status 8. only 1 taxon; \> 1 required" - this just means that only 1
taxon was created in the random process, so the function can't proceed

## Examples

``` r
if (FALSE) { # \dontrun{
# ph_ecovolve(speciation = 0.05)
# ph_ecovolve(speciation = 0.1)
# ph_ecovolve(extinction = 0.005)
# ph_ecovolve(time_units = 50)
# ph_ecovolve(out_mode = 2)
# ph_ecovolve(extant_lineages = TRUE)
# ph_ecovolve(extant_lineages = FALSE)
# ph_ecovolve(only_extant = FALSE)
# ph_ecovolve(only_extant = TRUE, speciation = 0.1)
# ph_ecovolve(taper_change = 2)
# ph_ecovolve(taper_change = 10)
# ph_ecovolve(taper_change = 500)

if (requireNamespace("ape")) {
  # library(ape)
  # x <- ph_ecovolve(speciation = 0.05)
  # plot(read.tree(text = x$phylogeny))
}

} # }
```
