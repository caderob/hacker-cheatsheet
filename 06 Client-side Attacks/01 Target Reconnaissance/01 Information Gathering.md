# Information Gathering

Displaying the metadata for brochure.pdf
>``` shell
>kali@kali:~$ cd Downloads 
>
>kali@kali:~/Downloads$ exiftool -a -u brochure.pdf 
>
># ========== Expected Result ==========
>ExifTool Version Number         : 12.41
>File Name                       : brochure.pdf
>Directory                       : .
>File Size                       : 303 KiB
>File Modification Date/Time     : 2022:04:27 03:27:39-04:00
>File Access Date/Time           : 2022:04:28 07:56:58-04:00
>File Inode Change Date/Time     : 2022:04:28 07:56:58-04:00
>File Permissions                : -rw-------
>File Type                       : PDF
>File Type Extension             : pdf
>MIME Type                       : application/pdf
>PDF Version                     : 1.7
>Linearized                      : No
>Page Count                      : 4
>Language                        : en-US
>Tagged PDF                      : Yes
>XMP Toolkit                     : Image::ExifTool 12.41
>Creator                         : Stanley Yelnats
>Title                           : Mountain Vegetables
>Author                          : Stanley Yelnats
>Producer                        : Microsoft® PowerPoint® for Microsoft 365
>Create Date                     : 2022:04:27 07:34:01+02:00
>Creator Tool                    : Microsoft® PowerPoint® for Microsoft 365
>Modify Date                     : 2022:04:27 07:34:01+02:00
>Document ID                     : uuid:B6ED3771-D165-4BD4-99C9-A15FA9C3A3CF
>Instance ID                     : uuid:B6ED3771-D165-4BD4-99C9-A15FA9C3A3CF
>Title                           : Mountain Vegetables
>Author                          : Stanley Yelnats
>Create Date                     : 2022:04:27 07:34:01+02:00
>Modify Date                     : 2022:04:27 07:34:01+02:00
>Producer                        : Microsoft® PowerPoint® for Microsoft 365
>Creator                         : Stanley Yelnats
># =====================================
>```

Lab 1 - Download old.pdf from the Mountain Vegetables website on VM #1 by clicking on the OLD button. Use exiftool to review the file's metadata. Enter the value of the Author tag.
>``` shell
># Download old.pdf (Navigate to http://192.168.103.197/old.pdf)
>
>kali@kali:~$ cd Downloads 
>
># Display the metadata for old.pdf
>kali@kali:~/Downloads$ exiftool -a -u old.pdf 
>
># ========== Expected Result ==========
>...
>PDF Version                     : 1.4
>XMP Toolkit                     : Image::ExifTool 11.88
>Author                          : OS{b657f9d1e4b94460c7684ecaf1708353}
>Create Date                     : 2018:01:17 05:52:23+00:00
>Modify Date                     : 2018:01:17 05:52:23+00:00
>...
># =====================================
>```
>OS{b657f9d1e4b94460c7684ecaf1708353}

Lab 2 - Start VM #2 and use gobuster to bruteforce the contents of the web server. Specify "pdf" as the filetype and find a document other than old.pdf and brochure.pdf. After you identify the file, download it and extract the flag in the metadata.
>``` shell
># Using gobuster to bruteforce the contents of the web server (Specifing "pdf" as the filetype)
>kali@kali:~$ gobuster dir -u http://192.168.103.197 -w /usr/share/wordlists/dirb/common.txt -x pdf -t 5
>
># ========== Expected Result ==========
>...
>/index.html           (Status: 200) [Size: 5443]
>/info.pdf             (Status: 200) [Size: 309737]
>/old.pdf              (Status: 200) [Size: 462554]
>...
># =====================================
>
># Download info.pdf (Navigate to http://192.168.103.197/info.pdf)
>
>kali@kali:~$ cd Downloads 
>
># Display the metadata for info.pdf
>kali@kali:~/Downloads$ exiftool -a -u info.pdf 
>
># ========== Expected Result ==========
>...
>Description                     : OS{9d27aa56204924e6fc0a9bb13934282a}
># =====================================
>```
>OS{9d27aa56204924e6fc0a9bb13934282a}
