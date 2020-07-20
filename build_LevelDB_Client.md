# Build LevelDB Client
LevelDB doesn't have any client server, so we need to use [simplehttp-simpleleveldb](https://github.com/liaojack8/simplehttp/tree/master/simpleleveldb) to build restful api to operate LevelDB.

The repository used below was forked by myself, and I was did something modify to let it work normolly.

If you want to try simpler client server tools, this also is my repository, I complete this client with Python.
To build it, just two line of command, go [my repository](https://github.com/liaojack8/LevelDB-httpServer) to know details. (But it worked slowly than simpleleveldb)
## Build Client
```
sudo apt install libjson-c-dev libsnappy-dev
wget https://github.com/downloads/libevent/libevent/libevent-1.4.14b-stable.tar.gz
tar vxf libevent-1.4.14b-stable.tar.gz && cd libevent-1.4.14b-stable
./configure && make
sudo ln -s /usr/local/lib/libevent-1.4.so.2 /usr/lib/libevent-1.4.so.2
git clone https://github.com/liaojack8/simplehttp.git && cd simplehttp/simplehttp
make
sudo make install
cd ../impleleveldb
env LIBLEVELDB=/usr/local make
sudo make install
```
## Run Client & Test
```
simpleleveldb --address=localhost --port=8888 --db-file=test
```
### open new terminal tab

insert (A,Airplane)
```
curl 'http://localhost:8888/put?key=A&value=Airplane'
```
```
Output:
        { "status_txt": "OK", "status_code": 200, "data": "" }
```
query key:A
```
curl 'http://localhost:8888/get?key=A'
```
```
Output:
        { "data": "Airplane", "status_txt": "OK", "status_code": 200 }
```
delete key:A
```
curl 'http://localhost:8888/del?key=A'
```
```
Output:
        { "status_txt": "OK", "status_code": 200, "data": "" }
```
query key:A
```
curl 'http://localhost:8888/get?key=A'
```
```
Output:
        { "status_txt": "NOT_FOUND", "status_code": 404, "data": "" }
```
