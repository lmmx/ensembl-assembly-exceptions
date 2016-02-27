# ensembl-assembly-exceptions

Tabulated mappings of _Homo sapiens_ and _Mus musculus_ genome "assembly exceptions" in core Ensembl database releases.

For more detail than chromosome and patch coordinates, see [grc-issues](https://github.com/lmmx/grc-issues).

## Source

The mappings in this repository are derived from the Ensembl Core `assembly_exception` and `seq_region` tables where the [`assembly_exception.exc_type`](http://www.ensembl.org/info/docs/api/core/core_schema.html#assembly_exception) is either `PATCH_FIX` or `PATCH_NOVEL` (i.e. excluding `PAR`, pseudoautosomal region, and `HAP`, haplotype).

The SQL query [suggested by Bert Overduin on Biostars](https://www.biostars.org/p/106355/) was used for each core _Homo sapiens_ and _Mus musculus_ genome (i.e. Ensembl release) database at ensembldb.ensembl.org :

```sh
echo "show databases;" | mysql -h ensembldb.ensembl.org -u anonymous | grep -P '(homo_.*core_)|(mus_.*core_)' | parallel "mysql -h ensembldb.ensembl.org -u anonymous -o {} < get_chromosome_patches.sql > {}.txt"
```

Where [get_chromosome_patches.sql](https://github.com/lmmx/genomics-code/blob/master/scripts/sql/get_chromosome_patches.sql) is:

```sql
SELECT DISTINCT s1.name AS patch,
                s2.name AS chromosome,
                a.exc_seq_region_start AS start,
                a.exc_seq_region_end AS end

           FROM assembly_exception AS a,
                seq_region AS s1,
                seq_region AS s2
         
          WHERE a.seq_region_id = s1.seq_region_id
            AND a.exc_seq_region_id = s2.seq_region_id
            AND a.exc_type IN ('PATCH_FIX', 'PATCH_NOVEL');
```

For a single, given, database, e.g. `homo_sapiens_core_75_37` instead use:

```sh
mysql -h ensembldb.ensembl.org -u anonymous -o homo_sapiens_core_75_37 < get_chromosome_patches.sql
```

which gives a tab-separated output of IDs corresponding to patches associated with GRC issues, e.g. [HG-989](http://www.ncbi.nlm.nih.gov/projects/genome/assembly/grc/human/issues/?id=HG-989) for HG989_PATCH in the output of the above command:

```
patch	chromosome	start	end
HG989_PATCH	1	31872759	32017063
HG79_PATCH	9	136049442	136369192
HG104_HG975_PATCH	8	144743526	145146062
HG183_PATCH	17	62273514	62649312
HG186_PATCH	3	51416109	51584055
...
```

## Information

"Assembly exceptions" were introduced to the human assembly in Ensembl release 59 from issues raised by the Genome Reference Consortium (GRC), documented on their blog [here](http://www.ensembl.info/blog/2010/08/05/human-grch37-assembly-patches/) (August 5 2010).

* Information on patches can be found on the GRC project site [here](http://www.ncbi.nlm.nih.gov/projects/genome/assembly/grc/info/patches.shtml).
* The ENSEMBL blog post details how to access such 'non-reference' or 'assembly exception' regions [here](http://www.ensembl.info/blog/2011/05/20/accessing-non-reference-sequences-in-human/).
  * [Ensembl FAQ help page](http://www.ensembl.org/Help/Faq?id=291).
  * [Schema diagram](http://www.ensembl.org/info/docs/api/core/assembly_core.pdf) and [docs](http://www.ensembl.org/info/docs/api/core/core_schema.html#seq_region) for Ensembl SQL databases. 
* A table of all alternate loci scaffolds in GRCh37.p13 can be found at ftp://ftp.ncbi.nlm.nih.gov/genomes/archive/old_genbank/Eukaryotes/vertebrates_mammals/Homo_sapiens/GRCh37.p13/PATCHES/alt_scaffolds/alt_scaffold_placement.txt (FTP area archived December 2015).
