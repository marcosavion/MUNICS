# Lab2



## VirusTotal

We are going to upload this file in order to check if it has a malicious signature and also see the details of that.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

After looked all this information we can conclude some many things:

* We can see that it is malware, obviously
* This program was compiled on 19/01/2011
* Has several imports

## Packet || Obfuscation

To see if this program is packet or obfuscated, we are open to analyze it with **PEiD**

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

We can see that this program doesn't tell us the program which was used to compile this malware. It can be a indicative to determine that this malware is packet but we are going to look for more information.

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

So now we can confirm that this malware is packed.&#x20;

When the value of the entropy is near to 8, it is because this program is packet.

Another way to see if this program is packet is by comparing the virtual and real size of that.

*   **It the program is not packed** -> There are not difference between these.

    <figure><img src="https://lh6.googleusercontent.com/NSKTKCW596q8acZSsXK_qUzNG4RDrlmUH6lM_ZpsBBKIceeeXgEmO8UIrDA-mzSBBilHBpWvbOaI0C3ZHsY5g919Z_3QfjFrMAQxWP1GZDWhpbpNo5qmPtrk2CvdtNi_akunL2gRIt--XiNaAZStHBg" alt=""><figcaption></figcaption></figure>
*   **If the program is packed** -> There exists a real difference&#x20;

    <figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Moreover, these sections called UPX0, UPX1, UPX2... it seems like that this program was packed using UPX. We can see the same by using PEview:

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Where we see less information than the previous lab malware.

All this information could confirm that the malware is packet but PEid is not foolproof. So we are going to use _**UPX**_ to unpacket this program

```bash
upx -o newFilename -d originalFilename
```

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

* **-o**: output
* **-d**: decompress

Now, if now we analyze the _newMalware_ file, we will get:

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>



## Looking for imports || strings

Now we can see the content of this file and we are going to use _**PEview**_ to see the imports and another useful information

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

&#x20;As we can see, this file has 4 imports:

* _**kernel32.dll**_
  * almost every program has this import
*   _**advapi32.dll**_

    * This tells us that malware tries to create a service

    <figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>
* _**msvcrt.dll**_
  * almost every program has this import
* **wininet.dll**
  *   This library tell us that this malware tries to connect to Internet&#x20;

      <figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

Knowing all that, we are going to see the string command output:

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

After watching that, we can conclude:

* The malware is likely to try to open the _**http://www.malwareanalysisbook.com**_
  * Network-based indicator
* _**MalService**_ is likely to be the name of the created service
  * Host-based indicator

