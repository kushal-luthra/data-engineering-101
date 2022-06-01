
## Row Vs Column File Format
When you are designing big data solution, one fundamental que is – “how data will be stored” <br>
It involves taking into consideration 2 things – <br>
•	File formats <br>
•	Compression techniques <br>

Why do we need different File Formats? <br>
•	To save storage <br>
•	To do fast processing <br> 
•	To have less time in I/O operations – since we are dealing in big data, I/O operations are a big bottleneck, and so spending as less time in this area as possible. <br>

Our file formats help us in all 3 above if we go with right file format. <br>

There are a lot of choices available on file formats.  <br>
Below are key aspects for deciding a file format -  <br>
1.	Faster reads. <br>
2.	Faster writes. <br>
3.	Splittable - Some are designed in such a way that they are splittable – if a file is splittable, we can do parallel processing – in big data solutions, we consider only splittable file formats. <br>
4.	Schema evolution support – some of the file formats support schema evolution -  i.e. to facilitate change in input data by allowing schema changes. <br>
5.	Advanced compression techniques <br>
6.	Most compatible platform - some work well with hive, some with spark, etc <br>

All the file formats have been divided into 2 broad categories – <br>
•	Row based <br>
•	Column based <br>

Row based – <br>
Here data is stored row-by-row. <br>
![img.png](images/img.png) <br>

At a time, whole record is saved.  <br>If a new record comes, it gets appended at the end. <br>
So, writing a record is very easy coz you simply append at the end.

Now let’s talk about reading –  <br>
While write is easy, but in order to get subset of columns, it has to read entire record. That is, performance is degraded when it comes to read. <br>
In data warehousing, wherein we scan specific set of records, row-based formats is not suggested. <br>

Regarding Compression of row-based file– <br>
Since data is stored record by record, so different data types are present together, next to each other. <br>
That means, compression is not as efficient as it could be. <br>

Column based file format – <br>
All column values are stored together. <br>
![img_1.png](images/img_1.png) <br>

For Reading data in column-based file format – it allows us to skip data and read only relevant columns. <br>
Column based file format is suggested for data warehouse based query system. <br>

For write on column-based file, it is time consuming as you need to write on multiple places. <br>

Regarding Compression of column-based file– <br>
Since data is stored column by column, so same data types are present together, next to each other. <br>
That means, compression can be applied efficiently for each data type. <br>
![img_2.png](images/img_2.png) <br>

To summarize – <br>
If you write once but read multiple times, go for column-based file format. <br>
If you read once, but write multiple times, go for row-based file format. <br>

| Category              | row based	     | column based   |
|-----------------------|----------------|----------------|
| read                  | slower reads   | faster reads   |
| writes                | 	faster writes | 	slower writes |
| compression| 	poor	| good           |


