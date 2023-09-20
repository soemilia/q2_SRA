# Parte inicial para baixar o SRA
**Pré análise**

* Criando as pastar de ancoragem:
```
# Criando a pasta:
if not os.path.exists(results_path):
    os.makedirs(results_path)
```
```
# Definindo o caminho para os resultados:
meta_path = os.path.join(results_path, "metadata.qza")
seq_path = os.path.join(results_path, "single_reads.qza")
```

* Gerando o arquivo de .tsv:
```
# Definindo os ID para baixar:
ls_ids = ['SRR9031850']
ser_ids = pd.DataFrame({'id': ls_ids}).squeeze()
```
```
#salvando o arquivo
id.save('ser_ids.tsv')
```

1. **Convertendo o arquivo**

Convertendo o arquivo do formato .tsv para .qza utilizando a semântica `NCBIAccessionIDs`. Foi utilizando o arquivo id.tsv no lugar do metadata_file_runs.tsv.

```
qiime tools import \
  --type NCBIAccessionIDs \
  --input-path id.tsv \
  --output-path ids.qza
  ```

2.  **Baixando os arquivos**

Utilizando a função `qiime fondue get-all` é possível baixar o metadado e a sequência dos dados (raw sequencing data). NOte que é necessário a utilzação de e-mail válido pra tal fim.

```
qiime fondue get-all \
      --i-accession-ids id.qza \
      --p-email so*****@***mail.com \
      --p-retries 3 \
      --verbose \
      --output-dir fondue-output
```

Como resultado quatro arquivos podem ser achados no diretório
* metadata.qza: contém o metadado com a semantinca tipos `SRAMetadata`
* paired_reads.qza: contém o reads pareados  com a semantinca tipos `SampleData[PairedEndSequencesWithQuality]`
* single_reads.qza: contém o single reads com a semantinca tipos `SampleData[SequencesWithQuality]`
* failed_runs.qza: contém os IDs falhados  com a semantinca tipos `SRAFailedIDs`

  Nesse caso em questão, os dados foram coletados como paired_reads. Deste modo, somente o paired_reads.qza contém reads. Para vizuaizar e tabela pode ser utilizado o código a seguir:

```
qiime feature-table tabulate-seqs \
  --i-data fondue-output/paired_reads.qza \
  --o-visualization rep-seqs.qzv
```
  
------------------
Saíndo do plugin e utilzando outras funçoes


# A partir daqui é análise dos dados

  3. **Realizando a retirada de ruídos**


```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs fondue-output/paired_reads.qza \
  --p-trunc-len-f 250 \
  --p-trunc-len-r 250 \
  --p-n-threads 7 \
  --o-representative-sequences fondue-output/asv-sequences-0.qza \
  --o-table fondue-output/feature-table-0.qza \
  --o-denoising-stats fondue-output/dada2-stats.qza
```

4. **Visualizar os dados de forma tabular**
   
```
qiime metadata tabulate \
  --m-input-file fondue-output/dada2-stats.qza \
  --o-visualization fondue-output/dada2-stats-summ.qzv
```
5. **Visualizar os dados de forma tabular sumarizando as featrures**
```
qiime feature-table summarize \
  --i-table fondue-output/feature-table-0.qza \
  --o-visualization fondue-output/feature-table-0-summ.qzv
```
6. **Visualizar os dados de forma tabular sumarizando as ASV**
```
qiime feature-table tabulate-seqs \
  --i-data fondue-output/asv-sequences-0.qza \
  --o-visualization fondue-output/asv-sequences-0-summ.qzv
```
7. **Rodaando o classificador**
classificador: 'gg-13-8-99-nb-classifier'
jobs 7
```
qiime feature-classifier classify-sklearn \
  --i-classifier fondue-output/gg-13-8-99-nb-classifier.qza \
  --i-reads fondue-output/asv-sequences-0.qza \
  --p-n-jobs 7 \
  --o-classification fondue-output/taxonomy.qza
```

8. **Visualizando o resultado após o classificador**
```
qiime metadata tabulate \
  --m-input-file fondue-output/taxonomy.qza \
  --o-visualization fondue-output/taxonomy.qzv
```

9. **Filtrando a tabela**
```
qiime taxa filter-table \
  --i-table fondue-output/feature-table-0.qza \
  --i-taxonomy fondue-output/taxonomy.qza \
  --p-mode contains \
  --p-include p__ \
  --p-exclude 'p__;,Chloroplast,Mitochondria' \
  --o-filtered-table fondue-output/filtered-table-3.qza
```
10. **Visulizando a tabela após filtragem**
```
qiime feature-table filter-samples \
  --i-table  fondue-output/filtered-table-3.qza \
  --p-min-frequency 10000 \
  --o-filtered-table  fondue-output/filtered-table-4.qza
```
11. **Gerando o gráfico *taxa barplot***
```
qiime taxa barplot \
  --i-table  fondue-output/filtered-table-4.qza \
  --i-taxonomy  fondue-output/taxonomy.qza \
  --o-visualization  fondue-output/taxa-bar-plots-1.qzv
```


