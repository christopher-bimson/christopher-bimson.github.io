---
layout: post
title: Lazy File Checksums
---
Calculating a file checksum is a routine activity I need to perform occasionally. 

If you are a Powershell fan, and you have Powershell 4.0 or above, you can use the [Get-FileHash](https://technet.microsoft.com/en-us/library/dn520872.aspx) cmdlet. I quite like Powershell but for some reason I can't quite explain I instictively reach for cmd.exe for these kind of tasks.

It turns out that [CertUtil.exe](https://technet.microsoft.com/en-us/library/cc732443.aspx) can hash files too, so I hacked together this:

```shell
@echo off
set x=.sha512
set hfn=%1%x%
@certutil -hashfile %1 SHA512 | sort /r | more +2 > %hfn%
```

To use:

1. Save it to a file called `hash-file.cmd` somewhere on your [path](http://stackoverflow.com/questions/9546324/adding-directory-to-path-environment-variable-in-windows).
2. Type `hash-file <your-file-here.file>` at a command prompt. 
3. A handy `<your-file-here.file.sha512>` will be created alongside the original.

![spacer](/img/spacer.gif)

