# Simple-Indexing
Simple Indexing on a text file

This program creates a simple index on a text file.  It will read a text file containing data and build the index, treating the first n columns as the key, where n will be specified by the user. There are two data sets for testing : The first one (TestDataA.txt) is indexed on the first 15 bytes.  The second one (TestDataB.txt) is indexed on the first 4 bytes.
For duplicates, which might be the case when we indexed the larger of the two test files on just one column, I have put them into the index with their correct record pointers.

This program is run from the command line, which takes four parameters:

    1.	The first parameter is one of two strings: The string –c indicates that you should create an index.  The string –l (lower-case L) indicates you should list the file using the index.
    2.	The name of an input text file.
    3.	The name of the output file.
    4.	The number of characters, starting with the first one, that comprise the key.

For example, (let's call our program INDEX) the following command line would create an index on a file called DATA.TXT, produce an output file DATA.IDX, with a key length of 4:
> INDEX -c DATA.TXT DATA.IDX 4

The output file must contain the key and the 8-byte binary offset of the record within the data file.  Thus records in the output file are fixed-length, containing the number of bytes in the key plus 8 bytes of pointer, or offset.  In the example, each index record would be 12 bytes long.  The output file is sorted for readability.
For example, if the input file is this:

    //Indexing starts from 0 index
    AAAATest data 1           //Length is 15	//slice is 0 to 14
    ZZZZAnother test record   //Length is 23	//slice is 15 to (15+23)
    CCCCThird test record     //Length is 21	//slice is 38 to (38+21)

The output file would look like this, with the brackets meaning that what is in them is a binary number, not decimal digits.  That is, there are no actual brackets in the file.  Each record, in this case, is exactly 12 bytes long, with 4 for the characters of the key and 8 for a long integer.

    AAAA[0]
    CCCC[39]
    ZZZZ[15]

Thus the first data file record is in position 0 in the input file, the second one with key CCCC is in position 39, and the third one with key ZZZZ, the second input record, is in position 15.

The second way to run this program, to list the contents of a file using the index, takes the following parameters:

    1.	The string –l (lower-case L) for “list”.
    2.	The name of the associated data file.
    3.	The name of an index file.
    4.	The length of the key.

This usage lists the records in order using the data from the index.  Thus the program reads a record from the index file, use its “pointer” to read a record from the data file, and displays it. Since the data records are variable length, we read in 1000 bytes, knowing where the record starts, and look for a newline character.  Then display the record.

This command lists all of the records in alphabetical order:

> INDEX -l DATA.TXT DATA.IDX 4

Given the above data, you would see:

    AAAATest data 1
    CCCCThird test record
    ZZZZAnother test record

If the key length is greater than a length of string we can append 0 to the starting to make it work.
