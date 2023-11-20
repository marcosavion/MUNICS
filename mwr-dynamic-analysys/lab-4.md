# Lab 4

## Static Analysis

First of all we are going to see the import that the malware uses in order to figure out how the mlaware works

### PEview

<figure><img src="https://lh7-us.googleusercontent.com/B4OPjgA63jauDGPCP8bS00s4a9Yoq-hPmQNdxTo7yi0IoDEPhDvyfs4KrskfbC2wIjMNv8nG2UdlYt2vnQhy7tvv1aSubIbl7n0qQyK7mLXpoL8j3a3NDNRbuqztqPOrfdk18waexmwE2t1PIgQjUiA" alt=""><figcaption></figcaption></figure>

We can conlcude that the malware performs networking functionality, service-manipulation functions and also registry-manipulation function

### Strings

Watching the strings we can find several handy strings&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/mjM-VQc4QOgsO8RNPieFjnsu4TEnHYWOCLEUC4soiPnA8xLldAANCoqk9aNN3Z_yc9vEaGQZ_VM_nH6iZdZprFm5DvqNw-pD-0xz9xFKZbrGKnI7_EqaP_tW0-FE76YqmLn5Vr80_IakhH1Fq2yZp9Y" alt=""><figcaption></figcaption></figure>

Here there are:

* Domain name
  * http://www.practicalmalwareanalysis.com
* Registry location
  * Software\Microsoft \XPS
* Request commands
  * DOWNLOAD
  * UPLOAD
  * SLEEP

Strings like DOWNLOAD and UPLOAD, combined with the HTTP/1.0 string, suggest that this malware is an HTTP **backdoor**

The strings -cc, -re, and -in could be command-line parameters (for example -in may stand for install)

### Dynamic Analysis



### Process Monitor

Once we run the malware, we can see that it is executed but deleted right away. So we are going to use process monitor to see the generated events by the malware

<figure><img src="https://lh7-us.googleusercontent.com/Euxxv5rllc4Z1BhCzKCBLBFBuOIs7uulypDtgrDUA4WnEj0NBfDgrSpmY2KOpYDI1k0sZxigH892VSLkCznwp6Y7YktyeS7cxCxQaEzz-PBlOZB0FLZHsu5aT5MoXOoRR1Mdsu2_CXNRDBtobtHys5k" alt=""><figcaption></figcaption></figure>

There are not any interesting WriteFile or RegSetValue entries, but upon further digging, we find an entry for **Process Create**. Double-clicking this entry brings up the dialog shown in the next screenshot

<figure><img src="https://lh7-us.googleusercontent.com/Ifr_A4bqDO7q-d-GmXh8gMtc4SCKoUUQb4rHYhUtE9V6pIlp4J387YsigrKQvPRcpDsF1cgPHRexy4sJs_IE_uhdQSFQ0Gy2cvCHkec3MCpxMuxJPcG_b75QsELWOEML0DHysjNNpOiYfEhGEeOAOjg" alt=""><figcaption></figcaption></figure>



&#x20;and we see that the malware is deleting itself from the system using "C:\WINDOWS\system32\cmd.exe" /c del Z:\Lab03-04.exe >> NUL, as seen at the figure

Even if we try to run the malware using options explained previously, we dont manage anything
