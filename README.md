<font color="red">Here is the only 'README.md' document in this repository, now. Just a moment, please.</font>

## Description

'CryptGrep' is an IDA python script which makes it possible to search a cryptographic function to analyze malware rapidly. The same malware family usually use the same cryptographic algorithm, and don't change their algorithm and implementation so frequently. Therefore, CryptGrep adopted signature based approach. Our approach also uses improved 'BinGrep' algorithm that was specialized for cryptographic function using several heuristic technique. And several pre-set signatures for typical malware are here. The usage is very simple and easy. And if you need additional signature, you can also create your original signature using your malware.


## Version

CryptGrep v1.0


## Requirement

* IDA Pro 6.x, 7.x
* Python 2.7


## Install

Uncompress and copy to any folder.


### Directories and files

```
 cryptgrep/  Sig/              <- Directory for cryptgrep signatures
             cgcore/           <- cryptgrep modules
             cryptgrep.py      <- main script to find cryptographic function
             cryptgrep-cmd.py  <- main script to find cryptographic function (in the CUI mode)
             sigcreate.py      <- script for creating signature
```


## Usage

There are three python scripts in 'cryptgrep' directory. These can be executed with IDA Pro.


### cryptgrep.py

This script finds cryptographic functions that are implemented in binary file by comparing between 'CryptGrep' signatures and malware. This can be executed in GUI of IDA Pro. The usage is very simple.

* Open malware with IDA Pro and run cryptgrep.py.
    * "File" -> "Script file" (or Alt-F7).
    * Select "cryptgrep.py".

* The output will be shown in new tab.

![Screenshot](img/screenshot1.png)


### cryptgrep-cmd.py

This script is the CUI version of cryptgrep.py and the output will be shown in stdout. This can be executed in a command-line terminal such as Cygwin.

* Usage
    * $ python cryptgrep-cmd.py <idb file>

* Example:
    * $ python cryptgrep-cmd.py Malware.idb
    * $ python cryptgrep-cmd.py Malware.exe

* Output
```
cryptgrep$ python cryptgrep-cmd.py sample_malware.idb
[+] CryptGrep finished
# Time: 12.280 sec.
# Results:
{
  'Signature': 'Himawari AES_Encrypt',
  'Function': 'sub_421000',
  'LCS': 100,
  'GED': 0
},
{
  'Signature': 'Himawari AES_Decrypt',
  'Function': 'sub_422000',
  'LCS': 99,
  'GED': 3?
},
{
  'Signature': 'Himawari AES_KeySetup',
  'Function': 'sub_422000',
  'LCS': 89,
  'GED': 2?
},
...
```

CryptGrep searches for cryptographic functions from 'sample_malware.idb'.
In this example, CryptGrep finds 3 functions related to AES in the IDB file.
Sub_421000 is detected as AES encrypt function based on a signature created from malware named Himawari.
The similarity score is 100 and the graph edit distance of functions' control flow graphs is 0.  

### sigcreate.py

If you want to create your original signature, you can use this script.

* Usage
    * $ python sigcreate.py -m <MalwareName> [-n <SeqNo>] <idb file> <idb function name> [<function name for signature>]
        * -m: Malware name for signature
        * -n: Sequence number for signature (if you need)
        * <idb file>: .idb file or executable file
        * <idb function name>: The function name in the .idb
        * <function name for signature>: Teh name of this function for your signature

* Example
    * $ python sigcreate.py -m Himawari -v 1 Himawari.idb Himawari-DES
    * $ python sigcreate.py -m Himawari -v 1 Himawari.idb sub_4032C8 Himawari-DES

* Output
```
[+] Signature created
    # Filepath: C:\cryptgrep\your-malware.dmp
    # Time: 2.075 sec.
```


## License

The 3-Clause BSD License


## Author

Copyright 2018, Hiroki Hada and Tomonori Ikuse
All rights reserved.

