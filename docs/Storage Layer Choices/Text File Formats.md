Text File format
Not used in production.
Types include –
•	Csv – raw files
•	Xml, Json – structured text data - These are files with some structure attached to them

These are human readable.

CSV file –
•	Advantage –
o	Data is stored in a human readable way
•	Drawbacks –
o	Everything is stored in a text form – so even an integer is stored as a text.
Issue – more bytes used to store an integer than needed as it is stored as a string.
Eg – val = 4561987.
If val = INT – then takes only 4 bytes to get stored
If val = STRING – then each position takes 2 bytes, so total 14 bytes used.

To conclude – text file takes a lot of storage since everything is stored as string.
o	When data is stored as string, and you want to read it and use it as integer, this type of conversion is time consuming. 
So, processing on text files can be very slow.
o	Further, I/O operations also take a lot of time – coz text file take lot of space as compared to other file formats, and transferring such data across the network will be I/O intensive process.


XML, JSON files
Disadvantage –
•	Same as CSV –
o	Storage space is large
o	Processing = slow
o	I/O = huge
•	Not splittable – JSON and XML have schema attached to them, due to which they are not splittable. This defeats the whole purpose of using Hadoop as not being splittable means no parallelism possible.

To summarize, in production we don’t use text file formats.
