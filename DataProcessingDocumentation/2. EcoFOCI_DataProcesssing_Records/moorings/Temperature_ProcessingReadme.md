# Workflow for Temperature Sensor Processing

This applies to SBE39, SBE56 and MTRs other units with temperature such as RCM/Seaguards, Seacats, MicroCats, etc. Each will have similar temperature processing but will need to account for other datastreams

For Seabird instruments, use seabird software to download data record which will have engineering values already converted to temperatures
For MTRs which are developed in house, data records that are downloaded will be resistance values in hex that need to be converted and have the Steinhart-Hart equation and the calibration coefficients applied. Same is true of RCM units, Seaguards have converted output already.

Walk through CSV+MetaData -> NetCDF JupyterNotebook.
- point to files
- point to metafiles
- choose depth corrections 
    + we round to nearest meter (or half meter)
    + use pressure gauges as first corrections.  In shallow waters (<250m ?) we use dbar and m interchangeably
    + use proxy's like ADCP bright band if no pressure sensor is available
    + use designed depth if estimated deployment depth and actual depth are in close agreement (+/- 2m), otherwise correct for depth of ocean bottom discrepancy
- choose time corrections: common time bases are 2hr, 1hr, 30m, 15m, 10m and 1m
    + account for drift if known.  The shorter the timebase (30m or less) the more important drift correction may be
    + we commonly round to nearest hour on longer time bases (2h, 1h) when drift is small, just to peg to hourly without changing data.  Shorter timebases (15min or less) are commonly resampled to there time base (via a mean).  Since this is not usually a downsampling, peaks are not smoothed.
- interpolate/extrapolate/resample timebase to appropriate interval - may smooth peaks
- plot initial values for quick inspection
- trim deployment dates

QC Steps:
- look for good comparisons with field data points taken from CTD's
    + often only preliminary data is immediatly available.  This is usually acceptable for temperature.  We are looking for failed sensors, singleton points, and large offsets.  We do not correct small offsets unless systematic bias can be shown.  ADCP internal temp sensors often have this characteristic.
- look for good comparisons with other temperature timeseries at periods where water column is well mixed in relevant layers

Data Submission to ERDDAP
- untrimmed initial values go to initial_archive
- trimmed and qc'd values go to final_data_cf