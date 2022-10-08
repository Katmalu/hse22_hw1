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
Оцениваем обрезанные чтения и создаем отчёт:
```bash
mkdir fastqc_trimmed
ls pe* mp*| xargs -tI{} fastqc -o fastqc_trimmed {}

mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```

![Снимок экрана (1998)](https://user-images.githubusercontent.com/103137801/194595982-ea13b30e-89b5-42e6-9631-eff540eeb22f.png)
![fastqc_per_base_sequence_quality_plot (1)](https://user-images.githubusercontent.com/103137801/194595696-7b5aad72-9f62-4e8b-9fd0-5ff3089606c1.png)
![fastqc_per_sequence_quality_scores_plot (1)](https://user-images.githubusercontent.com/103137801/194595781-5723b67a-651c-4518-9034-60115ebadc8f.png)
