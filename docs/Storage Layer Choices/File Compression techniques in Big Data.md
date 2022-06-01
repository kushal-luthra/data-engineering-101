## File Compression Techniques
These techniques are common to Hadoop ecosystem, not just Hive. <br>

Why need compression? <br>
•	Helps reduce storage especially when it comes to data being replicated across various nodes. <br>
•	Helps us process data faster as size of data is less. <br>
•	Since data is compressed, so I/O costs is less – A major overhead in processing large amounts of data is disk and network I/O, reducing the amount of data that needs to be read and written to disk can significantly decrease overall processing time. This includes compression of source data, but also the intermediate data generated as part of data processing. <br> 

Compression and Decompression comes with some cost in terms of time taken to compress and decompress. <br>
But when we compare I/O gains, we can actually ignore this additional time to compress-decompress. <br>

Important Compression Techniques – <br>
1.	Snappy <br>
2.	Lzo <br>
3.	Gzip <br>
4.	Bzip2 <br>

Some of the compression codecs are optimized for storage – they bring down size drastically. But this takes time. <br>
Some of compression codecs are optimized for speed – compression done quickly, but not efficiently. <br>

So trade-off is that –  <br>
•	if we want more compression ratio, we have to spend more time in compression. <br>
•	If we want faster compression, we spend less time in compression. <br>

#### Snappy
Snappy is a very fast compression. <br> 
However, in terms of compression ratio, it is not that efficient. <br>
But in most production scenarios, snappy is used as it provides a fine balance between speed and compression efficiency. <br>
So, snappy is optimized for speed, not storage. <br>

##### Splittablity in compression techniques
Although compression can greatly optimize processing performance, not all compression formats supported on Hadoop are splittable. <br> 
Because the MapReduce framework splits data for input to multiple tasks, having a non splittable compression format is an impediment to efficient processing. <br> 
If files cannot be split, that means the entire file needs to be passed to a single MapReduce task, eliminating the advantages of parallelism and data locality that Hadoop provides. <br> 
For this reason, splitability is a major consideration in choosing a compression format as well as file format. <br>


Snappy by default is not splittable – so if we use non splittable file formats like JSON and XML, snappy won’t give splittable output. <br>
**Is this a big concern?** No – because in production scenarios, we hardly use JSON and XML. <br>
In production scenarios, we use container-based formats like Avro, Parquet, Orc – which are splittable by their structure and no need for compression technique to handle this aspect. <br>

So, Snappy is intended to be used with a container format like Avro, Orc, Parquet since it’s not inherently splittable. <br>


#### Lzo
LZO is similar to Snappy in that it’s optimized for speed as opposed to size. <br> 
Unlike Snappy, LZO compressed files are splittable, but this requires an additional indexing step. <br> 
This makes LZO a good choice for things like plain-text files (like json, text and xml files) that are not being stored as part of a container format. <br> 
It should also be noted that LZO’s license prevents it from being distributed with Hadoop and requires a separate install, unlike Snappy, which can be distributed with Hadoop. <br>

But snappy is fastest among all compression techniques. <br>

#### Gzip
•	Gzip provides very good compression performance (on average, about 2.5times the compression that’d be offered by Snappy). <br> 
•	But in terms of processing speed its slow.  <br>
•	Gzip is also not splittable, so it should be used with a container format. <br> 
•	Note that one reason Gzip is sometimes slower than Snappy for processing is that Gzip compressed files take up fewer blocks, so fewer tasks are required for processing the same data. For this reason, using smaller blocks with Gzip can lead to better performance. <br>
•	Eg – <br>
o	1 gb file – split into 8 blocks. <br>
o	After gzip compression, we get 200 mb file – 2 block – so number of blocks coming down, which reduces parallelism. <br>
o	Solution – reduce block size to say, 50Mb, leading to 200mb file split into 4 blocks, and so parallelism doubles. <br>

#### Bzip2
•	Bzip2 provides excellent compression performance, but can be significantly slower than other compression codecs such as Snappy in terms of processing performance. <br> 
•	Unlike Snappy and Gzip, bzip2 is inherently splittable.  <br>
•	In the examples we have seen, bzip2 will normally compress around 9% better than GZip, in terms of storage space. <br> 
•	However, this extra compression comes with a significant read/write performance cost. This performance difference will vary with different machines, but in general bzip2 is about 10 times slower than GZip. <br> 
•	For this reason, it’s not an ideal codec for Hadoop storage, unless your primary need is reducing the storage footprint. One example of such a use case would be using Hadoop mainly for active archival purposes. <br>

| Codec	               | Splittable     | 	compression     | 	speed   |
|----------------------|----------------|------------------|----------|
| snappy| 	N| 	low| 	Highest |
|lzo| 	Y             | 	low| 	high    |        
|gzip| 	N             | 	High	| slow     |      
|bzip2| 	Y             | 	Highest	| Slowest  | 
