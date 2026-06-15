# Resources

Links to the software, tools, and packages used throughout the practicals.

## Programming & Environment

| Tool | Description |
|------|-------------|
| [R](https://cran.rstudio.com/) | Core language used for statistics, data wrangling, and visualization |
| [RStudio](https://posit.co/download/rstudio-desktop/) | IDE used to write and run R notebooks |
| [Miniconda](https://www.anaconda.com/download/success) | Package/environment manager used to install bioinformatics tools |

## Genome Assembly & Visualization

| Tool | Description |
|------|-------------|
| [Unicycler](https://github.com/rrwick/Unicycler) | Hybrid bacterial genome assembly pipeline (built on SPAdes) |
| [Bandage](https://github.com/rrwick/Bandage) | Visualizes genome assembly graphs |
| [Artemis](https://www.sanger.ac.uk/tool/artemis/) | Genome browser for viewing annotations, BLAST results, and variants |
| [AliView](http://www.ormbunkar.se/aliview/) | Alignment viewer |
| [BLAST+](https://blast.ncbi.nlm.nih.gov/Blast.cgi) | Sequence similarity search |

## Variant Calling & Phylogenetics

| Tool | Description |
|------|-------------|
| [Snippy](https://github.com/tseemann/snippy) | Variant calling and core genome alignment |
| [FastTree](https://morgannprice.github.io/fasttree/#Install) | Maximum-likelihood phylogenetic tree construction |
| [ape](https://cran.r-project.org/web/packages/ape/index.html) | R package for phylogenetic tree construction and manipulation |
| [ggtree](https://github.com/YuLab-SMU/ggtree) | R/Bioconductor package for tree visualization |
| [Gubbins](https://github.com/nickjcroucher/gubbins) | Detects and removes recombinant regions from alignments |

## Outbreak Analysis (R packages)

| Tool | Description |
|------|-------------|
| [adegenet](https://cran.r-project.org/web/packages/adegenet/index.html) | Genetic clustering and transmission reconstruction (SeqTrack) |
| [outbreaker2](https://cran.r-project.org/web/packages/outbreaker2/index.html) | Probabilistic outbreak/transmission reconstruction |

## Phylogeography (BEAST Suite)

| Tool | Description |
|------|-------------|
| [BEAST](https://github.com/beast-dev/beast-mcmc/releases) | Bayesian phylogenetic and phylogeographic inference |
| [BEAGLE](https://github.com/beagle-dev/beagle-lib) | Library for accelerating BEAST computations |
| [Tracer](https://github.com/beast-dev/tracer/) | Inspects MCMC output and convergence |
| [FigTree](https://github.com/rambaut/figtree/) | Phylogenetic tree viewer |
| [SPREAD4](https://spreadviz.org/home) | Web-based visualization of phylogeographic spread |

## Plasmid & AMR Analysis

| Tool | Description |
|------|-------------|
| [PlasmidFinder](https://cge.food.dtu.dk/services/PlasmidFinder/instructions.php) | Detects plasmid replicons |
| [Proksee](https://proksee.ca/) | Web tool for plasmid annotation and circular genome maps |
| [mge-cluster](https://gitlab.com/sirarredondo/mge-cluster) | Clusters plasmid sequences |

## HPC

KAUST's HPC system, **IBEX**, is used for compute-intensive steps in Week 2 (genome assembly, variant calling, and BEAST runs). Connect via SSH:

```bash
ssh -X your_username@ilogin.ibex.kaust.edu.sa
```
