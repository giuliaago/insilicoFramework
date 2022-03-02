# *In silico* framework 

<p align="center">
  <img src="https://user-images.githubusercontent.com/86804408/156333546-aca868b4-f3df-47d2-a148-9516b52ec0b4.jpg" alt="IN silico PCR framework"/>
</p>

This framework aims for a data-driven selection of the best primer pairs candidates to potentially use in a more conscious molecular characterization of any metabarcoding study. It integrates the widely used ecoPCR software <a href="https://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-11-434">[1]</a> from OBITools <a href=https://onlinelibrary.wiley.com/doi/full/10.1111/1755-0998.12428>[2]</a> with a user-defined database without the download of the entire EMBL database. 

The framework consists of the following steps: 
1. **Literature search: background information investigation.** This step consists in the exploration of the background information, i.e. ecological niche, already available in scientific literature to define the expected taxa present in the investigated environment.
2. **Literature search: molecular marker investigation.** Based on the predicted taxa, identify the most appropriate metabarcoding marker(s) with the resolution at the taxonomic levels of interest and, consequently, the suitable primer pairs to amplify them.
3. **Data availability and download.** Evaluate the genetic information available in the NCBI database (https://www.ncbi.nlm.nih.gov/) through different combination of queries, based on the TaxID, the locus of interest, and the metadata. This step is crucial to obtain a defined database.
4. ***In silico* amplification.** Amplify the sequences of the expected taxa using the primer pair(s) candidate(s) via ecoPCR from the obitools package. 
5. **Data exploration and visualization.** Evaluate primer pairs performance, i.e. producing visualizations. 

As shown in the figure, you can return to the literature search step in any phase of the framework if the output isn't as expected.

As evidence of the feasibility and relevance of the proposed framework, we have uploaded the results of its application to the investigation of the giant red shrimp *A. foliacea* diet. Please visit the **xxx** directory of this Github page to see ecoPCR outputs, scatter plots and sunburst plots extracting the taxonomy of the genes amplificated; both the visualizations were created via ExTaxsI tool <a href="https://academic.oup.com/gigascience/article/doi/10.1093/gigascience/giab092/6514924?login=true">[3]</a>.

## Download ecoPCR

ecoPCR is the program included in the OBITools package aimed at performing *in silico* PCR. Based on the pattern matching algorithm Agrep <a href="https://www.usenix.org/legacy/publications/library/proceedings/wu.pdf">[4]</a>, ecoPCR allows specifying the count of mismatches (up to a maximum of three) between the primer and the target sequence. Furthermore, this tool accepts multiple target sequences (i.e. the templates) which is crucial if you want to predict the amplification results of a metabarcoding study.

To use ecoPCR you have to install the OBITools package. Please, ckeck if you have the following required software:
* **Python 2.7**, that you can download at https://www.python.org/
* **gcc**, available at the GNU sites https://www.gnu.org/software/gcc/ and https://www.gnu.org/software/make/

To install OBITools package follow these instructions:
1.  Download <code>get-obitools.py</code> script available at https://pythonhosted.org/OBITools/_downloads/get-obitools.py
2.  When you have completed the download, move the file get-obitools.py to the folder where install OBITools. Run <code>python get-obitools.py</code> to create a new directory named "OBITools" followed by the number of latest version available, where all the OBITools are installed automatically
3.  Run the script <code>./obitools<code/> to activate the
4.  Once you have finish, type <code>exit<code/> to disactivate the OBITools

To download and install ExTaxsI, please visit ExTaxsI GitHub page https://github.com/qLSLab/ExTaxsI and follow the instructions. 

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

## References

[1] Ficetola, G. F., Coissac, E., Zundel, S., Riaz, T., Shehzad, W., Bessière, J., ... & Pompanon, F. (2010). An in silico approach for the evaluation of DNA barcodes. BMC genomics, 11(1), 1-10. https://doi.org/10.1186/1471-2164-11-434

[2] Boyer, F., Mercier, C., Bonin, A., Le Bras, Y., Taberlet, P., & Coissac, E. (2016). obitools: A unix‐inspired software package for DNA metabarcoding. Molecular ecology resources, 16(1), 176-182. https://doi.org/10.1111/1755-0998.12428

[3] Agostinetto, G., Brusati, A., Sandionigi, A., Chahed, A., Parladori, E., Balech, B., ... & Casiraghi, M. (2022). ExTaxsI: an exploration tool of biodiversity molecular data. GigaScience, 11. https://doi.org/10.1093/gigascience/giab092
