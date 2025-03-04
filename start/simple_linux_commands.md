# Linux Commands that could be helpful

# can show how to bypass file permissions if you dont have access to run chmod on target computer.
- locate universal.ovpn  - locate [filename] - show path to the file specified.
- ls -l newfilename.txt -  ls -l [optionalfilename] - will give us file permissions or list of the files in dir with permissions listed.
- cp /usr/bin/ls chmodfix - cp [filename] [newfilename] - will copy contents of first file to new one specified.
- cat /usr/bin/chmod > chmodfix - cat [filename] > [newfilename(exsisting)] - will take contents of one file and put it into a new file.
- when in doubt, if you know password/credentials use sudo



