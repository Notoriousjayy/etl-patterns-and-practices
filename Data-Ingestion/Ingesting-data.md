# Data Acquisition and Transfer

Data acquisition and transfer, at its core, is the process of moving data from its origin to a specified destination. The main objective is to bring data into an environment designed for preparation, transformation, analysis, and the application of advanced technologies like artificial intelligence and machine learning (AI/ML). While large enterprises often focus on internal data movement across teams, for most, the emphasis lies in extracting data from external sources and integrating it into internal systems.

In today’s data-driven world, where information plays a pivotal role in business strategies and product innovation, the value of accurate and timely data cannot be underestimated. This growing dependence has resulted in a vast array of “sources” that teams tap into for insights, enabling smarter decisions, better products, and streamlined operations. For example, a marketing team might pull data from platforms such as Meta, Google Ads and Analytics, LinkedIn, Snapchat, and Mailchimp to optimize campaigns.

Over time, however, data sources and APIs evolve. New columns might be added, existing fields renamed, deprecated, or removed, and older versions replaced with updates. While adapting to changes from a single source can be manageable, scaling to handle modifications from numerous sources—whether five, ten, or a hundred—becomes a complex challenge. This raises a critical question: *“How can an emerging data team effectively manage the variety of sources in a scalable and consistent manner?”* 

For data engineers, the goal is clear: to establish and maintain a reputation for delivering dependable and accessible data solutions, even as the demands from different departments continue to grow in complexity and scale.

## The Evolution of Data Acquisition: Past vs. Present

While the core principles of data acquisition have remained consistent, the methods and technologies have undergone significant evolution. With the ever-growing scale, speed, and diversity of data, our approaches must also adapt.

This evolution is evident through major industry shifts, such as the adoption of cloud-based systems, the progression from data warehouses to data lakes and lakehouses, and the simplification of streaming technologies. A prominent transformation has been the move from traditional extract-transform-load (ETL) workflows to extract-load-transform (ELT) processes. The primary distinction lies in loading all raw data directly into a target system, with transformations occurring afterward—a topic that will be explored further in Chapter 2.

While the terminology surrounding ETL and ELT is not something to overly fixate on, it's important to note that most modern data engineering workflows now involve staging the bulk of the data in the cloud. An exception arises in scenarios where extraordinarily large volumes of granular data—such as IoT or sensor data involving trillions of rows—are processed daily, necessitating pre-aggregation or filtering before staging.

Amid these changes, one enduring truth remains: the process of extraction involves retrieving data from a source and writing it to a target. For this reason, discussions about extraction should remain focused on this fundamental aspect.

### Data Sources and Destinations

Data ingestion is often associated with extraction, but it is inherently linked to loading—every source must have a corresponding destination. In this discussion, we assume you already have a data warehouse or lake in place, so the specifics of storage architecture won't be our focus here. Instead, we’ll delve into best practices for staging data and outline the key characteristics of effective storage solutions.

Our goal is to equip you with the tools needed to design robust architectures, acknowledging that there may be no universally "perfect" solution. We’ll explore a framework for evaluating data sources and addressing the complexities of ingestion workflows. This high-level perspective aims to provide you with a clear understanding of the landscape, empowering you to make well-informed and context-specific decisions.

### Understanding the Source

The starting point for any data ingestion process is the source and its unique characteristics. In most cases, you’ll deal with multiple sources, each requiring individual evaluation to allocate adequate resources and define the criteria for your ingestion strategy.

Given the growing number of data sources and evolving business needs (requests to add sources are routine, but rarely does anyone ask to remove one), it’s likely that some sources won’t fit neatly into a single solution. Building trust in your data pipelines takes time and consistency, but trust can be easily lost if ingestion fails to be reliable or timely. So, what factors are critical when evaluating a source?

#### Analyzing the Source
To help you understand and evaluate a source effectively, we present a set of time-tested questions. These will guide you in identifying the source’s characteristics and understanding its business value.

Adopt a critical perspective—some sources might be unnecessary, or alternative sources might better align with the business’s goals. As the data expert, it’s your responsibility to verify assumptions and focus on simplicity and necessity rather than adding unnecessary complexity.

When analyzing sources, remember that collaboration with software engineers on upstream data is crucial, but overlooking downstream requirements can lead to costly errors that may only surface after weeks of work.


### Key Questions to Consider

#### **1. Who are the stakeholders?**
Understanding the people who will interact with the data pipeline is essential. Who are the stakeholders involved, and what are their objectives? Aligning the pipeline’s goals with stakeholders’ motives—such as achieving specific OKRs (objectives and key results)—can streamline the process and ensure alignment with organizational needs.

#### **2. How will the data be used?**
The intended use of the data should guide decisions throughout the ingestion process. This question helps identify stakeholder needs and uncover the underlying problems they are trying to solve. Creating a list of clear, technical yes/no requirements can reduce ambiguity and keep expectations aligned.

#### **3. What’s the frequency of data updates?**
Data ingestion frequency often revolves around whether data is processed in batches or streams. Begin by determining if the data is bounded (e.g., a completed dataset like a census) or unbounded (e.g., continuous log data). The minimum frequency at which the source updates—such as daily API updates—sets a hard limit for how often data can be extracted.

#### **4. What is the expected data volume?**
While data storage has become more affordable, volume still affects how data is written, processed, and scaled. High-volume sources may require specific strategies to handle scalability, while smaller datasets might not demand the same level of optimization.

#### **5. What is the format of the data?**
The input format plays a significant role in the ingestion process. Data might come in JSON via APIs, from an FTP server, or be stored in a relational database. Understanding the format and schema (or lack thereof) helps design a compatible and efficient pipeline.

#### **6. What is the quality of the data?**
The quality of the source data determines the extent of transformation or enrichment required. As data engineers, your goal is to provide consistent, high-quality datasets to users. This might involve cleaning up columns, renaming fields, or supplementing the data with external sources to fill gaps.

#### **7. How will the data be stored?**
Even with a pre-determined destination like a data warehouse or lake, decisions around staging, business requirements, and stakeholder fit remain critical. Staging data is almost always recommended to ensure flexibility and manage transformations effectively.


### A Framework for Source Evaluation

For every source, consider the following checklist to guide your evaluation process:

| **Question**                | **Example**                                         |
|-----------------------------|----------------------------------------------------|
| **Who will we collaborate with?** | Engineering (Payments team)                      |
| **How will the data be used?**     | Financial reporting and quarterly planning       |
| **Are there multiple sources?**    | Yes                                              |
| **What’s the format?**             | Semi-structured APIs (e.g., Stripe and internal) |
| **What’s the frequency?**          | Hourly                                           |
| **What’s the volume?**             | ~1,000 new rows/day; ~100,000 existing records   |
| **What processing is required?**   | Cleaning (e.g., renaming columns) and enrichment |
| **How will the data be stored?**   | Staged in Delta tables via Databricks            |

While this process may feel repetitive, laying this groundwork will save significant time and effort in the long run, ensuring that each source is managed efficiently and aligns with business goals.

### The Destination: Analyzing Where Data Goes

In most scenarios, data engineers are tasked with building pipelines for existing systems where the storage technology has already been selected. While this guide does not focus on selecting storage technologies, destinations play a crucial role in determining the overall value generated by a data system. Evaluating destinations should follow a structured approach, similar to how sources are assessed, though the process is often less complex since there are typically fewer destinations than sources.


### Key Considerations for Destinations

#### **1. Stakeholder-Centric Design**
Destinations are unique in that they are directly tied to stakeholder needs. They often fuel business intelligence (BI), analytics, and AI/ML applications or indirectly support these through staged data. Evaluating destinations should prioritize stakeholder requirements while balancing engineering goals. Crafting the most suitable solution may involve compromises, as not every problem has a straightforward technological answer.

#### **2. Staging Data**
A recommended approach for data ingestion is staging data in a data lake before transferring it to a data warehouse. Cloud storage solutions like Amazon S3, Google Cloud Storage, or Azure Blob Storage serve as staging areas. Modern "lakehouse" architectures, such as Delta Lake, combine the scalability of lakes with warehouse-like capabilities. These systems enhance performance, reliability, and functionality while incorporating metadata layers for features like time travel and ACID compliance.

Formats such as Delta Lake, Apache Iceberg, or Apache Hudi—built on Parquet—offer efficient, metadata-centric staging, enabling advanced operations like historical backfilling and schema evolution. Incorporating medallion architecture further enhances data quality by processing data through layers (bronze, silver, gold) and preserving historical records.

#### **3. OLAP vs. OLTP Systems**
When selecting a data warehouse, the primary decision revolves around choosing an OLAP (Online Analytical Processing) or OLTP (Online Transaction Processing) system:

- **OLAP Systems**: Designed for large-scale data analysis with distributed, columnar architecture. Common options include Amazon Redshift, Google BigQuery, and Snowflake. These systems are often used by analytics and data science teams for their speed and scalability.

- **OLTP Systems**: Optimized for high-transaction workloads with row-oriented architecture. Examples include traditional databases like PostgreSQL and MySQL.

The choice between OLAP and OLTP should align with your organization’s technical stack, team requirements, and budget.


### Practical Factors to Evaluate

#### **1. Integration with Existing Systems**
Consider the broader tech stack. For example, if your organization primarily uses Amazon services, Redshift may offer a more seamless and cost-effective integration than Google BigQuery. Similarly, the Databricks ecosystem provides end-to-end solutions, including storage, governance, and compute capabilities.

#### **2. Functionality**
Different platforms excel in different areas. For instance, BigQuery is highly effective for semi-structured data and machine learning, while Delta Lake supports robust data versioning and governance. Evaluate platform-specific strengths based on your team’s workflows.

#### **3. Cost**
Storage and compute costs vary widely between platforms. Understanding how pricing scales and how to optimize usage (e.g., intelligent partitioning or filtering) is essential. Lakehouse architectures can reduce redundancy and save costs compared to maintaining separate lakes and warehouses.

#### **4. Change Data Capture (CDC)**
CDC is a design pattern that tracks changes in source databases and updates downstream systems incrementally. This pattern is essential for real-time analytics and simplifies processes like building Slowly Changing Dimension (SCD) tables. Solutions with native CDC support, such as Delta Live Tables on Databricks, streamline these tasks.


### A Destination Evaluation Checklist

Here’s a checklist to guide the selection and evaluation of a destination:

| **Question**                | **Example**                                                                                      |
|-----------------------------|--------------------------------------------------------------------------------------------------|
| **Who will we collaborate with?** | Human Resources                                                                           |
| **How will the data be used?**     | Analyze how employee tenure impacts organizational performance                           |
| **Are there multiple destinations?** | Data staged in Delta Lake; final outputs created in Databricks SQL                      |
| **What’s the format?**             | Parquet for staging; structured/semi-structured for analytics                           |
| **What’s the frequency?**          | Batch                                                                                   |
| **What’s the volume?**             | ~1,000 new rows/day; ~100,000 existing records                                          |
| **What processing is required?**   | Downstream transformations with DLT; ML and AI workflows in Spark                       |
| **How will the data be stored?**   | Hybrid of Databricks SQL and Delta Lake external tables                                 |


### Final Thoughts

Laying the groundwork for analyzing destinations ensures that pipelines are built to meet stakeholder needs while adhering to technical and business constraints. While no single approach fits all use cases, this structured evaluation process provides clarity and efficiency for addressing the complexities of modern data systems.

### Key Factors in Data Ingestion

This section highlights essential characteristics to evaluate during the data ingestion process. While not an exhaustive list and dependent on context, factors such as data frequency, volume, format, and required processing consistently stand out as critical considerations.

### Data Frequency: Determining the Right Approach

When considering the frequency of data ingestion, the first step is to assess whether the dataset is bounded (has a defined start and end) or unbounded (continuously generated). This distinction, combined with business needs, guides the choice between batch and streaming ingestion methods. As shown in Figure 1-3, batch processing aggregates data into groups before processing, while streaming handles events in near real-time as they occur.


### Ingestion Methods: Batch, Micro-Batch, and Streaming

#### **Batch Processing**
Batch processing involves dividing data into chunks and processing them sequentially, either from bounded datasets or by periodically handling unbounded datasets. It’s a reliable, straightforward method often used for lower-frequency data needs, such as daily updates.

#### **Micro-Batching**
Micro-batching operates similarly to batch processing but at much shorter intervals. For example, instead of daily batches, it processes data every hour or minute. While the line between micro-batch and streaming can blur—such as with systems operating on millisecond latencies—for simplicity, low-latency micro-batch solutions are often categorized as streaming. Tools like Spark Structured Streaming fall into this category, offering near real-time capabilities.

#### **Streaming**
Streaming continuously processes data as it is generated, suitable for both bounded and unbounded datasets. While we won’t cover streaming in depth here, resources like *Streaming 101* and *Fundamentals of Data Engineering* provide comprehensive overviews.


### Streaming Techniques and Tools

#### **Windowing**
Windowing segments streaming data into finite chunks for processing. Common methods include:

- **Fixed Windows**: Processes data in fixed, non-overlapping intervals.
- **Sliding Windows**: Similar to fixed windows but with overlapping intervals.
- **Sessions**: Creates dynamic windows based on event activity, where the “window” adjusts to periods of inactivity.

#### **Message Services**
Message services facilitate the transportation of streaming data and vary in complexity and architecture. Notable examples include:

- **Apache Kafka**: A distributed streaming platform known for high throughput and scalability but with a steep learning curve.
- **Redpanda**: A Kafka alternative with simplified configuration and comparable performance, built in C++.
- **Google Pub/Sub**: A fully managed, dynamic messaging service that simplifies scalability and minimizes maintenance overhead.
- **Amazon Kinesis**: An AWS service offering real-time data processing with seamless integration into the AWS ecosystem.

#### **Stream Processing Engines**
Stream processing tools analyze and act on real-time data. Popular options include:

- **Apache Flink**: Offers continuous processing with low latency and high fault tolerance.
- **Apache Spark Structured Streaming**: Brings the power of Spark to streaming, leveraging familiar DataFrame APIs.
- **Apache Kafka Streams**: Built on Kafka, it enables stateful stream processing but is Java-specific.


### Simplifying Streaming

Advancements in stream processing tools have made the landscape more accessible:

- **Managed Platforms**: Platforms like Databricks simplify streaming with tools like Delta Live Tables (DLT) and Spark Streaming jobs.
- **Python Libraries**: Tools like Confluent Kafka and Bytewax provide more user-friendly interfaces for stream processing, bridging gaps in traditional workflows.

Emerging tools like Apache Beam and Estuary Flow further unify stream processing capabilities, offering innovative ways to handle real-time data.


### Balancing Real-Time and Right-Time Processing

While real-time data ingestion may seem ideal, it’s crucial to adopt a “right-time” approach. Striving for sub-second latency can significantly increase costs and complexity. For many use cases, transitioning from daily or semi-daily updates to hourly or sub-hourly processing strikes a balance between timeliness and efficiency.

By understanding the trade-offs between batch, micro-batch, and streaming solutions, data engineers can align their ingestion strategies with business needs, ensuring robust and scalable data pipelines.

### Payload: Understanding Data Characteristics

The term *payload* refers to the core message being transmitted, along with any accompanying metadata or headers that facilitate routing, processing, or formatting. Payloads come in a wide array of shapes and structures, depending on the data's source and intended use. In this section, we’ll explore key characteristics that define payloads and impact data ingestion strategies.


### Volume

The volume of data being ingested plays a critical role in shaping the scalability and efficiency of both processing and storage solutions. Key considerations include:

- **Cost**: Higher data volumes can significantly increase costs for storage and compute resources. Align your budget with the project’s needs, factoring in the costs of staging and storing data in warehouses, lakes, or lakehouses.
- **Latency**: Real-time or near-real-time ingestion demands greater resource efficiency to handle sudden spikes in volume without delays.
- **Throughput and Scalability**: Choose ingestion tools capable of processing large data volumes without causing bottlenecks.
- **Retention**: For high-volume datasets, establish retention policies to archive or migrate old data to cost-effective, long-term storage while maintaining accessibility and security.

Compressed formats like Apache Parquet or Avro are recommended for managing large datasets, as they offer advantages in storage efficiency and schema evolution.


### Structure and Shape

Data comes in various forms, ranging from structured formats with fixed schemas to unstructured, free-form data. Each type presents unique challenges and opportunities:

#### **Unstructured Data**
Unstructured data lacks predefined schemas and includes formats like text, audio, video, and images. Its growing value in AI and machine learning applications underscores the importance of understanding its potential use before ingestion. Metrics such as text length or file size often help define its shape.

#### **Semi-Structured Data**
Semi-structured data, like JSON or XML, lies between structured and unstructured formats. These payloads may require additional processing but offer flexibility for schema changes. For instance, JSON payloads can be parsed and transformed in tools like Google BigQuery to generate tabular data for analysis.

#### **Structured Data**
Structured data adheres to fixed schemas, making it the gold standard for querying and analysis. SQL remains the dominant tool for working with structured data, as it supports efficient querying, materialization, and incremental processing.


### Format

The format of the data significantly impacts processing requirements and the choice of tools. Popular formats like JSON and CSV are widely supported, but specialized or legacy formats may introduce complexities. Opting for common, well-documented formats can streamline integration and troubleshooting. However, handling unique or outdated formats is a frequent challenge in data engineering.


### Variety

With multiple data sources comes a variety of payloads, each with its own schema, format, and processing needs. The ingestion solution must handle this diversity while accommodating schema changes and ensuring consistency. Failure to manage variety effectively can lead to inefficiencies, bottlenecks, and compromised data quality.


By understanding these core characteristics—volume, structure, format, and variety—you can design data pipelines that are both robust and adaptable to changing business and technical requirements. This foundation ensures smooth data ingestion and paves the way for scalable and efficient analytics.

### Selecting the Right Solution

The ideal tools for your team will be those that align with your specific sources and targets while addressing your organization’s unique data requirements. Because every organization’s needs are different, the selection process will be highly contextual. However, leveraging the expertise of mentors, colleagues, and industry professionals can provide invaluable guidance. While conferences might be expensive, they often deliver insights that justify their cost. For budget-conscious teams, engaging with data communities on platforms like Slack, LinkedIn, or through industry newsletters can be a cost-effective alternative.

When evaluating data ingestion solutions, it’s helpful to consider both **general** and **solution-specific** factors:


### General Considerations

These factors apply universally to all tools under consideration:

- **Extensibility**: Can the tool evolve with your needs and handle additional use cases as they arise?
- **Cost to Build**: What is the initial investment required for setup and integration?
- **Cost to Maintain**: How much effort and expense will ongoing support and updates require?
- **Cost to Switch**: How difficult and expensive would it be to migrate to another solution in the future?
- **System-Level Concerns**: Consider compatibility with existing systems, scalability, and the tool’s ability to integrate into your current tech stack.


### Solution-Specific Considerations

These factors depend on the type of tool or approach, which typically falls into one of two categories:

#### **Declarative Solutions**
Declarative tools focus on defining desired outcomes. For example:
- “Using the AWS Glue UI to configure a crawler that processes data automatically.”
- “Setting up a new Airbyte connection via the user interface.”

These tools simplify the user experience, often requiring minimal coding, but may be less flexible for complex or highly customized workflows.

#### **Imperative Solutions**
Imperative tools emphasize specifying the actions required to achieve a goal. For instance:
- “Writing a Lambda function to call the Stripe API, process the data, and load it into Snowflake incrementally.”

These solutions provide greater control and flexibility but often require more technical expertise and manual effort.



### Weighing the Options

Both declarative and imperative approaches come with their own advantages and drawbacks. Declarative tools are typically easier to use and faster to deploy, while imperative solutions offer customization and precision. The best approach will depend on your specific requirements, team expertise, and long-term objectives.

To navigate the selection process effectively, we recommend balancing these considerations with your organization’s context and seeking input from experienced professionals. This will ensure your data integration strategy aligns with both current needs and future scalability.

### Selecting the Right Solution

The ideal tools for your team will be those that align with your specific sources and targets while addressing your organization’s unique data requirements. Because every organization’s needs are different, the selection process will be highly contextual. However, leveraging the expertise of mentors, colleagues, and industry professionals can provide invaluable guidance. While conferences might be expensive, they often deliver insights that justify their cost. For budget-conscious teams, engaging with data communities on platforms like Slack, LinkedIn, or through industry newsletters can be a cost-effective alternative.

When evaluating data ingestion solutions, it’s helpful to consider both **general** and **solution-specific** factors:


### General Considerations

These factors apply universally to all tools under consideration:

- **Extensibility**: Can the tool evolve with your needs and handle additional use cases as they arise?
- **Cost to Build**: What is the initial investment required for setup and integration?
- **Cost to Maintain**: How much effort and expense will ongoing support and updates require?
- **Cost to Switch**: How difficult and expensive would it be to migrate to another solution in the future?
- **System-Level Concerns**: Consider compatibility with existing systems, scalability, and the tool’s ability to integrate into your current tech stack.


### Solution-Specific Considerations

These factors depend on the type of tool or approach, which typically falls into one of two categories:

#### **Declarative Solutions**
Declarative tools focus on defining desired outcomes. For example:
- “Using the AWS Glue UI to configure a crawler that processes data automatically.”
- “Setting up a new Airbyte connection via the user interface.”

These tools simplify the user experience, often requiring minimal coding, but may be less flexible for complex or highly customized workflows.

#### **Imperative Solutions**
Imperative tools emphasize specifying the actions required to achieve a goal. For instance:
- “Writing a Lambda function to call the Stripe API, process the data, and load it into Snowflake incrementally.”

These solutions provide greater control and flexibility but often require more technical expertise and manual effort.


### Weighing the Options

Both declarative and imperative approaches come with their own advantages and drawbacks. Declarative tools are typically easier to use and faster to deploy, while imperative solutions offer customization and precision. The best approach will depend on your specific requirements, team expertise, and long-term objectives.

To navigate the selection process effectively, we recommend balancing these considerations with your organization’s context and seeking input from experienced professionals. This will ensure your data integration strategy aligns with both current needs and future scalability.

### Declarative Solutions

Declarative solutions focus on defining desired outcomes rather than detailing the steps to achieve them. These solutions can be categorized into legacy, modern, or native, depending on how well they align with modern data stack (MDS) principles like infrastructure-as-code (IaC), the DRY (don’t repeat yourself) philosophy, and other software engineering best practices. Native solutions stand apart by being directly integrated with cloud platforms.


### Types of Declarative Solutions

#### **Legacy Tools**
Examples: Talend, WhereScape, Pentaho.  
Legacy tools come with extensive connectors, a rich support community, and a long history in the industry. However, many fail to keep pace with the rapidly evolving demands of modern data infrastructure. Unless your organization has specific legacy dependencies, exploring more current alternatives is often a better choice.

#### **Modern Tools**
Examples: Fivetran, Stitch, Airbyte.  
Modern declarative tools leverage advanced technology to connect sources and targets seamlessly. Built with the modern data stack in mind, they emphasize simplicity, scalability, and integration with cutting-edge technologies.

#### **Native Solutions**
Examples: Databricks.  
Unlike legacy or modern tools, native solutions are deeply integrated into cloud platforms and often provide out-of-the-box support for data ingestion. For example, Databricks can ingest data directly from cloud storage or messaging systems using simple commands:

```sql
CREATE STREAMING TABLE raw_data 
AS SELECT * 
FROM cloud_files("/raw_data", "json");

CREATE STREAMING TABLE clean_data 
AS SELECT SUM(profit) 
FROM raw_data;
```

Native solutions are particularly appealing for teams already invested in specific cloud ecosystems, offering cost efficiency and seamless integration.


### Key Considerations for Declarative Solutions

#### **Cost to Build and Maintain**
Declarative solutions minimize the need for hands-on engineering effort. Pre-built connectors are developed and maintained by dedicated teams, reducing the technical burden on your organization. Paid options often include client support and guidance, providing valuable insights into optimizing workflows and resolving ambiguities.

#### **Extensibility**
The ability to extend or customize solutions is critical. Tools like Airbyte or Fivetran support creating custom connectors, enabling you to address unique needs without waiting for vendor updates. However, limited extensibility can lead to inefficiencies and additional costs if multiple tools are required to cover gaps in functionality.

#### **Cost to Switch**
Switching between declarative tools can be challenging due to proprietary frameworks and change data capture (CDC) methods. Migration costs should be considered when choosing a solution, especially for long-term scalability. While sticking with a vendor may be unavoidable in some cases, understanding the trade-offs is essential.


### Final Thoughts

Choosing the right declarative solution depends on your organization’s context, technical ecosystem, and scalability needs. Modern and native tools often provide the flexibility and cost-efficiency required for today’s dynamic data environments, while legacy tools may serve specific use cases. Balancing extensibility, maintenance costs, and migration risks will help you select a solution that aligns with both immediate goals and long-term growth.

### Imperative Solutions

Imperative data ingestion approaches rely on custom, programmatic workflows tailored to specific business requirements. Examples include in-house Singer taps, Lambda functions, Apache Beam templates, or jobs orchestrated with systems like Apache Airflow.

These solutions are typically favored by larger organizations with significant resources, as maintaining and scaling custom ingestion frameworks often requires a dedicated team of data engineers. The key strength of imperative solutions lies in their extensibility and flexibility, allowing for precise tailoring to unique business needs.


### Key Advantages and Challenges

#### **Extensibility**
Imperative solutions are inherently custom, enabling each integration to be designed to meet exact specifications. Unlike standard tools, which may involve compromises, imperative workflows allow full control over every aspect of the data pipeline. However, this level of customization comes at a high cost, requiring substantial time and resources to develop and maintain. Without a large, dedicated team, achieving the desired flexibility can become prohibitively expensive.

#### **Cost to Build and Maintain**
Developing and maintaining imperative solutions demands significant engineering effort. For instance, managing a complex data source like Stripe, with its intricate entity relationships, can be highly time-intensive. The dynamic nature of data—such as schema changes or API deprecations—adds further complexity, especially when scaling to multiple sources.

To ensure resilience, imperative systems should adhere to software engineering best practices, such as modular design, testability, and clear documentation. Neglecting these principles can hinder system recovery and scalability, affecting overall business performance. As a result, fully imperative solutions are best suited for organizations with robust data engineering capabilities.

#### **Cost to Switch**
Migrating from one imperative solution to another can pose challenges due to potential incompatibilities in formats and workflows. However, frameworks based on common standards, like Singer, may offer greater compatibility and ease of transition compared to purely declarative tools like Fivetran or Airbyte.


### Final Thoughts

Imperative solutions are powerful for handling complex and bespoke data ingestion requirements. They provide unparalleled flexibility but demand substantial resources for development and maintenance. For organizations with the capacity to support a dedicated data engineering team, the investment in an imperative approach can yield a highly extensible and scalable solution. However, for those with limited resources, balancing customization with practical limitations may require exploring hybrid or declarative alternatives.

### Hybrid Solutions

Finding the right balance in data integration often leads to adopting a hybrid approach. This strategy might include using tools like Fivetran for most integration tasks while developing in-house solutions for unique or unsupported data sources. Alternatively, platforms like Airbyte or Meltano can be enhanced with custom components tailored to specific needs.


### Benefits of Hybrid Approaches

#### **Community Contributions and Open Source**
Hybrid models benefit from the extensive community support associated with open-source projects. Tools like Airbyte and Singer taps exemplify this, offering community-driven connectors that continue to evolve. Airbyte’s influence has even driven positive changes in the industry, such as competitors like Fivetran introducing free tiers. Exploring emerging tools, such as the open-source library *dlt* (data load tool, distinct from Delta Live Tables), can further expand your integration capabilities.

#### **Flexibility and Efficiency**
A hybrid approach allows you to blend the reliability and simplicity of established tools with the customization needed for complex or niche use cases. It’s akin to choosing the right vehicle for the job—not every situation requires the speed and complexity of a Formula One car. For most tasks, a dependable, versatile option suffices, but having access to high-performance solutions ensures readiness for specialized challenges.


### A Practical Strategy

By leveraging established platforms for routine integrations and developing custom solutions only when necessary, you can optimize both resource allocation and performance. This approach minimizes development time while maintaining the flexibility to address unique business requirements.

Hybrid solutions also encourage innovation and collaboration through open-source contributions, fostering continuous improvement and adaptability in a rapidly changing data landscape.
