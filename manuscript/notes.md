# Stuff We Could Talk About But Probably Shouldn't
* pairwise ANI w/in each clade/tribe - will show presence of multiple species
* core and accessory genome of each clade - could say something based on tribes, but maybe opening a can of worms
* core and accessory genome of each tribe - unable to say anything b/c of completeness issues
* accumulation curves for acIs - haven't yet fully sampled the pangenome

# Removed from Methods in Primary MS

## Single-Cell Genome Generation, Selection, Sequencing, and Assembly

SAGs were collected, sequenced, and assembled as described previously [@Martinez-Garcia2012; @Garcia2013; @Ghylin2014]. SAGs were phylogenetically classified using partial 16S rRNA genes [@Martinez-Garcia2012] and a controlled nomenclature for freshwater bacteria [@Newton2011a] by insertion into references trees created in the ARB software package [@Ludwig2004]. Genome sequences are available through IMG (https://img.jgi.doe.gov/cgi-bin/mer/main.cgi), and can be accessed by searching for the IMG Taxon OIDs given in Table 1. Additional information is available in the Supplemental Online Material.

## Metagenome Sampling, Sequencing, Assembly, and Binning

Sample collection, processing, DNA sequencing, metagenomic assembly, genomic binning, and phylogenetic classification for the Trout Bog samples have been described previously [@Bendall2016]. With the exception of sample collection, identical procedures were followed for the Lake Mendota samples, for which depth-integrated water samples were collected from the top 12 meters at 96 time points during ice-free periods from 2008 to 2011. Metagenomic sequence reads are publicly available on the JGI Genome Portal (http://genome.jgi.doe.gov/) under Proposal ID 394. Metagenome sequences are available through IMG (https://img.jgi.doe.gov/cgi-bin/mer/main.cgi), and can be accessed by searching for the IMG Taxon OIDs given in Table 1. Additional information is available in the Supplemental Online Material.

## Removed b/c no longer used

## Metabolic Network Reconstruction and Reverse Ecology
### Re-annotation of Peptidases and Glycoside Hydrolases

Many seed compounds were associated with reactions catalyzed by peptidases or glycoside hydrolases, and genes associated with these reactions were re-annotated. Peptidase sequences were annotated using the MEROPS batch BLAST interface using default parameters [@Rawlings2015]. Glycoside hydrolases were first annotated using dbCAN [@Yin2012] to assign these genes to glycoside hydrolase families, as defined in the Carbohydrate-Active enZYmes Database CAZY [@Lombard2014]. Hidden Markov Models for these sub-families were then downloaded from dbCAN, and HMMER3 [@Eddy2011] was used to assign these genes to individual sub-families using default parameters.

# Removed from Results in Primary MS

## Non-canonical Biosynthetic Routes

While the reverse ecology pipeline presented here identified aspects of acI metabolism not observed previously, our approach does have some significant limitations. First, because our analysis relied on constructing clade-level composite genomes, we cannot say anything about the metabolism of individual tribes or interactions between them. And second, metabolic network analysis does not account for biological pathway organization, and predicts organisms may synthesize compounds via non-canonical routes (see the Supplemental Online Material for an example). As a result, the pipeline may under-predict the number of auxotrophies for a genome.

I will illustrate this using the biosynthesis of iso-leucine, which has previously been predicted to be an auxotroph for clade acI-B [@Garcia2015]. Iso-leucine is most commonly synthesized from threonine, via the following pathway.

__Need to check in IMG__

The enzyme for the first step is missing.
Step 	EC # 	Reaction
1 	4.3.1.19 	L-threonine --> (2Z)-2-aminobut-2-enoate --> 2-iminobutanoate --> 2-oxobutanoate
2 	2.2.1.6 	2-oxobutanoate --> (S)-2-aceto-2-hydroxybutanoate
3 	1.1.1.86 	(S)-2-aceto-2-hydroxybutanoate --> (R)-2,3-dihydroxy-3-methylpentanoate
4 	4.2.1.9 	(R)-2,3-dihydroxy-3-methylpentanoate --> (S)-3-methyl-2-oxopentanoate
5 	2.6.1.42 	(S)-3-methyl-2-oxopentanoate --> L-isoleucine

Thus, I would anticipate 2-oxobutanoate being predicted as a seed compound.

Iso-leucine can also be synthesized from pyruvate via the following pathway. Figure 2 from Trevor's paper suggests acI uses this pathway. The enzyme for the second step is missing.
Step 	EC # 	Reaction
1 	2.3.1.182 	pyruvate --> R-citramalate
2 	4.2.1.35 	R-citramalate --> (2R,3S)-3-Methylmalate
3 	1.1.1.85 	(2R,3S)-3-Methylmalate --> 2-Oxobutanoate
4 	2.2.1.6 	2-oxobutanoate --> (S)-2-aceto-2-hydroxybutanoate
5 	1.1.1.86 	(S)-2-aceto-2-hydroxybutanoate --> (R)-2,3-dihydroxy-3-methylpentanoate
6 	4.2.1.9 	(R)-2,3-dihydroxy-3-methylpentanoate --> (S)-3-methyl-2-oxopentanoate
7 	2.6.1.42 	(S)-3-methyl-2-oxopentanoate --> L-isoleucine

Thus, I would anticipate R-citramalate to be predicted as a seed compound.

Neither compound is predicted as a seed. So what is the route for iso-leucine biosynthesis?

Using the shortest_path algorithm included as part of the networkX package, I computed the shortest path for L-isoleucine synthesis from both pyruvate and threonine. The path from pyruvate follows:
Step 	Reaction
A 	Pyruvate --> 2-Hydroxyethyl-ThPP
B 	2-Hydroxyethyl-ThPP --> (S)-2-aceto-2-hydroxybutanoate
C 	(S)-2-aceto-2-hydroxybutanoate --> (R)-2,3-dihydroxy-3-methylpentanoate
D 	(R)-2,3-dihydroxy-3-methylpentanoate --> (S)-3-methyl-2-oxopentanoate
E 	(S)-3-methyl-2-oxopentanoate --> L-isoleucine

The path from synthesis from threonine includes two additional steps to convert threonine to pyruvate. In this route, steps C, D, and E are the same as steps 5, 6, 7 in the canonical pathway. With this route, the missing step is bypassed by directly converting pyruvate to (S)-2-aceto-2-hydroxybutanoate via 2-Hydroxyethyl-ThPP instead of via the intermediates citramalate, (2R,3S)-3-Methylmalate and 2-Oxobutanoate.

The presence of alternative pathways reveals why iso-leucine was not identified as a seed compound.
