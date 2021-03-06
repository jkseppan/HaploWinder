## HaploWinder

[HaploWinder](HaploWinder_v111.pl) is a perl script that performs uniparental haplogrouping of genotyped samples according to a user-given hierarchical tree.

## README contents

 **[1. General information](#1.)**<br>
	[1.1. What is HaploWinder?](#1.1)<br>
	[1.2. How to install and run](#1.2)<br>
	[1.3. Technical stuff (License, How to cite)](#1.3)<br>
	
**[2. Input and output files](#2.)**<br>
	[2.1. In general](#2.1)<br>
	[2.2. File format specifications](#2.2)<br>
	[2.3. Example files](#2.3)<br>
	
**[3. Other stuff](#3.)**<br>
	[3.1. Pre-GitHub version history](#3.1)<br>
	[3.2. Known bugs & issues](#3.2)<br>
	[3.3. Troubleshooting](#3.3)<br>
	
[**Appendix 1.** Dealing with discrepant marker sets](#app)
<br>
<br>
## <a name="1."></a>1. General information

### <a name="1.1"></a>1.1. What is HaploWinder?

[HaploWinder](HaploWinder_v111.pl) is a small program written in perl that performs haplogrouping of samples according to a user-given hierarchical tree. It has been designed and used for assigning haplogroups to samples genotyped with Y-chromosomal and mitochondrial DNA markers.
Note: HaploWinder does **_not_** perform haplotype inference of unphased diploid genotypes.

**Advantages of HaploWinder:**
-	faster and more accurate than manual haplogrouping
-	easy and fast to run repeatedly - also while the genotyping project is in progress, for example to figure out which samples to redo for which markers, etc.
-	allows some flexibility for the input genotype data format, especially genotype coding
-	gives a possibility to change the genotype coding of the results (exploiting the Genotype code file)
-	user-specified codes for multiple and no matches in the results
-	infers and fills in missing genotypes when possible, i.e., when these are unambiguous based on the successfully genotyped markers
-	keeps the inferred genotypes distinguishable from the original observed ones
-	checks the genotype input to avoid mismatches due to data input errors
-	tries to produce sensible and detailed error messages in plain English<br><br>

### <a name="1.2"></a>1.2. How to install and run

<br>**A summary in layman terms**

1. Get perl (ask your system administrator).
2. Install [HaploWinder](HaploWinder_v111.pl), i.e., save the file to a place where perl can reach it.
3. Prepare the input files.
4. Save the input files to the same place as HaploWinder.
5. Run HaploWinder from the command line.
6. Check the log file.
7. Use the results.

If you run HaploWinder repeatedly, you only need to do steps 1 and 2 once, and in step 3 the files can usually be modified from existing ones - just provide the new Genotype file. HaploWinder is also fast: step 5 should not take more than a few seconds.

<br>**In a bit more detail**<br>
<br>
Save [the program](HaploWinder_v111.pl) somewhere in a text format but with a name ending '.pl'. Save your input files (see [section 2.1](#2.1)) to the same location, in a text format with file name ending '.txt'. Run the program by (going to that location and) writing onto the command line

    perl haplowinder.pl Settings.txt

where `haplowinder.pl` is the name you gave to the program file and `Settings.txt` the name you gave to the Settings file, (and hitting enter). A successful run ends in a line beginning "Analysis ready", and giving the names of the Result and Log files.<br><br>

### <a name="1.3"></a>1.3. Technical stuff

<br>**License**<br>
<br>
The HaploWinder program is distributed under the [MIT License](LICENSE.txt).

<br>**How to cite HaploWinder**<br>
<br>
If use the HaploWinder program in any published work, please cite it as follows:

	HaploWinder v. 1.11 (Lappalainen et al. 2009; https://github.com/esalmela/HaploWinder/)

    Lappalainen T, Hannelius U, Salmela E, von Döbeln U, Lindgren CM, Huoponen K, Savontaus ML, Kere J, Lahermo P.
    Population structure in contemporary Sweden – A Y-chromosomal and mitochondrial DNA analysis.
    Annals of Human Genetics 2009, 73: 61-73.

  The [paper](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1469-1809.2008.00487.x) deals mostly with other stuff but introduces the program briefly.<br><br>



## <a name="2."></a>2. Input and output files

### <a name="2.1"></a>2.1. In general

The program takes four input files and produces two output files. Don't be scared of the number: two of the input files you can most probably copy straight from the examples or use with minor modifications, one may come directly from your genotyping platform or database, and only one, the Haplogroup file, you will have to construct yourself. Furthermore, once you have constructed the files, you can use them repeatedly with different input Genotype data files, with minor modifications to the Settings file.
The name of the Settings file is given on the command line, and the Settings file contains the names of the other files.

<br>**Settings file**<br>
<br>
This input file contains the parameters for the run: filenames, etc. Give the name of this file on the command line when running HaploWinder (remember to give the file name extension (.txt)). See [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files.

<br>**Genotype file**<br>
<br>
This input file contains the actual genotypes of your samples. There is some flexibility in the format, in order to accept a format produced by your local genotyping platform or database either as such or with as small modifications as possible. Sample rows, marker columns; see [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files. 

<br>**Genotype code file**<br>
<br>
This input file contains the valid genotype codes that the input Genotype file may contain, plus the codes to be used for each in the Result file. HaploWinder checks that all the genotypes in the Genotype file match to one of these codes, to ensure that the non-matches found in the subsequent comparison of sample genotypes to haplogroup definitions are real and not only caused by errors in interpreting the Genotype file. This file can also be used to define how the genotypes will be presented in the Result file. See [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files. 

<br>**Haplogroup file**<br>
<br>
This input file contains the haplogroup information, i.e., it lists which set of genotypes belongs to which haplogroup. Constructing this file from a haplogroup tree ([example](example1_tree.jpg)) can be a bit tedious, but needs to be done only once per a given set of markers. The correctness of this file is essential for the correct haplogroup inference, and there is no built-in inspection functionality in HaploWinder, so please check your Haplogroup file carefully. An error in this file could, for example, manifest itself in a higher number of samples matching to no haplogroup; in the case of a rare haplogroup such samples might not be numerous enough to raise the user's suspicion, but the results would nevertheless be wrong. So do check the Haplogroup file once more... See [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files. 

<br>**Result file**<br>
<br>
This output file will contain the results of the HaploWinder run: the genotypes of each sample (including inferred genotypes), the numbers of markers with missing, inferred and originally observed genotypes, the number of haplogroups that are concordant with the observed genotypes, and the name of the concordant haplogroup if there is only one. See [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files. 

<br>**Log file**<br>
<br>
This output file will contain information of the success of the HaploWinder run: error messages etc. Although most of the error messages and notifications will also appear on the screen during the run, it is advisable to read the Log file carefully before proceeding with the analysis of the Result file. The Log file is also meant to serve in analysis documentation. Note that if you run HaploWinder repeatedly with the same Log file name, the new Log file will not replace the old one but be appended to the end of it. See [section 2.2](#2.2) for format details, and [section 2.3](#2.3) for example files.<br><br>

### <a name="2.2"></a>2.2. File format specifications

<br>**In general**<br>
<br>
All input files must be (and the Result file will be) tab separated text files. In theory, any text editor should be suitable for constructing them (but see [Troubleshooting section 3.3](#3.3)).

<br>**Settings file**<br>
<br>
No header row. One row per parameter. Each row must have two elements separated by a tab: a parameter name and its value. Empty rows and rows with no tab will produce a warning. Row order is arbitrary, but all of the following 11 parameter rows must appear:

|Parameter name | Description|
|-|-|
|    *input_genotype_file*	|	name of the Genotype file|
|    *input_tree_file*		|	name of the Haplogroup file |
|    *input_genotype_code_file*	|	name of the Genotype code file |
|    *output_results_file*	|	name of the Result file |
|    *output_log_file*		|	name of the Log file|

Remember to include the file name ending ".txt". It should be possible to include also a path, if the file is (or should be, in the case of output files) located in another directory, but this has not really been tested.

*extracols*<br>
Number of columns in the Genotype file that precede the actual marker columns containing the genotypes of the samples. The minimum value for this parameter is 1, because the file is supposed to contain at least the sample names.

*samplename_col*<br>
Number of the column that contains the sample names in the Genotype data input file. If you set the parameter *include_extracols* (see below) to "no", this column is the only non-genotype data column that will be included in the Result file. If the parameter *include_extracols* is set to "yes", this parameter will have no effect (except that some error messages may contain sample names, and they will display the entries from this column), but it needs to be included in the Settings file anyway and will produce an error if missing.

*include_extracols*<br>	
Determines whether the Result file will contain all genotype-preceding columns of the Genotype file (yes) or only the sample name column (no).

*missing_genotype*<br>
Code used for missing genotypes in the Genotype file in its original form (i.e., before the conversions possibly defined in the Genotype code file).

*code_for_several_matches*<br>
Code to be used in the Result file for genotypes that match more than one haplogroup.

*code_for_no_match*<br>
Code to be used in the Result file for genotypes that do not match any haplogroup.

It is crucial to spell the parameter names exactly correctly: in case of a typo, the program will print an error message saying it did not find the corresponding parameter value or file name.

<br>**Genotype file**<br>
<br>
One header row, then one row per sample. The file can have more than one column before the genotype columns; indicate their number in Settings file in the parameter *extracols*. One of these non-genotype columns should contain the sample names; indicate its number in Settings file in the *parameter samplename_col*. The header-row entries of these non-genotype columns are arbitrary, but their total number must be equal to *extracols*.

Each genotype column contains the genotype data from one marker. The header row must contain the name of the marker, and these names must be unique and match those in the Haplogroup file (see [Appendix 1](#app) for exceptions). For the genotypes, any alphabetic coding should work, as long as the individuals' genotypes do not contain the tab character; however, upper case letters are recommended. (Even a numeric coding might work, but this has not been tested.)

<br>**Haplogroup file**<br>
<br>
One header row, then one row per haplogroup. The first column contains the haplogroup names. Other columns contain the corresponding genotype from each marker. The genotype coding must be the same as in the original genotypes in the Genotype file, i.e., before the possible recoding defined in the Genotype code file. The header row must contain the names of the markers, preceded by an arbitrary header for the haplogroup name column.
The order of columns need not be the same as for the Genotype data file, but the marker set should of course be the same (see [Appendix 1](#app) for exceptions). If there are discrepancies in the marker names between the two files, the program will print a warning. Markers in the Result file will appear in the same order as in the Haplogroup file.

<br>**Genotype code file**<br>
<br>
No header row. One row per genotype code. Row order is arbitrary. Empty rows and rows with no tab will produce a warning.

Each row should have two elements separated by a tab: before the tab a genotype code in the form that it appears in the input data (in the Genotype data and Haplogroup files), and after the tab the same code in the form that it should appear in the Result file. These two forms can of course be identical if you are happy with the codes that your genotyping platform or database produces (lucky you!).
All the genotype codes that (can) appear in the Genotype data and Haplogroup files, including the code for missing genotype, must be included in the Genotype code file. However, all the codes listed in the Genotype code file need not appear in the Genotype data and Haplogroup files.

<br>**Result file**<br>
<br>
The Result file contains first the non-genotype column(s): either all of them or only the sample name column, depending on the value of the parameter *include_extracols* in the Settings file.

The subsequent columns contain the genotypes of each sample, including inferred genotypes in lowercase letters and missing genotypes denoted by a user-defined code. The order of markers is the same as in the Haplogroup file, and the order of the samples is the same as in the Genotype file.
The last five columns contain the numbers of markers with missing, inferred and originally observed genotypes, the number of haplogroups that are concordant with the observed genotypes, and the name of the concordant haplogroup if there is only one (if there is less or more than one, the corresponding user-defined code will be printed).<br><br>

### <a name="2.3"></a>2.3. Example files

There are two sets of example files. They contain the four input files (\*_settings, \*_data, \*_codes and \*_tree for Settings, Genotype, Genotype code and Haplogroup files, respectively), the two resulting output files (\*_result and \*_run_log), and two extra files: a jpg image showing the tree that is coded into the Haplogroup file, and an Excel file containing comments to some of the input and output files.

|Example 1 | Example 2 |
|-|-|
|[example1_settings.txt](example_files/example1_settings.txt)|[example2_settings.txt](example_files/example2_settings.txt)|
|[example1_data.txt](example_files/example1_data.txt)|[example2_data.txt](example_files/example2_data.txt)|
|[example1_codes.txt](example_files/example1_codes.txt)|[example2_codes.txt](example_files/example2_codes.txt)|
|[example1_tree.txt](example_files/example1_tree.txt)|[example2_tree.txt](example_files/example2_tree.txt)|
|[example1_results.txt](example_files/example1_results.txt)|[example2_results.txt](example_files/example2_results.txt)|
|[example1_run_log.txt](example_files/example1_run_log.txt)|[example2_run_log.txt](example_files/example2_run_log.txt)|
|[example1_comments.xls](example_files/example1_comments.xls)|[example2_comments.xls](example_files/example2_comments.xls)|
|[example1_tree.jpg](example_files/example1_tree.jpg)|[example2_tree.jpg](example_files/example2_tree.jpg)|

<br><br>
## <a name="3."></a>3. Other stuff

### <a name="3.1"></a>3.1. Pre-Github version history

<br>**Current version**<br>
<br>
HaploWinder v.1.11 (released September 2008)
Identical to version 1.1 except for some copyright-related notes.

<br>**Earlier versions**<br>
<br>
HaploWinder v.1.1 (completed March 1st, 2007)<br><br>

### <a name="3.2"></a>3.2. Known bugs & issues

There are no known bugs.<br><br>

### <a name="3.3"></a>3.3. Troubleshooting

HaploWinder tries to produce sensible and detailed error messages in plain and intelligible English. However, it is also possible that you get a strange-sounding one, but then it probably does not come from HaploWinder itself but from the operating system behind, and you can ask your local computer wizard how to fix the problem, even if they do not know that much about haplogroups.

<br>**Input file format problems**<br>
<br>
Some text editors tend to add invisible characters to the start and/or end of files, and such characters could confuse the data import into HaploWinder. Another possible source of problems is the end-of-row character: a newline (\n) is preferable, but HaploWinder should also tolerate a Windows-style one (with \r). If you experience strange problems with input file formats, try asking your local computer wizard.

<br>**Obscure error messages encountered so far**

    print() on closed filehandle RESULT ...

Problem: the program was rerun while the Result file from the previous round (with the same name) was open. Solution: close the file, then rerun.
<br>
<br>
## <a name="app"></a>Appendix 1

<br>**Dealing with discrepant marker sets: When markers are missing from either Genotype or Haplogroup files**<br>
<br>
Usually, the Genotype data and Haplogroup files will have the same set of markers, and the program will print a warning if they do not. It is possible, however, to run the program despite the warning, if the discrepancy between the two marker sets is intentional:

A marker that appears in the Haplogroup file can be missing from the Genotype data file, for example if it has not been genotyped yet. Such markers will be treated in the analysis as they had a missing genotype reading from all the samples, and they will be included in the Result file.

A marker that appears in the Genotype data file can be missing from the Haplogroup file, for example if it does not correspond to any particular haplogroup, e.g., Y-chromosomal STR markers or certain mtDNA HVS markers. Such markers must be located before all the other marker columns in the Genotype data file, and counted into the number of non-genotype columns. Thus they will not enter the analysis at all. If they are needed in the Result file, the parameter *include_extracols* must have value 'yes'. Please note: as all the non-genotype columns will enter the Result file unchanged, the genotype codes in these columns will not be replaced even if that was defined in the Genotype codes file. This is an unintentional feature, and maybe in some later version of HaploWinder it will be possible to indicate which markers are not meant to be analyzed, e.g., by including columns of missing genotypes in the Haplogroup file. But don't try this yet: the current version of the program would interpret every sample with a non-missing genotype reading from such a marker as a non-match. Thus, at the moment, if you need to change the genotype coding in such columns, you have to do it in some other program.


