# Spark SQL

<!-- wp:paragraph -->
<p>Spark SQL is a one of the Spark modules for structured data processing and analysing. Spark provides Spark SQL and also API for execution of SQL queries. Spark SQL can read data from Hive instance, but also from datasets and dataframe. The communication between Spark SQL and execution engine will always result in a dataset or datafrane. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>These formats are interchangeable. So interacting with SQL against result from a different API is possible, respectively. Plugging in the Java JDBD or standard ODBC drivers will also give your SQL interface access to different sources. This unification means that developers can easily switch back and forth between different APIs based on which provides the most natural way to express a given transformation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>With API unification, user can access Spark SQL using Scala <code>spark-shell</code>, using Python <code>pyspark</code> or using R <code>sparkR</code> shell.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="loading-data-comparison-sql-r-python">Loading data - comparison SQL, R, Python</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>From previous blogpost, we will use the  Parquet file (more info on this file format: <a href="https://parquet.apache.org/" target="_blank" rel="noreferrer noopener">here</a>) to see the comparison importing / loading data. The content of the file looks like and is directly available <a rel="noreferrer noopener" href="https://github.com/apache/spark/blob/master/examples/src/main/resources/users.parquet" target="_blank">here</a>:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">avro.schema{"type":"record","name":"User","namespace":"example.avro","fields":[{"name":"name","type":"string"},{"name":"favorite_color","type":["string","null"]},{"name":"favorite_numbers","type":{"type":"array","items":"int"}}]}</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>For Python:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">df = spark.read.parquet("examples/src/main/resources/users.parquet")
(df.write.format("parquet")
    .option("parquet.bloom.filter.enabled#favorite_color", "true")
    .option("parquet.bloom.filter.expected.ndv#favorite_color", "1000000")
    .option("parquet.enable.dictionary", "true")
    .option("parquet.page.write-checksum.enabled", "false")
    .save("users_with_options.parquet"))</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>For R:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"r"} -->
<pre class="wp-block-syntaxhighlighter-code">df &lt;- read.df("examples/src/main/resources/users.parquet", "parquet")
write.parquet(df, "users_with_options.parquet", parquet.bloom.filter.enabled#favorite_color = true, parquet.bloom.filter.expected.ndv#favorite_color = 1000000, parquet.enable.dictionary = true, parquet.page.write-checksum.enabled = false)</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>And using SQL:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"sql"} -->
<pre class="wp-block-syntaxhighlighter-code">CREATE TABLE users_with_options (
  name STRING,
  favorite_color STRING,
  favorite_numbers array&lt;integer>
) USING parquet
OPTIONS (
  `parquet.bloom.filter.enabled#favorite_color` true,
  `parquet.bloom.filter.expected.ndv#favorite_color` 1000000,
  parquet.enable.dictionary true,
  parquet.page.write-checksum.enabled true
)</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>The same file can be directly read from the parquet format, without persisting the content.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For Python:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">df = spark.sql("SELECT * FROM parquet.`examples/src/main/resources/users.parquet`")</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>And for R:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"r"} -->
<pre class="wp-block-syntaxhighlighter-code">df &lt;- sql("SELECT * FROM parquet.`examples/src/main/resources/users.parquet`")</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>Tomorrow we will look into further SQL bucketing and partitioning.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Compete set of code, documents, notebooks, and all of the materials will be available at the Github repository:&nbsp;<a rel="noreferrer noopener" href="https://github.com/tomaztk/Spark-for-data-engineers" target="_blank">https://github.com/tomaztk/Spark-for-data-engineers</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Happy Spark Advent of 2021! 🙂</p>
<!-- /wp:paragraph -->