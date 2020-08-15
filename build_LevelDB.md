# Build LevelDB
## Clone LevelDB [backup_v1.22](/src/leveldb-google-1.22.zip)
```
git clone --recurse-submodules https://github.com/google/leveldb.git
```
## Install CMake
You should **avoid** using `apt install cmake`, because of the version you install may be lower than the limit by leveldb.

So, let's install released latest on cmake [website](https://cmake.org/), or you can download the source and compiled by yourself.

* Go to [https://cmake.org/files/LatestRelease/](https://cmake.org/files/LatestRelease/), and select the file corresponding to the OS you're using.

```
wget https://cmake.org/files/LatestRelease/cmake-3.18.0-Linux-x86_64.tar.gz
tar zxvf cmake-3.18.0-Linux-x86_64.tar.gz
```

* Check the file you just get.

```
tree -L 2 cmake-3.18.0-Linux-x86_64
```
```
Output:
        cmake-3.18.0-Linux-x86_64
        ├── bin
        │   ├── ccmake
        │   ├── cmake
        │   ├── cmake-gui
        │   ├── cpack
        │   └── ctest
        ├── doc
        │   └── cmake
        ├── man
        │   ├── man1
        │   └── man7
        └── share
            ├── aclocal
            ├── applications
            ├── bash-completion
            ├── cmake-3.18
            ├── emacs
            ├── icons
            ├── mime
            └── vim
```
* Create Symbolic Links
```
sudo mv cmake-3.18.0-Linux-x86_64 /usr/cmake-3.18.0
sudo ln -sf /usr/cmake-3.18.0/bin/*  /usr/bin/
```
* check the version
```
cmake --version
```
```
Output:
        cmake version 3.18.0

        CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
## Compile LevelDB
Just follow the step by [here](https://github.com/google/leveldb#building)
```
mkdir -p build && cd build
cmake -DCMAKE_BUILD_TYPE=Release .. && cmake --build .
```
You'll get static library named `libleveldb.a`, you should install it to system.

```
cd ..
sudo cp build/libleveldb.a /usr/local/lib/
sudo cp -r include/leveldb /usr/local/include/
```
## Test program
### example code:
```
#include <cassert>
#include <iostream>
#include <string>
#include <leveldb/db.h>
using namespace std;

int main() {
  leveldb::DB* db;
  leveldb::Options options;
  options.create_if_missing = true;
  leveldb::Status status = leveldb::DB::Open(options, "/tmp/testdb", &db);
  assert(status.ok());

  string key = "A";
  string value = "Airplane";
  string get;
  leveldb::Status s = db->Put(leveldb::WriteOptions(), key, value);
  
  if (s.ok()) s = db->Get(leveldb::ReadOptions(), key, &get);
  if (s.ok()) cout << "Read in LevelDB (k,v) = (" << key << "," << get << ")" << endl;
  else cout << "Read Failed!" << endl;
 
  delete db;
 
  return 0;
}
```
### Then compile the test program and run it
```
g++ -o leveldbtest leveldbtest.cpp -pthread -lleveldb -std=c++11 && ./leveldbtest
```
```
Output:
        Read in LevelDB (k,v) = (A,Airplane)
```
