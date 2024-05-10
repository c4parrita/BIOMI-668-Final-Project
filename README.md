# BIOMI-668-Final-Project

For this analysis, we will be de-replicating protein sequence files using CD-HIT and analyzing the resulting clusters using BLAST. 

These protein sequences come from specific protein classes (tape measure) that we predict will group together in some way when dereplicated. However, some of these seqeunces are significantly longer than the others they are grouped with. We want to know where these smaller seqeunces match the centroid (larger) sequence and look further into the overhang of the centroid sequence that did not match with the others. The unmatched overhang sections of these centroid sequences can possibly match to other protein classes or even uncategorized proteins when they are ran through BLAST, and can help us gather more protein classes and curate a larger dataset or even classify new, uncategorized proteins.   

*Step 1: Preparing environment*

Please make sure you are connected to Wi-Fi when running all scripts.

To get started, activate a conda environment using:
```
conda activate
```
Install CD-HIT to perform our clustering analysis using: 
```
conda install -c bioconda cd-hit
```

*Step 2: Downloading and preparing data*

Download all fasta files provided under the folder 'example_fastafiles' to your ownn preferred directory. 

Once dowloaded, concatenate all the fasta files into one with: 
```
cat ~path/to/fasta/directory/*.fasta >> combined_files.fasta 
```

This is necessary to perform CD-HIT, as it requires all fasta sequences analyzed to be in the same file. 

*Step 3: Clustering Data*

Now that we have our singular fasta file containing all of our sequences, use that as the input file to run CD-HIT:

```
cd-hit -c 1.00 -n 5 -T 0 -M 0 -i combined_fasta.fasta -o cluster_100iden
```
The options listed in the command refer to the following:

-c: sequence identity threhold 

-n: word_length

-T: number of threads

-M: memory limit

-i: input file

-o: output file

For this analysis, we will be clustering at 100% identity threshold. 

*Step 4: Running BLAST on the overhang*

After running CD-HIT, we will have a output .clstr file. The file will follow the format:
```
>Cluster 24
0	81aa, >YP_009905121.1... at 100.00%
1	81aa, >YP_009905299.1... at 100.00%
2	3280aa, >YP_009905444.1... *
```
">" indicates the start of a cluster followed by the sequences in that cluster. In this example, sequence 2 contains the character "*" at end end, indicating this is the largest (centroid) sequence of that cluster, with the remaining sequences 0 and 1 matching the centroid at 100%.

To BLAST our overhang, run blast_overhang_clusters.ipynb. The input will be your .clstr file and the folder containing all fasta files. Be sure that the .clstr file and the example_fastafiles folder are all in the same directory. Under the main() function, change the variables "fasta_folder" and "clstr_file" to match the paths to where you have the necessary file and folder. NOTE: Running this file will take some time (~20 minutes)

*Step 5: Results*

Once blast_overhang_clusters.ipynb finishes running, you will have a .xml and .txt file for each cluster in your .clstr file. The .xml files are the BLAST results, containing all the queries and sequence information. We want to look at the .txt files, as that lists the description of our results, and contains which protein classes came up from our results. This information is also included in the .xml files, however the .txt files make it easier to read. 

Now that we have these descriptions, we can see if the overhang of our centroid sequences possibly resulted in new proteins classes that can be of interest or even found uncategorized proteins that can now be categorized. 

