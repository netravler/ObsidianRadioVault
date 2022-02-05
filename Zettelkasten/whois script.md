whois bash script

```
input="test.txt"
pheintz@Lenovo-PC:/mnt/c/Users/user$ while IFS= read -r line
 do
   echo -e "\n\n=====$line=====\n"
   curl "https://api.whoapi.com/?apikey=fa26b6ca2dc0e42a68f30588f103a076&r=ipwhois&domain=&ip="$line"" json_pp -json_opt pretty,canonical
 done < "$input"
```