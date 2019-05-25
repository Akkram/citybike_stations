**Brisbane-City-Bike**

Performing a clustering based on the location of bike stations in Brisbane (Python3)

First five lines of Brisbane_CityBike.json file:

+--------------------------------------------+-----+----------+----------+-------------------------------------------------+--------+
|address                                     |id   |latitude  |longitude |name                                             |position|
+--------------------------------------------+-----+----------+----------+-------------------------------------------------+--------+
|Lower River Tce / Ellis St                  |122.0|-27.482279|153.028723|122 - LOWER RIVER TCE / ELLIS ST                 |null    |
|Main St / Darragh St                        |91.0 |-27.47059 |153.036046|91 - MAIN ST / DARRAGH ST                        |null    |
|Browne St / James St                        |75.0 |-27.461881|153.046986|75 - BROWNE ST / JAMES ST                        |null    |
|Kurilpa Point / Montague Rd                 |99.0 |-27.469658|153.016696|98 - KURILPA POINT / MONTAGUE RD                 |null    |
|Montague Rd / Skinner St                    |109.0|-27.48172 |153.00436 |109 - MONTAGUE RD / SKINNER ST                   |null    |
|Macquarie St / Guyatt Park                  |149.0|-27.493626|153.001482|null                                             |null    |
|Bi-centennial Bike Way / Lang Pde           |139.0|-27.476076|153.002459|139 - BI-CENTENNIAL BIKE WAY / LANG PDE          |null    |
|Sir William McGregor Dr / Sir Fred Schonnell|24.0 |-27.493963|153.011938|24 - SIR WILLIAM MCGREGOR DR / SIR FRED SCHONNELL|null    |
|Vulture St / Tribune St                     |117.0|-27.482197|153.020894|117 - VULTURE ST / TRIBUNE ST                    |null    |
|Lamington St / Refinery Pde                 |73.0 |-27.465226|153.050864|73 - LAMINGTON ST / REFINERY PDE                 |null    |



The algorithm used to do the clustering was K-means, wich is unsupervised learning algorithm. The data is grouped according to feature similarity, in this case this similarity is represented by the Euclidian distance between the stations.

The K-means algorithm has a major drawback: it's necessary to provide the required number of clusters.

**Displaying a sample of data with labels**

+--------------------------------------------+-----+----------+----------+-------------------------------------------------+--------+-------+
|address                                     |id   |latitude  |longitude |name                                             |position|cluster|
+--------------------------------------------+-----+----------+----------+-------------------------------------------------+--------+-------+
|Lower River Tce / Ellis St                  |122.0|-27.482279|153.028723|122 - LOWER RIVER TCE / ELLIS ST                 |null    |4      |
|Main St / Darragh St                        |91.0 |-27.47059 |153.036046|91 - MAIN ST / DARRAGH ST                        |null    |0      |
|Browne St / James St                        |75.0 |-27.461881|153.046986|75 - BROWNE ST / JAMES ST                        |null    |0      |
|Kurilpa Point / Montague Rd                 |99.0 |-27.469658|153.016696|98 - KURILPA POINT / MONTAGUE RD                 |null    |4      |
|Montague Rd / Skinner St                    |109.0|-27.48172 |153.00436 |109 - MONTAGUE RD / SKINNER ST                   |null    |4      |
|Macquarie St / Guyatt Park                  |149.0|-27.493626|153.001482|null                                             |null    |4      |
|Bi-centennial Bike Way / Lang Pde           |139.0|-27.476076|153.002459|139 - BI-CENTENNIAL BIKE WAY / LANG PDE          |null    |4      |
|Sir William McGregor Dr / Sir Fred Schonnell|24.0 |-27.493963|153.011938|24 - SIR WILLIAM MCGREGOR DR / SIR FRED SCHONNELL|null    |4      |
|Vulture St / Tribune St                     |117.0|-27.482197|153.020894|117 - VULTURE ST / TRIBUNE ST                    |null    |4      |
|Lamington St / Refinery Pde                 |73.0 |-27.465226|153.050864|73 - LAMINGTON ST / REFINERY PDE                 |null    |0      |

**Running without installing Spark**

The project could be run locally without installing Spark. This will use an "emulated" Spark wish is included in the dependencies of the project.

To do this, you should change the content of the file build.sbt like following :

name := "CityBike_stations"

version := "0.1"

scalaVersion := "2.11.8"

val sparkVersion = "2.3.3"

resolvers ++= Seq(
  "apache-snapshots" at "http://repository.apache.org/snapshots/"
)

libraryDependencies++=Seq(

  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion,
  "org.apache.spark" %% "spark-mllib" % sparkVersion,
  "com.typesafe.scala-logging" %% "scala-logging" % "3.1.0",
  "com.github.scopt" %% "scopt" % "3.5.0"

)

What we did here, is removing the "provided" option. This will force sbt to include Spark dependencies when compiling the project.

You can then compile files :

sbt clean compile

Then run the job :

sbt run


**Running on Spark**

To build the project, run :

sbt clean assembly
This will produce citybike_Station/target/scala-2.11/citybike_stations-assembly-0.1.jar which contains the compiled project.

Then you can submit the job using spark-submit :

./bin/spark-submit --class "com.sedexo.StationsClustering" \                                                                                                                        
          ../citybike_stations/target/scala-2.11/citybike_stations-assembly-0.1.jar \
          ClusterNb \
          INPUT_DIR \
          OUTPUT_DIR 


**Backlog: The User Stories & Tasks (done or to do) related to industrializing this code**

Importing data - done.
Building the Pipeline for clustering - done.
Saving the dataset with labels - done.



