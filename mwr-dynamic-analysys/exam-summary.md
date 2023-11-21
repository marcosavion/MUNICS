# Exam Summary

## Static Analysis

### Import Table -> see the imports

* If we see the malware has few imports -> packed
* If we can see several imports -> unpacked

**Importants imports**

* CreateService ←&#x20;
  * Service manipulation function
* RegSetValueEx ←&#x20;
  * Registry manipulation function
* HttpSendRequest ←&#x20;
  * Suggests that the malware file perform http requests&#x20;

Others important imports

* OpenService
* DeleteService
* OpenSCManager
* RegOpenKeyEx&#x20;
* RegQueryValueEx&#x20;
* RegCreateKey&#x20;
* InternetOpen&#x20;
* InternetConnect&#x20;
* HttpOpenRequest&#x20;
* InternetReadFile

### Strings

We have to find strings like:

* Domain Names
* Registry location
* Others
* VideoDrivers









## Dynamic Analysis

### Process Explorer

If we see that the malware service has: (Lab1)

* ws23.dll
* wshtcpip.dll

We can definitely conclude that the malware has network functionality&#x20;

