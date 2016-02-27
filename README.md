# ensembl-assembly-exceptions
Tabulated mappings of Homo sapiens and Mus musculus genome "assembly exceptions" to their location in core Ensembl database releases

"Assembly exceptions" were introduced to the human assembly in Ensembl release 59 from issues raised by the Genome Reference Consortium (GRC), documented on their blog [here](http://www.ensembl.info/blog/2010/08/05/human-grch37-assembly-patches/) (August 5 2010).

* Information on patches can be found on the GRC project site [here](http://www.ncbi.nlm.nih.gov/projects/genome/assembly/grc/info/patches.shtml).
* The ENSEMBL blog post details how to access such 'non-reference' or 'assembly exception' regions [here](http://www.ensembl.info/blog/2011/05/20/accessing-non-reference-sequences-in-human/).
  * [Ensembl FAQ help page](http://www.ensembl.org/Help/Faq?id=291).
  * [Schema diagram](http://www.ensembl.org/info/docs/api/core/assembly_core.pdf) and [docs](http://www.ensembl.org/info/docs/api/core/core_schema.html#seq_region) for Ensembl SQL databases. The mappings in this repository are derived from the `assembly_exception` and `seq_region` tables where `assembly_exception.exc_type` is `PATCH_FIX` or `PATCH_NOVEL`.
* A table of all alternate loci scaffolds in GRCh37.p13 can be found [here](ftp://ftp.ncbi.nlm.nih.gov/genomes/archive/old_genbank/Eukaryotes/vertebrates_mammals/Homo_sapiens/GRCh37.p13/PATCHES/alt_scaffolds/alt_scaffold_placement.txt) (FTP area archived December 2015).
