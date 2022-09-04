---
title: TFX Standard Components
date: 2022-04-22 16:56:54
description:
tags: 
 - data_science
 - mlops
 - machine_learning
 - tfx
 - tfx_standard_components
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---

## Basic Overview of Components

A TFX pipeline contains modular components which are aligned in a sequence to produce a robust, scalable, high-performance machine learning system Basic Tasks in a Machine Learning process are :

![](https://www.tensorflow.org/tfx/guide/images/prog_fin.png)

## [ExampleGen](https://www.tensorflow.org/tfx/guide/examplegen)
The ExampleGen TFX pipeline component is the entry point to your pipeline, that ingests data. As inputs, ExampleGen supports out-of-the-box ingestion of external data sources such as CSV, TF Records, Avro, Parquet and BigQuery. As outputs, ExampleGen produces TF examples, TF sequence examples or proto format that are highly efficient in performant data set representations, that can be read consistently by downstream components.

### Span, Version and Split

A Span is a grouping of training examples. If your data is persisted on a filesystem, each Span may be stored in a separate directory. The semantics of a Span are not hardcoded into TFX; a Span may correspond to a day of data, an hour of data, or any other grouping that is meaningful to your task.

Each Span can hold multiple Versions of data. To give an example, if you remove some examples from a Span to clean up poor quality data, this could result in a new Version of that Span. By default, TFX components operate on the latest Version within a Span.

Each Version within a Span can further be subdivided into multiple Splits. The most common use-case for splitting a Span is to split it into training and eval data.

![](https://www.tensorflow.org/tfx/guide/images/spans_splits.png)

For example, let's assume there are input data 

-   '/tmp/span-1/ver-1/train/data'
-   '/tmp/span-1/ver-1/eval/data'
-   '/tmp/span-2/ver-1/train/data'
-   '/tmp/span-2/ver-1/eval/data'
-   '/tmp/span-2/ver-2/train/data'
-   '/tmp/span-2/ver-2/eval/data'

when triggering the pipeline, it should process:

-   '/tmp/span-2/ver-2/train/data' as train split
-   '/tmp/span-2/ver-2/eval/data' as eval split 

with span number as '2' and version number as '2'. If later on '/tmp/span-2/ver-3/...' are ready, simply trigger the pipeline again and it will pick up span '2' and version '3' for processing. Below shows the code example for using version spec:

```python 
input = proto.Input(splits=[
	proto.Input.Split(name='train', pattern='span-{SPAN}/ver-{VERSION}/train/*'),
	proto.Input.Split(name='eval', pattern='span-{SPAN}/ver-{VERSION}/eval/*')
])
example_gen = CsvExampleGen(input_base='/tmp', input_config=input)
```

### [Range Config](https://www.tensorflow.org/tfx/guide/examplegen#range_config)

```python

from  tfx.components.example_gen import utils
input = proto.Input(splits=[
			proto.Input.Split(name='train', pattern='{YYYY}-{MM}-{DD}/train/*'),
			proto.Input.Split(name='eval', pattern='{YYYY}-{MM}-{DD}/eval/*')
			])

# Specify date to be converted to span number to be processed using StaticRange.
span = utils.date_to_span_number(1970, 1, 2)

range= proto.RangeConfig(static_range=range_config_pb2.StaticRange(
						 start_span_number=span,end_span_number=span))

# After substitution, the train and eval split patterns will be 
# 'input_dir/1970-01-02/train/*' and 'input_dir/1970-01-02/eval/*',
# respectively.
example_gen = CsvExampleGen(input_base=input_dir, input_config=input,
							range_config=range)

```


## [StatisticsGen](https://www.tensorflow.org/tfx/guide/statsgen)
The StatisticsGen TFX pipeline component generates features statistics over both training and serving data, which can be used by other pipeline components. Internally It uses Beam to scale to large datasets.

### Using StatisticsGen 

Basic Usage of StatisticsGen is as follows
```python
compute_eval_stats = StatisticsGen(examples=example_gen.outputs['examples'],name='compute-eval-stats')
```

### Using the StatisticsGen Component With a Schema

For the first run of a pipeline, the output of StatisticsGen will be used to infer a schema. However, on subsequent runs we may have a manually curated schema that contains additional information about our data set. By providing this schema to StatisticsGen, TFDV can provide more useful statistics based on declared properties of our data set. In this setting, we will invoke StatisticsGen with a curated schema that has been imported by an ImporterNode like this:

```PYTHON
user_schema_importer = Importer(source_uri=user_schema_dir, # directory containing only schema text proto
								artifact_type=standard_artifacts.Schema).with_id('schema_importer')

compute_eval_stats = StatisticsGen(
						examples=example_gen.outputs['examples'],
						schema=user_schema_importer.outputs['result'],
						name='compute-eval-stats')
```

## [SchemaGen](https://www.tensorflow.org/tfx/guide/schemagen)
The TFX components use a description of your input data called a schema. The schema is an instance of [schema.proto](https://github.com/tensorflow/metadata/blob/master/tensorflow_metadata/proto/v0/schema.proto). It can specify data types for feature values, whether a feature has to be present in all examples, allowed value ranges, and other properties. Moreover, a SchemaGen pipeline component will automatically generate a schema by inferring types, categories, and ranges from the training data.

The auto-generated schema does its best but only  infers basic properties of the data. It is expected that developers review and modify it as needed. The modified schema can be brought back into the pipeline using ImportSchemaGen component. The SchemaGen component for the initial schema generation can be removed and all downstream components can use the output of ImportSchemaGen.

```python
# This will generate the a fresh Schema
schema_gen = tfx.components.SchemaGen(statistics=stats_gen.outputs['statistics'])
# This is used whenn we want to import a modified schema 
schema_gen = tfx.components.ImportSchemaGen(schema_file='/some/path/schema.pbtxt')
```

More details are available in the [ImportSchemaGen API reference](https://www.tensorflow.org/tfx/api_docs/python/tfx/v1/components/ImportSchemaGen).

## [ExampleValidator](https://www.tensorflow.org/tfx/guide/exampleval)
The ExampleValidator pipeline component identifies anomalies in training and serving data.  It identifies any anomalies in the example data by comparing data statistics computed by the [StatisticsGen](#statisticsgen) pipeline component against a schema. The inferred schema codifies properties which the input data is expected to satisfy, and can be modified by the developer.  It can perform following tasks

-   Perform validity checks by comparing data statistics against a schema that codifies expectations of the user
-   Detects training-serving skew by comparing training and serving data.
-   Detect data drift by looking at a series of data.

```
validate_stats = ExampleValidator(statistics=statistics_gen.outputs['statistics'],schema=schema_gen.outputs['schema'])
```

More details are available in the [ExampleValidator API reference](https://www.tensorflow.org/tfx/api_docs/python/tfx/v1/components/ExampleValidator).

## [Transform](https://www.tensorflow.org/tfx/guide/transform)
The Transform TFX pipeline component performs feature engineering on tf.Examples emitted from an [ExampleGen](#examplegen) component, using a data schema created by a [SchemaGen](#schemagen) component, and emits both a SavedModel as well as statistics on both pre-transform and post-transform data. When executed, the SavedModel will accept tf.Examples emitted from an [ExampleGen](#examplegen) component and emit the transformed feature data

The Transform TFX pipeline component performs feature engineering on the TF examples data artifact emitted from the [ExampleGen](#examplegen) component. Using the data schema artifact from the [SchemaGen](#schemagen), or imported from external sources.

TensorFlow Transform builds transformations into the TensorFlow graph for your model so the same transformations are performed at training and inference time. This approach helps avoid training/serving skew. You can define transformations that refer to global properties of the data, like the max value of a feature across all training instances.

### Configuring a Transform Component

Once your `preprocessing_fn` is written, it needs to be defined in a python module that is then provided to the Transform component as an input. This module will be loaded by transform and the function named `preprocessing_fn` will be found and used by Transform to construct the preprocessing pipeline.

```python
transform = Transform(examples=example_gen.outputs['examples'],
					  schema=schema_gen.outputs['schema'],
					  module_file=os.path.abspath(_taxi_transform_module_file))
```

Additionally, you may wish to provide options to the [TFDV](https://www.tensorflow.org/tfx/guide/tfdv)-based pre-transform or post-transform statistics computation. To do so, define a `stats_options_updater_fn` within the same module. 

In `preprocessing_fn` you define a series of functions that manipulate the input dict of tensors to produce the output dict of tensors.

```python
def preprocessing_fn(inputs):
  """tf.transform's callback function for preprocessing inputs.

  Args:
    inputs: map from feature keys to raw not-yet-transformed features.

  Returns:
    Map from string feature key to transformed feature operations.
  """
  outputs = {}
  for key in _DENSE_FLOAT_FEATURE_KEYS:
    # If sparse make it dense, setting nan's to 0 or '', and apply zscore.
    outputs[_transformed_name(key)] = transform.scale_to_z_score(
        _fill_in_missing(inputs[key]))

  for key in _VOCAB_FEATURE_KEYS:
    # Build a vocabulary for this feature.
    outputs[_transformed_name(
        key)] = transform.compute_and_apply_vocabulary(
            _fill_in_missing(inputs[key]),
            top_k=_VOCAB_SIZE,
            num_oov_buckets=_OOV_SIZE)

  for key in _BUCKET_FEATURE_KEYS:
    outputs[_transformed_name(key)] = transform.bucketize(
        _fill_in_missing(inputs[key]), _FEATURE_BUCKET_COUNT)

  for key in _CATEGORICAL_FEATURE_KEYS:
    outputs[_transformed_name(key)] = _fill_in_missing(inputs[key])

  # Was this passenger a big tipper?
  taxi_fare = _fill_in_missing(inputs[_FARE_KEY])
  tips = _fill_in_missing(inputs[_LABEL_KEY])
  outputs[_transformed_name(_LABEL_KEY)] = tf.where(
      tf.is_nan(taxi_fare),
      tf.cast(tf.zeros_like(taxi_fare), tf.int64),
      # Test if the tip was > 20% of the fare.
      tf.cast(
          tf.greater(tips, tf.multiply(taxi_fare, tf.constant(0.2))), tf.int64))

  return outputs

```

## [Trainer](https://www.tensorflow.org/tfx/guide/trainer)
The Trainer TFX pipeline component trains a TensorFlow model. The trainer component produces at least one model for inference and serves in a TensorFlow  SavedModelFormat and optionally another model for eval an EvalSavedModel. A safe model contains a complete TensorFlow program, including weights and computation.

Trainer takes in Input :

- tf.Examples used for training and eval.
- A user provided module file that defines the trainer logic.
- [Protobuf](https://developers.google.com/protocol-buffers) definition of train args and eval args.
- (Optional) A data schema created by a [SchemaGen](#schemagen) pipeline component and optionally altered by the developer.
- (Optional) transform graph produced by an upstream [Transform](#transform) component.
- (Optional) pre-trained models used for scenarios such as warmstart.
- (Optional) hyperparameters, which will be passed to user module function. 
- Details of the integration with Tuner can be found [here](https://www.tensorflow.org/tfx/guide/tuner).

### Configuring the Trainer Component
Typical pipeline DSL code for the generic Trainer would look like this:

```python
from tfx.components import Trainer
...
trainer = Trainer(module_file=module_file,
				  examples=transform.outputs['transformed_examples'],
				  transform_graph=transform.outputs['transform_graph'],
				  train_args=trainer_pb2.TrainArgs(num_steps=10000),
				  eval_args=trainer_pb2.EvalArgs(num_steps=5000))
```

Trainer invokes a training module, which is specified in the `module_file` parameter. Instead of `trainer_fn`, a `run_fn` is required in the module file if the `GenericExecutor` is specified in the `custom_executor_spec`. The `trainer_fn` was responsible for creating the model. In addition to that, `run_fn` also needs to handle the training part and output the trained model to a the desired location given by [FnArgs](https://github.com/tensorflow/tfx/blob/master/tfx/components/trainer/fn_args_utils.py):

```python
from tfx.components.trainer.fn_args_utils import FnArgs
def run_fn(fn_args: FnArgs) -> None:  
	"""Build the TF model and train it."""
	model = _build_keras_model()
	model.fit(...)  # Save model to fn_args.serving_model_dir.  
	model.save(fn_args.serving_model_dir, ...)
```

More details are available in the [Trainer API reference](https://www.tensorflow.org/tfx/api_docs/python/tfx/v1/components/Trainer).

## [Tuner](https://www.tensorflow.org/tfx/guide/tuner)
The Tuner component tunes the hyperparameters for the model. The Tuner component makes extensive use of the Python [KerasTuner](https://www.tensorflow.org/tutorials/keras/keras_tuner) API for tuning hyperparameters. As inputs, the tuner component takes in:

-   tf.Examples used for training and eval.
-   A user provided module file (or module fn) that defines the tuning logic, including model definition, hyperparameter search space, objective etc.
-   [Protobuf](https://developers.google.com/protocol-buffers) definition of train args and eval args.
-   (Optional) [Protobuf](https://developers.google.com/protocol-buffers) definition of tuning args.
-   (Optional) transform graph produced by an upstream Transform component.
-   (Optional) A data schema created by a SchemaGen pipeline component and optionally altered by the developer.

With the given data, model, and objective, Tuner tunes the hyperparameters and emits the best result.

A user module function `tuner_fn` with the following signature is required for Tuner:

```python
...

from keras_tuner.engine import base_tuner

TunerFnResult = NamedTuple('TunerFnResult', [('tuner', base_tuner.BaseTuner),
                                             ('fit_kwargs', Dict[Text, Any])])

def tuner_fn(fn_args: FnArgs) -> TunerFnResult:
  """Build the tuner using the KerasTuner API.
  Args:
    fn_args: Holds args as name/value pairs.
      - working_dir: working dir for tuning.
      - train_files: List of file paths containing training tf.Example data.
      - eval_files: List of file paths containing eval tf.Example data.
      - train_steps: number of train steps.
      - eval_steps: number of eval steps.
      - schema_path: optional schema of the input data.
      - transform_graph_path: optional transform graph produced by TFT.
  Returns:
    A namedtuple contains the following:
      - tuner: A BaseTuner that will be used for tuning.
      - fit_kwargs: Args to pass to tuner's run_trial function for fitting the
                    model , e.g., the training and validation dataset. Required
                    args depend on the above tuner's implementation.
  """
  ...
```

In this function, you define both the model and hyperparameter search spaces, and choose the objective and algorithm for tuning. The Tuner component takes this module code as input, tunes the hyperparameters, and emits the best result.

Trainer can take Tuner's output hyperparameters as input and utilize them in its user module code. The pipeline definition looks like this:

```python
...
tuner = Tuner(
    module_file=module_file,  # Contains `tuner_fn`.
    examples=transform.outputs['transformed_examples'],
    transform_graph=transform.outputs['transform_graph'],
    train_args=trainer_pb2.TrainArgs(num_steps=20),
    eval_args=trainer_pb2.EvalArgs(num_steps=5))

trainer = Trainer(
    module_file=module_file,  # Contains `run_fn`.
    examples=transform.outputs['transformed_examples'],
    transform_graph=transform.outputs['transform_graph'],
    schema=schema_gen.outputs['schema'],
    # This will be passed to `run_fn`.
    hyperparameters=tuner.outputs['best_hyperparameters'],
    train_args=trainer_pb2.TrainArgs(num_steps=100),
    eval_args=trainer_pb2.EvalArgs(num_steps=5))
...
```

You might not want to tune the hyperparameters every time you retrain your model. Once you have used Tuner to determine a good set of hyperparameters, you can remove Tuner from your pipeline and use `ImporterNode` to import the Tuner artifact from a previous training run to feed to Trainer.

```python
hparams_importer = Importer(
    # This can be Tuner's output file or manually edited file. The file contains
    # text format of hyperparameters (keras_tuner.HyperParameters.get_config())
    source_uri='path/to/best_hyperparameters.txt',
    artifact_type=HyperParameters,
).with_id('import_hparams')

trainer = Trainer(
    ...
    # An alternative is directly use the tuned hyperparameters in Trainer's user
    # module code and set hyperparameters to None here.
    hyperparameters = hparams_importer.outputs['result'])
```

## [Evaluator](https://www.tensorflow.org/tfx/guide/evaluator)
The evaluator component will use the model created by the trainer in the original input data artifact. In addition, it will perform a thorough analysis using the TensorFlow model analysis library.

## [InfraValidator](https://www.tensorflow.org/tfx/guide/infra_validator)
InfraValidator, which is a TFX component that is used as an early warning layer before pushing a model to production. The name InfraValidator came from the fact that it is validating the model in the actual model serving infrastructure. If the evaluator guarantees the performance of the model, InfraValidator guarantees that the model is mechanically fine.

## [Pusher](https://www.tensorflow.org/tfx/guide/pusher)
The Pusher component is used to push a validated model to a [deployment target](https://www.tensorflow.org/tfx/guide#deployment_targets) during model training or re-training. Before the deployment, Pusher relies on one or more blessings from other validation components to decide whether to push the model or not.

-   [Evaluator](#evaluator) blesses the model if the new trained model is "good enough" to be pushed to production.
-   (Optional but recommended) [InfraValidator](#infravalidator) blesses the model if the model is mechanically servable in a production environment.

A Pusher component consumes a trained model in [SavedModel](https://www.tensorflow.org/guide/saved_model) format, and produces the same SavedModel, along with versioning metadata.

```python
pusher = Pusher(model=trainer.outputs['model'],
				model_blessing=evaluator.outputs['blessing'],
				infra_blessing=infra_validator.outputs['blessing'], 
				push_destination=tfx.proto.PushDestination(
					filesystem=tfx.proto.PushDestination.Filesystem(base_directory=serving_model_dir))
				)
```

## [BulkInferrer](https://www.tensorflow.org/tfx/guide/bulkinferrer)
A BulkInferrer TFX component is used to perform batch inference on unlabeled tf.Examples. It is typically deployed after an [Evaluator](#evaluator) component to perform inference with a validated model, or after a [Trainer](#trainer) component to directly perform inference on exported model.

BulkInferrer consumes:

-   A trained model in [SavedModel](https://www.tensorflow.org/guide/saved_model.md) format.
-   Unlabelled tf.Examples that contain features.
-   (Optional) Validation result from [Evaluator](https://www.tensorflow.org/tfx/guide/evaluator.md) component.

BulkInferrer emits:

-   [InferenceResult](https://github.com/tensorflow/tfx/blob/master/tfx/types/standard_artifacts.py)

Typical code looks like this:

```python
bulk_inferrer = BulkInferrer(examples=examples_gen.outputs['examples'],
							 model=trainer.outputs['model'],
							 model_blessing=evaluator.outputs['blessing'],
							 data_spec=bulk_inferrer_pb2.DataSpec(),
							 model_spec=bulk_inferrer_pb2.ModelSpec())
```