# Spatial Index Evaluations for Apache Jena

This repository contains an evaluation setup that compares:

* Jena's default spatial index implementation, referred to as _vanilla_
* Our improved implementation, referred to as  _geoplus_

The evaluation is uses our simple [GridBench](https://github.com/AKSW/gridbench) benchmark.

## Result Datasets

We are in the process of finalizing the RDF evaluation dataset generation and deployment pipeline. The links will be updated to final versions in the coming days. ~ 2024-03-28

Alpha deployments of the datasets are deployed under:

http://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/

### Query Types

In our evaluation we use three sets of queries which target the same spatial regions but differ in the sets of graphs they affect.

* `ng-one`: Benchmark queries target a single named graph in the dataset.

<details>
  <summary>Click here to show the query</summary>

```sparql
PREFIX  geo:  <http://www.opengis.net/ont/geosparql#>
PREFIX  spatial: <http://jena.apache.org/spatial#>
PREFIX  geof: <http://www.opengis.net/def/function/geosparql/>

SELECT  (count(*) AS ?c)
WHERE
  { GRAPH <http://www.example.org/graph/0>
      { BIND("POLYGON((-90 -90, -90 -78.75, -78.75 -78.75, -78.75 -90, -90 -90))"^^geo:wktLiteral AS ?queryGeom)
        ?feature  spatial:intersectBoxGeom  ( ?queryGeom ) ;
                  geo:hasGeometry       ?featureGeom .
        ?featureGeom  geo:asWKT         ?featureGeomWkt
        FILTER geof:sfIntersects(?featureGeomWkt, ?queryGeom)
      }
  }
```

</details>


* `ng-all`: Benchmark queries target all named graphs in the dataset: `GRAPH ?g { }`

<details>
  <summary>Click here to show the query</summary>

```sparql
PREFIX  geo:  <http://www.opengis.net/ont/geosparql#>
PREFIX  spatial: <http://jena.apache.org/spatial#>
PREFIX  geof: <http://www.opengis.net/def/function/geosparql/>

SELECT  (count(*) AS ?c)
WHERE
  { GRAPH ?g
      { BIND("POLYGON((-90 -90, -90 -78.75, -78.75 -78.75, -78.75 -90, -90 -90))"^^geo:wktLiteral AS ?queryGeom)
        ?feature  spatial:intersectBoxGeom  ( ?queryGeom ) ;
                  geo:hasGeometry       ?featureGeom .
        ?featureGeom  geo:asWKT         ?featureGeomWkt
        FILTER geof:sfIntersects(?featureGeomWkt, ?queryGeom)
      }
  }
```

</details>

* `ug`: Benchmark queries target the union default graph, i.e. a view over all named graphs

<details>
  <summary>Click here to show the query</summary>

```sparql
PREFIX  geo:  <http://www.opengis.net/ont/geosparql#>
PREFIX  spatial: <http://jena.apache.org/spatial#>
PREFIX  geof: <http://www.opengis.net/def/function/geosparql/>

SELECT  (count(*) AS ?c)
WHERE
  { { BIND("POLYGON((-90 -90, -90 -78.75, -78.75 -78.75, -78.75 -90, -90 -90))"^^geo:wktLiteral AS ?queryGeom)
      ?feature  spatial:intersectBoxGeom  ( ?queryGeom ) ;
                geo:hasGeometry       ?featureGeom .
      ?featureGeom  geo:asWKT         ?featureGeomWkt
    }
    FILTER geof:sfIntersects(?featureGeomWkt, ?queryGeom)
  }
```

</details>

### Download Links

* [eval-geoplus-ng-one](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-geoplus-ng-one/0.0.1-SNAPSHOT/eval-geoplus-ng-one-0.0.1-20240328.193113-1.trig)
* [eval-geoplus-ng-all](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-geoplus-ng-all/0.0.1-SNAPSHOT/eval-geoplus-ng-all-0.0.1-20240328.193113-1.trig)
* [eval-geoplus-ug](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-geoplus-ug/0.0.1-SNAPSHOT/eval-geoplus-ug-0.0.1-20240328.193113-1.trig)
* [eval-vanilla-ng-one](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-vanilla-ng-one/0.0.1-SNAPSHOT/eval-vanilla-ng-one-0.0.1-20240328.193113-1.trig)
* [eval-vanilla-ng-all](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-vanilla-ng-all/0.0.1-SNAPSHOT/eval-vanilla-ng-all-0.0.1-20240328.193113-1.trig)
* [eval-vanilla-ug](https://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-vanilla-ug/0.0.1-SNAPSHOT/eval-vanilla-ug-0.0.1-20240328.193113-1.trig)

## Visualization

Coming soon. ~ 2024-03-28

## Reproducing Results

In an attempt to make the benchmark as reproducible as possible, we packaged it up as an Apache Maven build.

### Prerequisites

* Apache Maven must be installed
* A running Docker deamon

### Benchmark Execution

The default configuration requires ~48GB of free RAM.

1. Install the eval-template

```bash
cd eval-template
mvn install
```

2. Run the actual evaluation

```bash
cd eval-parent
mvn package
```

3. The query runtimes are collected in a trig dataset with one named graph per query under:

```bash
./eval-parent/eval-EXPERIMENT/target/bench.trig
```

## Adjusting the benchmark

The amount of grid cells and the number of graphs can be configured in the `eval-template/pom.yml`.


