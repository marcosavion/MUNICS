# Lab 3



## Dynamic Analysis

### Process Monitor

First of all, set the _Capture event to **off**_

<figure><img src="https://lh7-us.googleusercontent.com/pugtggEmhIi7o0Tal3oADnOmt4XFKvwqqgWT_vxsJNW80g4vLG9VyCyQxMwIt090ipSnxhc9VQzx9nX08J_KUkCMVpPyavW7wC463J38HMA7yGrU8k44lsEFEpFXsytOr3n5CKERSZd--dpHiF-Fcqw" alt=""><figcaption></figcaption></figure>

Then reset all the filter to only use the default ones.

<figure><img src="https://lh7-us.googleusercontent.com/vg_C9stDs7t8ZBBlq8btMf_4vRiXAg7TcqVr4dm5892x5zfyZXGkHhWtdWWiOHM3BOL6om6UrcYg88-IYCGcp00AaOuzmkYCe8i4n5hQfRCf8XOPpwucFcLyCymbxgViSfQow-LvqK_XXv-HP4QxYcs" alt=""><figcaption></figcaption></figure>

### Process Explorer

Once we have executed the malware, a service called _svchost.exe_ is created

<figure><img src="https://lh7-us.googleusercontent.com/erFkzkVnbso1ZmsNwdwqFhSVwg3g59wsg3C0ab8RhRzM24cEJ_0nE4CFc5kZXxhcVAtjl1Z-CiNij6T0gIrrC9l1RN1IfM_khW7p8M7pw6r1tpPloJhCe0mOzKJKzbhZYtyN-YnScvmSloZIjypF62M" alt=""><figcaption></figcaption></figure>

We can see that this service doesn't have parent -> it is highly suspicious&#x20;

Normally _svchost.exe_, is a child of _**services.exe**_ so the fact that the process is orphaned is highly unusual and highly suspicious. Moreover, we can check the parent of this suspicious service by watching the Properties of this scvhost.exe and we find that Lab03-03.exe is the parten of that

<figure><img src="https://lh7-us.googleusercontent.com/iP5JZBVgN0wG5x_p7ryN-iRcE4CtR9bYO20tdscOLyESKoSScVmUwE4wQEyYe99hWwQaRoRSAN7yCV6gk7uEreE12UU5hUahFmyjGZff-sJKnQOUfNCxQcQw6ZGTwocudnvJ-9X4k1-wj3dZi-85f9Y" alt=""><figcaption></figcaption></figure>

Switching to Sting tab, we can see the strings use in memory and disk and they differences.

<figure><img src="https://lh7-us.googleusercontent.com/QWbKZUDEZBKCMNr7VZQvOCAA_9z-T4g6gRw_4M5rosrp1UGuuh6x23rjOrsbUliz6rbOr_Lqz6wg1nwhWpZ5LLye1Cg4x0q_vhZHNFHJZuvYZYZ-DD_KUv8yHCIN8SOP60GO9ccSViP4J7okqIxPGWA" alt=""><figcaption></figcaption></figure>

We can see the _practicalmalwareanalysis.log, \[ENTER]_ and _\[CAPS LOCK]_ strings that they are very weird in a normal _svchost.exe_ so we can imagine that this malware is a kind of _**keylogger**_

To test our assumption, we open Notepad and type a short message to see if the malware will perform keylogging. To do so, we use the PID (found in Process Explorer) for the orphaned svchost.exe to create a filter in Process Monitor to show only events from that PID (3376 in our case).&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/_8g3WsgdFymzmjUDWfmC4kHiVNx59ylbmO7W2ZnmnvcHy4dFXno8Es4a1yr5wphUCJYwkQ0el8Qs-hJbBINvHDYjEShX0O2JwXaxWHxcOk7FEVL0BFk8xooeLoBl-oNns0jy7LZPhna0P8i9jCGmKaM" alt=""><figcaption></figcaption></figure>

Once we have applied this filter and we write any string on a notepad, we can see differents events of this service:

<figure><img src="https://lh7-us.googleusercontent.com/9FSh4LMQQbDV9PQA2zfc-wzEO8CAwBJtOJcByo78fWPSpb7Uceg4daF-Erv-nZKs-9O0n-F4KlFNLBSOPK_NUNiMYvFNbRalJGZq6Fsnf6rslJtoKnJaXUL6EF8ILzAKfPMP1vD3FORmRJkVeZYTxBM" alt=""><figcaption><p>El PID cambio porque antes no me creo el archvo porque no tenia permisos para hacerlo en el dispositivo D</p></figcaption></figure>

The CreateFile and WriteFile events for svchost.exe are writing to the file named practicalmalwareanalysis.log. (This same string is visible in the memory view of the orphaned svchost.exe process.)

If we see the content of the file:

<figure><img src="https://lh7-us.googleusercontent.com/WHoM6idl3Gy_QtHLo1AdG0Ymw0ReWlV7sx0y_Nh4VEFkovbQ7WR--StqH5sMsuqTkpuO79gj-5LCQBhp0ONIVbA_tmNw3X0Pj_UM-nymIymt-8xpEMU77w8wcFj1GIwpoTbulglf6y7NfsVkzf4pVTc" alt=""><figcaption></figcaption></figure>

