![qAlgorithms Logo](references/qAlogo.png)

# Introduction

Welcome to `qAlgorithms`, a comprehensive collection of C++ libraries designed specifically 
for processing analytical data. Our focus is on the non-target screening data domain, ensuring 
precise and reliable data processing for this complex field.

The `qBinning Beta` aims to expand the existing program by finding and eliminating edge
cases during the binning that lead to less accurate results and experiments with additional
control prameters throughout the entire program.

We are always open for suggestions and feedback regarding your useage of our algorithms,
so do not hesitate to open an issue on our github page.

# Installation and Usage
Please note that qAlgorithms is still in active development and result accuracy
cannot be guaranteed at this stage. 

## Windows
The entire qAlgorithms workflow is provided as an executable under "Releases" on
our github repository.
Note that qAlgorithms requires the libraries `libgcc_s_seh-1.dll`, `libgomp-1.dll` 
and `libwinpthread-1.dll`. If they are not present on your system already, you
can also download them under "Releases".

## Linux
Currently, no Linux releases are proveided. We recommend you to clone the repository
and compile from source using cmake and GCC.

## Usage
On windows, start qAlgorithms.exe using powershell. Avoid non-ASCII characters in 
filenames. If a folder or filename has a space in it, you need to enter the absolute
path with quotes to read in everything correctly.
To use qAlgorithms for processing mass spectrometry data, you need to convert your
measurements into .mzML files, for example with msconvert. Currently, only MS1 
data can be used, so you save some disk space if you filter them out at this stage.

qAlgorithms is a command line utility which reads mzML files and outputs them
as csv. You can select individual files or an entire directory to search for
mzML files recursively. All output is written into one folder, which you also
must specify. Below are some commands you will likely use:

`./qAlgorithms.exe -h` - Display the help menu, listing all availvable options.

`./qAlgorithms.exe -i C:/example/path/measurement.mzML -o ../my/results -printpeaks` - 
Process the file measurement.mzML and write a file with every detected peak
into the directory "results".

`./qAlgorithms.exe -i ./allMeasurements -o ./results -printall` - searches the directory
"allMeasurements" and all subdirectories for files ending in .mzML and process them.
All intermediate results, those being centroids, bins and peaks, are written to a .csv 
file and saved to the "results" directory. 

Some things to keep in mind:
* qAlgorithms can process both profile data and centroided data. When using
  centroided data, it is not possible to estimate an individual uncertainty
  for centroids, which leads to less accurate results. Where possible, process
  profile mode spectra instead.
* If you do not specify which results you want, no output will be written
* If multiple copies of the same file are found during recursive search,
  only one of them will be processed
* The different quality scores do not serve as a way to remove peaks from
  your results. They only indicate how well the data at every step fit our
  model assumptions regarding the mathematical properties of real peaks.
  All peaks which are provided in the peak table are statistically significant.
  The best current usage for quality scores is priorisation of peaks 
  during further analysis.

## Documentation
Full documentation can be found [here](https://odea-project.github.io/qAlgorithms/html/)

# Philosophy
Unlike many traditional data processing tools, our algorithms do not rely on manual user input 
parameters such as thresholds. Instead, they intelligently leverage the inherent properties of 
the measurement data itself. This approach allows the algorithms to dynamically assess and 
utilize the quality of the data, ensuring robust and reproducible results every time.

qAlgorithms aims to make your processing as fast as possible, so you don't have to spend more
time waiting for your computer to finish than evaluating results.

## Scientific Rigor
Our commitment to scientific validity is unwavering. The algorithms within `qAlgorithms` are 
rooted in well-established statistical tests. Our primary goal is to deliver results that 
aren't just accurate but also statistically significant, providing confidence in every analysis.

# Current Offerings

## qCentroids Algorithm
Our current flagship algorithm, `qCentroids` by Reuschenbach, Renner et al. 
[https://doi.org/10.1007/s00216-022-04224-y], is tailored for centroiding HRMS 
spectra. It excels in handling data from instruments like Orbitrap and TOF, 
converting their profile spectra into centroided data. Whether you're dealing with 
high-resolution mass spectrometry data or other analytical measurements, `qCentroids` 
offers a reliable solution without the usual hassles of parameter tweaks.

## qBinning Algorithm
The `qBinning` algorithm utilises the centroids generated by `qCentroids` to
produce extracted ion chromatograms. Like `qCentroids`, it requires no user
parameters. Binning allows you to reduce the amount of centroids considered
in future analysis by roughly 30%. The current `qBinning` program is based
on the algorithm presented by Reuschenbach, Renner et al. [https://doi.org/10.1021/acs.analchem.3c01079],
but implements additional steps for finding the highest amount of statistically
sound bins.

## qPeaks Algorithm
As the current end point of qAlgorithms, `qPeaks` uses a comprehensive peak model 
developed by Renner et al. [https://doi.org/10.1021/acs.analchem.4c00494] to 
identify peaks within the bins generated by qBinning. Every peak is statistically
significant, sidestepping the need for further filtering steps like a minimum
intensity requirement. The scores generated provide you with information about
how well every step of the process to your peak worked, and allow you to make
a statement about the confidence of your results. Like all parts of the qAlgorithms
project, `qPeaks` requires no user parameters.

## Future Roadmap
Our team is continuously researching and developing new algorithms to expand 
the capabilities of `qAlgorithms`. Stay tuned for more innovative solutions 
for analytical data processing!

---

**qAlgorithms** - Transforming data into insights.

