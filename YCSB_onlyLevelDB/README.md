YCSB
====================================

Building from source
--------------------

To build the full distribution, with all database bindings:

    mvn clean package

To build a single database binding:

    mvn -pl site.ycsb:leveldb-binding -am clean package
