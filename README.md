# SegPhrase

## Requirements

We will take Ubuntu for example.

* g++ 4.8
```
$ sudo apt-get install g++-4.8
```
* python 2.7
```
$ sudo apt-get install python
```
* scikit-learn
```
$ sudo apt-get install pip
$ sudo pip install sklearn
```

## Build

SegPhrase can be easily built by Makefile in the terminal.
```
$ make
```

## Default Run

```
$ ./train.sh  #train a segmenter and output phrase list as results/salient.csv
$ ./parse.sh  #use the segmenter to parse new documents
```

## Parameters - train.sh

```
RAW_TEXT=data/DBLP.10K.txt
```
RAW_TEXT is the input of SegPhrase, where each line is a single document.

```
AUTO_LABEL=1
DATA_LABEL=data/wiki.label.auto
```
When AUTO_LABEL is set to 1, SegPhrase will automatically generate labels and save it to DATA_LABEL. Otherwise, it will load labels from DATA_LABEL.

```
KNOWLEDGE_BASE=data/wiki_labels_quality.txt
KNOWLEDGE_BASE_LARGE=data/wiki_labels_all.txt
```
We have two knowledge bases, the smaller one contains high quality phrases for positive labels while the larger one is used to exclude medium quality phrases for negative labels.

```
SUPPORT_THRESHOLD=10
```
A hard threshold of raw frequency is specified for frequent phrase mining, which will generate a candidate set.

```
OMP_NUM_THREADS=10
```
You can also specify how many threads can be used for SegPhrase

```
DISCARD_RATIO=0.00
```
The discard ratio (between 0 and 1) controls how many positive labels can be broken. It is typically small, for example, 0.00, 0.05, or 0.10. It should be EXACTLY 2 digits after decimal point.

```
MAX_ITERATION=5
```
This is the number of iterations of Viterbi training.

```
ALPHA=0.85
```
Alpha is used in the label propagation from phrases to unigrams.

## Parameters - parse.sh

```
./bin/segphrase_parser results/segmentation.model results/salient.csv 50000 ./data/test.txt ./results/parsed.txt 0
```
The first parameter is the segmentation model, which we saved in training process. The second parameter is the high quality phrases ranking list (together with unigrams). The third one determines how many top ranked phrases (unigrams) will be considered in this run of segmentation. The later two are the input and the output of corpus. The last one is a debug flag and you can just leave it as 0.
