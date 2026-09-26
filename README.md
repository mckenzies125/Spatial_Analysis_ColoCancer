# Spatial Analysis of Colorectal Cancer in New York City

This project investigates geographic variation in the clinical, genomic,
demographic, and socioeconomic characteristics of colorectal cancer patients
treated at Memorial Sloan Kettering Cancer Center (MSK).

Using patient-level clinical and genomic data linked to geographic information,
the analysis evaluates whether characteristics of colorectal cancer patients
vary across New York City counties and neighborhoods. The project combines
statistical testing, multivariable analysis, spatial statistics, multiple-testing
correction, and geospatial visualization in R.

## Research Question

Is the clinical and genomic landscape of colorectal cancer geographically
uniform across New York City?

The analysis examines geographic variation in characteristics including:

- Stage at diagnosis
- Age at diagnosis
- Primary tumor location
- KRAS mutation status
- Microsatellite instability (MSI)
- Tumor mutational burden
- Body mass index
- Genetic ancestry
- Neighborhood socioeconomic characteristics

## Analytical Approach

### Geographic Data Processing

Patient ZIP-code information was linked to New York City geographic data to
construct county- and neighborhood-level datasets.

Using the `sf` package in R, geographic boundaries were cleaned, merged, and
aggregated to create regions suitable for statistical analysis and visualization.

### Univariate Analysis

Clinical, genomic, demographic, and socioeconomic characteristics were
compared across geographic regions using statistical methods appropriate for
continuous and categorical variables.

Analyses were conducted at both the county and neighborhood levels when
appropriate.

### Pairwise Geographic Comparisons

Pairwise Wilcoxon rank-sum tests were used to investigate differences in
continuous characteristics between individual counties and neighborhoods.

Because these analyses involved many simultaneous comparisons, p-values were
adjusted using the **Benjamini-Hochberg procedure** to control the false
discovery rate.

Adjusted p-values (q-values) were visualized using heatmaps to identify
specific geographic pairs contributing to overall differences.

### Spatial Autocorrelation

Spatial clustering was evaluated using:

- Global Moran's I
- Local Moran's I / Local Indicators of Spatial Association (LISA)

Neighborhood adjacency relationships were constructed from geographic
boundaries and converted into row-standardized spatial weights.

Local Moran's I analyses used permutation-based inference with **9,999
simulations**.

Spatial autocorrelation analyses were conducted at the neighborhood level
rather than the county level because the small number and geographic
configuration of NYC counties provided insufficient spatial structure for
meaningful county-level Moran's I analysis.

### Multivariable Analysis

Multivariable and stratified analyses were used to evaluate whether observed
geographic relationships persisted after accounting for other patient
characteristics and potential confounding factors.

Separate analyses examined relationships among clinical, genomic,
demographic, socioeconomic, and geographic variables.

## Results

The analysis found that the clinico-genomic landscape of colorectal cancer
among patients in this cohort was **not geographically uniform across New
York City**.

Statistically significant geographic variation was observed in several
characteristics, including:

- Yost socioeconomic index across counties and neighborhoods
- Genetic ancestry across counties and neighborhoods
- **Age at diagnosis across neighborhoods**
- First BMI measurement across counties and neighborhoods
- **Stage at diagnosis across neighborhoods**
- **KRAS mutation status across counties**

### Age at Diagnosis

Median age at diagnosis differed **significantly across New York City
neighborhoods**.

Pairwise neighborhood comparisons were subsequently performed using exact
Wilcoxon rank-sum tests. Because many neighborhood pairs were evaluated,
p-values were adjusted using the **Benjamini-Hochberg false discovery rate
procedure**.

These findings indicate that the age distribution of colorectal cancer
patients at diagnosis varied geographically within the MSK study population.

The project materials identify the overall neighborhood-level difference in
age at diagnosis as statistically significant but do not report a single
numerical overall p-value. Pairwise p-values and their Benjamini-Hochberg
adjusted q-values were therefore used to investigate which geographic
comparisons contributed to this pattern.

### Stage at Diagnosis

Stage at diagnosis varied significantly across NYC neighborhoods:

**p < 0.05 **

This finding indicates that the distribution of colorectal cancer stage at
diagnosis differed geographically within the study population.

### KRAS Mutation Status

KRAS mutation prevalence also demonstrated significant geographic variation
across NYC counties:

**p < 0.05 **

The analysis therefore provided evidence that the distribution of this
clinically relevant genomic characteristic differed geographically within
the study cohort.

### Primary Tumor Location

Variation in primary tumor location was observed across neighborhoods, but
the association did **not** reach statistical significance.

### Microsatellite Instability

Microsatellite instability rates also varied geographically, but the observed
differences were **not statistically significant**.

## Multiple Testing

Because many geographic comparisons were performed, interpreting raw
p-values alone could substantially increase the probability of false-positive
findings.

Pairwise analyses therefore used the **Benjamini-Hochberg false discovery
rate correction**:

```r
pairs$q_value <- p.adjust(pairs$p_value, method = "BH")
