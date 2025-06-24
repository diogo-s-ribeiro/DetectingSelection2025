# MLSpeciationGenomics Workshop 2025 
## Workshop 2: Detecting Targets of Selection in Experimental Evolution

In this hands-on workshop session, we will analyse time-series frequency data from experimental evolution studies. We compare two methods for detecting targets of selection: a mechanistic approach and a deep learning-based method.

## 1. Preparation
### Install required software - Bait-ER

```
## download and compile Bait-ER
git clone https://github.com/mrborges23/Bait-ER.git Bait-ER

## install dependencies
sudo apt-get install liblapack-dev
sudo apt-get install libblas-dev
sudo apt-get install libboost-dev
sudo apt-get install libarmadillo-dev

## compile using g++
g++ baiter.cpp -o baiter -O2 -std=c++14 -llapack -lblas -larmadillo

```
For more details on how to install and run `Bait-ER`, check the [GitHub repository](https://github.com/mrborges23/Bait-ER).

### Install required software - Machine-learning approach

```
## install Python dependencies
pip install numpy, pandas, tensorflow, scikit-learn, matplotlib

```

## 2. PART 1: Detecting targets of selection using Bait-ER
`Bait-ER` is a Bayesian inference tool that calculates selection coefficients from genomic time-series data. In this tutorial, we will use simulated sequences of an Evolve and Resequence experiment (E&R) that consists of 1000 loci and 3 replicates across 8 time points sampled at each 10 generations. 

This information can be found in the sync file `part1_bait-er/sync_workshop.txt`. Sync files are described in Kofler et al. (2011). Briefly the first 3 columns register the reference contig, the position and the reference allele then all following n columns include the observed cound for each repllicate and timepoint of the experiment, following the format `A:T:G:C:N:deletion`.

The file we will use for our tutorial has the same information as the testing dataset we will use in PART 2 with machine learning, so we can compare our results (`part2_ml/test.npz`). We will use the already premade sync file found in this repository, but a code on how to produce it from the simulated data can be found in `part1_bait-er/workshop_make_sync.py`. A preview of what the sync file looks like:


```
## preview the input dataset
head -n 5 part1_bait-er/sync_workshop.txt 
```
---
In order to run `Bait-ER`, we need to define all the necessary parameters using a control file. Here we define the input file, the output file and the parameters to run `Bait-ER` including the number of loci, number of replicates and the number of time points.

```
## look into the workshop control file
cat part1_bait-er/workhshop_dsc.cf
```
---
After we have the sync and the control files ready, we can run `Bait-ER` by putting them in the same directory and running the executable that should have been compiled during installation.

```
## run bait-er
./baiter workshop_dsc.cf
```
---
After a few minutes, we should get the message `Bait-ER has finished!` and we should have in our directory the output file that we specified in our control file. This output file has information about the posterior of the selection coefficient (s) per locus. The file also outputh a log Baues factor to conclude whether a single locus evolves under neutrality or selection (assuming the hypothesis that s is different to 0). Finally, it also give the postirior values for the shape (a) and rate (b) parameters of the postirion gamma distribution of s, which can be used to calculate other quantities of interest (quantiles, credibility intervals, etc). For more information on this, , check the [GitHub repository](https://github.com/mrborges23/Bait-ER). 

Finally, return rows with NAs correspond to flat trajectories that change very little during the experiment. 

## 3. PART 2. Detecting targets of selection using Machine Learning 
For the tutorial using the machine learning approach, check the jupiter file in `part2_ml/ml_dsc.ipynb`.


---
Thank you for participating! If you have any questions or feedback, please feel free to reach out.










