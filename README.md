TODO:

1 - Make changes to the update functions, such as re-newing the row keys for updated files (to do this, request the state matrix from the server and also initialize the state matrix with all 1s)
2 - Encrypt/Decrypt the string D
3 - Store the keyword hash table at the server side and request and get it in search
4 - Optimize the data type for D (OPTIONAL)

# Forward-private Sublinear DSSE (FS-DSSE)
Basic implementation of FS-DSSE. The full paper is published in IEEE ICC 2018 and available at https://eprint.iacr.org/2017/1222.pdf. 

This project is built on CodeLite IDE (link: http://codelite.org). It is recommended to install CodeLite to load the full FS_DSSE workspace. 


# Required Libraries
1. ZeroMQ (download link: http://zeromq.org/intro:get-the-software)

2. Libtomcrypt (download link: https://github.com/libtom/libtomcrypt)

3. Google sparsehash (download link: https://github.com/sparsehash/sparsehash)

4. Intel AES-NI (*optional*) (download link: https://software.intel.com/en-us/articles/download-the-intel-aesni-sample-library)

## Intel AES-NI installation guide (optional)

FS_DSSE leverages Intel AES-NI to accelerate cryptographic operations. The Intel-AES-NI is available in Intel® Core™ i5, Intel® Core™ i7, Intel® Xeon® 5600 series and newer processor (see https://ark.intel.com/Search/FeatureFilter?productType=processors&AESTech=true for a complete list). This functionality can be *disabled* to test FS_DSSE with other CPU models (see the Configuration Section below). Here the brief instruction to install Intel-AES-NI:


1. Extract the .zip file downloaded from https://software.intel.com/en-us/articles/download-the-intel-aesni-sample-library
2. Open the Terminal and go to `Intel_AESNI_Sample_Library_v1.2/intel_aes_lib`
3. Run ``./mk_lnx_lib64.sh``, which will generate the header and library files in `intel_aes_lib/include` and `intel_aes_lib/include` directories, resp.
4. Copy header files and library files to your local folders (e.g., `/usr/local/include` and `/usr/local/lib`).


# Configuration
All FS_DSSE configurations are located in ```FS_DSSE/config.h```. 

## Highlighted Parameters:
```

#define INTEL_AES_NI				-> If enabled, use Intel AES-NI library

#define DISK_STORAGE_MODE            	   	-> If enabled, encrypted index will be stored on HDD (RAM if disabled)
	
#define SEND_SEARCH_FILE_INDEX        		-> If enabled, search result will contain specific file indexes

#define PEER_ADDRESS "tcp://localhost:5555"	-> Server IP Address & Port

const std::string SERVER_PORT = "5555";		-> Server Port number

#define  MAX_NUM_OF_FILES 1024              	-> Maximum number of files (It MUST be the power of 2 and divisible by 8)

#define  MAX_NUM_KEYWORDS 12000             	-> Maximum number of keywords

```

### Notes

The folder ``FS_DSSE/data`` as well as its structure are required to store generated FS_DSSE data structures. The database is located in ``FS_DSSE/data/DB``. The implementation recognize DB as a set of document files so that you can copy your DB files to this location. The current DB contains a small subset of enron DB (link: https://www.cs.cmu.edu/~./enron/).

# Build & Compile
Goto folder ``FS_DSSE/`` and execute
``` 
make
```

, which produces the binary executable file named ```FS_DSSE``` in ``FS_DSSE/Debug/``.

### If there is an error regarding to BOOL/bool type when compiling with Intel-aes-ni

- Access the header file named ``iaesni.h``, go to line 51, and comment that line as follows:

```
#ifndef bool
//#define bool BOOL 			-> line 51
#endif
```

### If the hardware does not support Intel-aes-ni

1. Disable INTEL_AES_NI in ``FS_DSSE/config.h``

2. Change the make file in ``FS_DSSE/MakeFile`` by removing the library linker ``-lintel-aes64`` 



# Usage

Run the binary executable file ```FS_DSSE```, which will ask for either Client or Server mode. The FS_DSSE implementation can be tested using either **single** machine or **multiple** machines with network:


## Local Testing:
1. Set ``PEER_ADDRESS`` in ``FS_DSSE/config.h`` to be ``localhost``. 
2. Choose  ``SERVER_PORT`` identical with what indicated in ``PEER_ADDRESS``. 
3. Compile the code with ``make`` in the ``FS_DSSE/`` folder. 
4. Go to ``FS_DSSE/Debug`` and run the compiled ``FS_DSSE`` file with two different Terminals, each playing the client/server role.

## Real Network Testing:
1. Set ``PEER_ADDRESS`` and  ``SERVER_PORT`` in ``FS_DSSE/config.h`` with the corresponding server's IP address  and port number.
2. Run ``make`` in ``FS_DSSE/`` to compile and generate executable file ``FS_DSSE`` in ``FS_DSSE/Debug`` folder.
3. Copy the file ``FS_DSSE`` in ``FS_DSSE/Debug`` to different machines
4. Execute the file and follow the instruction on the screen.


# Further Information
For any inquiries, bugs, and assistance on building and running the code, please contact Muslum Ozgur Ozmen (ozmenmu@oregonstate.edu) or Thang Hoang (hoangmin@oregonstate.edu).
