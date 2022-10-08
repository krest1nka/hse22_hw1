#### Создаем симлинки на файлы:

```
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2.fastq 
```

#### Далее создаем рандомные семплы:

```
seqtk sample -s225 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s225 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s225 oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
seqtk sample -s225 oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
```

#### Анализируем данные:

```
mkdir fastqc
fastqc -o fastqc sub1.fastq 
fastqc -o fastqc sub2.fastq 
fastqc -o fastqc matep1.fastq
fastqc -o fastqc matep2.fastq
mkdir multiqc
multiqc -o multiqc fastqc
```

#### Получившиеся графики:

![alt-текст](https://github.com/krest1nka/hse22_hw1/tree/main/pics/1.jpg)

![alt-текст](https://github.com/krest1nka/hse22_hw1/tree/main/pics/2.jpg)

Все остальные можно посмотреть в html отчете в репозитории.

#### Подрезаем чтения и удаляем ненужные файлы:

```
platanus_trim sub1.fastq sub2.fastq
platanus_internal_trim matep1.fastq matep2.fastq
rm sub1.fastq sub2.fastq matep1.fastq matep2.fastq
```

#### Проделываем аналогичные шаги с подрезанными версиями:

```
mkdir fastqc_t
fastqc -o fastqc_t sub*
fastqc -o fastqc_t matep*
mkdir multiqc_t
multiqc -o multiqc fastqc_t
```

#### Получившиеся графики:

![alt-текст](https://github.com/krest1nka/hse22_hw1/tree/main/pics/4.jpg)

![alt-текст](https://github.com/krest1nka/hse22_hw1/tree/main/pics/5.jpg)


Все остальные можно посмотреть в html отчете в репозитории.

#### Собираем контиги, скаффолды и уменьшаем промежутки:

```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> scaffold.log
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed  matep2.fastq.int_trimmed 2> gapclose.log
```

#### [Анализируем данные](https://colab.research.google.com/drive/1aFFALSBt1pCwUcAKlgRUGLcMpVNV441r?usp=sharing):

##### Анализ контигов

Количество: 602

Общая длина: 3924007

Наибольшая длина: 179307

N50: 53980


##### Анализ скаффолдов

Общее количество: 73,

Общая длина: 3876007,

Длина самого длинного: 3831713,

N50: 3831713


##### Подсчет гэпов для необрезанных чтений

Количество: 1657

Длина: 6723


##### Подсчет гэпов для обрезанных чтений

Количество: 469

Длина: 1887






