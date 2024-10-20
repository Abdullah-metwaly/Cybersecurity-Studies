>The [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) (`FTP`) is a standard network protocol used to transfer files between computers. It also performs directory and files operations, such as changing the working directory, listing files, and renaming and deleting directories or files. By default, FTP listens on port `TCP/21`.

---


## Enumeration

		 Using nmap for -sC for default scripts and -sV for version information

```
$ sudo nmap -sC -sV -p 21 192.168.2.142 
```

#nmap #ftp 

		 connecting the ftp 


```
$ ftp 192.168.2.142
```

#ftp 
#### Brute Forcing with Medusa

```
$ medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp 
```

#medusa #ftp 

		The `Nmap` -b flag can be used to perform an FTP bounce attack:

```
$ nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```

```
$ hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142
```

```
$ nc -v 10.129.19.96
```

```
hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp
```

#hydra #ftp #nmap #medusa #nc 


## File manipulation 


#### Compare two files print the difference

```
$ diff file1.txt file2.txt
```

```
$ diff -u file1.txt file2.txt
```

```
$ diff -u file1.txt file2.txt > differences.txt
```

#### Store the intersection of two files in one file

```
$ comm -12 <(sort file1.txt) <(sort file2.txt) > intersection.txt
```

1. `comm -12`: The `-12` option suppresses lines that are unique to each file, and it only prints the lines that are common between both files.
2. `<(sort file1.txt)`: This uses process substitution to sort the lines of `file1.txt` and provide the sorted output as input to the `comm` command.
3. `<(sort file2.txt)`: Similarly, this sorts the lines of `file2.txt` and provides the sorted output to the `comm` command.
4. `> intersection.txt`: This redirects the output of the `comm` command to a new file named `intersection.txt`.

#### Remove whatever exists in file1 from file2


```
$ grep -vxFf file1.txt file2.txt > result.txt
```


1. `grep`: This command searches for patterns in text.
2. `-v`: This option inverts the match, meaning it will exclude lines that match the patterns.
3. `-xF`: These options ensure that the patterns are treated as fixed strings and match whole lines exactly.
4. `-f file1.txt`: This specifies that the patterns to search for are in the `file1.txt`.
5. `file2.txt`: This is the file in which you want to remove lines.
6. `> result.txt`: This redirects the output to a new file named `result.txt`.


#### Compare two files and remove duplication

```
$ sort -u file1.txt file2.txt > unique_lines.txt
```

#linux #commands
