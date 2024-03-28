# Spatial Index Evaluations for Apache Jena

This repository contains an evaluation setup that compares:

* Jena's default spatial index implementation, referred to as _vanilla_
* Our improved implementation, referred to as  _geoplus_

## Result Datasets

We are finalizing the RDF datasets ~ 2024-03-28

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


