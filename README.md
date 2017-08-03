# SparkRDMA Shuffle Manager Plugin
The SparkRDMA Plugin is a high performance shuffle manager that uses RDMA (instead of TCP) when
performing the shuffle phase of the Spark job.

This open-source project is developed, maintained and supported by [Mellanox Technologies](http://www.mellanox.com).

## Runtime requirements
* Apache Spark 2.0.0 (more versions to be supported)
* Java 8
* libdisni 1.2
* An RDMA-supported network, e.g. RoCE or Infiniband

## Build

* Building the SparkRDMA plugin requires [Apache Maven](http://maven.apache.org/) and Java 8

1. Obtain a clone of [SparkRDMA](https://github.com/Mellanox/SparkRDMA)

2. Build the plugin:
```
mvn -DskipTests package
```

3. Obtain a clone of [DiSNI](https://github.com/zrlio/disni) for building libdisni 1.2:

```
git clone https://github.com/zrlio/disni.git
cd disni
git checkout -b v1.2 247fe8abe54c90b450d2a4b0679e59cfa83205f6
```

4. Compile and install only libdisni (the jars are already included in the SparkRDMA plugin):

```
cd libdisni
sh autoprepare.sh
./configure --with-jdk=/path/to/java8/jdk
make
make install
```
5. libdisni **must** be installed on every Spark Master and Worker

## Configuration

* Provide Spark the location of the SparkRDMA plugin jars by using the extraClassPath option.  For standalone mode this can
be added to either spark-defaults.conf or any runtime configuration file.  For client mode this **must** be added to spark-defaults.conf

```
spark.driver.extraClassPath   /path/to/sparkRdmaPlugin/target/spark-rdma-1.0-jar-with-dependencies.jar
spark.executor.extraClassPath /path/to/sparkRdmaPlugin/target/spark-rdma-1.0-jar-with-dependencies.jar
```

## Running

* To enable and use the SparkRDMA Shuffle Manager plugin, add the following line to either spark-defaults.conf or any runtime configuration file:

```
spark.shuffle.manager   org.apache.spark.shuffle.rdma.RdmaShuffleManager
```

## Contributions

Any PR submissions are welcome
