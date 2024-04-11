# AMC_Confidence_Intervals

This repository contains code illustrating the use of the [Approximate Monte Carlo Method](https://www2.census.gov/programs-surveys/decennial/2020/technical-documentation/complete-tech-docs/demographic-and-housing-characteristics-file-and-demographic-profile/data_analysis_resources/1_estimating_conf_intervals_2010_demo/1_Approx_Monte_Carlo_confidence_interval_paper.pdf) (AMC method), applied to 2010 Decennial Census data as an empirical evaluation of the method's reliability and as an illustration for how this method is expected to work with 2020 Decennial Census data; in the future, we expect to provide products based on the same AMC method, applied directly to the 2020 Decennial Census. The AMC approach was designed by United States Census Bureau research staff to provide a method for generating estimates of the amount of uncertainty introduced by the [2020 Decennial Census Top Down Algorithm](https://hdsr.mitpress.mit.edu/pub/7evz361i/release/2) (TDA), the formally private mechanism used to protect the confidentiality of individuals' census responses in the 2020 Census Redistricting (P.L. 94-171) and Demographic and Housing Characteristics data products.

The AMC method was inspired by traditional Monte Carlo methods, and works by taking a Privacy-Protected Microdata File generated by TDA and then generating a large number of replicates, executing the TDA repeatedly, in iterations which each treat this initial `PPMF0` as the ground truth. That is, the `PPMF0` is substituted for the confidential 2020 Census Edited File (CEF), and then TDA is run repeatedly in this mode; the set of files this illustration code is based on were constructed by deploying this procedure but using the 2010 CEF in lieu of the 2020 CEF, as a demonstration. This generates a series of iterates, `PPMFi`, for `i=1,2,...,25`, where `25` was a value determined empirically to be reasonable, as discussed in [the AMC paper](https://www2.census.gov/programs-surveys/decennial/2020/technical-documentation/complete-tech-docs/demographic-and-housing-characteristics-file-and-demographic-profile/data_analysis_resources/1_estimating_conf_intervals_2010_demo/1_Approx_Monte_Carlo_confidence_interval_paper.pdf). These PPMF files, based on 2010 Decennial Census data, can be downloaded from [this page](https://registry.opendata.aws/census-2010-amc-mdf-replicates/). Comparisons between the `PPMF0` and `PPMFi`, and variability in these comparisons, can then be used to construct estimates of uncertainty, including intervals that behave like traditional confidence intervals. That paper also discusses appropriate use of the AMC method, using the 2010 Decennial Census to examine when the AMC method works well and when its results should be interpreted cautiously.

The full AMC paper examines multiple approaches for generating confidence intervals. This repository does not illustrate every method in the paper, but in the Jupyter Notebook `3a_Approx_Monte_Carlo_confidence_interval_notebook.ipynb`, we provide a full working code example for constructing a bias-adjusted, RMSE-based confidence interval assuming a Student’s T distribution with 5 degrees of freedom. We hope external groups will find this example useful as a starting point for building their own uncertainty estimates. The Jupyter Notebook reads an example data file, `3c_jupyter_data.csv`, which contains aggregated counts from the PPMF files for several example queries. For data users who wish to view the example confidence interval code without using Jupyter, they can use a browser to open `3b_Approx_Monte_Carlo_Confidence_Intervals.html`. `3d_requirements.txt` and `3e_spark_requirements.txt` include all the dependencies required to run the pandas and pyspark code sections of the Jupyter notebook.

The Jupyter Notebook was run and tested using EMR 6.15.0.
