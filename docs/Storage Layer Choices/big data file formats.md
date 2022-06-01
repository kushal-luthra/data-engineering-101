## Avro, ORC and Parquet

There major ones that work well with Big Data Environment are – <br>
•	Avro <br>
•	Orc <br>
•	Parquet <br>

All of above are – <br>
•	Splittable
•	Agnostic compression -  <br>Any compression can be used with them, without readers having to know the codec. <br> This is possible because codec is stored in the header metadata of the file format. <br>
Reader needn’t know in advance what kind of compression technique is used with these files. <br> Compression codec is kept in file metadata, and whenever reader wants to read the data, he gets to know compression codec from metadata and can easily read the data. <br>

Avro File Formats <br>
1.	It is a row-based file format – data is stored row-by-row. <br>
So, it supports faster writes, but slower reads (when you want to read a subset of columns). <br>
2.	Self-describing schema - Schema is stored in JSON format and this metadata is embedded as part of data itself. <br>
3.	Actual data is stored in Compressed Binary format, which is quite efficient in terms of storage. <br>
4.	Language Neutral - Avro file format is general file format, and supports processing using lot of programming languages like C++, java, Python, Ruby, etc <br>
5.	Schema Evolution – Avro is quite mature in terms of schema evolution as compared to other file formats. Schema evolution includes aspects like – <br>
a.	Adding new columns <br>
b.	Removing old column <br>
c.	Renaming columns, etc <br>
6.	Splittable – file can be divided into parts which can be processed independently. <br>

Avro is a Serialization format. <br>
Serialization – converting data into a form which can be easily transferred over a network and stored in a file system. <br>
Deserialization – reading data and converting it into form which can be read by human.  <br>

In which scenario, Avro is best suited – <br>
•	For storing data in **landing zone of data lake** – why – <br>
o	In lake, chances are different team requiring raw data, and since Avro is language neutral, so different teams can use it. <br>
o	In lake, data is unprocessed, that is no ETL done. For ETL kind of operations, we tend to read whole row and not a subset of it, and here again Avro is suited. <br>
	KL – in dh, we read only subset of data in lake to build warehouse. So, this point is debatable.  <br>
o	Finally, in lake, data schema evolves over time, and so Avro is suited for handling Schema evolution. <br>
•	Avro is typically the format of choice for write-heavy workloads given it is easy to append new rows. <br>

**Avro Vs Orc** <br>

|Category| Avro                                                        | Parquet                                                                     |
|--------|-------------------------------------------------------------|-----------------------------------------------------------------------------|
|format| 	row based                                                  | 	column based                                                               |
|reads	| slower reads	                                               | faster reads - so useful for analytical querying                            |
|writes	| faster writes -  so suited to ETL operations.| 	slower writes                                                              |
|schema evolution| 	it is quite mature wrt Schema Evolution                    | 	Limited schema evolution support - you can add/delete column from the end. |
|complex data types| 	No such support.                                           | 	Provides support for deeply nested data structure.                         |

**Orc Vs Parquet**

|Category| Orc                                                                                                            | Parquet           |
|--------|----------------------------------------------------------------------------------------------------------------|-------------------|
|format| 	column based	                                                                                                 | column based      |
|predicate push down| 	provides predicate push down to push the predicates at storage level - that allows us to query relevant data. | 	 No such support |
|ACID properties| 	Now supports ACID properties to an extent.	                                                                   | No such support.  |
|Compression| 	Better - has lot more encodings used as compared to parquet.	                                                 | good              |
|complex data types	| No such support.                                                                                               |	Provides support for deeply nested data structure.|

![img.png](images/img_3.png)