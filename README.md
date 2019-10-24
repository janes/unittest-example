## Databricks test skeleton

#### Example unit and integration tests for Databricks Spark Scala jobs

This repository holds a sample scala project demonstrating:

- unit testing of individual methods containing Spark DataFrame transformations (in the `com.stuartdb.unittestexample.aggregationFuncsTest` class); and 
- integration testing of a basic pipeline which might consist of calls to many such methods (see example class  `com.stuartdb.unittestexample.pipelineTest`).

The objective is to demonstrate how tests and code can be written in a developer's local environment and executed remotely.

#### Support for remote Continuous Integration pipelines

The project also contains a `Dockerfile` and `buildspec.yml` file that would be required to create an AWS CodeBuild CI project.

This could be set to trigger build and test stages every time code is checked in to a given branch of your pipeline's repository or, perhaps more likely, a pull request is raised

#### Pre-requisites

- A compatible IDE (IntelliJ, VSCode, Eclipse)
- [Java JDK 8](https://openjdk.java.net/install/) , [Scala](https://www.scala-lang.org/) 2.11.12, [SBT](https://www.scala-sbt.org/) 1.3.2
- [Conda](https://docs.conda.io/projects/conda/en/latest/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) (the latter is recommended for simplicity)
- [Databricks Connect](https://docs.databricks.com/dev-tools/databricks-connect.html)
- The input dataset used for the example classes (stored in `/data` in this repository)
- [Docker](https://www.docker.com/), if you want to experiment with performing build and test stages in a remote CI service such as [AWS CodeBuild](https://aws.amazon.com/codebuild/) or [CircleCI](https://circleci.com/).
- Access to AWS CodeBuild if you want to import the repository as a new project.

#### Preparing your environment

(assumes a compatible IDE with Java 8 and SBT 1.3.2 is already available on your local machine)

1. Create a new Conda environment according to the instructions in the Databricks Connect documentation.
2. Activate the *dbconnnect* conda environment by running `conda activate dbconnect`.
3. Install Databricks Connect and run `databricks-connect configure` to enable access to a cluster running in your workspace.
4. Clone this repository to your local machine and import the sbt project into your IDE. This should automatically download the correct version of Scala and the relevant dependencies (_scalatest_ and _deequ_).
5. Determine the path to the Databricks Connect Spark JARs by running `databricks-connect get-jar-dir`. Update the value of `spark_libs` in the `build.sbt` file to reflect.

#### Running the tests

1. If your IDE has integration support for _scalatest_, you may be able to run the tests directly from the editor (no special configuration options are required).
2. Otherwise, create a run task / configuration that calls `sbt test` (to run all tests) or `sbt "testOnly com.stuartdb.unittestexample.aggregationFuncsTest"` to just run a single test. The `pipelineTest` integration test requires copying the parquet dataset to DBFS in your Databricks workspace.

#### Running the full pipeline

1. Copy the parquet dataset to DBFS in your Databricks workspace.
2. You should now be able to run the pipeline class, providing DBFS input and output paths as arguments to the `main()` method.