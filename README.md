# Antimicrobial resistance (AMR) surveillance analysis in R

Reproducible analysis of a hospital isolate collection in R: first-isolate selection, pathogen
distribution, antibiograms, resistance by ward, resistance trend over time, and multidrug-resistant
organism (MDR) classification.

**Question:** which bacteria are most common, how resistant are they to key antibiotics, does resistance differ
between wards and over time, and what share of isolates are multidrug-resistant?

**Data:** the `example_isolates` dataset from the `AMR` package: 2,000 bacterial isolates with S/I/R results,
date, patient, age, gender and ward. It is an example dataset, so the project demonstrates the workflow and its results
are illustrative, not real surveillance findings.

## Pipeline
1. One isolate per patient episode (`first_isolate`)
2. Pathogen and Gram-stain distribution
3. Traditional and combination antibiograms (`antibiogram`)
4. *E. coli* resistance by ward with 95% confidence intervals
5. Ciprofloxacin resistance trend (logistic regression on year) and associated factors
6. MDR classification (`mdro`, Magiorakos et al. 2012)

## Run it
```r
install.packages(c("AMR", "dplyr", "ggplot2", "rmarkdown"))
rmarkdown::render("analysis.Rmd")
```
The data comes with the package, so nothing needs to be downloaded.

## Results
- **Data:** 2,000 isolates; 1,387 first isolates (one per patient episode) kept for analysis.
- **Pathogens:** the most common were *E. coli* (19.5%), coagulase-negative staphylococci (19.5%), *S. epidermidis* (10.6%) and *S. aureus* (8.8%). 67.3% of first isolates were Gram-positive and 31.8% Gram-negative.
- **Antibiogram:** *E. coli* susceptibility was 49% for amoxicillin, 86% for amoxicillin/clavulanic acid, 86% for ciprofloxacin, 97% for gentamicin and 94% for piperacillin/tazobactam. *K. pneumoniae* showed 0% susceptibility to amoxicillin, as expected from its intrinsic resistance. Adding gentamicin to piperacillin/tazobactam raised coverage from 86% to 98% in Gram-negatives and from 85% to 98% in Gram-positives (the number of isolates tested differs between the combinations).
- ***E. coli* by ward:** resistance was about 50-55% for amoxicillin, 34-41% for trimethoprim/sulfamethoxazole, 11-18% for ciprofloxacin and amoxicillin/clavulanic acid, and 3-6% for gentamicin. Confidence intervals overlapped between Clinical, ICU and Outpatient wards, so no ward difference was shown (Outpatient had only 16-17 isolates per antibiotic).
- **Trend:** ciprofloxacin resistance in *E. coli* rose over 2002-2017 (odds ratio 1.13 per year, 95% CI 1.04 to 1.22, p = 0.005).
- **Associated factors:** ward, sex and age group (65+ vs younger) were not significantly associated with ciprofloxacin resistance (age 65+ OR 2.42, 95% CI 0.71 to 8.32), but only 36 of 265 tested isolates were resistant, so power was low.
- **MDR:** 81 of 1,387 first isolates (5.8%) were multidrug-resistant, with no XDR or PDR; 146 isolates could not be classified because of too few test results. By ward: Clinical 5.6%, ICU 7.0%, Outpatient 2.1% (percentages of all first isolates in that ward).

The full code, plots and session information are in `analysis.Rmd` and the rendered `analysis.html`. Tables are in `results/`.

## Limitations
- The data are an example dataset shipped with a software package, so the numbers illustrate the workflow and are not real surveillance results.
- Some groups have few isolates, so confidence intervals are wide.
- The trend model is a simple logistic regression and ignores changes in testing practice over time.
- Susceptibility interpretation and intrinsic resistance rules follow the `AMR` package defaults.
