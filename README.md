# Gaussian16-simulations

# Data and code for "Effect of benzothiadiazole-based π-spacers on fine-tuning of optoelectronic properties of oligothiophene-core donor materials for efficient organic solar cells: a DFT study."

**doi.org/10.1021/acs.jpca.3c04866**

## Contents

* *data-{type}/*: reproducible data
* *job.job : to submit a gaussion job

## Description of the data

The main data corresponding to the best structures are organized in subdirectories *data-{type}/{system}/* corresponding to
the considered molecules and simulation type:

* data-gs: Ground state calculations
* data-td: TD-DFT calculations

The contents of each subdirectory are:

* *data-gs/{system}/structure.xyz*: physical atomic structure
* *data-td/{system}/td-dft/td_uvvis.txt*: photoabsorption spectrum

The spectrum plots in the article correspond to the first (x values) and
second (y values) columns of the spectrum files.

## Reproduction of the data

The data was produced using Gaussian version g16.A.01

The calculation of the data of a system consists of
the following steps (in *bash* shell):

1. Ground-state calculation:
    * Prepare the inpute file of gs by adjasting the parameters of ground state calculations : 
       " # opt b3lyp/6-311+g(d,p) scrf=(smd,solvent=chloroform) geom=connectivity
        empiricaldispersion=gd3bj out=wfn "
        ** out = wfn keyword to create a wfn file of the ground state that will be used for EDD and RDG investigations
    * Submit the job.job file for the gs calculation as appropriate for the particular input file of the system
    * The optimized sturctures are visualised using GaussView
2. Time-propagation calculation:
    * Requires finished ground-state calculation
    * Set up the TD-DFT calculaton parameters as necessary
    ** # td=(nstates=6) wb97xd/6-311+g(d,p) scrf=(smd,solvent=chloroform)
      guess=read density out=wfn **
      ** density out = wfn keywords to create a wfn file of the escited state that will be used for EDD investigation
    * Submit the job.job file for TD-DFT calculation as appropriate for the particular system
    * The photoabsoption specta are visualised using GaussView
3. RDG calculation:
   * Put the .wfn file of the gs calculation in the command window of the open source Multiwfn software and follow the sturcture in Section 3.23.1 in the manual
4. DOS calculation:
   * The dos curves are plotted starting from the .fchk of the ground state geometry, select the atoms index corresponding to the diffrents subpart of the studied molecules (donor, acceptor, pi-sapcer) 
   * Put the .fchk file of the gs calculation in the command window of the open source Multiwfn software and follow the sturcture in Section 4.10.1 in the manual
      ** the output generates .chk file which is transformed to .fchk file : formchk .chk .fch **
5. TDM calculations:
   *    * Requires finished ground-state calculation
    * Set up the TD-DFT calculaton parameters as necessary
    ** # td=(nstates=6) wb97xd/6-311+g(d,p) scrf=(smd,solvent=chloroform)
      guess=read density transition=1 iop(6/8=3) out=wfn **
    * Put the .fchk file of the gs calculation in the command window of the open source Multiwfn software and follow the sturcture in Section 4.18.8 in the manual
6. EDD calculation:
   * Edd plots are plotted based on the es.wfn and gs.wfn following Section 4.18.1 in the manual
