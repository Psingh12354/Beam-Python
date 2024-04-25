# Beam-Python

Apache beam python is a tool for writing data processing pipelines in python, allowing you to handle large scale data efficiently. It provide easy to use API for building complex data processing workflows that can run on various execution engine such as apache flink, apache spark and google cloud dataflow.

<img width="489" alt="image" src="https://github.com/Psingh12354/Beam-Python/assets/55645997/330a73f1-044c-4486-a42c-d9569b7207db">

<img width="600" alt="image" src="https://github.com/Psingh12354/Beam-Python/assets/55645997/66dcbe12-b508-46c1-8f32-dc5e2f95b0d0">

<img width="532" alt="image" src="https://github.com/Psingh12354/Beam-Python/assets/55645997/7f101993-5f96-477e-8ce2-91cfe56a1219">

## Apache Beam Architecture


**Apache Beam** is built around three core components:


### 1. Pipeline


The *Pipeline* represents the entire data processing workflow. It serves as the blueprint for the series of data transformations and operations that will be applied to the input data.


### 2. PCollection


*PCollection*, short for "**parallel collection**," is the fundamental data abstraction in **Apache Beam**. It represents a distributed, immutable collection of elements (data) that are processed within the pipeline. *PCollection* acts as the conduit through which data flows between pipeline transforms.


### 3. Transforms


*Transforms* are the operations that manipulate data within the pipeline. They can vary from simple operations like mapping or filtering data to complex operations like joining datasets or aggregating data. *Transforms* define the logic and behavior of the data processing pipeline.


**Apache Beam** offers a unified programming model, enabling you to write your data processing logic once and execute it on various distributed processing backends such as Apache Flink, Apache Spark, Google Cloud Dataflow, etc. This architecture promotes portability and scalability, allowing you to run your data processing pipelines seamlessly across different environments without the need for significant code modifications.
