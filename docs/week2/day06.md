
In this guide, we walk through several approaches—ranging from exploratory analyses to statistical modeling—for examining pathogen genome data in the context of infectious disease outbreaks using R. The aim is to reveal underlying transmission patterns and generate insights that may support outbreak containment strategies.

To achieve this, we draw on a variety of R packages: ape supports the construction of phylogenetic trees, adegenet facilitates genetic clustering and implements the SeqTrack algorithm for reconstructing transmission histories, and outbreaker2 enables probabilistic modeling of outbreak scenarios. Although the dataset used is fictional, the tools and techniques reflect real-world practices in genomic epidemiology.

**Emergence of a New Pathogen:**

A mysterious virus has been reported in Arkham, Massachusetts. The outbreak is characterized by neurological symptoms—including memory loss, hallucinations, and in rare cases, violent or animalistic behavior. Some patients with advanced disease develop grotesque physical changes. Authorities have issued warnings and are monitoring transmission closely.

**Goal of the Analysis:**

Your role is to analyze the available sequence and case data to trace possible transmission pathways. While the precise links between patients are unknown, viral genomes have been sequenced, providing the basis for genetic inference of who-infected-whom.

**Exploring the Data:**

We begin by loading the required R packages for the analysis:

```bash
library(ape) # Phylogenetic reconstruction

library(adegenet) # Genetic clustering and SeqTrack transmission analysis library(igraph) # Network analysis and visualization

library(outbreaker2) # Probabilistic outbreak reconstruction version 1.1.3
```

**Load and inspect case metadata:**

The dataset includes two key components: a CSV file named cases.csv, which contains information on the first 30 reported cases, and a FASTA file alignment.fa, which holds one genome sequence per case. These files are accessed directly from an online server.

We’ll begin by importing the case metadata:

The case data include several important fields. Each row represents a patient, identified by the id column. The *collec.dates* field records the sample collection date in yyyy-mm-dd format. Additional patient information includes sex (gender), age, and peak.fever, which reflects the highest recorded body temperature during illness. The outcome column summarizes the clinical result for each case (e.g., recovered or deceased). An extra column, notes, contains supplementary information. Notably, case 28 is flagged for potential DNA contamination, possibly due to a mixed sample.

```bash
cases <- read.csv("http://adegenet.r-forge.r project.org/files/fakeOutbreak/cases.csv")

head(cases)
```

**Parse collection dates:**

Since we’ll need to work with the collection dates for temporal analysis, we first convert the collec.dates column from character strings to Date objects. We then calculate a new variable, days, which represents the number of days since the first sample was collected (considered as day 0). This will allow us to easily compare timing across cases.

```bash
dates <- as.Date(cases$collec.dates)

range(dates)

days <- as.integer(difftime(dates, min(dates), unit="days"))

days
```

**Load genomic sequences:**

The viral genome sequences corresponding to the 30 cases are retrieved from the server and imported into R using the *fasta2DNAbin()* function, which reads the FASTA file and stores the sequences in a format suitable for genetic analysis:

```bash
dna <- fasta2DNAbin("<http://adegenet.r-forge.r-project.org/files/fakeOutbreak/alignment.fa>")
```

**Genetic distance distribution:**

To get a sense of the genetic diversity among the sampled viral genomes, we calculate pairwise Hamming distances, which count the number of nucleotide differences between each pair of sequences. This gives a simple measure of genetic divergence. We then visualize the distribution of these distances to explore how similar or different the sequences are:

```bash
D <- dist.dna(dna, model="N")

hist(D, col="royalblue", nclass=30,

main="Distribution of Pairwise Genetic Distances",

xlab="Number of Differing Nucleotides")
```

Despite the short timeframe and small genome size, the observed genetic variation is quite substantial. The bimodal shape of the distance distribution hints at the presence of at least two distinct genetic groups, or clades, among the samples.

**SNP positions and density:**

To investigate whether this variation is randomly spread across the genome—or concentrated in specific regions—we can extract the positions of single nucleotide polymorphisms (SNPs). This can be done easily using the *seg.sites()* function, which identifies all variable sites in the alignment:

```bash
snps <- seg.sites(dna)

head(snps)

length(snps)
```

A total of 79 variable sites (SNPs) were identified across the viral genomes in our sample. To explore whether these mutations are randomly distributed or concentrated in specific regions, we can visualize their genomic positions and assess SNP density across the genome. This helps us identify potential hotspots of polymorphism, which may indicate regions under selective pressure or prone to mutation.

```bash
plot(density(snps, bw=100), col="royalblue",

xlab="Nucleotide position", ylab="SNP density",

main="Location of the SNPs in the genome", lwd=2)

points(snps, rep(0, length(snps)), pch="|", col="red")

mtext(side=3, text="blue: density of SNPs red bars: actual SNP positions")
```

Here, the polymorphism seems to be distributed fairly randomly.

**Phylogenetic Analysis:**

To explore the evolutionary relationships among the taxa, we generated a phylogenetic tree based on pairwise genetic distances. The Neighbour-Joining method was used for tree reconstruction, as it efficiently clusters taxa based on their genetic divergence. Rather than relying on simple nucleotide mismatch counts—which may overlook important substitution patterns—we applied the Tamura-Nei model. This distance metric accounts for unequal substitution rates between transitions and transversions, offering a more nuanced view of sequence divergence. Alternative distance measures are also available through the *dist.dna* function in R.

```bash
# Compute pairwise genetic distances using the Tamura-Nei 93 (TN93) model

D.tn93 <- dist.dna(dna, model = "TN93")

# Construct a phylogenetic tree using the Neighbor-Joining (NJ) algorithm

tre <- nj(D.tn93)

# Root the tree at the first taxon (assumed to be the first case)

tre <- root(tre, 1)

# Rearrange the tree in a ladderized format for improved readability

tre <- ladderize(tre)

# Customize the tip labels to show case number and corresponding collection day tre$tip.label <- paste("Case", 1:30, "/ Day", days)

# Plot the tree with thicker edges and color-coded tips based on collection day plot(tre, edge.width = 2, tip.col = num2col(days, col.pal = seasun)) title("Neighbor-Joining Tree (TN93 distances)")

mtext(side = 3, text = "Rooted to first case")
```

**Genetic Clustering:**

Identifying epidemiological clusters based on phylogenetic trees can be challenging, particularly when sequence divergence is minimal or uneven. The adegenet package provides a practical solution through its gengraph function, which groups sequences into clusters based on a user-defined genetic distance threshold. Specifically, sequences are assigned to the same cluster if the number of mutations separating them falls below this cutoff. By default, gengraph runs in interactive mode, allowing users to explore and adjust clustering in real time:

Try a few values; you should see that 3 groups are obtained for anything between 15 and 25 mutations, with the result looking like this:

```bash
clust <- gengraph(D)
```
```bash
clust

plot(clust$g, main="Clusters obtained by gengraph") cases[28,]

plot(tre, tip.color=clust$col[clust$clust$membership], type="unrooted") title("Neighbour-Joining tree (TN93 distances)")

mtext(side=3, text="(unrooted tree)")

legend("bottomleft", fill=clust$col, legend=paste("group",1:3), title="Cluster of cases")

cases[28,]
```

**Transmission Reconstruction with SeqTrack:**

While the phylogenetic tree provides insights into potential transmission links, it does not take into account the timing of sample collection. To address this, the SeqTrack algorithm was developed. SeqTrack reconstructs ancestral relationships among sampled sequences by combining genetic distances with collection dates, generating a tree that minimizes the number of mutations (i.e., achieves maximum parsimony). In adegenet, this approach is implemented via the seqTrack function. In the following example, we apply seqTrack to the matrix of pairwise distances (distmat), specifying the case identifiers *(x.names = cases\$id)* and their associated collection dates *(x.dates = dates)*:

```bash
distmat <- as.matrix(D)

sqtk.res <- seqTrack(distmat, x.names=cases$id, x.dates=dates)

class(sqtk.res)

sqtk.res
```

The result *sqtk.res* is a *data.frame* with the special class *seqTrack*, containing the following information:

- *sqtk.res$id*: the indices of the cases.

- *sqtk.res$ances*: the indices of the putative ancestors of the cases.

- *sqtk.res$weight*: the number of mutations for each putative ancestry.

- *sqtk.res$date*: the collection dates of the cases.

- *sqtk.res$ances.date*: the collection dates of the putative ancestors.

seqTrack objects can be plotted simply using:

```bash
# Assuming sqtk.res$weight contains your edge weights

res <- sqtk.res

weights <- sqtk.res$weight

# Replace NA with a small positive value

weights[is.na(weights)] <- 1e-6

weights[weights==0] <- 0.01

# Update sqtk.res with the adjusted weights

res$weight <- weights

# Now, you can plot using the adjusted weights

g<-plot(res, main = "SeqTrack reconstruction of the outbreak")

mtext(side=3, text="red: no/few mutations; grey: many mutations")

g

tkplot(g)
```

**Outbreak Reconstruction with outbreaker2:**

The SeqTrack algorithm offers a parsimonious reconstruction of the transmission tree, integrating pathogen genetic data with the order of sample collection dates. While informative, this approach has important limitations: it assumes a single introduction event, does not incorporate time intervals between infections, does not estimate individual infection dates, overlooks plausible alternative transmission pathways, and provides no statistical confidence measures for inferred ancestries.

To address these gaps, the outbreaker method was developed \[2\]. outbreaker integrates pathogen genetic sequences, sample collection dates, and available (even vague) information about the generation time—the interval between infections in primary and secondary cases—to reconstruct transmission events. Importantly, it also estimates the timing of infections and provides a probabilistic framework for evaluating different transmission scenarios.

In this study, no external empirical data on the generation time distribution were available. Therefore, we constructed a minimally informative distribution designed to satisfy two criteria:

Inclusiveness: All possible transmission intervals observed during the outbreak must have non-zero probability. Monotonicity: The probability of transmission should decrease with increasing time intervals, favoring shorter delays between linked cases. This assumption aligns with the short timescales observed between early outbreak cases. Given the overall brief duration of the outbreak, this approach provides a flexible yet conservative modeling of possible transmission timelines.

**Prepare Input for outbreaker2:**

We now proceed to run outbreaker, supplying the pathogen DNA sequences, their corresponding collection dates (expressed as days since the first recorded sample), and the specified generation time distribution. Given the high confidence that all cases involved in the outbreak have been observed, we constrain the model to assume exactly one transmission event between consecutive cases by fixing the number of generations (kappa) to one. This is achieved by setting *move.kappa = FALSE* and initializing *init.kappa* to 1. Additionally, we initialize the Markov Chain Monte Carlo (MCMC) procedure using a star-like transmission tree, where all cases are assumed to originate directly from a common ancestor. This approach allows the MCMC to explore alternative transmission histories from a neutral starting point.

```bash
fake_outbreak
```
```bash
col <- "#6666cc"

plot(fake_outbreak$w, type = "h", xlim = c(0, 5),

lwd = 30, col = col, lend = 2,

xlab = "Days after infection",

ylab = "p(new case)",

main = "Generation time distribution")
```

**Running the analysis with defaults:**

In outbreaker2, outbreak reconstruction is conducted using the outbreaker() function. The primary input required is a dataset prepared with *outbreaker_data()*. Additional settings, such as prior distributions, MCMC parameters, and options for estimating specific model components, are configured through *create_config()*. Unless specified otherwise, default options generated by *create_config()* are applied. Users seeking to extend or modify the standard model—for example, by providing custom priors, likelihoods, or movement rules—should consult the "Customisation" vignette.

```bash
args(outbreaker)

dna <- fake_outbreak$dna

dates <- fake_outbreak$sample

ctd <- fake_outbreak$ctd

w <- fake_outbreak$w

data <- outbreaker_data(dna = dna, dates = dates, ctd = ctd, w_dens = w)

## we set the seed to ensure results won't change

set.seed(1)

res <- outbreaker(data = data)
```

This analysis typically requires about 40 seconds to complete on a standard modern computer. Although outbreaker2 may appear slower than the original outbreaker package when comparing the same number of iterations, the underlying algorithms differ substantially. Notably, outbreaker2 executes a greater number of parameter updates during each MCMC iteration, promoting more effective mixing. As a result, despite the increased computational time per iteration, fewer total iterations are needed to achieve convergence.

The output is returned as a data.frame assigned the special class outbreaker_chains:

Each row in res corresponds to a single MCMC sample. For every sample, information is recorded about the MCMC iteration (step), the log-posterior, log-likelihood, log-prior values, along with all estimated parameters and augmented data. Ancestral relationships—representing the inferred infector for each case—are stored as *alpha\_\[case index\]*. The estimated infection times are labeled as *t_inf\_\[case index\]*, and the number of generations separating each case from its inferred ancestor is indicated by *kappa\_\[case index\]*:

```bash
class(res)

#> [1] "outbreaker_chains" "data.frame" dim(res)

#> [1] 201 98

res names(res)
```

**Explore Results:**

The results can be explored using the plot function, which offers several options for generating different types of visualizations (see *?plot.outbreaker_chains* for details). By default, the plot displays the trace of the log-posterior values, providing a way to evaluate the mixing of the MCMC chain. Additionally, the second argument of plot allows users to visualize the trace of any other variable stored in res. A burnin parameter can also be specified to exclude the early iterations before the chain has reached stationarity:

```bash
plot(res) plot(res, "prior")

plot(res, "mu")

plot(res, "t_inf_15")

## compare this to plot(res)

plot(res, burnin = 2000)
```

The type argument controls the style of plot to generate, with several options available:

"trace" (default) displays MCMC traces for assessing convergence. "hist" or "density" show the distributions of numerical parameters. "alpha" and "network" visualize inferred ancestries or transmission trees; note that "network" produces an interactive plot that requires a web browser with Javascript enabled. The min_support setting can be adjusted to highlight only the most strongly supported transmission links, helping to simplify the network. "kappa" plots the distribution of the number of generations separating each case from its infector.

```bash
plot(res, "mu", "hist", burnin = 2000)

#> \`stat_bin()\` using \`bins = 30\`. Pick better value with \`binwidth\`.

plot(res, "mu", "density", burnin = 2000)

plot(res, type = "alpha", burnin = 2000)

#> Warning: \`guides(<scale> = FALSE)\` is deprecated. Please use \`guides(<scale> =

#> "none")\` instead.

plot(res, type = "t_inf", burnin = 2000)

#> Warning: \`guides(<scale> = FALSE)\` is deprecated. Please use \`guides(<scale> = #> "none")\` instead.

plot(res, type = "kappa", burnin = 2000)

#> Warning: \`guides(<scale> = FALSE)\` is deprecated. Please use \`guides(<scale> = #> "none")\` instead.

plot(res, type = "network", burnin = 2000, min_support = 0.01)
```

The summary function extracts key distributional statistics from the results, including summaries for the posterior, likelihood, and prior densities, as well as for the estimated quantitative parameters. It also constructs a consensus transmission tree by identifying, for each case, the most commonly inferred ancestor across the posterior samples.

The level of certainty for each inferred link is reported as the "support" value. In addition, the most frequently observed number of generations between a case and its infector is recorded under "generations":

```bash
summary(res)
```

**Customising settings and priors:**

```bash
config2 <- create_config(n_iter = 3e4,

> sample_every = 20, init_tree ="star",
>
> move_kappa = FALSE, prior_mu = 10)

set.seed(1)

res2 <- outbreaker(data, config2)

plot(res2)
```

