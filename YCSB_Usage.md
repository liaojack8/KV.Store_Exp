# YCSB Usage

## Build

    $ mvn -pl site.ycsb:leveldb-binding -am clean package
    
```bash
Output:
		[INFO] ------------------------------------------------------------------------
		[INFO] Reactor Summary for YCSB Root 0.18.0-SNAPSHOT:
		[INFO] 
		[INFO] YCSB Root .......................................... SUCCESS [  0.506 s]
		[INFO] Core YCSB .......................................... SUCCESS [  3.902 s]
		[INFO] LevelDB Binding .................................... SUCCESS [  1.138 s]
		[INFO] ------------------------------------------------------------------------
		[INFO] BUILD SUCCESS
		[INFO] ------------------------------------------------------------------------
		[INFO] Total time:  5.647 s
		[INFO] Finished at: 2020-08-15T20:40:13+08:00
		[INFO] ------------------------------------------------------------------------
```

## Run Benchmark

```bash
#load the data
$ ./bin/ycsb.sh load leveldb -s -P workloads/workloada
#run the workload
$./bin/ycsb.sh run leveldb -s -P workloads/workloada
```