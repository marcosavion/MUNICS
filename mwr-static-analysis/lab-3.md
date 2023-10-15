# Lab 3

## VirusTotal

As always, we are going to upload the file to VirusTotal in order to check if it is really a malware and the details of that

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>



## Packet || Obfuscation

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

This program identifies the packet program as FSG 1.0 -> dulek/xt

We can see that appears to be packed. We are going to see the virtual and real size to compare them.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

&#x20;We can conclude that this file is packed becuase:

* The file sections have not names
* The difference between virtual size and real size

Another way to see if the malware is packed or not is watching the imports of this file

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<mark style="color:red;">**An executable file without an import table is extremely rare, and its absence tells us that we should try another tool, because PEview is having trouble processing this file.**</mark>

By opening _**Dependency Walker**_ we can see that this file it does have an import table but it only imports two functions: _LoadLibrary_, _GetProcessAddress_

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Packed files often import only these two functions, which further indicate that this file is packed

We can try to unpack the file using UPX, but we know that the file is packed with FSG, rather than UPX.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Even if we run _string command,_ we wont get any handy information

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

1. **Do any imports hint at this programs functionality? If so, which imports are they and what do they tell you?**
   1. _This question cannot be answered without unpacking the file._
2. _**What host- or network-based indicators could be used to identify this malware on infected machines?**_
   1. _This question cannot be answered without unpacking the file_
