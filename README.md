# Machine Learning Benchmark Scripts

Develop branch build status:

[![Build Status](http://ci.mlpack.org/job/benchmark%20-%20checkout%20-%20all%20nodes/badge/icon)](http://ci.mlpack.org/job/benchmark%20-%20checkout%20-%20all%20nodes/)

Visit http://www.mlpack.org/benchmark.html to see our latest results.

This repository contains a collection of benchmark scripts for various machine learning libraries. The scripts serves as an infrastructure for measuring and comparing the performance, of different algorithms and libraries on various datasets using visual tools and different metrics. It aims to give the machine learning community a streamlined tool to get information on those changesets that may have caused speedups or slowdowns.

The system has several key attributes that lead to its highly and easily customizable nature. It makes extensive use of the Python standard library and the YAML file format to provide a very easy way to efficiently run the performance measurements on custom setups with different operating systems. The tools and metrics used for the visualization are highly flexible, e.g. with one line you can measure the size of your program’s stack, get the variance or standard deviation of the measurements. The architecture is easily maintainable since each part is a single module. with the results that the framework can be easily integrated in the main workflow.


Quick links to this file:
* [Competing libraries](#competing-libraries)
* [Prerequisites](#prerequisites)
* [Running](#running)
* [Directory Structure](#directory-structure)
* [Getting the datasets](#getting-the-datasets)
* [Configuration](#configuration)
* [Citation details](#citation-details)

## Competing libraries

Machine learning toolkits:
* [mlpack](http://mlpack.org)
* [Shogun-toolbox](http://shogun-toolbox.org)
* [scikit-learn](http://scikit-learn.org)
* [MATLAB](http://mathworks.com)
* [Weka](http://cs.waikato.ac.nz/ml/weka/)
* [elki](https://elki-project.github.io/)
* [mlpy](http://mlpy.sourceforge.net)
* [dlibml](http://dlib.net/ml.html)
* [milk](https://github.com/luispedro/milk/)
* [R](https://www.r-project.org/)

Nearest Neighbour Algorithms:
* [ANN](http://www.cs.umd.edu/~mount/ANN/)
* [FLANN](http://www.cs.ubc.ca/research/flann/)
* [nearpy](http://pixelogik.github.io/NearPy/)
* [annoy](https://github.com/spotify/annoy)
* [mrpt](https://github.com/vioshyvo/mrpt)

Inactive toolkits:
* [HLearn](https://github.com/mikeizbicki/HLearn)
NOTE: `HLearn` is not currently being benchmarked by this repository.

## Prerequisites

* **[Python 3.3+](http://www.python.org "Python Website")**: The main benchmark script is written with the programming language python: The benchmark script by default uses the version of Python on your path.
* **[numpy](https://www.numpy.org/)**: Numpy provides a powerful N-dimensional array object and sophisticated (broadcasting) functions useful for handling and transforming data.
* **[Python-yaml](http://pyyaml.org "Python-yaml Website")**: PyYAML is a YAML parser and emitter for Python. We've picked YAML as the configuration file format for specifying the structure for the project.
* **[SQLite](http://www.sqlite.org "SQLite Website")** (**Optional**): SQLite is a lightweight disk-based database that doesn't require a separate server process. We use the python built-in SQLite database to save the benchmark results.
* **[python-xmlrunner](https://github.com/lamby/pkg-python-xmlrunner "python-xmlrunner github")** (**Optional**): The xmlrunner module is a unittest test runner that can save test results to XML files. This package is only needed if you want to run the tests.

### Prerequisites for Setting up Competing Libraries
All the following pre-requisite packages are needed to be installed before running `make setup` command (see the next [section](#running)):  
**FLANN library:**  
* [hdf5](https://www.hdfgroup.org/solutions/hdf5/): This is a high performance data software library.
* [gtest](https://github.com/google/googletest): This package used for writing C++ tests.

**mlpack:**  
* [Armadillo](http://arma.sourceforge.net/download.html): This package is a c++ library for linear algebra and scientific computing.
* [Boost C++](https://www.boost.org/): This package is required for compiling mlpack from source.

**mlpy:**  
* [scipy](https://www.scipy.org/): Python-based ecosystem of open-source software for mathematics, science, and engineering
* [GSL](https://www.gnu.org/software/gsl/): The is a numerical library for C and C++ programmers.

**scikit-learn:**  
* [scipy](https://www.scipy.org/): Python-based ecosystem of open-source software for mathematics, science, and engineering
* [joblib](https://joblib.readthedocs.io/): Joblib is a set of tools to provide lightweight pipelining in Python.
* [Cython-0.25.2](https://cython.org/): C-extentions for python. Required for compiling the scikit from source.

**Nearpy:**  
* [scipy](https://www.scipy.org/): Python-based ecosystem of open-source software for mathematics, science, and engineering
* [redis](https://redislabs.com/lp/python-redis/): The Python interface to the Redis key-value store.

**shogun:**  
* [swig](https://github.com/swig/swig): SWIG is a compiler that integrates C and C++ with languages including Perl, Python, Tcl, Ruby, PHP, Java, C#, D, Go, Lua, Octave, R, Scheme (Guile, MzScheme/Racket), Scilab, Ocaml. SWIG can also export its parse tree into XM

**weka:**  
* [java](https://www.java.com/en/): Java is a programming language on which weka is based.

**elki:**  
* [java](https://www.java.com/en/): Java is a programming language on which weka is based.

**milk**
* [Eigen3](http://eigen.tuxfamily.org/index.php?title=Main_Page): Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.

**R**
* [gfortran](https://gcc.gnu.org/wiki/GFortran): A free Fortran 95/2003/2008 compiler for GCC
* [readline](https://www.gnu.org/software/readline/): This library provides a set of functions for use by applications that allow users to edit command lines as they are typed in.
* [libbz2-dev](https://www.sourceware.org/bzip2/): This is a freely available, patent free, high-quality data compressor. Header files of this software are required.
* [liblzma-dev](https://tukaani.org/xz/): XZ Utils is free general-purpose data compression software with a high compression ratio. Header files of this software are required.
* [libcurl4](https://curl.haxx.se/libcurl/): libcurl is a free and easy-to-use client-side URL transfer library


## Running

Benchmarks are run with the `make` command.

* `make run`        -- Perform the benchmark.
* `make memory`     -- Get memory profiling information.
* `make test`       -- Test the configuration file. Check for correct syntax and then try to open files referred in the configuration file.
* `make setup`      -- Download and set up all of the libraries to compare against.
* `make datasets`   -- Download datasets into the datasets/ folder.
* `make scripts`    -- Make additional scripts.


Running `make` with no additional arguments except the task option will use the default parameters specified in the `Makefile` (e.g. config file). You can set an alternate config file with the `CONFIG` flag. You can also run a single benchmark script with the `BLOCK` and `METHODBLOCK` flag. Use `make help` to see a full list of options.


## Running the scripts

#### Benchmarking and save the Output
By default running the benchmarks will produce some logging to standard out. To save the results in the databse, set the `LOG` flag. If you wanted to run all scripts and save the output in the database located in the reports directory use the following command line:

    $ make run LOG=True

#### Benchmarking a Single Method

If you are making changes to any of the scripts, or if you simply want to benchmark a single method, you can benchmark the method with the `METHODBLOCK` flag. For example, if you only wanted to benchmark all K-Means scripts use the following command line:

    $ make run METHODBLOCK=KMEANS

You can also run a list of methods with the following command line:

    $ make run METHODBLOCK=KMEANS,ALLKNN

#### Benchmarking a Single Library

If you are making changes to any of the scripts for a specified library, or if you simply want to benchmark a single library, you can benchmark the library with the `BLOCK` flag. For example, if you only wanted to benchmark all MLPACK scripts use the following command line:

    $ make run BLOCK=mlpack

You can also benchmark a list of libraries with the following command line:

    $ make run BLOCK=mlpack,shogun

#### Benchmarking a Single Libary and a Single Method

You can also combine the `BLOCK` and `METHODBLOCK` flag to benchmark single methods for a specific libraries. For example, if you only wanted to benchmark the MLPACK and Shogun, K-Means scripts use the following command line:

    $ make run BLOCK=mlpack,shogun METHODBLOCK=KMEANS

#### Update Benchmark Results

In case of an failure you can update the last benchmark results stored in the database. You can combine the other flag to specifie the libary or method you like to update. For example, if you only wanted to update the MLPACK, HMM script use the following command line:

    $ make run UPDATE=True BLOCK=mlpack METHODBLOCK=HMM

## Directory Structure

Source directories

    ./                      -- config file and the Makefile to start the benchmarks
    ./datasets              -- several datasets which are used for the benchmarks
    ./util                  -- common files used by several scripts
    ./tests                 -- source code for tests
    ./benchmark             -- executables for the different benchmarks tasks
    ./methods/<library>     -- source code for scripts

Working directories

    ./
    ./reports               -- output from the memory_benchmark executable
    ./reports/benchmark.db  -- database for benchmark runs

## Getting the datasets

You can get the datasets by running:

    $ make datasets

from within your working directory. This will download the datasets listed in ``datasets/dataset-urls.txt``.

## Configuration
The benchmark script requires several parameters that specify the benchmark runs, the parameters of the graph to be generated, etc.

For complete details, refer the wiki page : https://github.com/zoq/benchmarks/wiki/Google-Summer-of-Code-2014-:-Improvement-of-Automatic-Benchmarking-System

The benchmark script comes with a default configuration. The default configuration will run all available benchmarks. This configuration can take quite a while to run (more than two weeks), so it would be best to adjust the configuration to to suite your time constraints. You can also command line options to selectively run benchmarks the options are described below.


### General Block

The general block contains some settings that control the benchmark itself, and the output of the reports page.

```yaml
library: general
settings:
    timeout: 9000
    database: 'reports/benchmark.db'
    keepReports: 20
    topChartColor: '#F3F3F3'
    chartColor: '#FFFFFF'
    textColor: '#6E6E6E'
```
* `timeout`: Limit the execution time for the benchmarks. This can be an easy way to keep a benchmark from eating up all the execution time.
* `database`: The location of the databse. If there is no database at the specified location, the script creates a new database.
* `keepReports`: Limit the report pages. This can be an easy way to keep a benchmark from eating up all your space.
* `topChartColor`: The background color of the top chart.
* `chartColor`: The background color of the charts.
* `textColor`: The font color of the charts.


### Library Block

The library block contains some settings that control the specified benchmark scripts.

```yaml
library: mlpack
methods:
    PCA:
        run: ['timing', 'metric', 'bootstrap']
        script: methods/mlpack/pca.py
        format: [csv, txt]
        datasets:
            - files: [['datasets/iris_train.csv', 'datasets/iris_test.csv', 'datasets/iris_labels.csv']]

            - files: [['datasets/wine_train.csv', 'datasets/wine_test.csv', 'datasets/wine_labels.csv']]
              options: '-d 2'
    NMF:
        run: []
        script: methods/mlpack/nmf.py
        format: [csv, txt]
        datasets:
            - files: [['datasets/iris_train.csv', 'datasets/iris_test.csv', 'datasets/iris_labels.csv']]
              options: '-r 6 -s 42 -u multdist'
```

| **library**  |  |
| :------------ | :----------- |
| Description     | A name to identify the library. The name is also used for the output, for this reason it should be avoided to choose a name with more than 23 characters. |
| Syntax | `library: name` |
| Required | Yes |
| **script** | |
| Description | Path to the current method which should be tested. You can use the relative path from the benchmark root folder, a absolute path or a symlink. |
| Syntax | `script: name` |
| Required | Yes |
| **files** | |
| Description | List of datasets for this method. You can use the relative path from the benchmark root folder, a absolute path or a symlink. Requires a method more than one data set, you should add the data sets in an extra list. |
| Syntax | `files: [...] or [ [...] ]` |
| Required | Yes |
| **run** | |
| Description | List of benchmark tasks for this method. |
| Syntax | `run: ['timing', 'metrics']` |
| Default | `[]` |
| Required | No |
| **iterations** | |
| Description | The number of executions for this method. It is recommended to set the value higher than one in order to obtain meaningful results. |
| Syntax | `iterations: number` |
| Default | `3` |
| Required | No |
| **format** | |
| Description | A array of supported file formats for this method. If this data set isn't available in this format, the benchmark script tries to convert the data set. |
| Syntax | `format: [...]` |
| Required | No |
| **options** | |
| Description | description  The string contains options for this method. The string is passed when the script is started. |
| Syntax | `options: String` |
| Default   | `None` |
| Required | No |

#### Minimal Configuration

The configuration described here is the smallest possible configuration. The configuration combines all required options to benchmark a method.

```yaml
# MLPACK:
# A Scalable C++  Machine Learning Library
library: mlpack
methods:
    PCA:
        script: methods/mlpack/pca.py
        format: [csv, txt, hdf5, bin]
        datasets:
            - files: ['isolet.csv']
```

In this case we benchmark the pca method located in methods/mlpack/pca.py and use the isolet dataset. The pca method supports the following formats txt, csv, hdf5 and bin. The benchmark script use the default values for the non-specified values.

#### Full Configuration

Combining all the elements discussed above results in the following configuration, which should be placed typically in the config.yaml.

```yaml
# MLPACK:
# A Scalable C++  Machine Learning Library
library: mlpack
methods:
    PCA:
        script: methods/mlpack/pca.py
        format: [csv, txt, hdf5, bin]
        run: ['timing', 'metric', 'bootstrap']
        iterations: 2
        datasets:
            - files: [['datasets/iris_train.csv', 'datasets/iris_test.csv', 'datasets/iris_labels.csv']]
              options: '-s'
```

In this case we benchmark the pca method located in methods/mlpack/pca.py with the isolet and the cities dataset. The pca method scales the data before running the pca method. The benchmark performs twice for each dataset. Additionally the pca.py script supports the following file formats txt, csv, hdf5 and bin. If the data isn't available in this particular case the format will be generated.

## Citation details

If you use the benchmarks in your work, we'd really appreciate it if you could cite the following paper (given in BiBTeX format):

    @inproceedings{edel2014automatic,
      title={An automatic benchmarking system},
      author={Edel, Marcus and Soni, Anand and Curtin, Ryan R},
      booktitle={NIPS 2014 Workshop on Software Engineering for Machine Learning (SE4ML’2014)},
      volume={1},
      year={2014}
    }
