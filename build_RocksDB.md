# RocksDB Installation

## Install Dependent Libaraies

* compression libaraies
	- [zlib](http://www.zlib.net/) - a library for data compression.
	- [bzip2](http://www.bzip.org/) - a library for data compression.
	- [lz4](https://github.com/lz4/lz4) - a library for extremely fast data compression.
	- [snappy](http://google.github.io/snappy/) - a library for fast
	      data compression.
	- [zstandard](http://www.zstd.net) - Fast real-time compression
	      algorithm.

* tools
	- [gflags](https://gflags.github.io/gflags/) - a library that handles
      command line flags processing. You can compile rocksdb library even
      if you don't have gflags installed.

```bash
$ sudo apt-get install libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev libgflags-dev
```

## Compile
```bash
$ git clone https://github.com/facebook/rocksdb.git && cd rocksdb
$ make static_lib && sudo make install-static
$ make clean
$ make shared_lib && sudo make install-shared
$ sudo cp -r include/* /usr/local/include
```

**Important**: If you plan to run RocksDB in production, don't compile using default make or make all. That will compile RocksDB in debug mode, which is much slower than release mode.

### options
options|explanation
:--|:--
release mode|
`make static_lib` **[recommended]**|will compile librocksdb.a, RocksDB static library. Compiles static library in release mode.
`make shared_lib`|will compile librocksdb.so, RocksDB shared library. Compiles shared library in release mode.
debug mode|
`make check`|will compile and run all the unit tests. `make check` will compile RocksDB in debug mode.
`make all`|will compile our static library, and all our tools and unit tests. Our tools depend on gflags. You will need to have gflags installed to run `make all`. This will compile RocksDB in debug mode. Don't use binaries compiled by `make all` in production.

* By default the binary we produce is optimized for the platform you're compiling on (-march=native or the equivalent). SSE4.2 will thus be enabled automatically if your CPU supports it. To print a warning if your CPU does not support SSE4.2, build with USE_SSE=1 make static_lib or, if using CMake, cmake -DFORCE_SSE42=ON. If you want to build a portable binary, add PORTABLE=1 before your make commands, like this: PORTABLE=1 make static_lib.

## Test Program

### Example Code

```C++
#include <cassert>
#include <iostream>
#include <string>
#include <rocksdb/db.h>
using namespace std;

int main() {
  rocksdb::DB* db;
  rocksdb::Options options;
  options.create_if_missing = true;
  rocksdb::Status status = rocksdb::DB::Open(options, "/tmp/testdb", &db);
  assert(status.ok());

  string key = "A";
  string value = "Airplane";
  string get;
  rocksdb::Status s = db->Put(rocksdb::WriteOptions(), key, value);
  
  if (s.ok()) s = db->Get(rocksdb::ReadOptions(), key, &get);
  if (s.ok()) cout << "Read in RocksDB (k,v) = (" << key << "," << get << ")" << endl;
  else cout << "Read Failed!" << endl;
 
  delete db;
 
  return 0;
}
```

## Compile Test Program And Execute

```bash
$ g++ -o rocksdbtest rocksdbtest.cpp -pthread -lrocksdb -std=c++11 && ./rocksdbtest
```

```bash
Output:
        Read in RocksDB (k,v) = (A,Airplane)
```