# DKM #
## A generic C++11 k-means clustering implementation ##

[![Build Status](https://travis-ci.org/genbattle/dkm.svg?branch=master)](https://travis-ci.org/genbattle/dkm)

This is a k-means clustering algorithm written in C++, intended to be used as a header-only library. Requires C++11.

The algorithm is based on [Lloyds Algorithm](https://en.wikipedia.org/wiki/Lloyd%27s_algorithm) and uses the [kmeans++](https://en.wikipedia.org/wiki/K-means%2B%2B) initialization method.

The library is located in the `include` directory and may be used under the terms of the MIT license (see LICENSE.md). The tests in the `src/test` directory are also licensed under the MIT license, except for `lest.hpp`, which has its own license (src/test/LICENSE_1_0.txt), the Boost Software License. The benchmarks located within the `bench` directory also fall under the MIT license. Benchmark data was obtained from the UCI Machine Learning Repository [here](https://archive.ics.uci.edu/ml/datasets/Iris).

A basic benchmark can be found in the bench folder. An example of the current results on an Intel i5-4210U:

```
OpenCV: 0.0791842ms
DKM: 0.0421392ms
```

This is only running k-means on a small data set (150 samples), and is only a single measurement, so do not interpret the results to mean that DKM is always faster than OpenCV.

### Usage ###

To use the DKM k-means implementation, simply include `include/dkm.hpp` and call `dkm::kmeans_lloyd()` with your data (`std::vector<std::array<>>`) and the number of cluster centers the algorithm should calculate for the data set. 

Example:

```cpp
std::vector<std::array<float, 2>> data{{1.f, 1.f}, {2.f, 2.f}, {1200.f, 1200.f}, {2.f, 2.f}};
auto cluster_data = dkm::kmeans_lloyd(data, 2);
```

The return value of the `kmeans_lloyd` function is a `std::tuple<std::array<T, N>>, std::vector<uint32_t>>` where the first element of the tuple is the cluster centroids (means) and the second element is a vector of indices that correspond to each of the input data elements. The indices returned in the second element of the tuple are cluster labels that map each corresponding element of the input data to a centroid in the first element of the tuple.

Printing the contents of the tuple for the example gives the following output:

```
Means:
	(1200,1200)
	(1.66667,1.66667)

Cluster labels:
	Point:         (1,1)         (2,2)   (1200,1200)         (2,2)
	Label:             1             1             0             1
```

We can see from the output that the means are at (1200, 1200) and (1.66667, 1.66667). The cluster labels show that the third data point is the only member of the first cluster. The first, second and fourth data points are members of the second cluster. The code used for this example is available in `src/example/main.cpp`.

### Building (tests and benchmarks) ###

For tests and benchmarks DKM uses a standard CMake out-of-tree build model.

To make everything:

```console
mkdir build
cd build
cmake .. && make
```

To build only the tests run `make dkm_tests` instead of `make`. To build only the benchmarks run `make dkm_bench`.

The tests can be run using the `make test` command or executing `./dkm_tests` in the build directory, and the benchmarks can likewise be run with `./dkm_bench`.


### Compatability ###

The following compilers are officially supported on Linux and tested via [Travis](https://travis-ci.org/genbattle/dkm):

- Clang 3.6
- Clang 3.7
- GCC 4.9
- GCC 5.0+

GCC/Clang versions prior to the above are intentionally unsupported due to poor C++11 support. Other compilers may be considered, Microsoft VC++ is intended to be supported, but does not currently have a CI build set up.

### Dependencies (test) ###

- CMake

### Dependencies (bench) ###

- OpenCV 2.4
- CMake
