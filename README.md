# **q2-SRA**

Baixanado os dados do NCBI/SRA utilizando o q2-Fondue, seguindo o tutorial do plugin.

## Arquivos necessários

* metadata_file.tsv: Contendo os número de acesso do BioProject;
* metadata_file_runs.tsv: Contendo os número de acesso do Run.

Código de acesso específico para cada Database:

|Prefixo|	Nome|	Plataforma|
|-------|-----|----------|
|PRJNA|	BioProject accession number	|SRA (NCBI)
|PRJEB|	EBI Project accession	|ENA (EMBL-EBI)
|PRJD|	DDBJ BioProject	|DDBJ
|SRP|	SRA study accession|	SRA (NCBI)
|ERP|	EBI study accession	|ENA (EMBL-EBI)
|DRP|	DDBJ study accession|	DDBJ
|SRR|	SRA run accession	|SRA (NCBI)
|ERR|	ERA run accession	|ENA (EMBL-EBI)
|DRR|	DRA run accession	|DDBJ


## Referência
* https://github.com/bokulich-lab/q2-fondue/blob/main/tutorial/tutorial.md
