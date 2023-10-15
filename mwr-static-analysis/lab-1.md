# Lab 1

## VirusTotal

If we upload the _**Lab01-01.dll**_ file to [virusTotal](https://www.virustotal.com/gui/home/upload), it warns us that it is a virus, obviously

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

Going to _Details tap_, we can also see the history or imports of that file.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

Now we are going to see the reports for _**Lab01-01.exe**_

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

And watching the history and import sections:

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>



## Compilation Date

To see the compilation date we can use:

*   **VirusTotal**

    * In _Details_ tap, we can see the _Header_ section that show us this particular date

    <figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>
*   **PEView**

    * Using this program we can also see the compilation date of that malware going to&#x20;
      * IMAGE\_NT\_HEADERS
        * IMAGE\_FILE\_HEADER
          * Time Data Stamp

    <figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption><p>Compilation Date Lab01-01.dll</p></figcaption></figure>

    <figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption><p>Compilation Date Lab01-0.exe</p></figcaption></figure>

### Conclusions of these compilation dates

After seeing that both files were compiled within only one minute of each other, we can conclude that both files are related and also were created by the same author.

It is likely that the _.exe_ file will use or install the _.dll_ because DLLs cannot be executed on their own.



## Packet || Obfuscation

We are going to use _**PEiD**_ to see if these files were packed or obfuscated.

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption><p>PEiD - Lab01-01.exe</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption><p>PEiD - Lab01-01.dll</p></figcaption></figure>

We can see that both files were compiled with _Microsoft Visual C++ 6.0_ so it tells us that are not packed.

Besides, we can check that using a more accurate way by hit the button _**>>**_

*

    <figure><img src="../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

The fact that the files have few imports tells us that they are likely small programs. Notice that the DLL file has no exports, which is abnormal, but not indicative of the file being packed

## Looking for imports || strings

This can be done with the same program as we used before: PEview or with VirusTotal.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Some considerations:

* All of the imports from _msvcrt.dll_ are functions that are included in nearly every executable as part of the wrapper code added by the compiler.
*   When we look at the imports from kernel32.dll, we see functions for opening and manipulating files like:

    * CreateFileA
    * CopyFile

    As well as other functions like:

    * FindFirstFileA
    * FindNextFileA

    This tells us that the malware are trying to find and open a file so it wants to modify a particular file. Maybe a .exe because when we use _strings tool_, we can see something like this

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Moreover, if we analyze the _.dll_ we can see two interesting things:

* there is another library called _**WS2\_32.dll**_ which is a library for network functionality.
* _**Sleep**_ and _**CreateProcessA**_ are used.
  * These functions are usually used to create a **backdoor**.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>



## Host-Based Indications

Running _strings_ tool, we can see that in the file _Lab01-01.exe_ it is created a file called _kerne132.dll_ which tries to spoof _kernel32.dll_. So we suppose that malware delete the last one to insert _kerne132.dll_ and obviously, this is a host-based indicatator.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

## Network-Based Indications

Watching the _.dll_ content, we can find:

* _**Sleep**_ and _**CreateProcess**_ functions which are used to create a backdoor.
* An local IP Address - 127.26.154.13 (made for educational purpuses)
* _**exec**_ command
  * The _**exec**_ string is probably sent over the network to command the backdoor to run a program with CreateProcess.
* _**sleep**_ command
  * The _**sleep**_ string is probably used to command the backdoor program to sleep.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>









