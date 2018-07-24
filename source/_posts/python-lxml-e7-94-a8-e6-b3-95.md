---
title: python lxml用法
tags:
  - python
url: 681.html
id: 681
categories:
  - python
date: 2017-11-09 15:45:01
---

from lxml import etree

text = '''<?xml version="1.0" encoding="utf-8"?>
<env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/">
   <env:Body>
      <m:reply xmlns:m="http://www.crifst.ac.cn/2013/OTSAPI">
         <m:msg>
             <?xml version="1.0" encoding="utf-8"?>
            <OnlineTicketingServiceQuery Version="1.0" Datetime="2013-01-01T00:00:00" Username="wenquan" Password="wenquan1234">
               <QueryCinema CinemaCode="12345678" Id="ID_QueryCinema"/>
                    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                        <ds:SignedInfo>
                            <ds:Reference URI="#ID_QueryCinema">
                                <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                                <ds:DigestValue>
                                    12345678
                                </ds:DigestValue>
                            </ds:Reference>
                        </ds:SignedInfo>
                        <ds:SignatureValue>
                            qwertyuiop
                        </ds:SignatureValue>
                    </ds:Signature>
            </OnlineTicketingServiceQuery>
         </m:msg>
      </m:reply>
   </env:Body>
</env:Envelope>
'''

  

以上格式不是标准的xml格式，先把前后两部分多余的字符串处理掉：

 text = text.split("<m:msg>")\[1\]
 text = text.split("</m:msg>")\[0\].strip()#注意1中问题

root = etree.fromstring(text.encode())

print(root.attrib)#返回所有属性
body = root\[\]
print(body.get())#get()根据属性名称返回

  

返回：

{'Version': '1.0', 'Datetime': '2013-01-01T00:00:00', 'Username': 'wenquan', 'Password': 'wenquan1234'}  

12345678  

  

  

  

注意1:在处理xml数据时出现下面问题：

lxml.etree.XMLSyntaxError: XML declaration allowed only at the start of the document

  

这段话的意思是：XML声明仅允许在文档的开始

解决方法是将xml内容紧跟三引号开头