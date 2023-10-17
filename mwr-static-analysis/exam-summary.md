# Exam Summary

## VirusTotal

To see what is the program used for

In Details tab:

* Imports
* History -> Compilation Date
* If there is a binary hidding within that



## Compilation Date

* **VirusTotal**
  * Details Tab -> History
* **PEView**
  * IMAGE\_NT\_HEADERS -> IMAGE\_FILE\_HEADER -> Time Data Stamp

If there are two files and the two complitation dates are closely, we can conclude that both files are related and also were created by the same author.

## Packet || Obfuscation

* **PEID**
  * Malware packed
    * If it doesn't show us information about the compiler used
    * If the entropy is close to 8
    * If the program tells us that this malware is packed, obviously
    * If the virtual size and real size are too different
    * If we can't see the usual name of the file sections
      * If appears something like UPX0, UPX1 or UPX2 as file sections
* **PEview**
  * If we can't see the import of that file
  * If it appears something like UPX0, UPX1 or UPX2 as file sections&#x20;
* **Packed files often import only these two functions, which further indicate that this file is packed -> Lab3**
  * LoadLibraryA
  * GetProcAddress&#x20;
* By using _**strings**_ we can't see anything







## UPX unpacket

```
upx -o newFilename -d originalFilename
```









## DLL

Notice that the DLL file has no exports, which is abnormal, but not indicative of the file being packed

DLL cannot be executed on their own so they need a .exe



## Libraries

* _**msvcrt.dll**_
  * are functions that are included in nearly every executable as part of the wrapper code added by the compiler.
* _**kernel32.dll**_
  * Almost every file has this import
  * functions for opening and manipulating files like
    * CreateFileA
      * CopyFile
      * FindFirstFileA
      * FindNextFileA
* _**WS2\_32.dll**_
  * library for network functionality
    * _**Sleep**_ and _**CreateProcessA ->**_ These functions are usually used to create a **backdoor**.
  * See _string command output_ to verify that **exec** and **sleep** are found
    * The _**exec**_ string is probably sent over the network to command the backdoor to run a program with CreateProcess.
    * The _**sleep**_ string is probably used to command the backdoor program to sleep.
* _**advapi32.dll**_
  * To create services
* **wininet.dll**
  * This library tell us that this malware tries to connect to Internet&#x20;
  * See _string command output_ to check if there is any url on that.



## Problems

* If we dont see any import table with PEview -> **Depency Walker** -> lab 3



## Hidden Files

VirusTotal tells us that exist this file -> Lab 4

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

We have to use **Resource Packet**

[#resource-hacker](lab-4.md#resource-hacker "mention")
