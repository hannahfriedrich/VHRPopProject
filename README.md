# Very High Resolution (VHR) Commercial Satellite Imagery and Gridded Population Data

> This repository contains elements from an ongoing research project, which is assessing how repeat coverage of commercial satellite imagery collections and gridded population data correlate. This initial research will aid in finding patterns between commerical satellite imagery and more complex spatial phenomenea.

To replicate the Google Earth Engine part of this research, you can follow the following steps **_OR_** go to this [link][]:
1. Create a [Google Earth Engine][] account
2. Load [assets][]
3. Load [script][]
4. Export CSVs and run statistical analysis

## Introduction
Population distribution modeling using satellite derived imagery has become a ubiquitous method for identifying human settlement, by utilizing land cover, vegetation indices, specific bands, nighttime lights, and image texture to map populations in areas of the world where there is infrequent and spatially coarse census data ([Lung et al., 2013][], [Pesaresi et al., 2016][]). Using satellite and machine learning, the spatial resolution of population estimates is becoming more refined, less expensive, scalable and increasingly in high demand as the applications for high spatial resolution population distribution are broad and numerous international projects are underway to track indicators for the United Nations Sustainable Development Goals (Jean et. al, 2016). For this project, instead of using spectral information from satellite imagery to estimate population density, density of commercial satellite imagery is investigated to statistically determine the parallels between very high resolution (VHR) commercial satellite coverage and satellite derived gridded population data.

An increasing spatial and temporal abundance of VHR commercial imagery is changing how the United States, who is engaged in multi-million dollar a year contracts with companies like [Digital Globe][], interface with international conflict and development ([Werner, 2017][]). A unique perspective can be gleaned from image coverage attained by commercial satellites operated by Digital Globe like GeoEye, WorldView3, and WorldView4. Because these satellites can be targeted to collect images at particular locales and angles, areal coverage and density of coverage may illuminate certain motivations for instances of high repeat coverage and alternatively, instances of low repeat coverage in areas of interest, like locations with high population density. Given this framework, we expect VHR coverage to be linearly correlated with population density. Using [Google Earth Engine][] (GEE), a first pass method of evaluating the relationship between VHR and population is constructed and simple summary statistics of simple linear regression coefficients and residuals, alongside pixel by pixel correlation plots are evaluated to determine how related coverage of VHR and WorldPop population density is on a country and yearly basis. Anomalies, or deviations from a line of regression, may reveal nuanced tendencies in where VHR is being collected, and for what purpose. In seeking out what these motivations may be, such as securitization, development, or surveillance of ongoing conflict areas, a baseline analysis must be conducted to first establish how coverage of VHR correlates with estimates of modeled population density. Using VHR to estimate population density is a departure for crafting future hypothesis regarding VHR and other variables like accessibility to cities, proximity to surface water, and locations of informal settlement locations, drone bases or drone strikes.

## Data
For this analysis, the two main datasets used were country aggregated VHR coverage from [Digital Globe’s publicly searchable archive][] and WorldPop gridded population density. Unless only 2010 WorldPop population data was available, years of 2010 and 2015 were evaluated for each country (See Figure 1 below for a breakdown of what WorldPop data set was available for country by year). The VHR scene footprints were acquired through a Digital Globe maintained site that is a GUI operated search engine which allows potential buyers to view scenes of their available imagery, as well as metadata such as collection date, satellite platform, angle off nadir, as well as a thumbnail of the image. Each country was queried and collected separately in 500,000 sq kim portions from 2002 - present. This data was merged in QGIS by each country and then was processed in a Python script which removes duplicates, aggregates by daily occurrence, and populates a gridded raster with the count of images on a yearly basis as well as for the full temporal domain (2002-present). The country and year VHR grid count layers were ingested into GEE as data layer assets.

The gridded population data is from WorldPop, which already exists as a GEE provided assets, is an ongoing project which utilizes a semi-automated dasymetric modeling approach that includes census and ancillary settlement data and uses a Random Forest modeling approach to estimate population density at a scale of approximately 100 meters ([Stevens et al., 2015][]). This method of using satellite informed settlement data along with detailed census data and scaling this approach globally using a Random Forest convolutional neural network is a leading high spatial resolution model population distribution dataset that is publically available.

## Methods
With both the VHR layers by country and year and the WorldPop data ingested to GEE, geometries of each country are generated from the Large Scale International Boundary Polygon dataset, a GEE available asset. Next, because the the WorldPop data is at a scale of 0.000833333 decimal degrees (~100 meters at the equator), and the VHR is at a scale of 0.10 decimal degrees, the WorldPop datasets for each country must be “resampled” up to the VHR resolution, which in GEE terms means applying a reduceResolution() method and reprojecting the WorldPop country dataset to its respective country’s VHR resolution. While this resolution is consistent for all countries, each dataset is registered to its respective country’s VHR data resolution for comprehensibility. Once the two datasets are at the correct scale and “registered”, a simple linear regression is then conducted which produces image level correlation coefficient and residuals (See Figure 1). With the goal of calculating image wide correlation, features are created for pixel level values of VHR and WorldPop for each country and year. These features are then combined into Feature Collections, which are then exported to a CSV, and VHR and WorldPop values for every pixel are plotted in R for each country by year (See Figures 3 thru 7), and finally by country with years combined (See Figure 8). These plots also contain the line of regression.The linear regression coefficients, residual value, multiple R-squared, and pixel level plots are then interpreted to evaluate the relationship between VHR coverage and modeled population distribution.

## Results
From the simple linear regressions conducted for each country, by year, I conclude that Pakistan from 2015 has the strongest linear relationship between VHR coverage and population density. This is corroborated by the fact that linear regression analysis for Pakistan 2015 has the largest linear regression coefficient of approximately 0.5, a residual value of 7.2 (see Figure 1) and a multiple R-squared value of approximately 0.15 (See Figure 2), which suggests that 15% of modeled population density can be explained by VHR coverage. For the other countries and years, the correlation coefficients fall within a similar range from 0.03 to 0.08, with residuals ranging from 0.6 to 4.3. Interestingly, Pakistan which has the highest correlation, also has the largest residual as well. This suggests that while VHR coverage and population density for Pakistan has a strong linear relationship, there is also large variability in population density; there are pixels that deviate relatively far from the fitted line of regression. These pixels which have high residual values are the pixels of greatest interest in looking for anomalies. Overall, while the correlation coefficients and multiple R-squared values are rather small when looking at all countries with years combined, there is, nevertheless, evidence for positive linear relationships between VHR coverage and WorldPop population estimates.

### Niger VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/niger.png)

### Mali VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/mali.jpg)

### Somalia VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/somalia.jpg)

### Pakistan VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/pakistan.jpg)

### Afghanistan VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/afghanistan.jpg)

### Combined Countries VHR and Population Correlation
![](https://github.com/hannahfriedrich/VHRPopProject/blob/master/img/combined.jpg)

### Data Used and Correlation Coeffiecients and Residuals

|   Country   | Year | WorldPop Dataset | Coeffiecient | Residuals |
| :---------: | :--: | :--------------: | :----------: | :-------: |
|    Niger    | 2010 |     Adjusted     |     0.04     |   0.62    |
|    Niger    | 2015 |     Adjusted     |     0.04     |   0.74    |
|    Mali     | 2010 |     Adjusted     |     0.04     |   0.88    |
|    Mali     | 2010 |     Adjusted     |     0.06     |   1.06    |
|   Somalia   | 2010 |    Unadjusted    |     0.05     |   1.57    |
|    Yemen    | 2010 |    Unadjusted    |     n/a      |    n/a    |
|  Pakistan   | 2010 |     Adjusted     |     0.08     |   4.30    |
|  Pakistan   | 2015 |     Adjusted     |     0.50     |   7.20    |
| Afghanistan | 2010 |     Adjusted     |     0.08     |   1.70    |
| Afghanistan | 2015 |     Adjusted     |     n/a      |    n/a    |

## Discussion
Future work involves further evaluation of the relationship between VHR and established gridded population datasets. As noted in Figure 1, by using the current methodology, certain years or countries returned an unequal amount of features for the registered and masked VHR and WorldPop data. This inconsistency does not allow for a simple linear relationship to be conducted, nor the individual feature creation and plots to be created on a pixel by pixel basis. Next steps include troubleshooting this issue to ensure that pixels are correctly being evaluated on a one by one basis. As of now, this remains a technical impediment that, with more time, will hopefully be resolved.

An additional next step is doing a comparison between WorldPop Adjusted versus Unadjusted datasets if both are available for a given country. Adjusted simply means they are, quite literally, adjusted to meet United Nations estimates, whereas the Unadjusted do not undergo this transformation. While I do not think this comparison is necessary, it may be useful to better understand which dataset (Adjusted or Unadjusted) should be used moving forward. For this analysis, the not unadjusted (Adjusted) were used by default, unless an “Unadjusted” was only available for that country or year. 

Finally, additional next steps will be taken to visualize pixel by pixel correlations for each country by year to support the statistical analysis that has been conducted with this analysis. As mentioned, the pixels which deviate relatively far from the line of regression are the locales which would be regarded as anomalies. These pixels may be further evaluated to understand what spatial phenomena is happening at that particular location, which may explain why there is high VHR coverage and low population density, or vice versa. The goal of visualizing on a pixel by pixel basis is to elucidate the particular anomalies which we hypothesize to find instances of acute conflict, temporary informal settlements, and development of drone bases in other wise low density population locales. So far, this primary analysis has established that there is a positive, albeit weak to moderate, linear relationship between VHR coverage and modeled population distribution.

## Conclusion
This analysis is intended to be an experiential project which explores the relationship between known coverage of VHR commercial satellite imagery and disaggregated population estimates. In order to conduct further research, which explores the role that VHR plays in tracking more complex, less stationary and spatially variable events, this baseline study of the relationship between VHR coverage and modeled population density can be utilized to better filter and adjust hypotheses regarding how VHR coverage relates to such topics. Given this promising approach which asserts that there is a subtle, but positive relationship between commercial VHR coverage and estimated population density, next steps can be explored to further assess the relationship of VHR coverage and more complex cases of spatial phenomena.

[Digital Globe]: https://www.digitalglobe.com/
[Werner, 2017]: http://spacenews.com/mda-seeks-to-provide-extensive-support-to-u-s-intelligence-and-defense-agencies/
[Lung et al., 2013]: https://www.sciencedirect.com/science/article/pii/S0143622813000684
[Pesaresi et al., 2016]: https://www.researchgate.net/profile/Martino_Pesaresi/publication/299597485_Operating_procedure_for_the_production_of_the_Global_Human_Settlement_Layer_from_Landsat_data_of_the_epochs_1975_1990_2000_and_2014/links/573192c208aed286ca0e1831/Operating-procedure-for-the-production-of-the-Global-Human-Settlement-Layer-from-Landsat-data-of-the-epochs-1975-1990-2000-and-2014.pdf
[Google Earth Engine]: https://earthengine.google.com/
[Stevens et al., 2015]: http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0107042
[assets]: https://github.com/hannahfriedrich/VHRPopProject/tree/master/assets
[script]: https://github.com/hannahfriedrich/VHRPopProject/blob/master/code/GEE_VHRPop.txt
[link]: https://code.earthengine.google.com/58214a4955a46ffc5d51d3be8ff0d0d9
