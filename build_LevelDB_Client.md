# Build LevelDB Client
### LevelDB doesn't have any client server, so we need to use simplehttp-simpleleveldb to build restful api to operate LevelDB.
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
* open new terminal tab
```
curl 'http://localhost:8888/put?key=name&value=Niko'
```
```
Output:
        { "status_txt": "OK", "status_code": 200, "data": "" }
```
