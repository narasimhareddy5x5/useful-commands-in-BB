# useful-commands-in-BB

**Convert Multiple parameters URLs into single parameter URLs**
```
awk -F'?|=&|=' '{for(i=2;i<NF;i++) print $1 "?" $i "="}'
```
```
$ cat urls.txt
http://example.com/test/test/test?apple=&bat=&cat=&dog=
https://test.com/test/test/test?aa=&bb=&cc=
http://target.com/test/test?hmm=


$ cat urls.txt | awk -F'?|=&|=' '{for(i=2;i<NF;i++) print $1 "?" $i "="}' 
http://example.com/test/test/test?apple=
http://example.com/test/test/test?bat=
http://example.com/test/test/test?cat=
http://example.com/test/test/test?dog=
https://test.com/test/test/test?aa=
https://test.com/test/test/test?bb=
https://test.com/test/test/test?cc=
http://target.com/test/test?hmm=
```

**Convert single or /n parameter URLs into multiple parameters URLs in bash**
```
awk -F? 'NF==1 { s[$1] } NF>1  { split($2,q,"&"); for (i in q) d[$1][q[i]] } END   { for (u in d) {c=""; for (i in d[u]) c=(c "&" i); print u "?" substr(c,2)}; for (u in s) print u }'
```

```
$ cat urls.txt
http://example.com/path1/path2/path3/path4?apple=
http://example.com/path1/path2/path3/path4?bat=
http://example.com/path1/path2/path3/path4?cat=
http://example.com/path1/path2/path3/path4?apple=&cat=
http://example.com/path1/path2/path3/path4?apple=&diff=
http://example.com/path1/path2/path3/path4?cat=&apple=
http://example.com/path1/path2/path3?aa=
http://example.com/path1/path2/path3?bb=
http://example.com/path1/path2?cc=
http://example.com/path1/path2/path3/path4
http://example.com/path1/path2/path3
http://example.com/path1/path2
http://example.com/path1/path2/path3/path4
http://example.com/path1/path2/path3
http://example.com/path1/path2

$ cat urls.txt | awk -F? 'NF==1 { s[$1] } NF>1  { split($2,q,"&"); for (i in q) d[$1][q[i]] } END   { for (u in d) {c=""; for (i in d[u]) c=(c "&" i); print u "?" substr(c,2)}; for (u in s) print u }'
http://example.com/path1/path2/path3?aa=&bb=
http://example.com/path1/path2?cc=
http://example.com/path1/path2/path3/path4?bat=&cat=&apple=&diff=
http://example.com/path1/path2/path3
http://example.com/path1/path2
http://example.com/path1/path2/path3/path4
```

