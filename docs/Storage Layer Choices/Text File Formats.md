## Text File format
Not used in production. <br>
Types include – <br>
•	Csv – raw files <br>
•	Xml, Json – structured text data - These are files with some structure attached to them <br>

These are human readable. <br>

**CSV file** <br>
•	Advantage – <br>
o	Data is stored in a human readable way <br>
•	Drawbacks – <br>
o	Everything is stored in a text form – so even an integer is stored as a text. <br>
Issue – more bytes used to store an integer than needed as it is stored as a string. <br>
Eg – val = 4561987. <br>
If val = INT – then takes only 4 bytes to get stored <br>
If val = STRING – then each position takes 2 bytes, so total 14 bytes used. <br>

To conclude – text file takes a lot of storage since everything is stored as string. <br>
o	When data is stored as string, and you want to read it and use it as integer, this type of conversion is time consuming. <br> 
So, processing on text files can be very slow. <br>
o	Further, I/O operations also take a lot of time – coz text file take lot of space as compared to other file formats, and transferring such data across the network will be I/O intensive process. <br>


**XML, JSON files** <br>
Disadvantage – <br>
•	Same as CSV – <br>
o	Storage space is large <br>
o	Processing = slow <br>
o	I/O = huge <br>
•	Not splittable – JSON and XML have schema attached to them, due to which they are not splittable. This defeats the whole purpose of using Hadoop as not being splittable means no parallelism possible. <br>

To summarize, in production we don’t use text file formats. <br>