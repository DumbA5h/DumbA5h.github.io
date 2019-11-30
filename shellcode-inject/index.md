## In memory shellcode injection

### Background
* This program downloads the shellcode in memory without touching the disk then executes it.
* The shellcode can be hosted on a web server for malware to be downloaded and executed.

### Process
* The program downloads the shellcode in memory by a HTTP request.
* Then it allocates memory for the shellcode, the size of memory is allocated using Content-Length header.
* It also modifies the memory page permissions to executable, to bypass DEP.
* Finally the shellcode is executed.

### Benefits
* The program can be used to execute any type of shellcode, as the shellocde is retrieved from web server and is not hard coded.
* The detection rate is lower as the actual malicious shellcode never touches the disk, so hard for AVs to detect.
