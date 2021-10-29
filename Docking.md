
<h1 align = "center" >Introduction </h1 >
Cheminformatics is the use of computational techniques and information about molecules to solve problems in chemistry. This involves a number of steps: retrieving data on chemical compounds, sorting data for properties which are of interest, and extracting new information. This tutorial will provide a brief overview of all of these, centered around protein-ligand docking, a molecular modelling technique. The purpose of protein-ligand docking is to find the optimal binding between a small molecule (ligand) and a protein. It is generally applied to the drug discovery and development process with the aim of finding a potential drug candidate. First, a target protein is identified. This protein is usually linked to a disease and is known to bind small molecules. Second, a ‘library’ of possible ligands is assembled. Ligands are small molecules that bind to a protein and may interfere with protein function. Each of the compounds in the library is then ‘docked’ into the protein to find the optimal binding position and energy.

**Docking** is a form of molecular modelling, but several simplifications are made in comparison to methods such as molecular dynamics. Most significantly, the receptor is generally considered to be rigid, with covalent bond lengths and angles held constant. Charges and protonation states are also not permitted to change. While these approximations reduce accuracy to some extent, they increase computational speed, which is necessary to screen a large compound library in a realistic amount of time.

 we will perform docking of ligands into the N-terminus of Hsp90 (heat shock protein 90). The tools used for docking are based on the open-source software AutoDock Vina (Trott and Olson 2009).

<h2 align = "center" > Getting the data related to the Hsp 90 Protein </h2> 


Create a new history for this tutorial

 Rename the dataset to ‘Hsp90 structure’


	<h3>Search Galaxy for the Get PDB Tool: Request the accession code 2brc.</h3>
![image](https://user-images.githubusercontent.com/68779543/139439463-a6fff0f4-880d-4ca6-aa13-4fe16656f081.png)

	<h3>Rename the dataset tp 'Hsp90 structure .</h3>
![image](https://user-images.githubusercontent.com/68779543/139439613-99d4219c-af98-4c75-834a-a91c90e78c5a.png)


	<h3>	Check that the datatype is correct (PDB file).</h3>
 ![image](https://user-images.githubusercontent.com/68779543/139440098-5a44bc54-36d0-42e1-8b13-6b93f687fd61.png)


![image](https://user-images.githubusercontent.com/68779543/139321512-81da1b9d-5eb2-4b59-8ced-b9ae7aa20dba.png)



<h1 align = "center" >Separating protein and ligand structures </h1 >

<h2 align = "left" >Grep </h2 >

We will use the grep tool to separate these molecules into separate files, and then convert the ligand file into SDF/MOL format using the ‘Compound conversion’ tool, which is based on OpenBabel, an open-source library for analyzing chemical data

**We will follow the below instrunctions**
```
Search in textfiles 
(grep)  Tool: with the following parameters:
“Select lines from”: Downloaded PDB file ‘Hsp90 structure’
“that”: Don't match
“Regular Expression”: HETATM
All other parameters can be left as their defaults.
Rename the dataset ‘Protein (PDB)’
```
<h3>The result is a file with all non-protein (HETATM) atoms removed.</h3>

![image](https://user-images.githubusercontent.com/68779543/139320094-d2b67da4-6c5b-42c5-ab76-08855950b8ef.png)


Now we shall do the same but for the Ligands.
```
Search in textfiles (grep) Tool: with the following parameters. Here, we use grep again to produce a file with only non-protein atoms.
“Select lines from”: Downloaded PDB file ‘Hsp90 structure’
“that”: Match
“Regular Expression”: CT5 (the name of the ligand in the PDB file)
All other parameters can be left as their defaults.
Rename the dataset ‘Ligand (PDB)’.
```
**This produces a file which only contains ligand atoms.**

![image](https://user-images.githubusercontent.com/68779543/139320235-8929b777-f5f8-4675-97a6-b16624eb3252.png)



```
Compound conversion  Tool:  with the following parameters:
“Molecular input file”: Ligand PDB file created in step 2.
“Output format”: MDL MOL format (sdf, mol)
“Add hydrogens appropriate for pH”: 7.4
All other parameters can be left as their defaults.
Change the datatype to ‘mol’ and rename the dataset ‘Ligand (MOL)'
```
Applying this tool will generate a representation of the structure of the ligand in MOL format.

![image](https://user-images.githubusercontent.com/68779543/139320333-bf34ac6d-34c5-4a90-a542-4f63829a5734.png)



















<h1 align="center"> Creating and processing the compound library</h1>
<h2 align="center"> Chembl Database</h2>


Chembl is a chemical database of bioactive with Druglike properties.

Description: A file named "Ligand SMILES" is a text file in Simplified Molecular Input Line Entry System(SMILES) format that contains the requisite information about the ligand complexed with the Hsp90 protein.

ID: CHEMBL399530

Molecular weight: 352.39

Molecular formular:C20H20N2O4

Iupac Name: 4-[4-(2,3-DIHYDRO-1,4-BENZODIOXIN-6-YL)-3-METHYL-1H-PYRAZOL-5-YL]-6-ETHYLBENZENE-1,3-DIOL

We will generate our compound library by searching ChEMBL for compounds which have a similar structure to the ligand in the PDB file we downloaded in the first step. There is a Galaxy tool for accessing ChEMBL which requires data input in SMILES format; thus, the first step is to convert the ‘Ligand’ PDB file to a SMILES file. Then the search is performed, returning a SMILES file. For docking, we would like to convert to SDF format, which we can do once again using the ‘Compound conversion’ tool.

Proceed with  the below instrunctions.

```

Compound conversion Tool: with the following parameters:
“Molecular input file”: ‘Ligand’ PDB file
“Output format”: SMILES format (SMI)
Leave all other options as default.
Rename the output of the ‘compound conversion’ step to ‘Ligand SMILES’.

```

<h2>Following which we look up the Chembl database for our ligands </h2>


```
Search ChEMBL database Tool:  with the following parameters:
“SMILES input type”: File
“Input file”: ‘Ligand SMILES’ file
“Search type”: Similarity
“Tanimoto cutoff score”: 40
“Filter for Lipinski’s Rule of Five”: Yes
All other parameters can be left as their defaults.

```

**Given below are the step by step screenshots of the workflow**.
<p align="center">
 <img src="https://user-images.githubusercontent.com/68779543/139316408-957a222d-7c5a-469f-a3bc-18a5f3976283.png">

We search for  the Chembl database and use the Ligand SMILES file as our input  and we set the  parameters 'Search type ' as Similarity & the "Tanimoto cutoff Score ' as 40 . Also we apply the 'Filter for Lipsinki's Rule of Five '.
 
![image](https://user-images.githubusercontent.com/68779543/139316685-a84ec431-ea89-408e-bf9d-86aedd10b81b.png)
 
 
 Here we can see that our file has been executed and saved .
![image](https://user-images.githubusercontent.com/68779543/139317100-84ca9504-1035-4481-ae03-2c3b09bef9ef.png)

 Finally we have the output saved under the name ' Compound Library '
 <p align="center">
 <img src="https://user-images.githubusercontent.com/68779543/139318539-669abee5-d542-40d3-ba47-f75bd8e4e67b.png">



<h1> Prepare files for docking </h1>
 A processing step now needs to be applied to the protein structure and the docking candidates - each of the structures needs to be converted to PDBQT format before using the AutoDock Vina docking tool.

Further, docking requires the coordinates of a binding site to be defined. Effectively, this defines a ‘box’ in which the docking software attempts to define an optimal binding site. In this case, we already know the location of the binding site, since the downloaded PDB structure contained a bound ligand. There is a tool in Galaxy which can be used to automatically create a configuration file for docking when ligand coordinates are already known.

In this , we worked on compound conversion form of the protein and the CHEMBL search ligands to PDBQT format.  We searched for ‘Prepare receptor’ in the Galaxy tools and selected  “Protein PDB file” as the ‘Select a PDB file’ while all other parameters were left as default. 

 For the Ligands conversion, we searched for  Compound conversion in the Galaxy tools and select  “Compound library” file as the “Molecular input file”; selected “SDF” as the “Output format”; select “Yes” for the “Generate 3D coordinates”; set “Add hydrogens appropriate for pH” to 7.4 while all other parameters were left as default. Rename to ‘Prepared ligands’


![image](https://user-images.githubusercontent.com/68779543/139435148-339087aa-8ac8-466f-8197-1a96629aaa09.png)

Output of 'Prepared Ligands '.
 
``` 
Calculate the box parameters for an AutoDock Vina job Tool:  with the following  “Input ligand or pocket”: Ligand (MOL) file.
“x-axis buffer”: 5
“y-axis buffer”: 5
“z-axis buffer”: 5
“Random seed”: 1
Rename to ‘Docking config file’.
```

![image](https://user-images.githubusercontent.com/68779543/139435702-5bba954a-25e6-4e39-90ad-964007b452bf.png)

 
 
<h1> Docking </h1>
Now that the protein and the ligand library have been correctly prepared and formatted, docking can be performed
Search for **Docking** in the search tools  and set it with the following paramters.
```
Docking Tool: with the following parameters:
“Receptor”: ‘Protein PDBQT’ file.
“Ligands”: ‘Prepared ligands’ file.
“Specify pH value for ligand protonation”: 7.4
“Specify parameters”: ‘Upload a config file to specify parameters’
“Box configuration”: ‘Docking config file’
“Exhaustiveness”: leave blank (it was specified in the previous step)
```

![image](https://user-images.githubusercontent.com/68779543/139438093-fed1e404-a226-4b95-8130-b3a3b6fd648a.png)

This is how the data looks like of ligand 10 docked.

<h3>The output consists of a collection, which contains an SDF output file for each ligand, containing multiple docking poses and scoring files for each of the ligands. We will now perform some processing on these files which extracts scores from the SD-files and selects the best score for each.</h3>


<h1> Visualization </h1>
It can be useful to visualize the compounds generated.
We followed the below instructions for visualising the compounds.
```
Visualisation Tool: toolshed.g2.bx.psu.edu/repos/bgruening/openbabel_svg_depiction/openbabel_svg_depiction/3.1.1+galaxy0 with the following parameters:

“Molecular input file”: Compound library

“Embed molecule as CML”: No

“Draw all carbon atoms”: No

“Use thicker lines”: No

“Property to display under the molecule”: Molecule title

“Sort the displayed molecules by”: Molecular weight

“Format of the resultant picture”: SVG
```

<h3>This produces an SVG image of all the structures generated ordered by molecular weight.</h3>

![image](https://user-images.githubusercontent.com/68779543/139438782-3e50e8a7-6c27-4b4b-a553-7e1989e7d534.png)







<h1 align="center"> Calculation of fingerprints and clustering</h1>
<p align="center">
 <img src="https://user-images.githubusercontent.com/68779543/139301860-ff3b6f49-2f50-49b1-84ff-0f4abf989e8b.png" width="350" heigth="350">




In this step, we will group similar molecules together. 
A key tool👩‍🔬👩‍ in cheminformatics for measuring molecular similarity is fingerprinting, which entails extracting chemical properties of molecules and storing them as a bitstring.
These bitstrings can be easily compared computationally, for example with a clustering method.

We will label each compound first before proceeding with clustering.

**We will label each compound first before proceeding with clustering.**
  
We do this  by adding a second column to the ‘SMILES coumpound library 
Before clustering, let’s label each compound. To do so add a second column to the SMILES compound library containing a label for each molecule. The Ligand smiles are already labelled  like ‘/data/dnb02/galaxy_db/files/010/406/dataset_10406067.dat’ and we will label them in a more convenient manner.

  
```
Search for Replace in the search tools
“File to process”: Ligand SMILES.
“Find pattern”: add the current label of the SMILES here
“Replace with”: ligand
Save the output as Replace
```
**Given below is the already given label for the ligand** 
 
  ![image](https://user-images.githubusercontent.com/68779543/139303302-dfa3a7d4-f1c1-4f41-a4c4-cc533ac677e7.png)
 
  And this is the output of our operations saved as Replace.
  ![image](https://user-images.githubusercontent.com/68779543/139303920-40300466-bffc-4fe1-bbe1-a18c7ce74da6.png)



 <h3 align="center">  When labelling is complete 🚀, we can concatenate (join together) the library file with the original SMILES file for the ligand from the PDB file.</h3>
 
  ```

Search for Concatenate datasets Tool:  with the following parameters:
“Datasets to concatenate”: Output of the previous step which will be Replace
Click on Insert Dataset and in the new selection box  & select ‘Compound library’.
Run the step and rename the output dataset ‘Labelled compound library’.
  ```
  
 Given below is the concatenated dataset which has both the ligand Smiles and  the labels too saved under the name ‘Labelled compound library’.

  <p align="center"><img src="https://user-images.githubusercontent.com/68779543/139304398-dbaaf693-93a0-445a-85e6-b6090d718126.png" width="500" heigth="500">
  
  <h2 align="center">  Molecule to Fingerprints</h2>

```
Molecule to fingerprint Tool: with the following parameters:
“Molecule file”: ‘Labelled compound library’ file.
“Type of fingerprint”: Open Babel FP2 fingerprints
Rename to ‘Fingerprints’.
```
Fingerprinting has been completed  , in which the output consists all the chemical properties extracted from each ligand & stored in a bitstring format , later used for clustering.

![image](https://user-images.githubusercontent.com/68779543/139305469-45f05455-fd33-4e91-ae8a-c8a04b6f7a86.png)


**In the Next step we will proceed with clustering🔬🖥️**

Taylor-Butina clustering (Butina 1999) provides a classification of the compounds into different groups or clusters, based on their structural similarity. This methods shows us how similar the compounds are to the original ligand, and after docking, we can compare the results to the proposed clusters to observe if there is any correlation.

```
Taylor-Butina clustering Tool: toolshed.g2.bx.psu.edu/repos/bgruening/chemfp/ctb_chemfp_butina_clustering/1.5 with the following parameters:
 “Fingerprint dataset”: ‘Fingerprints’ file.
 “threshold”: 0.8
NxN clustering Tool: with the following parameters:
 “Fingerprint dataset”: ‘Fingerprints’ file.
 “threshold”: 0.0
 “Format of the resulting picture”: SVG
 “Output options”: Image
```

 <p align="center"><img src="https://user-images.githubusercontent.com/68779543/139308377-8d443db6-7c86-4394-b7e0-18cf135cfccb.png" width="600" heigth="600">


The image produced by the NxN clustering shows the clustering in the form of a dendrogram, where individual molecules are represented as vertical lines and merged into clusters. Merges are represented by horizontal lines. The y-axis represents the similarity of data points to each other; thus, the lower a cluster is merged, the more similar the data points are which it contains. Clusters in the dendogram are colored differently. For example, all molecules connected in red are similar enough to be grouped into the same cluster.

Dendrogram produced by NxN clustering. The library used to produce this image is generated with a Tanimoto cutoff of 80; here 15 search results are shown, plus the original ligand contained in the PDB file.

 <h1 align="center"> 💻Post-processing💻</h1>

These steps help us to analyse the docking results.

**Extract values from an SD-file**📋📝:  This tool comes under the section chemical tool box.
We extracted all stored values into tabular format from our collection of SD-files obtained in docking step and then combined the files together to create a single tabular file for each of the 13 ligands in the Compound library. The tabular file contains RMSD values and Docking score.

**Collapse Collection**: This tool comes under the section Collection operations. Using this tool we combined all the 13 tables obtained above into one single file for easier visualisation 💷 💶.

We now have a tabular file available which contains all poses calculated for all ligands docked together with scores and RMSD values.

![image](https://user-images.githubusercontent.com/68779543/139310232-43cbe7a8-f48e-4378-88e4-ceb2004a324d.png)

**Table 1 RMSD and Docking score for each ligand**

 <h3 align="center"> Visualisation of Protein and ligand by Docking</h3>
 
 <h3 align="Left"> Compound conversion</h3>
This tool comes under the section chemical tool box. The SD-files obtained from the docking step is converted to PDF format for visualisation.

 <h3 align="Left"> Visualisation</h3>
 We used NGLViewer for visualising the protein ligand interaction.

![image](https://user-images.githubusercontent.com/68779543/139311184-30283a9d-733a-46cf-ba7e-09e91b411f0d.png)

Docking of ligand 1 to the active site of HSP90

![image](https://user-images.githubusercontent.com/68779543/139311247-cc8fcdec-0829-49bf-8605-d3a601e52739.png)

Docking of ligand 2 to the active site of HSP90

Scatter plot 📊 📈 📉: A scatter plot is a type of plot or mathematical diagram used to display values of two variables for a set of data. If the variables are correlated, the points will fall along a line or curve. A scatter plot of RMSD (compared to optimal docking pose) against docking score is calculated below

![image](https://user-images.githubusercontent.com/68779543/139312492-389e24df-99fc-4979-bd64-a4e78db744bb.png)



<h1> List of contributors </h1>
1. G rep - @ohdrija 2️⃣ @Akhereose

2. compound conversion- @SuvaniErranki @Aribisala

3. Chembl database @priss @israelebhohimen

4. Calculating box parameters -@Prem

5. vina docking @chaarvi @mitykay

6. visualization @toluwalase @yash11

7. calculating molecular fingerprints & clustering @Prem  @Toheeb

8. Post processing @JananiSankar @ZainabAdamu  
