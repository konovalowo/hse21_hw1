# hse21_hw1

# Команды, выполненные на сервере

```
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s211 oil_R1.fastq 5000000 > sub1.fq
seqtk sample -s211 oil_R2.fastq 5000000 > sub2.fq
seqtk sample -s211 oilMP_S4_L001_R1_001.fastq 1500000 > submp1.fq
seqtk sample -s211 oilMP_S4_L001_R1_002.fastq 1500000 > submp2.fq

mkdir fastqc
fastqc sub1.fq sub2.fq submp1.fq submp2.fq -o fastqc
mkdir multiqc
multiqc fastqc -o multiqc

platanus_trim sub1.fq sub2.fq
platanus_internal_trim submp1.fq submp2.fq

rm sub*.fq

mkdir fastqc_trim
ls | grep 'trimmed' | xargs -tI{} fastqc -o fastqc_trim {}
multiqc fastqc_trim/ -o multiqc_trim/

platanus assemble -f sub1.fq.trimmed sub2.fq.trimmed
platanus scaffold -c out_contig.fa -IP1 sub*.fq.trimmed -OP2 submp*.fq.int_trimmed
platanus gap_close -c out_scaffold.fa -IP1 sub*.fq.trimmed -OP2 submp*.fq.int_trimmed 

rm *trimmed
```

# MultiQC исходных чтений

![](/screen/fastqc_sequence_counts_plot.png)
![](/screen/fastqc_per_base_sequence_quality_plot.png)
![](/screen/fastqc_per_sequence_quality_scores_plot.png)
![](/screen/fastqc_per_sequence_gc_content_plot.png)
![](/screen/fastqc_per_base_n_content_plot.png)
![](/screen/fastqc_sequence_duplication_levels_plot.png)
![](/screen/fastqc_adapter_content_plot.png)
![](/screen/fastqc-status-check-heatmap.png)
![](/screen/fastqc_sequence_length_distribution_plot.png)

# MultiQC подрезанных чтений

![](/screen/trim/fastqc_sequence_counts_plot.png)
![](/screen/trim/fastqc_per_base_sequence_quality_plot.png)
![](/screen/trim/fastqc_per_sequence_quality_scores_plot.png)
![](/screen/trim/fastqc_per_sequence_gc_content_plot.png)
![](/screen/trim/fastqc_per_base_n_content_plot.png)
![](/screen/trim/fastqc_sequence_duplication_levels_plot.png)
![](/screen/trim/fastqc_adapter_content_plot.png)
![](/screen/trim/fastqc-status-check-heatmap.png)
![](/screen/trim/fastqc_sequence_length_distribution_plot.png)

# Анализ контигов и скаффолдов

Google colab: https://colab.research.google.com/drive/1yHjrkK5bstXiA3KNOg1n391SiLDGWyqX?usp=sharing

## Анализ контигов

- общее кол-во контигов: 594
- общая длина: 3923785
- длина самого длинного контига: 179304
- N50: 54904

## Анализ скаффолдов

- общее кол-во: 70
- общая длина: 3875624
- длина самого длинного: 3833789
- N50: 3833789

### Анализ исходных гэпов

- Количество гэпов самого длинного скаффолда: 59
- Общая длина гэпов самого длинного скаффолда: 5595

### Анализ гэпов после уменьшения с помощью подрезанных чтений

- Количество гэпов самого длинного скаффолда: 11
- Общая длина гэпов самого длинного скаффолда: 1880
