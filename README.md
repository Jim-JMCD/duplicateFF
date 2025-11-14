## duplicateFF -  Duplicate File Finder
#### A small Linux app that will search supplied directory(s) and compare files using sha256 checksum and produces CSV reports on duplicate and unique files that can be used with spreadsheets or scripts.      

#### This repository only contains X86-64 excutable.  For the ARM version go to [duplicateFF_for_ARM_aarch64](https://github.com/Jim-JMCD/DuplicateFF_for_ARM_aarch64)

*duplicateFF* created from the bash script [duplicate_FF](https://github.com/Jim-JMCD/duplicate-File-Finder) (private Github repository) using shc. 

### Dependency
This requires a Linux bash environment to run. Will run in Microsoft WSL2(Linux), duplicateFF will not run in MSYS2, Gitbash and Cygwin environmants
An exceutable created from the *shc* utility always requires bash. More : [Github shc](https://github.com/neurobin/shc)   
                                                                              
### Notes

* It requires a least one directory to search many directories can be compared.  
* Searches can be only the current directory or all subdiorectories, or any subdirectory level that is required.
* Files names and maximum file sizes can be used as filters to narrow searches and save time.
* Reports are CSV format which can be imported into a spreadsheet. 

Directories with name '$RECYCLE.BIN' are ignored. Linux sees some MS Windows directories as executable only, a user or app can go into them but can't read them. If the Windows "executable only" directory is user accessible, it be easily corrected by respondiong to "You don't currently have permission to access this folder".  If the directory is not user accessible then its probably a system directory that is not worth checking for dulicate files. 

Linux hidden files are ignored, hidden directories are processed. 

To move or remove duplicate files there are some suggestions in [How to delete duplicate files](https://github.com/Jim-JMCD/duplicateFF/blob/main/How%20to%20delete%20duplicate%20files.md) using the **_duplicateFF_KeepCopy_** and **_duplicateFF_RemoveALL_** scripts in this repository.

__Foreign Language Characters:__ If Microsoft Excel is the default application for CVS files, Microsoft Excel will not display foreign language characters correctly.  To fix this problem, change the default app for CSV files to Notepad or Wordpad and manually __import__ into excel, alternatively rename *.csv file to a *.txt and manually import into Microsoft Excel. Do not attempt to use __Open with__ and select Excel, it always has to be an __Import__.   
  
__________________________________________________________________________________________

### Usage 
#### duplicateFF -l \<search level\> [-f \<filter\>] [-k|K|m|M|g|G \<size\>] -s \<source directory\> ... -s \<source directory\>  

Check all mp4 files that are smaller 300MB in Fred's downloads and ./video, all subdirectories will be processed 
~~~
duplicateFF -f '.mp4' -m 300 -s './video/' -s '/home/fred/downloads' -l 0   
~~~
Check all image (jpg) files my home directory, ignoring directory paths longer that three directories i.e. only search three levels deep.
~~~
duplicateFF -l 3 -f '.jpg' -s .      
~~~
Note: **-s .** could be replaced with **-s $PWD** or the full path. Search filter will process .jpg and .JPG files

Inputs of 'filter' 'source directory' 'output directory' should have single or double quotes otherwise any names with white space will not be processed.

**-l Search level** 

* -l 0 Process the current directory and all subdirectories.
* -l 1 Process only the current directory
* -l 2 Process the current directory and one level below.
* -l n Process n levels deep in directory structure, includes current directory.

**-f File name filter** 

Optional case insensitive filter, filter the source by part or the whole name of a file. This option can only be used once.

**-s Directories to process** 

One or many directories can be entered each must start with -s.  

**Maximum file size filter**

Maximum file sixe filter is optional, default is 20 GiB.  Ignoring large files can save time.

**-k or -K** kilobytes (KiB)

**-m or -M** megabytes (MiB)

**-g or -G** gigabytes (GiB)
    
_Additional Notes_
* Default output directory, will be is created in the directory from which the script is run.
* Temporary files are created in the directory from which the script is run - all removed or moved on scipt termination.
* All hidden (dot) files are ignored, hidden directories are processed.
* The 'filter' and 'source directory' require single or double quotes for spaces in the filter and directory input.
* Filtering example : **_-f 'rar'_** that will pick both the word 'LIBRARY' and suffix '.rar'.  They will be ignored
*  Only the last instances of -f filter is used, the rest are ignored.
*  Only the last instances of file size filter is used, the rest are ignored.  
* Files with identical names and have differeing checksums are considered different files. File contents determines if they are copies.
* Files with different contents and the same SHA256 have known to occur.
* The app is designed to be thorough, not designed for speed.
* Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.

### OUTPUTS 
Output directory created in current directory with name **duplicate_chk_\<date-time\>**  where date-time = yymmdd-HHMMSS. Output directory contains: 

**duplicate_FILES1_\<date-time\>.csv**  Format – One file per row

CSV Columns 
1. sha256 checksum
2. fully pathed file name
3. full path of containing directory
4. file size in KiB
------------------------------------

**duplicate_FILES2_\<date-time\>.csv**  Format – Every row is as unique sha256 value with file size and all files of the same shar256 value. 

If more files match the checksum they are added as columns in <file> <directory> pairs i.e. repeats of columns 5 and 6.  

CSV Columns
1. sha256 checksum
2. file size in KiB 
3. fully pathed file name (file #1)
4. full path of containing directory (file #1)
5. fully pathed file name (file #2)
6. full path of containing directory (file #2)
------------------------------------
#### Outputs _cont._
A CSV list of all files processed **all_files_\<date-time\>**   ...... Format: check_sum,\"\<full path\>\/\<file name\>\"

A CSV list of all unique files  **unique_files_\<date-time\>.csv**  ... Format: check_sum,\"\<full path\>\/\<file name\>\"

If unique files is missing then there are no unique files.

Basic logging **log__<date-time>.txt**  


   



