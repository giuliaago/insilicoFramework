# *In silico* Framework 

<p align="center">
  <img src="https://user-images.githubusercontent.com/86804408/156333546-aca868b4-f3df-47d2-a148-9516b52ec0b4.jpg" alt="IN silico PCR framework"/>
</p>

This framework aims for a data-driven selection of the best primer pairs candidates for a more conscious molecular characterization of any metabarcoding study. It integrates the widely used ecoPCR software [[1]](https://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-11-434) from OBITools [[2]](https://onlinelibrary.wiley.com/doi/full/10.1111/1755-0998.12428) with a user-defined database without the download of the entire EMBL database. 

The framework consists of the following steps: 
1. **Literature search: background information investigation.** This step consists in the exploration of the background information, i.e. ecological niche, already available in scientific literature to define the expected taxa present in the investigated environment.
2. **Literature search: molecular marker investigation.** Based on the predicted taxa, identify the most appropriate metabarcoding marker(s) with the resolution at the taxonomic levels of interest and, consequently, the suitable primer pairs to amplify them.
3. **Data availability and download.** Evaluate the genetic information available in the NCBI database (https://www.ncbi.nlm.nih.gov/) through different combination of queries, based on the TaxID, the locus of interest, and the metadata, and data visualization. This step is crucial to obtain a defined database.
4. ***In silico* amplification.** Amplify the sequences of the expected taxa using the primer pairs candidates via ecoPCR from the OBITools package. 
5. **Data exploration and visualization.** Evaluate primer pairs performance, i.e. producing visualizations. 

As shown in the figure, you can return to the literature search step in any phase of the framework if the output isn't as you expect.

As evidence of the feasibility and relevance of the proposed framework, we have uploaded the results of its application to the investigation of the giant red shrimp *A. foliacea* diet. Please visit the <code>Case_study</code> directory of this Github page to see the dedicated framework, the ecoPCR outputs, scatter plots and sunburst plots extracting the taxonomy of the genes amplificated; both the visualizations were created via ExTaxsI tool [[3]](https://academic.oup.com/gigascience/article/doi/10.1093/gigascience/giab092/6514924?login=true).

## Data availability and download

Once you have identified target taxa, metabarcoding marker(s), and primer pair candidate(s), use a combination of queries to assess the sequences available in the NCBI database. ExTaxsI tool was used to create the queries and download the data. You can check if the query used and the downloaded data are appropriate by visualizing the downloaded taxa. You can do this in two ways:

1. **Extract the taxIDs list from the downloaded sequences and then convert it to a taxonomy list**

or

2. **Download the taxonomy list**

### 1. Extract taxIDs list to get taxonomy list

Use the script <code>taxid_list_gb.sh</code> we have developed to extract the list of taxIDs from the sequences downloaded from NCBI database (GenBank). To download the script:

* Clone github repository (link: https://github.com/giuliaago/insilicoFramework) and:

  * Download ZIP from insilicoFramework github home page, then decompress it

or

  * Use git from command line: <code>git clone https://github.com/giuliaago/insilicoFramework</code>

Once download the script, 

* Move <code>taxid_list_gb.sh</code> in the directory where the downloaded sequences are 

* Check if the script is executable with command <code>ls -l</code>. If it is not run the command <code>chmod +x taxid_list_gb.sh</code>

* Execute the script using <code>./taxid_list_gb.sh</code>

As an output, the script generates a TSV file named <code>taxid_list_gb.tsv</code>, which contains a list of taxIDs extracted from GenBank downloaded sequences.

To convert the taxIDs list generated to a taxonomy list use <code>ExTaxsI</code> “Taxonomy ID converter” module. ExTaxsI is an open-source user friendly bioinformatic tool, written in Python 3.7, that you can download and install following the instructions present in its GitHub page https://github.com/qLSLab/ExTaxsI. If you want to know more about this tool, see [[3]](https://academic.oup.com/gigascience/article/doi/10.1093/gigascience/giab092/6514924?login=true).

Now you are ready to visualize the results. Use <code>ExTaxsI</code> "Visualization" module to generate a variety of plots from the previous taxonomy file.

### 2. Download the taxonomy list

* Use <code>ExTaxsI</code> “Database creation” module to download files with accession and taxonomy

Then generate a variety of plots from the previous taxonomy file using <code>ExTaxsI</code> "Visualization" module

## In silico amplification

### Download ecoPCR

ecoPCR is the program included in the OBITools package aimed at performing *in silico* PCR. Based on the pattern matching algorithm Agrep [[4]](https://www.usenix.org/legacy/publications/library/proceedings/wu.pdf), ecoPCR allows specifying the count of mismatches (up to a maximum of three) between the primer and the target sequence. Furthermore, this tool accepts multiple target sequences (i.e. the templates) which is crucial if you want to predict the amplification results of a metabarcoding study.

To use ecoPCR you have to install the OBITools package. Please, ckeck if you have the following required software:
* **Python 2.7**, that you can download at https://www.python.org/
* **gcc**, available at the GNU sites https://www.gnu.org/software/gcc/ and https://www.gnu.org/software/make/

To install OBITools package follow these instructions:
1.  Download <code>get-obitools.py</code> script available at https://pythonhosted.org/OBITools/_downloads/get-obitools.py
2.  When you have completed the download, move the file <code>get-obitools.py</code> to the folder where install OBITools. Run <code>python get-obitools.py</code> to create a new directory named "OBITools-VERSION" (i.e. version stands for the number of latest tool version available), where all the OBITools are installed automatically
3.  Run the script <code>./obitools</code> to activate the OBITools
4.  Finally, once you have finish, type <code>exit</code> to disactivate the OBITools

### Download sequences with Entrez tools

Now you can create the database that will be used for the following *in silico* PCR by downloading the sequences of interest according to the information (i.e  target taxa, the barcode marker(s), and primer pairs) found in the literature search. 

To download the target sequences from NCBI database use Entrez Direct (EDirect) [[5]](https://www.ncbi.nlm.nih.gov/books/NBK179288/) with the following command <code>esearch -db nucleotide -query | efetch -format</code>. 

Visit EDirect book to explore all the command specific options at https://www.ncbi.nlm.nih.gov/books/NBK179288/

### Import sequences as ecoPCR database

ecoPCR requires a database in the *ecoPCR* format in order to perform analyses. To convert the downloaded sequences to *ecoPCR* format, run <code>obiconvert --[inputformat] -t --ecopcrdb-output</code>. 

Please check which input file formats the command <code>obiconvert</code> supports at the page https://pythonhosted.org/OBITools/scripts/obiconvert.html

### Run ecoPCR

Perform *in silico* PCR on the *ecoPCR* format sequences running the following command:

<code>ecoPCR -d [mydatabase]</code>

Check all the command specific options avaialable at the page https://pythonhosted.org/OBITools/scripts/ecoPCR.html 

After the amplification, evaluate the primer pairs performance comparing ecoPCR output, i.e. the amplified sequences, to the original ecoPCR database. To do so, use this command: <code>ecotaxstat -d [mydatabase]</code>. You can add some specific option, check out there: https://pythonhosted.org/OBITools/scripts/ecotaxstat.html

## Data exploration and visualization

In order to explore and visualize the data generated you need to have a list of organisms that have been amplified. You can do this by extracting the species taxIDs list and then converting it to a taxonomy list. 

Use the script <code>taxid_list_ecopcr.sh</code> we have developed to extract the list of taxIDs from the ecoPCR outputs. To download the script:

* Clone github repository (link: https://github.com/giuliaago/insilicoFramework) and:

  * Download ZIP from insilicoFramework github home page, then decompress it

or

  * Use git from command line: <code>git clone https://github.com/giuliaago/insilicoFramework</code>

Once downloaded, 

* Move <code>taxid_list_ecopcr.sh</code> in the directory where there are ecoPCR outputs 

* Check if the script is executable with command <code>ls -l</code>. If it is not run the command <code>chmod +x taxid_list_ecopcr.sh</code>

* Execute the script using <code>./taxid_list_ecopcr.sh</code>

As an output, the script generates a TSV file named <code>taxid_list_ecopcr.tsv</code>, which contains a list of taxIDs extracted from ecoPCR amplified sequences.

To convert the taxIDs list generated to a taxonomy list use <code>ExTaxsI</code> “Taxonomy ID converter” module. 

Now you are ready to visualize the results. Use <code>ExTaxsI</code> "Visualization" module to generate a variety of plots from the previous taxonomy file.

## Case study: *Aristomorpha foliacea*

We have applied the descripted framework to the investigation of the giant red shrimp diet for food traceability aims. View the dedicated framework, the ecoPCR amplification outputs, scatter plots, and sunburst plots of the genes amplificated in the <code>Case_study</code> directory of this Github page. 

## References

[1] Ficetola, G. F., Coissac, E., Zundel, S., Riaz, T., Shehzad, W., Bessière, J., ... & Pompanon, F. (2010). An in silico approach for the evaluation of DNA barcodes. BMC genomics, 11(1), 1-10. https://doi.org/10.1186/1471-2164-11-434

[2] Boyer, F., Mercier, C., Bonin, A., Le Bras, Y., Taberlet, P., & Coissac, E. (2016). obitools: A unix‐inspired software package for DNA metabarcoding. Molecular ecology resources, 16(1), 176-182. https://doi.org/10.1111/1755-0998.12428

[3] Agostinetto, G., Brusati, A., Sandionigi, A., Chahed, A., Parladori, E., Balech, B., ... & Casiraghi, M. (2022). ExTaxsI: an exploration tool of biodiversity molecular data. GigaScience, 11. https://doi.org/10.1093/gigascience/giab092

[4] Wu, S., & Manber, U. (1992, January). Agrep–a fast approximate pattern-matching tool. In Usenix Winter 1992 Technical Conference (pp. 153-162). 

[5] Kans, J. (2022). Entrez direct: E-utilities on the UNIX command line. In Entrez Programming Utilities Help [Internet]. National Center for Biotechnology Information (US). URL: https://www.ncbi.nlm.nih.gov/books/
