# Spatial Index Evaluations for Apache Jena

This repository contains an evaluation setup that compares:

* Jena's default spatial index implementation, referred to as _vanilla_
* Our improved implementation, referred to as  _geoplus_

The evaluation is uses our simple [GridBench](https://github.com/AKSW/gridbench) benchmark.

## Result Datasets

We are in the process of finalizing RDF dataset deployment ~ 2024-03-28

Alpha deployments of the datasets are deployed under:

http://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/

### Query Types

ng-one: Benchmark queries target a single named graph in the dataset.

```sparql
SELECT (COUNT(*) AS ?c) {
  GRAPH <CONST> {
    ...
  }
}
```

ng-all: Benchmark queries target all named graphs in the dataset: `GRAPH ?g { }`

```sparql
SELECT (COUNT(*) AS ?c) {
  GRAPH ?g {
    ...
  }
}
```

ug: Benchmark queries target the union default graph, i.e. a view over all named graphs

```sparql
SELECT (COUNT(*) AS ?c) {
  ...
}
```

### Download Links

* eval-geoplus-ng-one
* eval-geoplus-ng-all
* [eval-geoplus-ug](http://maven.aksw.org/repository/snapshots/org/aksw/eval/gridbench/jena/eval-geoplus-ug/0.0.1-SNAPSHOT/eval-geoplus-ug-0.0.1-20240328.193113-1.trig)
* eval-vanilla-ng-one
* eval-vanilla-ng-all
* eval-vanilla-ug


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


