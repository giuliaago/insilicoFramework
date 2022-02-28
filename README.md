# in silico PCR Framework using ecoPCR

This framework aims to use ecoPCR from obitools on a user-defined database without the download of the entire EMBL database.

## Before starting

These softwares are required in order to install OBITools and ExTaxsI:
1. Download Python 2.7 and Python 3 if you haven’t already at https://www.python.org/
2. Download gcc at the GNU sites https://www.gnu.org/software/gcc/ and https://www.gnu.org/software/make/

## Download ecoPCR

Follow the instructions below to install OBITools:
1.  Download <code>get-obitools.py</code> script available at https://pythonhosted.org/OBITools/_downloads/get-obitools.py
2.  Run the command python <code>get-obitools.py</code>. All the OBITools will be installed in a new directory at the location where you run the script.

## Download sequences with Entrez tools

Download the locus sequences for the taxa of interest from GenBank using the command <code>esearch -db nucleotide -query "txid29178[ORGN] AND GENE " | efetch -format gb > file.gb</code>. 

## Import sequences as ecoPCR database

ecoPCR requires a database in the ecoPCR format in order to perform analyses. To convert the downloaded sequences to ecoPCR format, use the <code>obiconvert --genbank -t ./TAXO --ecopcrdb-output=file file.gb</code> command. 

## Run ecoPCR

Perform in silico PCR on the ecoPCR format sequences running the following command:

<code>ecoPCR -d file -e 3 -l 80 -L 300 \
   FORWARDPRIMER REVERSEPRIMER > file.ecopcr</code>

We have decided to:
* allow a maximum of three errors per primer with the option <code>-e 3</code> 
* have at least 80 bases long outputs with the option <code>-l 80</code> 
* have a maximum length of 200 bases outputs with the option <code>-L 300</code>

"FORWARDPRIMER" and "REVERSEPRIMER" indicate the sequences of the forward and reverse primers, respectively.


Run <code>ecotaxstat</code> command to evaluate the primer pairs performance comparing ecoPCR output, i.e. the amplified sequences, to the original ecoPCR database:

<code>ecotaxstat -d file -r 29178 file.ecopcr > ecotaxstat_file</code>

NB file stands for ecoPCR format database, file.ecopcr stands for ecoPCR amplified sequences, ecotaxstat_file is the output of the current command.

## Visualization of results

Extract the list of taxIDs from the ecoPCR outputs, i.e. amplified sequences, using this script: <code>xxx</code>

Convert the list of taxIDs to a 6 level rank file using the Taxonomy ID converter module (Module number 2) of the tool <code>ExTaxsI</code>. Download ExTaxsI at https://github.com/qLSLab/ExTaxsI

Now you are ready to visualize the results. Use the Visualization module (Module number 2) of <code>ExTaxsI</code> to create scatter plots and sunburst plots from the taxonomy file.
