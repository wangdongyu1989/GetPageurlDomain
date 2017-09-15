
```python
#!/usr/bin/env python
# encoding=utf-8

import sys
import urllib                                                                                                                       
from urlparse import urlparse

tldFile = 'data/tld.dat'

def singleton(cls, *args, **kw):
    instances = {}

    def _singleton():
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return _singleton

@singleton
class Url(object):

    def __init__(self):
        self.tld = {}
        tldFp = open(tldFile, 'r')
        for line in tldFp:
            self.tld[line.strip().lower()] = 1
        tldFp.close()

    def getHost(self, url):
        urlResult = urlparse(url)
        return urlResult.netloc

    def getDomain(self, url):
        host = self.getHost(url)
        if host is None:
            return None
        parts = host.split('.')
        if len(parts) < 2:
            return None
        domain = None
        if parts[-2] in self.tld:
            domain = '.'.join(parts[-3:])
        else:
            domain = '.'.join(parts[-2:])
        return domain

shell example ：
        echo "http://www.baidu.com:80/ABCD/a.txt" | awk -F'[/:]' '{print $4}'
        # 输出：www.baidu.com


def readFile(File):
    data = {}
    with open(File, 'r') as fp:
        for line in fp:
            data[line.strip()] = True
    return data

def readColumn(line, sep, num):
    data=line.strip().split(sep)[num]
    return data

def readColumnNum(line, sep):
    data=line.strip().split(sep)
    return len(data)

def main():
    
    ProcessFile = sys.argv[1]

    inputurl=""
    output_picurl=""
    output_pageurl=""
    picurl_code=""
    pageurl_code=""
    pageurl_domain=""
    with open(ProcessFile, 'r') as fp:
        for line in fp:
            if "input url is" in line:
                inputurl=readColumn(line," ",4)
            else :
                try:
                    columnnum=readColumnNum(line," ")
                    if columnnum<4 :
                        continue
                    output_picurl=readColumn(line, " ", 0)
                    picurl_code=readColumn(line, " ", 1)
                    output_pageurl=readColumn(line, " ", 2)
                    pageurl_code=readColumn(line, " ", 3)
                    pageurl_domain=Url().getDomain(output_pageurl)
                    output=inputurl+" "+output_picurl+" "+output_pageurl+" "+pageurl_domain+" "+picurl_code+" "+pageurl_code
                    print output.strip()
                except Exception, e:
                    pass
                        
if __name__ == '__main__':
    main()  
```
