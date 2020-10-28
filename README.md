# gnmt
GNMT Benchmarking in GPUs 

# 1. Problem

This problem uses recurrent neural network to do language translation.

## Requirements
* [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)
* [PyTorch 20.06-py3 NGC container](https://ngc.nvidia.com/registry/nvidia-pytorch)
* Slurm with [Pyxis](https://github.com/NVIDIA/pyxis) and [Enroot](https://github.com/NVIDIA/enroot)

# 2. Directions
## Steps to download and verify data
Download the data using the following command:

```
cd ..
bash download_dataset.sh
cd -
```

Verify data with:

```
cd ..
bash verify_dataset.sh
cd -
```
## Steps to launch training

### NVIDIA DGX A100 (single node)
Launch configuration and system-specific hyperparameters for the NVIDIA DGX A100
single node submission are in the `config_DGXA100.sh` script.

Steps required to launch single node training on NVIDIA DGX A100:

1. Build the container and push to a docker registry:

```
docker build --pull -t <docker/registry>/mlperf-nvidia:rnn_translator .
docker push <docker/registry>/mlperf-nvidia:rnn_translator
```

2. Launch the training:

```
source config_DGXA100.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME run.sub
```

#### Alternative launch with nvidia-docker

When generating results for the official v0.7 submission with one node, the
benchmark was launched onto a cluster managed by a SLURM scheduler. The
instructions in [NVIDIA DGX A100 (single node)](#nvidia-dgx-a100-single-node) explain

However, to make it easier to run this benchmark on a wider set of machine
environments, we are providing here an alternate set of launch instructions
that can be run using nvidia-docker. Note that performance or functionality may
vary from the tested SLURM instructions.

```
docker build --pull -t mlperf-nvidia:rnn_translator .
source config_DGXA100.sh
CONT="mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> ./run_with_docker.sh
```

### NVIDIA DGX-2H (single node)
Launch configuration and system-specific hyperparameters for the NVIDIA DGX-2H
single node submission are in the `config_DGX2.sh` script.

Steps required to launch single node training on NVIDIA DGX-2H:

1. Build the container and push to a docker registry:

```
docker build --pull -t <docker/registry>/mlperf-nvidia:rnn_translator .
docker push <docker/registry>/mlperf-nvidia:rnn_translator
```

2. Launch the training:

```
source config_DGX2.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME run.sub
```

#### Alternative launch with nvidia-docker

When generating results for the official v0.7 submission with one node, the
benchmark was launched onto a cluster managed by a SLURM scheduler. The
instructions in [NVIDIA DGX-2H (single node)](#nvidia-dgx-2h-single-node) explain
how that is done.

However, to make it easier to run this benchmark on a wider set of machine
environments, we are providing here an alternate set of launch instructions
that can be run using nvidia-docker. Note that performance or functionality may
vary from the tested SLURM instructions.

```
docker build --pull -t mlperf-nvidia:rnn_translator .
source config_DGX2.sh
CONT="mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> ./run_with_docker.sh
```

### NVIDIA DGX-1 (single node)
Launch configuration and system-specific hyperparameters for the NVIDIA DGX-1
single node submission are in the `config_DGX1.sh` script.

Steps required to launch single node training on NVIDIA DGX-1:

1. Build the container and push to a docker registry:

```
docker build --pull -t <docker/registry>/mlperf-nvidia:rnn_translator .
docker push <docker/registry>/mlperf-nvidia:rnn_translator
```

2. Launch the training:

```
source config_DGX1.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME run.sub
```

#### Alternative launch with nvidia-docker

When generating results for the official v0.7 submission with one node, the
benchmark was launched onto a cluster managed by a SLURM scheduler. The
instructions in [NVIDIA DGX-1 (single node)](#nvidia-dgx-1-single-node) explain
how that is done.

However, to make it easier to run this benchmark on a wider set of machine
environments, we are providing here an alternate set of launch instructions
that can be run using nvidia-docker. Note that performance or functionality may
vary from the tested SLURM instructions.

```
docker build --pull -t mlperf-nvidia:rnn_translator .
source config_DGX1.sh
CONT="mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> ./run_with_docker.sh
```
### NVIDIA DGX A100 (multi node)
Launch configuration and system-specific hyperparameters for the NVIDIA DGX A100
multi node submission are in the following scripts:

* for the 2-node NVIDIA DGX A100 submission: `config_DGXA100_multi_2x8x192_dist.sh`
* for the 32-node NVIDIA DGX A100 submission: `config_DGXA100_multi_32x8x32_dist.sh`
* for the 128-node NVIDIA DGX A100 submission: `config_DGXA100_multi_128x8x16_dist.sh`

Steps required to launch multi node training on NVIDIA DGX A100:

1. Build the docker container and push to a docker registry
```
docker build --pull -t <docker/registry>/mlperf-nvidia:rnn_translator .
docker push <docker/registry>/mlperf-nvidia:rnn_translator
```

2. Launch the training

2-node NVIDIA DGX A100 training:

```
source config_DGXA100_multi_2x8x192_dist.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME --ntasks-per-node $DGXNGPU run.sub
```

32-node NVIDIA DGX A100 training:

```
source config_DGXA100_multi_32x8x32_dist.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME --ntasks-per-node $DGXNGPU run.sub
```

128-node NVIDIA DGX A100 training:

```
source config_DGXA100_multi_128x8x16_dist.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME --ntasks-per-node $DGXNGPU run.sub
```

### NVIDIA DGX-2H (multi node)
Launch configuration and system-specific hyperparameters for the NVIDIA DGX-2H
multi node submission are the following scripts:

* for the 16-node NVIDIA DGX-2H submission: `config_DGX2_multi_16x16x32.sh`
* for the 64-node NVIDIA DGX-2H submission: `config_DGX2_multi_64x16x16.sh`

Steps required to launch multi node training on NVIDIA DGX-2H:

1. Build the docker container and push to a docker registry
```
docker build --pull -t <docker/registry>/mlperf-nvidia:rnn_translator .
docker push <docker/registry>/mlperf-nvidia:rnn_translator
```
2. Launch the training

16-node NVIDIA DGX-2H training:

```
source config_DGX2_multi_16x16x32.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME --ntasks-per-node $DGXNGPU run.sub
```

64-node NVIDIA DGX-2H training:

```
source config_DGX2_multi_64x16x16.sh
CONT="<docker/registry>/mlperf-nvidia:rnn_translator" DATADIR=<path/to/data/dir> LOGDIR=<path/to/output/dir> sbatch -N $DGXNNODES -t $WALLTIME --ntasks-per-node $DGXNGPU run.sub
```

# 3. Dataset/Environment
### Publication/Attribution
We use [WMT16 English-German](http://www.statmt.org/wmt16/translation-task.html)
for training.

### Data preprocessing
Script uses [subword-nmt](https://github.com/rsennrich/subword-nmt) package to
segment text into subword units (BPE), by default it builds shared vocabulary of
32,000 tokens.
Preprocessing removes all pairs of sentences that can't be decoded by latin-1
encoder.

### Vocabulary
Vocabulary is generated by the following lines from the `download_dataset.sh`
script:

```
# Create vocabulary file for BPE
cat "${OUTPUT_DIR}/train.tok.clean.bpe.${merge_ops}.en" "${OUTPUT_DIR}/train.tok.clean.bpe.${merge_ops}.de" | \
  ${OUTPUT_DIR}/subword-nmt/get_vocab.py | cut -f1 -d ' ' > "${OUTPUT_DIR}/vocab.bpe.${merge_ops}"
```

Vocabulary is stored to the `rnn_translator/data/vocab.bpe.32000` plain text
file. Tokens are separated with a newline character, one token per line. The
vocabulary file doesn't contain special tokens like for example BOS
(begin-of-string) or EOS (end-of-string) tokens.

Here are first 10 lines from the `rnn_translator/data/vocab.bpe.32000` file:
```
,
.
the
in
of
and
die
der
to
und
```

### Text datasets
The `download_dataset.sh` script automatically creates training, validation and
test datasets. Datasets are stored as plain text files. Sentences are separated
with a newline character, and tokens within each sentence are separated with a
single space character.

Training data:
* source language (English): `rnn_translator/data/train.tok.clean.bpe.32000.en`
* target language (German): `rnn_translator/data/train.tok.clean.bpe.32000.de`

Validation data:
* source language (English): `rnn_translator/data/newstest_dev.tok.clean.bpe.32000.en`
* target language (German): `rnn_translator/data/newstest_dev.tok.clean.bpe.32000.de`

Test data:
* source language (English): `rnn_translator/data/newstest2014.tok.bpe.32000.en`
* target language (German): `rnn_translator/data/newstest2014.de`
  * notice that the `newstest2014.de` file isn't tokenized, BLEU evaluation is
    performed by the sacrebleu package and sacrebleu expects plain text raw data
    (tokenization is performed internally by sacrebleu)

Here are first 5 lines from the `rnn_translator/data/train.tok.clean.bpe.32000.en` file:
```
Res@@ um@@ ption of the session
I declare resumed the session of the European Parliament ad@@ jour@@ ned on Friday 17 December 1999 , and I would like once again to wish you a happy new year in the hope that you enjoyed a pleasant fes@@ tive period .
Although , as you will have seen , the d@@ read@@ ed &apos; millenn@@ ium bug &apos; failed to materi@@ alise , still the people in a number of countries suffered a series of natural disasters that truly were d@@ read@@ ful .
You have requested a debate on this subject in the course of the next few days , during this part-session .
In the meantime , I should like to observe a minute &apos; s silence , as a number of Members have requested , on behalf of all the victims concerned , particularly those of the terrible stor@@ ms , in the various countries of the European Union .
```

And here are first 5 lines from the `rnn_translator/data/train.tok.clean.bpe.32000.de` file:
```
Wiederaufnahme der Sitzungsperiode
Ich erkläre die am Freitag , dem 17. Dezember unterbro@@ ch@@ ene Sitzungsperiode des Europäischen Parlaments für wieder@@ aufgenommen , wünsche Ihnen nochmals alles Gute zum Jahres@@ wechsel und hoffe , daß Sie schöne Ferien hatten .
Wie Sie feststellen konnten , ist der ge@@ für@@ ch@@ tete &quot; Mill@@ en@@ i@@ um-@@ Bu@@ g &quot; nicht eingetreten . Doch sind Bürger einiger unserer Mitgliedstaaten Opfer von schrecklichen Naturkatastrophen geworden .
Im Parlament besteht der Wunsch nach einer Aussprache im Verlauf dieser Sitzungsperiode in den nächsten Tagen .
Heute möchte ich Sie bitten - das ist auch der Wunsch einiger Kolleginnen und Kollegen - , allen Opfern der St@@ ür@@ me , insbesondere in den verschiedenen Ländern der Europäischen Union , in einer Schwei@@ ge@@ minute zu ge@@ denken .
```

### Training and test data separation
Training uses WMT16 English-German dataset, validation is on concatenation of
newstest2015 and newstest2016, BLEU evaluation is done on newstest2014.


### Data filtering
Training is executed only on pairs of sentences which satisfy the following equation:
```
    min_len <= src sentence sequence length <= max_len AND
    min_len <= tgt sentence sequence length <= max_len
```
`min_len` is set to 0, `max_len` is set to 75. Source and target sequence
lengths include special BOS (begin-of-sentence) and EOS (end-of-sentence)
tokens.

Filtering is implemented in `pytorch/seq2seq/data/dataset.py`, class
`LazyParallelDataset`.

### Training data order
Training script does bucketing by sequence length. Bucketing algorithm uses 5
equal-width buckets (`num_buckets = 5`). Pairs of training sentences are
assigned to buckets by the value of
`max(src_sentence_len // bucket_width, tgt_sentence_len // bucket_width)`, where
`bucket_width = (max_len + num_buckets - 1) // num_buckets`.
Before each training epoch batches are randomly sampled from the buckets (last
incomplete batches are dropped for each bucket), then all batches are
reshuffled.


Bucketing is implemented in `pytorch/seq2seq/data/sampler.py`, class
`BucketingSampler`.

# 4. Model
### Publication/Attribution

Implemented model is similar to the one from [Google's Neural Machine
Translation System: Bridging the Gap between Human and Machine
Translation](https://arxiv.org/abs/1609.08144) paper.

Most important difference is in the attention mechanism. This repository
implements `gnmt_v2` attention: output from first LSTM layer of decoder goes
into attention, then re-weighted context is concatenated with inputs to all
subsequent LSTM layers in decoder at current timestep.

The same attention mechanism is also implemented in default
GNMT-like models from [tensorflow/nmt](https://github.com/tensorflow/nmt) and
[NVIDIA/OpenSeq2Seq](https://github.com/NVIDIA/OpenSeq2Seq).

### Structure
* general:
  * encoder and decoder are using shared embeddings
  * data-parallel multi-gpu training
  * trained with label smoothing loss (smoothing factor 0.1)
* encoder:
  * 4-layer LSTM, hidden size 1024, first layer is bidirectional, the rest of
    layers are unidirectional
  * with residual connections starting from 3rd LSTM layer
  * uses standard pytorch nn.LSTM layer
  * dropout is applied on input to all LSTM layers, probability of dropout is
    set to 0.2
  * hidden state of LSTM layers is initialized with zeros
  * weights and bias of LSTM layers is initialized with uniform(-0.1, 0.1)
    distribution
* decoder:
  * 4-layer unidirectional LSTM with hidden size 1024 and fully-connected
    classifier
  * with residual connections starting from 3rd LSTM layer
  * uses standard pytorch nn.LSTM layer
  * dropout is applied on input to all LSTM layers, probability of dropout is
    set to 0.2
  * hidden state of LSTM layers is initialized with zeros
  * weights and bias of LSTM layers is initialized with uniform(-0.1, 0.1)
    distribution
  * weights and bias of fully-connected classifier is initialized with
    uniform(-0.1, 0.1) distribution
* attention:
  * normalized Bahdanau attention
  * model uses `gnmt_v2` attention mechanism
  * output from first LSTM layer of decoder goes into attention,
  then re-weighted context is concatenated with the input to all subsequent
  LSTM layers in decoder at the current timestep
  * linear transform of keys and queries is initialized with uniform(-0.1, 0.1),
  normalization scalar is initialized with 1.0 / sqrt(1024),
    normalization bias is initialized with zero
* inference:
  * beam search with beam size of 5
  * with coverage penalty and length normalization, coverage penalty factor is
    set to 0.1, length normalization factor is set to 0.6 and length
    normalization constant is set to 5.0
  * BLEU computed by [sacrebleu](https://pypi.org/project/sacrebleu/)


Implementation:
* base Seq2Seq model: `pytorch/seq2seq/models/seq2seq_base.py`, class `Seq2Seq`
* GNMT model: `pytorch/seq2seq/models/gnmt.py`, class `GNMT`
* encoder: `pytorch/seq2seq/models/encoder.py`, class `ResidualRecurrentEncoder`
* decoder: `pytorch/seq2seq/models/decoder.py`, class `ResidualRecurrentDecoder`
* attention: `pytorch/seq2seq/models/attention.py`, class `BahdanauAttention`
* inference (including BLEU evaluation and detokenization): `pytorch/seq2seq/inference/inference.py`, class `Translator`
* beam search: `pytorch/seq2seq/inference/beam_search.py`, class `SequenceGenerator`

### Loss function
Cross entropy loss with label smoothing (smoothing factor = 0.1), padding is not
considered part of the loss.

Loss function is implemented in `pytorch/seq2seq/train/smoothing.py`, class
`LabelSmoothing`.

### Optimizer
Adam optimizer with learning rate 1e-3, beta1 = 0.9, beta2 = 0.999, epsilon =
1e-8 and no weight decay.
Network is trained with gradient clipping, max L2 norm of gradients is set to 5.0.

Optimizer is implemented in `pytorch/seq2seq/train/fp_optimizers.py`, class
`Fp32Optimizer`.

### Learning rate schedule
Model is trained with exponential learning rate warmup for 200 steps and with
step learning rate decay. Decay is started after 2/3 of training steps, decays
for a total of 4 times, at regularly spaced intervals, decay factor is 0.5.

Learning rate scheduler is implemented in
`pytorch/seq2seq/train/lr_scheduler.py`, class `WarmupMultiStepLR`.

# 5. Quality

### Quality metric
Uncased BLEU score on newstest2014 en-de dataset.
BLEU scores reported by [sacrebleu](https://pypi.org/project/sacrebleu/)
package (version 1.2.10). Sacrebleu is executed with the following flags:
`--score-only -lc --tokenize intl`.

### Quality target
Uncased BLEU score of 24.00.

### Evaluation frequency
Evaluation of BLEU score is done after every epoch.

### Evaluation thoroughness
Evaluation uses all of `newstest2014.en` (3003 sentences).
