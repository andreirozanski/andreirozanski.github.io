---
layout: post
title:  "Pandas : pipe - Tablewise function application"
tag: DataAnalysis
---

[pipe](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pipe.html#pandas.DataFrame.pipe) is designed to help 
chaining function calls on [DataFrames](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) or
[Series](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html).

As showcase, lets grab ensembl genomes table and play with that. 

First, import libraries:

```
# import libraries 

import pandas as pd
import numpy as np

# show full columns

pd.set_option('display.max_colwidth', None)
```

As an example, lets fetch ensembl genomes table:

```
# get ensembl genomes table

colnames = ["name","species","division",
            "taxonomy_id","assembly","assembly_accession",
            "genebuild","variation","pan_compara",
            "peptide_compara","genome_alignments","other_alignments",
            "core_db","species_id",""]

df = pd.read_csv("ftp://ftp.ensemblgenomes.org/pub/current/species.txt",skiprows=1,sep="\t",names = colnames)
```

Lets define a few functions to work with the table:

```
# filter for EnsemblMetazoa only and get 'species' and 'taxonomy_id' columns

def clean_table(df):
    df = df[df['division']=="EnsemblMetazoa"]
    df = df[['species','taxonomy_id']]
    return(df)



# add a column 'short_name' that displays i.e. H.sapiens for homo_sapiens

def get_shorten_specie_name(df):
    name1 = df['species'].str.replace("^_","").str[0:1].str.upper()
    name2 = df['species'].str.replace("^_","").str.split("_").str[1]
    df['short_name'] = name1+"."+name2
    return(df)



# add https link to NCBI taxonomy database

def add_taxon_id_link(df):
    df['taxonomy_id_link'] = "https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id="+df['taxonomy_id'].astype(str)
    return(df)
```

And now lets use pipe to chain clean_table, get_shorten_specie_name, add_taxon_id_link:

```
df.pipe(clean_table).pipe(get_shorten_specie_name).pipe(add_taxon_id_link)
```

And the first 4 lines of the resulting table:

<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td>species</td><td>taxonomy_id</td><td>short_name</td><td>taxonomy_id_link</td><td></td></tr><tr><td>acyrthosiphon_pisum</td><td>7029</td><td>A.pisum</td><td>https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=7029</td><td></td></tr><tr><td>adineta_vaga</td><td>104782</td><td>A.vaga</td><td>https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=104782</td><td></td></tr><tr><td>aedes_aegypti_lvpagwg</td><td>7159</td><td>A.aegypti</td><td>https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=7159</td><td></td></tr><tr><td>aedes_albopictus</td><td>7160</td><td>A.albopictus</td><td>https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=7160</td><td></td></tr></tbody></table></figure>
<!-- /wp:table -->
