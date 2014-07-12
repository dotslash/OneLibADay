Apache commons IO
==
+ [CharSets](#CharSets)
+ [FileSystemUtils](#FileSystemUtils)
+ [FileNameUtils](#FileNameUtils)
+ [FileUtils](#FileUtils)
+ [IOUtils](#IOUtils)
+ [Filters](#Filters)

###CharSets
Same as CharSets provided in guava. Has static references for standard char sets. For general info on charsets read [this](http://www.joelonsoftware.com/articles/Unicode.html)

###FileSystemUtils
Has a simple function to get the free space in file system (takes path as parameter)

###FileNameUtils
Has very handy functions for basic checks and opeations on file names (comparing file names of File, getting the file  extension, full path, normalised path etc)

###FileUtils
This class more or less does everything you one needs to do with files

**_Modifying/Creating files_**
+ copyDirectory, copyFile : copy dirs/files to a specified destination 
+ copyFileToDirectory, copyUrlToFile, copyInputStreamToFile : similar as above
+ deleteDirectory, forceDelete : delete dir/ file respectively . deleteQuitely can be used if exceptions can be ignored
+ forceDeleteOnExit : delete a file when JVM exits
+ forceMkDir : Makes a directory, including any necessary but nonexistent parent directories.
+ writeByteArrayToFile, writeStringToFile, writeLines : writing to files 
+ moveDirectory, moveFile, moveFileToDirectory, moveToDirectory : to move files/dirs to specidifed locations

**_Read only info about files_**
+ iterateFiles, iterateFilesAndDirs : to iterate over a files in directory
+ Multipliers for conversion between file sizes (ex, ONE\_GB\_BI, ONE_GB for number of bytes in GB)
+ byteCountToDisplaySize (ex : 1100 -> 1 KB, 1900 -> 1 KB , only integral multiple of sizes are returned)
+ checksum : returns as a long, the checksum of the file passed a param
+ contentEquals : compare contents of 2 files
+ directoryContains : checks if a dir contains a file
+ getTempDirectory, getTempDirectoryPath : get system temp directory as File/String respectively
+ getUserDirectory, getUserDirectoryPath
+ isFileNewer, isFileOlder : check the freshness of file. Freshness can be checked by providing a data object or timelimit in ms
+ sizeOf, sizeOfDirectory : size of File/Directory(recursive)
+ openInputStream, openOutputStream (append/overwrite mode) : takes a file as input and opens it as input/output stream
+ readFileAsString (encoding charset can be specified): Read the file into a single string. Not to be used with large files.
+ readLines (encoding charset can be specified) : read file into a list of Strings
+ toFile : converts URL as file (has an array counterpart toFiles)


###IOUtils
* closeQuietly : Close inputStrem/OutputStrem/Reader/Writer. not much useful from java 8
* contentEquals : check if content in 2 readers/ input streams is equal
* copy : takes source, destination, encoding and does the copy (copyLarge for large streams/readers)
* lineIterator : provides a String iterator for inputStrem/reader. Use readLines to get a List of Strings
* toByteArray, toCharArray, toInputStream, toString : return to content in stream/reader
* read, readFully, write , skip, skipFully : functions to read/write/skip reading from/to Streams/Readers/Writers
* writeLines : write from a Collection of Strings to outputstream/writer
