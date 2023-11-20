# Lab 2

## Basic Static Analysis

### Import Table -> PEview

First of all, we are going to see the import table by using _**PEview**_ to see the import that the malware has

<figure><img src="https://lh7-us.googleusercontent.com/oTahyd3lfpPYenx1tcptElBGQtBe1V2Ge5yXfmfJobiQADtgITo5up89eozOF_STld32iuejTUqzsIP8PXxbiQB4QodckFg612MBaxmwMpmqUtRdGpmyKM-NP5PUIpUwQ3O-GCh0fy2S9WFFDhwUFEQ" alt=""><figcaption></figcaption></figure>

We can see that the malware has 5 imports:

* KERNEL32.dll
* ADVAPI32.dll
* WS2\_32.dll
* WINNET.dll
* MSVCRT.dll

The following list show the important import that malware uses:

* OpenService
* DeleteService
* OpenSCManager
* CreateService ←&#x20;
  * Service manipulation function
* RegOpenKeyEx&#x20;
* RegQueryValueEx&#x20;
* RegCreateKey&#x20;
* RegSetValueEx ←&#x20;
  * Registry manipulation function
* InternetOpen&#x20;
* InternetConnect&#x20;
* HttpOpenRequest&#x20;
* HttpSendRequest ←&#x20;
  * Suggests that the malware file perform http requests&#x20;
* InternetReadFile

### Strings&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/YptjufwO4rxPK8zinK_Xo6NSdv2WqBm5aLl3LZQmu466bTNwWQPzKnBddAezuKfXYkCUYmHPpqTRn2oYUss4eck_xexEpaFeXpJH6QqOXOHT8CGqiNFJx5sy7ySCYz9tJJy7A6gcarEm5MO0XcxeHzg" alt=""><figcaption></figcaption></figure>

We see several interesting strings, including registry locations, a domain name, unique strings like IPRIP and serve.html, and a variety of encoded strings. Basic dynamic techniques may show us how these strings and imports are used.

The results of our basic static analysis techniques lead us to believe that this malware needs to be installed as a service using the exported function **installA**. We will use that function to attempt to install this malware, but before we do that, we will launch Regshot to take a baseline snapshot of the registry and use Process Explorer to monitor the processes running on the system.

After setting up Regshot and Process Explorer, we install the malware using rundll32.exe, as follows:

```
rundll32.exe Lab03-02.dll, installA
```

Now we are going to take a second registry snapshot and compare them.



## Dynamic Analysis

### RegShot

We can see that malware is installed as a IPRIP services

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

Since the malware is a DLL, it depends on an executable to launch it. In fact, we see that the ImagePath is set to svchost.exe, which means that the malware will be launched inside an svchost.exe process

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

If we examine the strings closely, we see SOFTWARE\Microsoft\Windows NT\CurrentVersion\SvcHost and a message "You specify service name not in Svchost // netsvcs, must be one of following".&#x20;

If we follow our hunch and examine the \SvcHost\netsvcs registry key, we can see other potential service names we might use, like 6to4 AppMgmt.&#x20;

Running&#x20;

```
Lab03-02.dll, installA 6to4
```

will install this malware under the 6to4 service instead of the IPRIP service, as in the previous listing.









<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

the result tells us that Lab03-02.dll is loaded by svchost.exe with the PID 1024. (The specific PID may differ on your system.)

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>





<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>



### FakeNet

The malware did a DNS request to _practicalmalwareanalysis.com_

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

### NetCat

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

We can create a couple of network signatures from this data. Because the malware consistently does a GET request for serve.html, we can use that GET request as a network signature. The malware also uses the User-Agent MalwareAnalysis2 Windows XP 6.11. MalwareAnalysis2 is our malware analysis virtual machine's name (so this portion of the User-Agent will be different on your machine). The second part of the User-Agent (Windows XP 6.11) is consistent and can be used as a network signature.

