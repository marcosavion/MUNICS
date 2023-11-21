# Lab 1

## Basic Static Analysis

### Import Table - PEview

As always, we are going to see the import table by using PEview

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

As we can see, there are very few imports, so we are going to conclude:

* Malware is may packed and the import can be resolved in runtime

### Strings

We could think that we are not going to see any string because the malware is packet, however we  see many interesting strings, such as:

* registry locations&#x20;
* a domain name
* WinVMX32
* VideoDriver
* vmx32to64.exe

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



## Dynamic Analysis

### Process Monitor --> clear out all the events

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### Process Explorer --> Open to see the process

### Fakenet -> just execute it

### Netcat

Open port 80 y 443

```
nc -l -p 80,443
```

After running the malware we should see



## Running the malware

### Process Explorer

Once we have executed the malware, we can see in Process Explorer that it created a mutex called WINVMX32

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

We can also see the import that dynamically did the malware and there are two interesting imports:

* ws23.dll
* wshtcpip.dll

So we can conclude that the malware has networking functionallity

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



### Process Monitor

We are going to set up 3 filters

1.  The firt one to filter the process name

    <figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
2.  isdjfiodsj

    <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
3.  sidjsijf

    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>



Once we have applied the filter, we can see one filter to WriteFile and nine RegSetValue

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

We often need to filter out a certain amount of noise, such as entries 0 and 3 through 9 in Figure 4. The RegSetValue on HKLM\SOFTWARE\Microsoft\Cryptography\RNG\Seed is typical noise in the results because the random number generator seed is constantly updated in the registry by software.















