---
title: TFX Introduction
date: 2022-04-22 15:57:17
description:
tags: 
 - data_science
 - mlops
 - tfx
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---
TFX is a Tensorflow-based platform for running end-to-end Machine Learning pipelines. We can construct pipelines to clean data, train, and serve production-ready machine learning systems using the TFX configuration framework. TFX is used to build a robust ML production pipeline. TFX's ML Ops Platform is modular, adaptable, collaborative, accessible, and simple to use. Each TFX component enables proper ML Model storage, configuration, and orchestration.  TFX is a natural fit for researchers and engineers looking for quick and secure solutions to use ML. 

## TFX Orchestrators
Orchestrators automates task executions and monitors TF components. TFX supports 3 orchestrators  : [Apache Beam](https://www.tensorflow.org/tfx/guide/beam_orchestrator), [Apache Airflow ](https://www.tensorflow.org/tfx/guide/airflow), [Kubeflow Pipelines](https://www.tensorflow.org/tfx/guide/kubeflow)

1. [Apache Beam](https://beam.apache.org/documentation/) is the unified batch and stream distributed API which acts as an abstraction layer to run on top of the distributed processing framework. This allows you to work on diverse backends such as Apache Spark, Local, Dataflow, etc.

2. [Apache Airflow](https://airflow.apache.org/docs/) is a platform to programmatically author, schedule and monitor workflows. Use Airflow to author workflows as Directed Acyclic Graphs (DAGs) of tasks. The Airflow scheduler executes your tasks on an array of workers while following the specified dependencies

3. [Kubeflow Pipelines](https://www.kubeflow.org/docs/components/pipelines/introduction/)  is a platform for building and deploying portable, scalable machine learning (ML) workflows based on Docker containers.


## [Metadata Store](https://www.tensorflow.org/tfx/guide/mlmd)
Metadata is an open source framework for storing ML metadata in a generic database that is accessible by SQL based languages. This metadata saves all component records and allows us to compare results with the previous models and carry over from the models. It audits and optimizes the pipeline to focus on data cache and allows us to trace forward and start from where we left off. In production deployment of TFX, you access metadata using ML Metadata [MLMD](https://www.tensorflow.org/tfx/guide/mlmd) API which stores properties in MySQL database or persistent storage.

## [Understanding TFX Pipelines](https://www.tensorflow.org/tfx/guide/understanding_tfx_pipelines) 
### Artifact
The outputs of steps in a TFX pipeline are called **artifacts**. Subsequent steps in your workflow may use these artifacts as inputs. In this way, TFX lets you transfer data between workflow steps.

### Parameter
**Parameters** are inputs to pipelines that are known before your pipeline is executed. Parameters let you change the behavior of a pipeline, or a part of a pipeline, through configuration instead of code.

### Component
A **component** is an implementation of an ML task that you can use as a step in your TFX pipeline. Components are composed of:

-   A component specification, which defines the component's input and output artifacts, and the component's required parameters.
-   An executor, which implements the code to perform a step in your ML workflow, such as ingesting and transforming data or training and evaluating a model.
-   A component interface, which packages the component specification and executor for use in a pipeline.

TFX provides several [[TFX Standard Components]] that you can use in your pipelines. If these components do not meet your needs, you can build custom components. [Learn more about custom components](https://www.tensorflow.org/tfx/guide/understanding_custom_components).

### TFX Pipeline
A TFX pipeline is a portable implementation of an ML workflow that can be run on various orchestrators, such as: Apache Airflow, Apache Beam, and Kubeflow Pipelines. A pipeline is composed of component instances and input parameters.

Component instances produce artifacts as outputs and typically depend on artifacts produced by upstream component instances as inputs. The execution sequence for component instances is determined by creating a directed acyclic graph of the artifact dependencies.

For example, consider a pipeline that does the following:

-   Ingests data directly from a proprietary system using a custom component.
-   Calculates statistics for the training data using the StatisticsGen standard component.
-   Creates a data schema using the SchemaGen standard component.
-   Checks the training data for anomalies using the ExampleValidator standard component.
-   Performs feature engineering on the dataset using the Transform standard component.
-   Trains a model using the Trainer standard component.
-   Evaluates the trained model using the Evaluator component.
-   If the model passes its evaluation, the pipeline enqueues the trained model to a proprietary deployment system using a custom component.

![](https://www.tensorflow.org/tfx/guide/images/tfx_pipeline_graph.svg)

To determine the execution sequence for the component instances, TFX analyzes the artifact dependencies.

-   The data ingestion component does not have any artifact dependencies, so it can be the first node in the graph.
-   StatisticsGen depends on the _examples_ produced by data ingestion, so it must be executed after data ingestion.
-   SchemaGen depends on the _statistics_ created by StatisticsGen, so it must be executed after StatisticsGen.
-   ExampleValidator depends on the _statistics_ created by StatisticsGen and the _schema_ created by SchemaGen, so it must be executed after StatisticsGen and SchemaGen.
-   Transform depends on the _examples_ produced by data ingestion and the _schema_ created by SchemaGen, so it must be executed after data ingestion and SchemaGen.
-   Trainer depends on the _examples_ produced by data ingestion, the _schema_ created by SchemaGen, and the _saved model_ produced by Transform. The Trainer can be executed only after data ingestion, SchemaGen, and Transform.
-   Evaluator depends on the _examples_ produced by data ingestion and the _saved model_ produced by the Trainer, so it must be executed after data ingestion and the Trainer.
-   The custom deployer depends on the _saved model_ produced by the Trainer and the _analysis results_ created by the Evaluator, so the deployer must be executed after the Trainer and the Evaluator.

Based on this analysis, an orchestrator runs:

-   The data ingestion, StatisticsGen, SchemaGen component instances sequentially.
-   The ExampleValidator and Transform components can run in parallel since they share input artifact dependencies and do not depend on each other's output.
-   After the Transform component is complete, the Trainer, Evaluator, and custom deployer component instances run sequentially.

## Further Reading 
- Read More About [[TFX Standard Components]]
- Read About TFX in more details from the [User Guide](https://www.tensorflow.org/tfx/guide) 
