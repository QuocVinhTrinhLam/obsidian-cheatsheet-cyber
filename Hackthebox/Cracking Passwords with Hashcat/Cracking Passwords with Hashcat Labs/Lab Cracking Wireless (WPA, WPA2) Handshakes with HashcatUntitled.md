# [Cracking Wireless (WPA, WPA2) Handshakes with HashcatUntitled](Cracking%20Wireless%20(WPA,%20WPA2)%20Handshakes%20with%20HashcatUntitled.md)
### Perform MIC cracking using the attached .cap file.

```sh
/usr/bin/hcxpcapngtool -o corp_question1-01.hc22000 corp_question1-01.cap
```

![](Screenshot%202026-10-01%20at%2008.21.15.png)

```sh
hashcat -m 22000 corp_question1-01.hc22000 /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.22.34.png)
### Extract the PMKID hash from the attached .cap file and crack it.

```sh
hcxpcapngtool --pmkid=pmkid.hash -o pkmid.hash cracking_pmkid_question2.cap
```

```sh
hashcat -m 22000 pmkid.hash /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2008.30.04.png)