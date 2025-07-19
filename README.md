# Apache Beam with Python: Detailed Notes and Example Code

Apache Beam (Python API) is a unified programming model for defining both batch and streaming data-parallel processing pipelines. It allows you to write a pipeline once and run it using a variety of execution engines (runners) including Apache Flink, Apache Spark, and Google Cloud Dataflow.

## Key Features of Apache Beam (Python)

- **Unified Model:** One API for batch and streaming data processing.
- **Portability:** Run your code on multiple runners with minimal changes.
- **Scalability:** Efficiently handles large-scale datasets.
- **Extensible:** Supports custom transforms and complex workflows.


## Apache Beam Architecture

Apache Beam is built on three fundamental components:

### 1. Pipeline

- **Definition:** The Pipeline is the *container* for your entire data processing workflow.
- **Role:** It encapsulates every step — from data ingestion, through transformations, to output.
- **Usage:** You build a pipeline by creating a `Pipeline` object and chaining various transforms.


### 2. PCollection

- **Definition:** A PCollection (“parallel collection”) is the core data structure in Beam.
- **Role:** Represents a *distributed, immutable* collection of elements (data).
- **Characteristics:** Acts as an intermediate dataset passed between pipeline transforms.


### 3. Transforms

- **Definition:** Transforms are operations to process data (e.g., Map, Filter, GroupBy).
- **Types:**
    - **Simple**: Map, Filter, FlatMap.
    - **Composite**: GroupBy, Combine, Join.
- **Usage:** Define the logic to transform input PCollections into output PCollections.

<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/d0be0f0f-7d92-48df-b87b-b21bd45a9901" />


## Example: Basic Apache Beam Pipeline in Python

Here’s a step-by-step example of how to create and run a simple Beam pipeline (batch mode) that reads a list of numbers, multiplies each by 2, filters out numbers less than 10, and writes the result to a file.

```python
import apache_beam as beam

def multiply_by_two(x):
    return x * 2

def is_gte_ten(x):
    return x >= 10

with beam.Pipeline() as pipeline:
    (
        pipeline
        | "Create numbers" >> beam.Create([1, 4, 5, 7, 10])
        | "Multiply by 2" >> beam.Map(multiply_by_two)
        | "Filter >= 10" >> beam.Filter(is_gte_ten)
        | "Print output" >> beam.Map(print)
    )
```


### Explanation

- `beam.Create()`: Initializes a PCollection with input numbers.
- `beam.Map(multiply_by_two)`: Multiplies each element by 2.
- `beam.Filter(is_gte_ten)`: Retains only elements greater than or equal to 10.
- `beam.Map(print)`: Outputs the result.


## Running on Different Runners

To run your Beam pipeline on a specific runner like Dataflow or Flink, configure the `PipelineOptions`:

```python
from apache_beam.options.pipeline_options import PipelineOptions

options = PipelineOptions(
    runner='DataflowRunner',  # Or FlinkRunner, SparkRunner, DirectRunner, etc.
    project='your-gcp-project',
    temp_location='gs://your-bucket/temp'
)
with beam.Pipeline(options=options) as pipeline:
    # ... pipeline definition ...
```


## Transforms: More Examples

### Applying a ParDo (custom function)

```python
class SplitWords(beam.DoFn):
    def process(self, element):
        return element.split()

with beam.Pipeline() as pipeline:
    lines = pipeline | 'ReadLines' >> beam.Create(['Apache Beam is powerful'])
    words = lines | 'SplitWords' >> beam.ParDo(SplitWords())
    words | 'PrintWords' >> beam.Map(print)
```


### GroupByKey Example

```python
with beam.Pipeline() as pipeline:
    pairs = pipeline | 'CreatePairs' >> beam.Create([('fruit', 'apple'), ('fruit', 'banana'), ('veg', 'carrot')])
    grouped = pairs | 'GroupByKey' >> beam.GroupByKey()
    grouped | 'PrintGrouped' >> beam.Map(print)
```


## Advantages of Using Apache Beam

- **Write once, run anywhere:** The same code works across multiple distributed engines.
- **Mix batch and streaming:** No need to redesign for streaming or batch.
- **Built-in windowing and triggers:** Handle late data natively.
- **Portable and extensible:** Open source with a prospering ecosystem.


## Summary Table of Key Concepts

| Concept | Description |
| :-- | :-- |
| Pipeline | Blueprint for the data processing workflow |
| PCollection | Distributed, immutable dataset between transforms |
| Transforms | Operations for manipulating PCollections (Map, Filter, Combine) |
| Runner | Execution engine (Dataflow, Flink, Spark, Direct, etc.) |

These notes and sample codes provide a practical introduction to Apache Beam’s core Python concepts. For more complex use-cases, you can leverage additional transforms, windowing, stateful processing, and custom DoFns as you advance.

---

## Apache Beam Interview Questions with Detailed Answers and Examples

Below is a set of common Apache Beam interview questions along with comprehensive answers and practical Python examples to help you understand key concepts and demonstrate your skills effectively.

### 1. What is Apache Beam? Explain its use cases.

**Answer:**
Apache Beam is an open-source unified programming model for both batch and streaming data processing pipelines. It allows users to define pipelines once and run them on multiple distributed processing backends such as Apache Flink, Apache Spark, and Google Cloud Dataflow. Beam simplifies large-scale data processing by abstracting the complexities of different execution engines.

**Use Cases:**

- Real-time data processing (streaming analytics)
- Batch data processing (ETL jobs)
- Event-time processing with late data handling
- Large-scale data integration and transformation pipelines


### 2. What are the core components of Apache Beam?

**Answer:**


| Component | Description |
| :-- | :-- |
| Pipeline | The blueprint or container for your entire data processing workflow |
| PCollection | Distributed, immutable dataset passed between pipeline steps |
| Transform | Operations that manipulate data in the pipeline (e.g., Map, Filter, GroupByKey) |

### 3. How do you create a simple Apache Beam pipeline in Python?

**Example:** A pipeline to multiply numbers by 2 and filter out results less than 10.

```python
import apache_beam as beam

def multiply_by_two(x):
    return x * 2

def is_gte_ten(x):
    return x >= 10

with beam.Pipeline() as pipeline:
    (
        pipeline
        | "Create numbers" >> beam.Create([1, 4, 5, 7, 10])
        | "Multiply by 2" >> beam.Map(multiply_by_two)
        | "Filter >= 10" >> beam.Filter(is_gte_ten)
        | "Print output" >> beam.Map(print)
    )
```

**Explanation:**

- `Create` initializes the collection.
- `Map` applies multiplication.
- `Filter` filters results.
- The pipeline runs with DirectRunner by default.


### 4. What is a ParDo, and how do you use it?

**Answer:**
ParDo is a parallel processing transform in Apache Beam that applies a user-defined function (DoFn) to each element of a PCollection. It is the core element for custom processing.

**Example:** Splitting lines of text into words.

```python
class SplitWords(beam.DoFn):
    def process(self, element):
        return element.split()

with beam.Pipeline() as pipeline:
    lines = pipeline | 'CreateLines' >> beam.Create(['Apache Beam is powerful'])
    words = lines | 'SplitWords' >> beam.ParDo(SplitWords())
    words | 'PrintWords' >> beam.Map(print)
```


### 5. How does Apache Beam handle windowing and late data?

**Answer:**
Windowing in Apache Beam allows grouping of data based on event time for streaming data. Common types are fixed, sliding, and session windows. Watermarks are used to track the progress of event time and determine when a window can be closed. Triggers define when to emit results, including handling late-arriving data.

**Example:** Fixed window with a trigger.

```python
import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions
from apache_beam.transforms.window import FixedWindows
from apache_beam.transforms.trigger import AfterWatermark, AccumulationMode, AfterProcessingTime

with beam.Pipeline(options=PipelineOptions()) as pipeline:
    lines = pipeline | 'CreateEvents' >> beam.Create([
        ('event1', 1), ('event2', 2), ('event3', 55)
    ])

    windowed = (lines 
        | "WindowIntoFixed" >> beam.WindowInto(
            FixedWindows(60),
            trigger=AfterWatermark(late=AfterProcessingTime(10)),
            accumulation_mode=AccumulationMode.DISCARDING)
    )

    windowed | 'PrintWindowed' >> beam.Map(print)
```


### 6. What is a side input? Provide an example.

**Answer:**
A side input allows a transform to access additional data besides the main input, often used to enrich or look up data during processing.

**Example:** Using a side input dictionary to enrich stream data.

```python
with beam.Pipeline() as pipeline:
    main_input = pipeline | "MainInput" >> beam.Create(['key1', 'key2', 'key3'])
    side_input = pipeline | "SideInput" >> beam.Create({'key1': 'value1', 'key2': 'value2'})

    def enrich(element, lookup_dict):
        return (element, lookup_dict.get(element, 'N/A'))

    enriched = main_input | beam.Map(enrich, beam.pvalue.AsDict(side_input))
    enriched | beam.Map(print)
```


### 7. What is GroupByKey, and when is it used?

**Answer:**
GroupByKey groups pairs by their keys, aggregating all values for a given key into an iterable. It is used for operations like aggregation, join, or secondary grouping.

**Example:**

```python
with beam.Pipeline() as pipeline:
    pairs = pipeline | 'CreatePairs' >> beam.Create([
        ('fruit', 'apple'), ('fruit', 'banana'), ('veg', 'carrot')
    ])
    grouped = pairs | 'GroupByKey' >> beam.GroupByKey()
    grouped | 'PrintGrouped' >> beam.Map(print)
```

Output:

```
('fruit', ['apple', 'banana'])
('veg', ['carrot'])
```


### 8. What are runners in Apache Beam?

**Answer:**
Runners are execution engines that run Apache Beam pipelines. Examples include:

- DirectRunner (local testing)
- DataflowRunner (Google Cloud Dataflow)
- FlinkRunner (Apache Flink)
- SparkRunner (Apache Spark)

You specify them with `PipelineOptions`.

### Summary Table of Key Concepts

| Concept | Explanation | Python Example Reference |
| :-- | :-- | :-- |
| Pipeline | Defines the whole data processing workflow | Example 3 |
| PCollection | Distributed dataset passed between transforms | Example 3 |
| Transform | Operations that act on data | Example 3,4,5 |
| ParDo/DoFn | User-defined function transform | Example 4 |
| Windowing | Handling time-based grouping | Example 5 |
| Side Inputs | Additional input for transforms | Example 6 |
| GroupByKey | Group values by a shared key | Example 7 |
| Runners | Execution engines for pipelines | Example 3 (options usage) |

These examples illustrate practical Apache Beam usage and provide you with ready-to-use answers for interview scenarios, demonstrating both conceptual understanding and coding proficiency.

<div style="text-align: center">⁂</div>

[^1]: https://bhaveshbhadricha4806.ongraphy.com/blog/apache-beam-interview-questions-and-answers

[^2]: https://interviewprep.org/apache-beam-interview-questions/

[^3]: https://www.hellointern.in/blog/beam-worker-interview-questions-and-answers-62372

[^4]: https://www.theaiops.com/top-25-apache-beam-interview-questions-with-answers/

[^5]: https://learnbeam.dev/article/Top_10_Apache_Beam_interview_questions_and_answers.html

[^6]: https://www.techgeeknext.com/apache-beam-interview-questions

[^7]: https://climbtheladder.com/apache-beam-interview-questions/

[^8]: https://www.thedataops.org/top-apache-beam-interview-questions-with-answers/

[^9]: https://beam.apache.org/documentation/basics/

[^10]: https://www.jesse-anderson.com/2016/07/question-and-answers-with-the-apache-beam-team/



