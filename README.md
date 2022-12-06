# DLIO has been migrated to [https://github.com/argonne-lcf/dlio_benchmark](https://github.com/argonne-lcf/dlio_benchmark). Please clone repo from there.

# DLIO Benchmark
This is repository for a I/O benchmark which represents Scientific Deep Learning Workloads. DLIO benchmark is aimed at emulating the behavior of scientific deep learning applications, as described in the previous section. The benchmark is delivered as an executable that can be configured for various I/O patterns. It uses a modular design to incorporate more data formats, datasets, and configuration parameters. It emulates modern scientific deep learning applications using Benchmark Runner, Data Generator, Format Handler, and I/O Profiler modules. These modules utilize state-of-the-art design patterns to build a transparent and extensible framework. The DLIO benchmark has been designed with the following goals in mind.

## DLIO features include:

- Easy-to-use and highly configurable argument list to emulate any DL application's I/O behavior.
- Fast prototyping through highly modular components to enhance the benchmark with more data formats.
- Full transparency over emulation of I/O access with logging at different levels.
- Easy to use data generator to test the performance of different data layouts and its impact on the I/O performance.
- Compatible with modern profiling tools such as Tensorboard and Darshan to extract and analyze I/O behavior.

## Usage
```bash
# Get the options available using
python ./dlio_benchmark.py -h

# Example option list
DATA_DIR=~/dlio_datasets/temp
OPTS=(-f tfrecord -fa multi -nf 1024 -sf 1024 -df ${DATA_DIR} -rl 262144 -gd 1 -k 1)
python ./dlio_benchmark.py ${OPTS[@]}

# To only generate data
DATA_DIR=~/dlio_datasets/temp
OPTS=(-f tfrecord -fa multi -nf 1024 -sf 1024 -df ${DATA_DIR} -rl 262144 -gd 1 -go 1 -k 1)
python ./dlio_benchmark.py ${OPTS[@]}

# To run on already generated data
DATA_DIR=~/dlio_datasets/temp
OPTS=(-f tfrecord -fa multi -nf 1024 -sf 1024 -df ${DATA_DIR} -rl 262144 -gd 0 -k 1)
python ./dlio_benchmark.py ${OPTS[@]}
```

## Installation

### Requirements
- horovod[tensorflow]>=0.19.5
- tensorflow>=2.2.0
- numpy>=1.19.1
- h5py~=2.10.0
- pandas>=1.1.3
- mpi4py>=3.1.3

### Installations Instructions
To install VaniDL, the easiest way is to run

For the bleeding edge version (recommended):
```bash
pip install git+https://github.com/hariharan-devarajan/dlio_benchmark
```

For the latest stable version:
```bash
pip install dlio_benchmark
```

Otherwise, you can also install from source by running (from source folder):
```bash
python setup.py install
# this install dlio_benchmark as an executable.
dlio_benchmark -h
```
On Theta
```bash
module load DLIO
```

Locally

```bash
git clone https://github.com/hariharan-devarajan/dlio_benchmark
cd dlio_benchmark/
python3 -m venv ./venv
source venv/bin/activate
pip install -r requirements.txt 
export PYTHONPATH=$PWD/src:$PYTHONPATH
python ./src/dlio_benchmark.py -h
```

### Command line options for DLIO

```
$ $python ./src/dlio_benchmark.py -h
usage: dlio_benchmark.py [-h] [-f {tfrecord,hdf5,csv,npz,hdf5_opt}] [-r {off,seed,random}] [-ms SHUFFLE_SIZE] [-m {off,seed,random}] [-rt {memory,on_demand}] [-fa {multi,shared,collective}] [-rl RECORD_LENGTH] [-nf NUM_FILES] [-sf NUM_SAMPLES] [-bs BATCH_SIZE] [-e EPOCHS]
                         [-se SEED_CHANGE_EPOCH] [-gd GENERATE_DATA] [-df DATA_FOLDER] [-of OUTPUT_FOLDER] [-fp FILE_PREFIX] [-go GENERATE_ONLY] [-k KEEP_FILES] [-p PROFILING] [-l LOGDIR] [-s SEED] [-c CHECKPOINT] [-sc STEPS_CHECKPOINT] [-ts TRANSFER_SIZE]
                         [-tr READ_THREADS] [-tc COMPUTATION_THREADS] [-ct COMPUTATION_TIME] [-rp PREFETCH] [-ps PREFETCH_SIZE] [-ec ENABLE_CHUNKING] [-cs CHUNK_SIZE] [-co {none,gzip,lzf,bz2,zip,xz}] [-cl COMPRESSION_LEVEL] [-d DEBUG]

DLIO Benchmark

optional arguments:
  -h, --help            show this help message and exit
  -f {tfrecord,hdf5,csv,npz,hdf5_opt}, --format {tfrecord,hdf5,csv,npz,hdf5_opt} data reader to use.
  -r {off,seed,random}, --read-shuffle {off,seed,random} Enable shuffle during read.
  -ms SHUFFLE_SIZE, --shuffle-size SHUFFLE_SIZE Size of a shuffle in bytes.
  -m {off,seed,random}, --memory-shuffle {off,seed,random} Enable memory during pre-processing.
  -rt {memory,on_demand}, --read-type {memory,on_demand} The read behavior for the benchmark.
  -fa {multi,shared,collective}, --file-access {multi,shared,collective} How the files are accessed in the benchmark.
  -rl RECORD_LENGTH, --record-length RECORD_LENGTH Size of a record/image within dataset
  -nf NUM_FILES, --num-files NUM_FILES Number of files that should be accessed.
  -sf NUM_SAMPLES, --num-samples NUM_SAMPLES  Number of samples per file.
  -bs BATCH_SIZE, --batch-size BATCH_SIZE Batch size for training records.
  -e EPOCHS, --epochs EPOCHS Number of epochs to be emulated within benchmark.
  -se SEED_CHANGE_EPOCH, --seed-change-epoch SEED_CHANGE_EPOCH change seed between epochs. y/n
  -gd GENERATE_DATA, --generate-data GENERATE_DATA Enable generation of data. y/n
  -df DATA_FOLDER, --data-folder DATA_FOLDER  Set the path of folder where data is present in top-level.
  -of OUTPUT_FOLDER, --output-folder OUTPUT_FOLDER  Set the path of folder where output can be generated.
  -fp FILE_PREFIX, --file-prefix FILE_PREFIX  Prefix for generated files.
  -go GENERATE_ONLY, --generate-only GENERATE_ONLY  Only generate files.
  -k KEEP_FILES, --keep-files KEEP_FILES Keep files after benchmark. y/n
  -p PROFILING, --profiling PROFILING  Enable I/O profiling within benchmark. y/n
  -l LOGDIR, --logdir LOGDIR Log Directory for profiling logs.
  -s SEED, --seed SEED  The seed to be used shuffling during read/memory.
  -c CHECKPOINT, --checkpoint CHECKPOINT Enable checkpoint within benchmark. y/n
  -sc STEPS_CHECKPOINT, --steps-checkpoint STEPS_CHECKPOINT How many steps to enable checkpoint.
  -ts TRANSFER_SIZE, --transfer-size TRANSFER_SIZE Transfer Size for tensorflow buffer size.
  -tr READ_THREADS, --read-threads READ_THREADS Number of threads to be used for reads.
  -tc COMPUTATION_THREADS, --computation-threads COMPUTATION_THREADS  Number of threads to be used for pre-processing.
  -ct COMPUTATION_TIME, --computation-time COMPUTATION_TIME  Amount of time for computation.
  -rp PREFETCH, --prefetch PREFETCH Enable prefetch within benchmark.
  -ps PREFETCH_SIZE, --prefetch-size PREFETCH_SIZE Enable prefetch buffer within benchmark.
  -ec ENABLE_CHUNKING, --enable-chunking ENABLE_CHUNKING  Enable chunking for HDF5 files.
  -cs CHUNK_SIZE, --chunk-size CHUNK_SIZE  Set chunk size in bytes for HDF5.
  -co {none,gzip,lzf,bz2,zip,xz}, --compression {none,gzip,lzf,bz2,zip,xz} Compression to use.
  -cl COMPRESSION_LEVEL, --compression-level COMPRESSION_LEVEL  Level of compression for GZip.
  -d DEBUG, --debug DEBUG Enable debug in code.
```

## Application Configurations (I/O)
```bash
# DLIO_ROOT directory of DLIO benchmark
# APP_DATA_DIR directory where application data would be generated
```

### Neutrino and Cosmic Tagging with UNet

```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f hdf5 -fa shared -nf 1 -sf 6000 -rl 40960 -bs 1 -ec 1 -cs 4096 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f hdf5 -fa shared -nf 1 -sf 6000 -rl 40960 -bs 1 -ec 1 -cs 4096 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```

### Distributed Flood Filling Networks (FFN)

```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f hdf5 -fa shared -nf 1 -sf 43008 -rl 32768 -bs 1 -ec 1 -cs 4096 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f hdf5 -fa shared -nf 1 -sf 43008 -rl 32768 -bs 1 -ec 1 -cs 4096 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```

### TensorFlow CNN Benchmarks

```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f tfrecord -fa multi -nf 1024 -sf 1024 -rl 262144 -ts 1048576 -tr 8 -tc 8 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f tfrecord -fa multi -nf 1024 -sf 1024 -rl 262144 -ts 1048576 -tr 8 -tc 8 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```

### CosmoFlow for learning universe at scale

```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f tfrecord -fa multi -nf 1024 -sf 512 -rl 131072 -tc 64 -bs 1 -ts 1048576 -tr 8 -tc 8 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f tfrecord -fa multi -nf 1024 -sf 512 -rl 131072 -tc 64 -bs 1 -ts 1048576 -tr 8 -tc 8 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```

### Fusion Recurrent Neural Net (FRNN) for representation learning in plasma science

```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f npz -fa multi -nf 28000 -sf 1024 -rl 2048 -bs 1 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f npz -fa multi -nf 28000 -sf 1024 -rl 2048 -bs 1 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```

### Cancer Distributed Learning Environment (CANDLE) for cancer research
```bash
# Generate data
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f csv -fa shared -nf 1 -sf 1120 -rl 32768 -bs 1 -df ${APP_DATA_DIR} \
		-gd 1 -go 1 -k 1

# Run application
mpirun -n 1 python ${DLIO_ROOT}/src/dlio_benchmark.py \
		-f csv -fa shared -nf 1 -sf 1120 -rl 32768 -bs 1 -df ${APP_DATA_DIR} \
		-gd 0 -k 1
```


## Contributions
This is the first release of DLIO, if you find any bug, please report it in the GitHub issues section.

Improvements and requests for new features are more than welcome! Do not hesitate to twist and tweak DLIO, and send pull-requests.

## Remaining feature list
- Add argument validations
    - Shared should use one file
    - multiple should have atleast files = nranks
- Add Collective reading
    - create g groups within communicator (configurable when g == # of processes then there is no collective)
    - randomly select 1 process from all groups which will read and then send read data to other processes. in the group
- Add Computations
    - Synchronous: Add computation cycles after reading data (busy waiting).
    - Asynchronous: Add I/O on a different thread to overlap with previous compute.
        - use a queue
            - io thread uses queue to read next element.
            - main thread (compute) puts element to queue and then goes to compute.

## Citation
```
@article{devarajan2021dlio,
  title={DLIO: A Data-Centric Benchmark for Scientific Deep Learning Applications},
  author={H. Devarajan and H. Zheng and A. Kougkas and X.-H. Sun and V. Vishwanath},
  booktitle={IEEE/ACM International Symposium in Cluster, Cloud, and Internet Computing (CCGrid'21)},
  year={2021},
  volume={},
  number={81--91},
  pages={},
  publisher={IEEE/ACM}
}
```
## License
MIT License
