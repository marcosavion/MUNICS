# Lab 4

## VirusTotal

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

## Packet || Obfuscation

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

This program doesn't appear to be packed because&#x20;

1. We can see the compiler that was used&#x20;
2. The size difference between real and virtual space is not too big
3. Names sections are visible
4. The entropy shows us that this malware is not packed.

## Compilation date

There are different ways to see this particular date:

1.  VirusTotal

    <figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>
2.  PEview

    <figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

We can see that the compilation date is: 30/08/2019 but we can see in VirusTotal report that this file was seen in 2011 so we suppose that this compilation date is facked. Thus, we cannot know the real compilation date.

## Looking for imports  || strings

Using VirusTotal or PEview we can see the imports

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

There are 3 libraries:

1. _**ADVAPI32.dll**_
   1. We can assume that this malware tries to do something with permissions. Maybe it is trying to access to privilege files using special permissions.
2. _**KERNEL32.dll**_
   1. The imports from _kernel32.dll_ tell us that the program loads data from the resource section
      1. **LoadResource, FindResource, and SizeOfResource**
   2. writes a file to disk **(**
      1. **CreateFile and WriteFile**
   3. and executes a file on the disk
      1. _**WinExec**_
   4. We can also guess that the program writes files to the system directory because of the calls to _**GetWindowsDirectory**_

### strings

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

We conclude:

1. The malware tries to access to http://www.practicalmalwareanalysis.com/updater.exe which is probably the location where another malware is stored.
2. \system32\wupdmgrd.exe in combination with GetWindowsDirectory, suggests that a file in C:\Windows\System32\wupdmgr.exe is created or edited by this malware.

So:

1. We now know with some confidence that this malicious file downloads new malware.
2. We know where it downloads the malware from, and we can guess where it stores the downloaded malware.&#x20;
3. The only thing that is odd is that the program does not appear to access any network functions

## Resource Hacker

We can use this program to see the sources that a particular malware has.

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

So we can see that this program identifies one resouce as _binary_ whit a string _This program cannot be run in DOS mode_

This string is the error message included in the DOS header at the beginning of all PE files. We can therefore conclude that this resource is an additional executable file stored in the resource section of Lab01-04.exe. This is a fairly common technique used in malware.

We are going to save this binary file

1.  Action > Save Resource to a BIN file...

    <figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Once we have stored it, we will analyze it with PEview in order to see the imports.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Looking at the imports, we see that the embedded file is the one that accesses the network functions. It calls _**URLDownloadToFile**_, a function commonly used by malicious downloaders. It also calls _**WinExec**_, which probably executes the downloaded file
