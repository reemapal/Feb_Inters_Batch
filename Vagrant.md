# Project: Create Two windows95 on Vagrant Machine

## These are the steps which I followed in Table
|**S.No**|**Sub S.No**|**Task Description**|**Run Command on <br> Terminal**|
|:----:|:----:|:-----|:-----|
|1|| Make a directory | mkdir Windows95|
|2| |Get into that directory| cd windows95|
|3| |Place Vagrant file into it| vagrant init danimaetrix/windows-95|
|4| |Create guest machine to <br> your Vagrantfile| vagrant up|
|| |If Windows show error download zip file| http://www.tmeeco.eu/9X4EVER/GOODIES/FI ... _FINAL.ZIP | 
|5| |unzip file through terminal| unzip *File name*
|6||Open your Virtual Machine and follow the Steps||
||1.| Open Settings ||
||2.|Go to storage||
||2.1.|Storage Devices|| 
||2.2.|**Controller:IDE** *single tap* on Empty floppy and then add image file (of unzipped file) ||
||3| Open Windows , if error persist (in loop form) exit from that window |||
||4| Go to Storage again from Settings and remove that Image||
|7|| Windows Installation Completed||
