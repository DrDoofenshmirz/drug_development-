<h1 align="center"> Calculation of fingerprints and clustering</h1>
<p align="center">
 <img src="https://user-images.githubusercontent.com/68779543/139301860-ff3b6f49-2f50-49b1-84ff-0f4abf989e8b.png" width="350" heigth="350">




In this step, we will group similar molecules together. 
A key tool in cheminformatics for measuring molecular similarity is fingerprinting, which entails extracting chemical properties of molecules and storing them as a bitstring.
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



 <h3 align="center">  When labelling is complete, we can concatenate (join together) the library file with the original SMILES file for the ligand from the PDB file.</h3>
 
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


**In the Next step we will proceed with clustering**

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




