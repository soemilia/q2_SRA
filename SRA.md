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
  --output-path metadata_file_runs.qza
  ```

2.  ** **

```
qiime fondue get-all \
      --i-accession-ids metadata_file_runs.qza \
      --p-email your_email@somewhere.com \
      --p-retries 3 \
      --verbose \
      --output-dir fondue-output
``` 
