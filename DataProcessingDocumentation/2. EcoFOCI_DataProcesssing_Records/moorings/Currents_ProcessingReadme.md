# Current Processing Basis

## Workflow for ADCP Processing:

Run BBATCH software (on windows or wine) to get output processed data in ascii form.  
- This requires config files (.FMT) for processing
*** pyadps [https://pypi.org/project/pyadps/] or D.Strausz custom software in python (found in EcoFOCIpy/tools) no longer require BBATCH ***

Walk through CSV+MetaData -> NetCDF JupyterNotebook.
- point to files
- point to metafiles
- choose depth
- choose time corrections
- plot initial values for quick inspection
- trim deployment dates

QC Steps:
- find first good bin
- trim in the vertical
- remove excessively large values (500cm/s or 750 cm/s)
- interpolate max 1 hours linearly in time
- interpolate max 1 bins in the vertical

- compare to RCM if available (not often but sometimes collocated)

Tidal Evaluation
- Use Depth averaged u/v
- calculate tidal ellipses and compare to known good ellipses

Data Submission to ERDDAP
- untrimmed initial values go to initial_archive
- trimmed and qc'd values go to final_data_cf

## Workflow for RCM 7/9/11 & SG Processing:
- if RCM 7/9/11 , convert raw DSU data to ascii
- if SG , convert to tsv file

Walk through CSV+MetaData -> NetCDF JupyterNotebook.
- point to files
- point to metafiles
- choose depth
- choose time corrections
- apply temperature and pressure coefficients (***RCM 7/9/11 only***)
- plot initial values for quick inspection
- trim deployment dates

QC Steps:
- remove excessively large values
- compare to ADCP if collocated
- calculate tidal ellipse for reference location (or on whole stack)
