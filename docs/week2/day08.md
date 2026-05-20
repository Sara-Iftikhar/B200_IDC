
Welcome back! In practical 4, we explored hybrid genome assembly, where the integration of Oxford Nanopore Technologies (ONT) data significantly enhanced the continuity and accuracy of bacterial genome assemblies. By leveraging the long-read capabilities of ONT, we were able to resolve complex genomic regions, including repetitive elements and structural variations, which are often poorly assembled using short-read technologies alone.

Building on that foundation, today’s session will focus on an essential application of high-quality assemblies: plasmidome analysis in the context of practical pathogen research. Plasmids play a critical role in microbial adaptation and evolution, often carrying genes associated with antibiotic resistance, virulence, and metabolic functions. Understanding the structure and diversity of plasmids within a genome is key to interpreting pathogen behaviour and transmission.

In this session, we will:

- Identify plasmid contigs from hybrid assemblies using Bandage & BLAST
- Cluster similar plasmids across isolates to explore patterns of horizontal gene transfer and plasmid diversity
- Annotate plasmid sequences to determine functional content, such as resistance or virulence genes.
- Visualise the plasmid through the online website Proksee.

By the end of today’s session, you should be comfortable with the key steps and tools involved in plasmidome analysis and understand how this information contributes to broader epidemiological and functional studies of pathogens.

## Plasmid Replicons and Detection Using Bandage and PlasmidFinder

In plasmidome analysis, a key step is the identification of plasmid replicons—short DNA sequences that initiate and regulate plasmid replication. These replicons are critical because they define a plasmid’s incompatibility group (Inc group), which reflects its replication system and compatibility with other plasmids within the same bacterial cell. Recognizing replicons allows us to classify plasmids into known types, providing insight into their host range, stability, and potential for horizontal gene transfer. This is particularly important in the context of pathogenic bacteria, where certain plasmid types are strongly associated with antimicrobial resistance or virulence traits.

In today’s session, we will use replicon sequences from the PlasmidFinder database (https://cge.food.dtu.dk/services/PlasmidFinder/instructions.php), which offers a curated collection of known plasmid replicons associated with a wide range of incompatibility groups. Rather than using the web-based or command-line version of PlasmidFinder, we will apply a practical, visual approach to detecting these replicons directly in our assembly graphs.

### Log in to IBEX and copy today’s tutorial materials to your scratch folder

```bash
ssh -X <YourUserName>@ilogin.ibex.kaust.edu.sa
cp -r /ibex/project/c2325/B294c/Practical_sessions/Day8_Plasmidome/* /ibex/user/<YourUserName>
ls
```

### Let’s open one hybrid genome assembly file from last week by Bandage (these are the same from practical 4):

Download _HKPR018.fasta_ to your local machine, open the terminal on your computer and try below command:

```bash
scp <YourUserName>@ilogin.ibex.kaust.edu.sa:/ibex/project/c2325/B294c/Practical_sessions/Day8_Plasmidome/HKPR018.fasta /path/to/local/destination
```

![image](./images8/image01.png)

![image](./images8/image02.png)

Note that Bandage assigns colors to contigs randomly each time a file is opened, so the colors in your view may differ from those shown in this tutorial figure.

Here we provide you the replicon sequences in FASTA format from the PlasmidFinder website (Download _plasmidfinder.fasta_ from IBEX). We will use Bandage’s BLAST search function to identify any matching regions within our assembly graph.

![image](./images8/image03.png)

![image](./images8/image04.png)

![image](./images8/image05.png)

These matches are visualised directly within the graph, making it easier to distinguish plasmid sequences, which often appear as circular or separate components from chromosomal contigs.

![image](./images8/image06.png)

Once you finish the BLAST, you can view the blast hit result is labelled on the contigs. We consider the circular contigs with plasmid replicon detected as the plasmid sequence. Next, we will save these sequences in a separate file.

![image](./images8/image07.png)

![image](./images8/image08.png)

Open the saved fasta file and rename the fasta header to the corresponding format as ‘>SampleName_ContigNum’:

![image](./images8/image09.png)

Now that you’ve successfully identified the plasmid and extracted it in fasta format, you can apply the same steps to the remaining contigs in your assembly graph. However, consider this: what if you were working with more than 50 samples? Manually searching and inspecting each contig would quickly become time-consuming and impractical. This is where scripting and automation become essential in bioinformatics workflows.

To keep today’s session focused and efficient, we’ve already prepared the plasmid sequences for you. These include all the plasmid contigs identified across our dataset, so you can jump straight into the downstream analysis without having to repeat the identification process manually for each sample.

## Clustering Similar Plasmids Using mge-cluster

Identifying and grouping similar plasmids is a crucial step in plasmidome analysis, as it helps reveal shared mobile genetic elements (MGEs) and transmission patterns. This approach enables researchers to reduce dataset redundancy, identify dominant plasmid types, and better understand the structure of the plasmidome in complex microbial communities. In this section, we demonstrate how to apply mge-cluster to cluster plasmid assemblies and interpret the resulting groups.

mge-cluster (<https://gitlab.com/sirarredondo/mge-cluster>) is a bioinformatics tool designed specifically for scalable clustering of plasmid sequences based on sequence similarity and gene content. It uses sequence alignment and t-SNE to group plasmids into clusters that likely represent related replicons or functional units. t-SNE transforms complex, high-dimensional data (like the unitig presence/absence) into a lower-dimensional space (typically 2D or 3D) while preserving the local structure of the data—meaning, points that are close in the original space stay close in the embedded space.

This approach enables easy sharing of clustering models and standardisation of plasmid typing across research groups. As such, mge-cluster is a valuable tool for developing and maintaining a consistent classification framework for MGEs, which is critical for tracking the spread of elements carrying genes of concern, such as antimicrobial resistance determinants across bacterial populations.

Log in to your IBEX account and copy today’s file to your folder if you haven’t done yet. We provide 84 distinct plasmid contigs identified from our research data _(/ibex/project/c2325/B294c/Practical_sessions/Day8_Plasmidome/plasmids)_, see the _plasmid_list_ for your reference.

### View the run_mge_cluster.sh:

![image](./images8/image10.png)

1.  Load the software package mge-cluster with a specific version 1.1.0

2. creates a text file (mge_input.txt) that lists the full paths of all .fa (FASTA) files in the current directory

3. This command initialises a new clustering model with mge-cluster.

_--create_: tells mge-cluster to create a new clustering model (as opposed to projecting new sequences into an existing one).

_--input_: specifies the input file listing all the .fasta plasmid sequences to be clustered.

_--outdir_: sets the output directory for all results.

_--prefix_: assigns a prefix called new model to all output files, allowing you to identify and reuse the model later

_--perplexity_: This parameter sets the perplexity value used by openTSNE to embed the presence/absence of unitigs. controlling how local vs. global structure is preserved in the embedding. For thousands of sequences, we would recommend running mge-cluster with a higher perplexity value (100) than its default (30). Here we only give 69 seqeunces, default should be proper. If you had the time, we would recommend running mge-cluster with distinct perplexity values and observe the cluster stability.

_--min_cluster_: sets the minimum number of sequences to consider a cluster. You should set this parameter as the smallest size grouping you want to consider. In practice, when you increase this parameter, you get more conservative results, which means fewer clusters but also more sequences unassigned. When you decrease this parameter, you will get more clusters resulting from splitting previous groups, and more points will be assigned to them.

_--threads_: sets the number of CPU threads

Submit the job to IBEX and wait for it to run, it should be finished within one minute.

Download the results folder to your computer by scp and open the R Markdown _(Day8_Plasmidome_analysis.Rmd)_ in R Studio. We will use the R code to visualise the clustering pattern.

![image](./images8/image11.png)

The main output produced will be present at results/new-model_results.csv

![image](./images8/image12.png)

- tsne1D and tsne2D correspond to the coordinates of your samples in the embedding created by openTSNE.
- Standard_Cluster corresponds to the cluster assigned for the plasmid. Sequences that cannot be assigned to any of the defined clusters are labelled as -1
- Membership_Probability gives the strength of cluster membership for each sample. Unassigned sequences will have a value of 0.00000
- Sample_Name shows the name of the plasmid sequences provided in the input file

Let’s plot the clustering pattern:

![image](./images8/image13.png)

You can clearly observe that the sequences form four (0-2 and 3) distinct clusters, while one sequences remain unclustered (classified as noise). The clustered sequences are considered to be highly similar in sequence, suggesting shared structural or functional characteristics.

![image](./images8/image14.png)

## Annotating and Visualising Plasmid Sequences Using Proksee

To further investigate the structure and gene content of individual plasmids within each cluster, we will now move on to genome annotation and visualisation. For this step, we will use an online webpage portal called Proksee, which provides an interactive interface to annotate plasmid sequences and generate circular maps for easier interpretation.

Open your browser and access: <https://proksee.ca/> and click ‘login’

![image](./images8/image15.png)

![image](./images8/image16.png)

After you finish the registration and login, you can see the portal again and allow you to upload the sequence. Here we use the plasmid sequence HKPB511_2 from cluster 2 as an example:

![image](./images8/image17.png)

You can see a circle plot shown below. Let’s make it look fancier by adding the gene annotation, especially for the AMR genes.

Bakta is a fast and fully automated genome annotation tool tailored for bacterial genomes. It uses curated reference data to assign high-quality functional annotations, including gene names, product descriptions, and enzyme classifications. Bakta is especially useful for generating standardised and consistent annotations.

![image](./images8/image18.png)

![image](./images8/image19.png)

Wait for it to run, the running process will show on the page:

![image](./images8/image20.png)

![image](./images8/image21.png)

In the Proksee visualization, you can see that the origin of replication (ori) is clearly labeled on the plasmid map. Additionally, the presence of the tra locus is highlighted, indicating the plasmid’s potential for conjugation. We are also particularly interested in identifying antimicrobial resistance (AMR) genes and virulence factors. Proksee conveniently integrates the CARD (Comprehensive Antibiotic Resistance Database), allowing you to run resistance gene detection with just a few clicks.

![image](./images8/image22.png)

![image](./images8/image23.png)

After running, we can see some resistance genes are annotated with red color, hide the Bakta annotation label to make it clear:

![image](./images8/image24.png)

The CARD label OXA-1 will present

![image](./images8/image25.png)

Continue to the rest AMR genes, Scroll your mouse to zoom and view it clearly and repeat the step.

![image](./images8/image26.png)

After you zoom out, the view will look like this:

![image](./images8/image27.png)

We can also annotate the virulence gene through BLAST of the known VFDB database:

![image](./images8/image28.png)

![image](./images8/image29.png)

Add the feature like we run Bakta and CARD

![image](./images8/image30.png)

For the virulence gene feature, we can use the Bakta annotation gene name and change its label.

![image](./images8/image31.png)

Repeat the same steps for the remaining genes labelled in green. Below is the final annotated view. You can see that the resistance genes, including those for aminoglycosides and ESBLs, are scattered across the plasmid. Additionally, a cluster of iuc virulence genes is also present, suggesting enhanced pathogenic potential.

![image](./images8/image32.png)
