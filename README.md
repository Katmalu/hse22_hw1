# hse22_hw1
### часть 1 на сервере
Создаем ссылки на файлы:
```bash
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
Берем случайные чтения по random seed 1211:
```bash
seqtk sample -s1211 oil_R1.fastq 5000000 > pe1.fastq
seqtk sample -s1211 oil_R2.fastq 5000000 > pe2.fastq
seqtk sample -s1211 oilMP_S4_L001_R1_001.fastq 1500000 > mp1.fastq
seqtk sample -s1211 oilMP_S4_L001_R2_001.fastq 1500000 > mp2.fastq
```
Оцениваем чтения и создаем отчёт:
```bash
mkdir fastqc
ls pe* mp* | xargs -tI{} fastqc -o fastqc {}

mkdir multiqc
multiqc -o multiqc fastqc
```
Отчёт multiqc по необработанным чтениям:
![Снимок экрана (1993)](https://user-images.githubusercontent.com/103137801/194580645-8a39a3ee-e5ff-4ac7-8073-04979919b856.png)
![fastqc_per_base_sequence_quality_plot](https://user-images.githubusercontent.com/103137801/194581263-136809ff-8fca-4e5e-a932-f255aba5ea63.png)
![fastqc_per_sequence_quality_scores_plot](https://user-images.githubusercontent.com/103137801/194581456-3f814eea-971f-4ae1-8013-1b4f17e13507.png)
 
Обрезаем чтения:
```bash
platanus_trim pe*
platanus_internal_trim mp*
```
Удаляем ненужные данные:
```bash
rm pe1.fastq
rm pe2.fastq
rm mp1.fastq
rm mp2.fastq
```
Оцениваем обрезанные чтения и создаем отчёт:
```bash
mkdir fastqc_trimmed
ls pe* mp*| xargs -tI{} fastqc -o fastqc_trimmed {}

mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```

Отчёт multiqc по обрезанным чтениям:
![Снимок экрана (1998)](https://user-images.githubusercontent.com/103137801/194595982-ea13b30e-89b5-42e6-9631-eff540eeb22f.png)
![fastqc_per_base_sequence_quality_plot (1)](https://user-images.githubusercontent.com/103137801/194595696-7b5aad72-9f62-4e8b-9fd0-5ff3089606c1.png)
![fastqc_per_sequence_quality_scores_plot (1)](https://user-images.githubusercontent.com/103137801/194595781-5723b67a-651c-4518-9034-60115ebadc8f.png)

Строим контиги:
```bash
platanus assemble -f pe1.fastq.trimmed  pe2.fastq.trimmed -o files
```

Строим скафолды:
```bash
platanus scaffold -c files_contig.fa pe1.fastq.trimmed pe2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed -o files
```

Сокращаем гэпы:
```bash
platanus gap_close -c files_scaffold.fa -IP1 pe1.fastq.trimmed pe2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed -o files
```

Удаляем ненужные данные:
```bash
rm pe1.fastq.trimmed
rm pe2.fastq.trimmed
rm mp1.fastq.int_trimmed
rm mp2.fastq.int_trimmed
```
Переименование для удобства:
```bash
mv files_contig.fa contig.fa
mv files_scaffold.fa gap_scaffold.fa
mv files_gapClosed.fa scaffold.fa
```
Скачиваем contig.fa, gap_scaffold.fa и scaffold.fa.

Для доп части проднлаем аналогичные действия на 500000 и 150000 чтениях соответственно:
```bash
seqtk sample -s1211 oil_R1.fastq 500000 > pe1.fastq
seqtk sample -s1211 oil_R2.fastq 500000 > pe2.fastq
seqtk sample -s1211 oilMP_S4_L001_R1_001.fastq 150000 > mp1.fastq
seqtk sample -s1211 oilMP_S4_L001_R2_001.fastq 150000 > mp2.fastq
platanus_trim pe*
platanus_internal_trim mp*
rm pe1.fastq
rm pe2.fastq
rm mp1.fastq
rm mp2.fastq
platanus assemble -f pe1.fastq.trimmed  pe2.fastq.trimmed -o dop
platanus scaffold -c dop_contig.fa pe1.fastq.trimmed pe2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed -o dop
platanus gap_close -c dop_scaffold.fa -IP1 pe1.fastq.trimmed pe2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed -o dop
rm pe1.fastq.trimmed
rm pe2.fastq.trimmed
rm mp1.fastq.int_trimmed
rm mp2.fastq.int_trimmed
mv dop_scaffold.fa dop_gap_scaffold.fa
mv dop_gapClosed.fa dop_scaffold.fa
```
Скачиваем dop_contig.fa, dop_gap_scaffold.fa и dop_scaffold.fa в другую папку.

### часть 2 в Colab


```bash

```
