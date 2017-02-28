---
title: msys2-here-context-menu-items
date: 2017-02-28 12:03:13
tags: [msys2,mingw]
---

## register.reg

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MSYS here\command]
@="C:\\msys64\\msys2.exe bash"

[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MINGW64 here\command]
@="C:\\msys64\\mingw64.exe bash"

[HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MINGW32 here\command]
@="C:\\msys64\\mingw32.exe bash"
```

## unregister.reg

```
Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MSYS here]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MINGW64 here]
[-HKEY_CURRENT_USER\Software\Classes\Directory\Background\shell\MINGW32 here]
```