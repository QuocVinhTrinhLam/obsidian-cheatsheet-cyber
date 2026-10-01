# [Combination Attack](Combination%20Attack.md)
### Using the Hashcat combination attack find the cleartext password of the following md5 hash: 19672a3f042ae1b592289f8333bf76c5. Use the supplementary wordlists shown at the end of this section.

```sh
hashcat -a 1 -m 0 hash wordlist1.txt wordlist2.txt
```

![](Screenshot%202026-09-29%20at%2015.32.18.png)

