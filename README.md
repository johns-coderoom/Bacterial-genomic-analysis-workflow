Here's a breakdown of the BacFluxL workflow:


Here's a breakdown of the BacFluxL workflow:

Preprocessing of Long Reads:

ONT sequences are trimmed and quality filtered using  https://github.com/rrwick/Filtlong.
Assembly of Long Reads:

Filtered ONT reads are assembled using Flye. For more details, see the next step.Preprocessing of Long Reads:

ONT sequences are trimmed and quality filtered using Filtlong.
Assembly of Long Reads:

Filtered ONT reads are assembled using Flye. For more details, see the next step.
Decontamination and Error Correction:

Filtered reads are mapped back to contigs using minimap2 and samtools.
Local alignments of contigs are performed against the NCBI nt database using BLAST+.
Contaminant contigs are checked with BlobTools. Unless otherwise specified (see configuration section for more details), the output of this step will be parsed automatically to discard contaminants based on the relative taxonomic composition of the contigs.
Medaka can be used to generate a consensus sequence from the assembled contigs and the original long reads. This consensus sequence should have a higher accuracy than the original assembled contigs, but this is not always the case, especially if ONT reads were base-called with the latest versions of the super accurate model of Dorado. For a deeper insight, please refer to Ryan Wick's bioinformatics blog. By leaving the medaka_model parameter blank, the error correction step won't be performed. By providing a fast model as parameter, BacfluxL will set Flye to assemble long reads using the --nano-raw parameter. If either an accurate or super accurate model is provided, Flye will use the --nano-hq parameter, instead. Same will happen if the medaka_model is left blank, assuming that data are high quality.
Bacterial chromosomes are reoriented using dnaapler, to start canonically with the dnaA sequence. Other replicons like plasmids and bacteriophages are also reoriented, using repA and terL, respectively, as starting point.
Genome completeness and contamination of long-read assembled bacterial chromosomes are assessed with CheckM using taxon-specific markers.
Taxonomic Analysis:

Accurate taxonomic placement is performed with GTDB-Tk using a curated reference database.
Annotation:

Contigs are annotated using Prokka and Bakta for functional prediction.
Further functional annotation is provided with EggNOG.
Secondary metabolites are inferred with antiSMASH.
Antimicrobial Resistance (AMR):

Contigs are screened for antimicrobial resistance and virulence genes using ABRicate.
Plasmids:

The presence of plasmids is investigated with Platon and confirmed with a BLAST search.
Prophages:

Contigs are screened for viral sequences with VirSorter2, followed by CheckV for refinement.
Reporting:

Results are parsed and aggregated to generate a report using MultiQC.

