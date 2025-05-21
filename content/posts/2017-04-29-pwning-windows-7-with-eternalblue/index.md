+++
date = '2017-04-29T17:17:00+00:00'
draft = true
title = 'Pwning Windows 7 with ETERNALBLUE & DOUBLEPULSAR (Metasploit)'
+++

{{< youtube -AMiSj6KwXo >}}

Thanks to `@UnaPibaGeek` & `@pablogonzalezpe` for their efforts to develop the Metasploit modules.

Modules can be found here (Scanner + Exploit):

- [https://packetstormsecurity.com/files/142181/Microsoft-Windows-MS17-010-SMB-Remote-Code-Execution.html](https://packetstormsecurity.com/files/142181/Microsoft-Windows-MS17-010-SMB-Remote-Code-Execution.html)
- [https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit](https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit)

This vulnerability affects:

- Windows 2000
- Windows XP
- Windows 7
- Windows 8
- Windows Server 2000 up to 2012 R2

---

## How to protect yourself

If you still haven't updated your system, you should probably do it right away. If for some reason you aren't able to apply updates, consider disabling SMB protocols.

To disable **SMBv1**, **SMBv2**, and **SMBv3** under **Windows 8** and **Windows Server 2012**, run the following PowerShell commands:

```powershell
Set-SmbServerConfiguration -EnableSMB1Protocol $false
Set-SmbServerConfiguration -EnableSMB2Protocol $false
```
