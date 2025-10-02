Description
Here's a breakdown of the BacFluxL workflow:
1.	Preprocessing of Long Reads:
o	ONT sequences are trimmed and quality filtered using Filtlong.
2.	Assembly of Long Reads:
o	Filtered ONT reads are assembled using Flye. For more details, see the next step.
3.	Decontamination and Error Correction:
o	Filtered reads are mapped back to contigs using minimap2 and samtools.
o	Local alignments of contigs are performed against the NCBI nt database using BLAST+.
o	Contaminant contigs are checked with BlobTools. Unless otherwise specified (see configuration section for more details), the output of this step will be parsed automatically to discard contaminants based on the relative taxonomic composition of the contigs.
o	Medaka can be used to generate a consensus sequence from the assembled contigs and the original long reads. This consensus sequence should have a higher accuracy than the original assembled contigs, but this is not always the case, especially if ONT reads were base-called with the latest versions of the super accurate model of Dorado. For a deeper insight, please refer to Ryan Wick's bioinformatics blog. By leaving the medaka_model parameter blank, the error correction step won't be performed. By providing a fast model as parameter, BacfluxL will set Flye to assemble long reads using the --nano-raw parameter. If either an accurate or super accurate model is provided, Flye will use the --nano-hq parameter, instead. Same will happen if the medaka_model is left blank, assuming that data are high quality.
o	Bacterial chromosomes are reoriented using dnaapler, to start canonically with the dnaA sequence. Other replicons like plasmids and bacteriophages are also reoriented, using repA and terL, respectively, as starting point.
o	Genome completeness and contamination of long-read assembled bacterial chromosomes are assessed with CheckM using taxon-specific markers.
4.	Taxonomic Analysis:
o	Accurate taxonomic placement is performed with GTDB-Tk using a curated reference database.
5.	Annotation:
o	Contigs are annotated using Prokka and Bakta for functional prediction.
o	Further functional annotation is provided with EggNOG.
