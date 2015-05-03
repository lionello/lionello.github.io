---
layout: post
title: How to add a reference to an assembly inside the GAC
categories:
- Knowledge Base
permalink: /archives/6-How-to-add-a-reference-to-an-assembly-inside-the-GAC.html
s9y_link: http://www.lunesu.com/index.php?/archives/6-How-to-add-a-reference-to-an-assembly-inside-the-GAC.html
date: 2007-03-04 13:41:00.000000000 +08:00
---
From <a href="http://www.mztools.com/articles/2008/..%5C2007%5CMZ2007012.aspx">mztools.com</a>:<br />
<br />
You can follow this procedure to extract the assembly from the GAC:<br />
<br />
Open a MS-DOS command window (click Start, Run... type cmd and press enter) <br />
Use the SUBST command to map a drive letter to the GAC folder: <br />
```
SUBST G: C:\Windows\Assembly
```
<br />
That maps the drive letter G: to the GAC folder. With the SUBST command you bypass the shell extension.<br />
<br />
Use Windows Explorer to browse the G: folder. You will see the actual folders, with names like GAC, GAC_32, GAC_MSIL and one subfolder for each assembly. Once you locate the assembly DLL that you are looking for, you can copy it outside the GAC, to the \bin folder of your add-in project and add a reference to it. <br />
<br />
When done, you can undo the mapping using:<br />
```
SUBST G: /D
```
