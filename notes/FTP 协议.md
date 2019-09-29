---
title: FTP 协议
created: '2019-09-28T03:53:49.945Z'
modified: '2019-09-29T03:14:15.623Z'
---

# FTP 协议

　谷歌旗下的Chrome浏览器之前将要标注了HTTP类网址为不安全，而近日处于不安全领域的名单又一次增加，那就是大名鼎鼎的FTP网站。事实上，FTP服务被标记不安全并不意外，其安全性相比之下比之前被标记为不安全的HTTP还要差，因此其被关小黑屋也就不奇怪了。

A Secure FTP Alternative
Just like CD players and fax machines, FTP servers are bulky and outdated. They’re difficult for employees to use and time consuming for IT to manage. And the lack of security controls and ongoing maintenance and hardware costs make it insecure and impractical as your business grows. Leave the past behind with Box—the secure alternative to FTP.

01
SHARE FILES EASILY
Enable easy file access, sharing and collaboration while mitigating data loss with centralized security controls, granular sharing permissions and audit logs.

02
ENABLE MOBILE ACCESS
Monitor content usage with comprehensive reporting, protect sensitive data with robust access permissions and enforce application passcodes on mobile devices.

03
SEAMLESS COLLABORATION
Employees can work together in Microsoft Word or Excel. Plus, they'll always have the latest file version on hand and can revert to earlier versions if needed.

04
SIMPLIFY MANAGEMENT
As a fully cloud-based solution, Box eliminates the burden of provisioning new hardware, increasing storage and maintaining network infrastructure.


Today's data\-driven world is demanding, requiring accuracy, speed, integrity and above all \-\- security. It's a tall order to fill, and in the past, many organizations relied heavily on the legacy FTP protocol to transmit files. But over time, FTP alternatives have become necessary as the security of this method has been tested by hackers.

![FTP alternatives include SFTP, HTTPS, AS2, and MFT](https://static.goanywhere.com/img/blog-images/2016/06/GAMFTftp-300x300.png)For example, a serious breach occurred at Yale University in 2001, when more than [43,000 user IDs](http://www.computerworld.com/article/2510557/security0/yale-warns-43-000-about-10-month-long-data-breach.html) were exposed and all data was carefully harvested from an FTP server. [Acer customer](http://www.itnews.com.au/news/acer-loses-40k-customer-details-in-security-blunder-259638) details were stolen in a similar fashion the same year. And most recently, [7,000 FTP sites](http://www.computerworld.com/article/2487767/cybercrime-hacking/hackers-circulate-thousands-of-ftp-credentials--new-york-times-among-those-hit.html) had their credentials circulated in underground forums, including an FTP server run by The New York Times.

Security and file transfers are a significant concern for IT security professionals, but what is the best way to safeguard your company's data?

## Leveraging More Secure Options

As many organizations have evolved past traditional FTP, they are opting for modern and secure FTP alternatives for transmitting data, including:

### **#1 \- SFTP**

Also known as FTP over SSH, [SFTP](https://www.goanywhere.com/blog/2016/07/08/managed-file-transfer/connectivity/sftp) brings down the risk during data exchange by using a secure channel between computer systems to prevent unauthorized disclosures during transactions. Authentication of an SFTP connection involves a user id and password, SSH keys, or using both. It is also firewall friendly, only needing a single port number to be opened.

### **#2 \- HTTPS**

Many sites are gravitating to [HTTPS](https://www.goanywhere.com/blog/2016/07/08/managed-file-transfer/connectivity/https) instead of the traditional HTTP, but what are the major differences? For starters, traditional HTTP doesn't encrypt traffic to your browser, which poses a security risk. In contrast, HTTPS provides an added encryption layer using Transport Layer Security (TLS). This creates a secure channel so the integrity of the data is not changed without your knowledge. HTTPS is ideally suited for file transfers where a trading partner requires a simple, browser\-based interface for uploading data.

### **#3 \- AS2**

This is a popular method for transporting EDI data safely and reliably over the Internet. The [AS2](https://www.goanywhere.com/blog/2016/07/08/managed-file-transfer/connectivity/as2) generates an "envelope" for the data, allowing it to be sent using digital certifications and encryption. For example, Walmart has become well known for [using EDI through AS2](http://www.edibasics.com/edi-by-industry/the-retail-industry/) and has played an important role in driving adoption in the retail industry.

### **#4 \- MFT**

A method that supports the above options and makes FTP more secure is [managed file transfer](https://www.goanywhere.com/blog/2016/07/08/solutions/managed-file-transfer "Managed File Transfer Solution") (MFT). This secure option streamlines the exchange of data between systems, employees, and customers. Numerous protocols and encryption standards are supported, and MFT provides extensive security features that meet strict security policies to comply with PCI DSS, HIPAA, GLBA, and other regulatory requirements.

MFT solutions provide advanced authentication and data encryption to provide secure and reliable file transfers. You can also track user access and transfer activity through reporting features.

More so, managed file transfer can meet other problems or headaches you might have, including the need for automatic file encryption, workflow and project automation, file transfer monitoring and notifications, and easy, secure file sharing with third\-party vendors and trading partners.

Overall, managed file transfer offers the best option as a secure FTP alternative for managing the transfer of data quickly, efficiently with detailed audit trails. It's preventive, rather than reactive, which is what security professionals in today's environment need most. And it is supported on many platforms, including on\-premises ([Windows](https://www.goanywhere.com/blog/2016/07/08/platforms/windows) and [Linux](https://www.goanywhere.com/blog/2016/07/08/platforms/linux) ), in the cloud (Azure and AWS), and within hybrid environments, so you can securely transmit files from anywhere at any time.

## Why replace FTP?

FTP first came into existence in the 1970's, at a time when people who developed solutions for the Internet were more focused on functionality and weren't really as concerned in security. That was expected. There weren't many threats to data then (or any at all). But times have changed. Today, Internet\-based threats abound and we've now come to realize how vulnerable archaic systems like FTP actually are.

First of all, FTP transmits files in plaintext. This means, FTP file transfers are vulnerable to eavesdropping. Attackers can carry out man\-in\-the\-middle attacks and use [packet sniffers to grab confidential information](https://www.jscape.com/blog/bid/91906/Countering-Packet-Sniffers-Using-Encrypted-FTP) like usernames and passwords. They can then use those login credentials to gain access into the server.

Secondly, FTP doesn't have any built\-in data integrity mechanism. You'll need to integrate third party solutions in order to perform integrity checks on your transmitted files. Its lack of security mechanisms is the main reason why FTP is unsuitable for today's business file transfers. That explains why laws and regulations like [HIPAA](https://www.jscape.com/blog/bid/75489/Guide-to-HIPAA-Compliant-File-Transfers-Part-1) and [PCI\-DSS](https://www.jscape.com/blog/bid/75802/Guide-to-PCI-DSS-Compliant-File-Transfers-Part-1) discourage covered entities from using it.

But why do people stick with FTP? Aside from its ubiquity, FTP's ability to transfer large files or a large number of files is one of the major reasons why FTP servers are still common in B2B communications. If you're ever going to replace FTP, it has to be with a file transfer protocol that has the same capabilities as FTP but doesn't have the security deficiencies. So if we can't use FTP, what are the possible options?

We believe these are the top choices to consider:

## FTPS

FTPS (FTP\-SSL, where SSL stands for Secure Sockets Layer) is essentially a secure upgrade of FTP, so it retains all of FTP's functionality but gains a bunch of security features. Introduced in [RFC 2228](http://www.ietf.org/rfc/rfc2228.txt), FTPS gets its security features from SSL. Through SSL, FTPS provides:

*   client and server authentication through [digital certificates](https://www.jscape.com/blog/an-overview-of-how-digital-certificates-work),
*   data privacy through a combination of [symmetric and asymmetric encryption](https://www.jscape.com/blog/bid/84422/Symmetric-vs-Asymmetric-Encryption), and
*   data integrity checking through message authentication code (MAC) algorithms.

FTPS is great for businesses who still can't totally get rid of FTP and hence need to have some "backward compatibility" with FTP servers. This can be done through the [explicit mode of FTPS](https://www.jscape.com/blog/bid/82991/Choosing-Between-SSL-Implicit-Explicit-and-Forced-Explicit-Modes), which still runs on port 21.

## SFTP

Unlike FTPS, SFTP is an entirely different protocol. It's based on SSH, where it draws its security functions. Like FTPS, SFTP also supports client and server authentication, data privacy, and data integrity checking.

SFTP operates on a single port (port 22). This makes it more firewall friendly than FTPS, which has to operate on two channels (a command channel and a data channel). The presence of those two channels and their modes of operation (active and passive) can cause firewall issues. Read the article "[Active v.s. Passive FTP Simplified \- Understanding FTP Ports](https://www.jscape.com/blog/bid/80512/Active-v-s-Passive-FTP-Simplified)" to understand what I mean.

Recommended post: [Business Benefits Of An SFTP Server](https://www.jscape.com/blog/benefits-of-an-sftp-server)

## AS2

[AS2](https://www.jscape.com/blog/topic/as2) is an entirely different creature altogether. It's common in industries that use [EDI](https://www.jscape.com/blog/what-is-edi) in their B2B transactions. AS2 typically runs over HTTP/S. Since HTTPS is also protected by SSL, it therefore has all the security features we mentioned above for FTPS. In addition, it's also equipped with what is known as an [MDN](https://www.jscape.com/blog/bid/100671/What-is-an-AS2-MDN), a sort of electronic return receipt that (if activated) the sender gets after the recipient receives the EDI document.

Electronic receipts can be quite handy in EDI transactions because what is being exchanged are business documents like invoices, purchase orders, ship manifests, price information, patient information, health care claims, and many others. Hence, sending parties need to ascertain that the document was received by the intended recipient. Because MDNs can be [digitally signed](https://www.jscape.com/blog/what-is-a-digital-signature), they allow trading partners to enforce non\-repudiation.

We've included AS2 here because some businesses who carry out [EDI transmissions](https://www.jscape.com/blog/edi-transmission) actually employ FTP. So if you're using FTP to exchange EDI messages, you might want to seriously consider replacing it with the more appropriate AS2.

If you're operating in Europe, you might also want to check out [OFTP](https://www.jscape.com/blog/oftp-odette-file-transfer-protocol). Like AS2, it's best known for transmitting EDI documents, except that majority of its user base is found in Europe.

## HTTPS

As mentioned, AS2 runs over HTTPS. And yes, HTTPS is also a good alternative to FTP. Because admins usually set firewalls to allow HTTP/S traffic, HTTPS\-based file transfers can likewise flow freely. In addition, HTTPS can be quite appealing to end users because they can use popular Web browsers like Chrome, Firefox, or Internet Explorer instead of file transfer clients, which most of them are unfamiliar with.

HTTP actually has an extension that's also a good FTP altnerative. It's called [WebDAV](https://www.jscape.com/blog/what-is-webdav). You might also want to check that out.

## All FTP alternatives in one solution

All of the protocols listed above are better than FTP, particularly security\-wise. And because they practically have the same bulk file transfer capabilities as FTP, they're also all suitable for business use. But at the end of the day, it really boils down to the question of interoperability. Which of these protocols do your suppliers, customers, and trading partners use? You can't insist on FTPS if all of them are using SFTP.

Unless your company is big enough to dictate the terms, you'll likely need to adapt. Does that mean you have to set up multiple file transfer servers? Not necessarily. There's an easier way. You can use a multi\-protocol managed file transfer server like [JSCAPE MFT Server](https://www.jscape.com/products/file-transfer-servers/jscape-mft-server). JSCAPE MFT Server supports all major file transfer protocols, including: FTPS, SFTP, AS2, OFTP, HTTPS, WebDAV and even insecure protocols like FTP and HTTP.

A single multi\-protocol solution will allow you to easily interoperate with all your trading partners without requiring a significant increase in administrative overhead. JSCAPE MFT Server comes with a free, fully\-functional evaluation edition, so you can try it out now.



　　我们经常使用的文件传输协议（FTP），早在1985年的时候就已经是发布了，我们使用过FTP都会知道，它是一个跨平台的，简单又可以实现的一种文件协议，FTP到现在已经有一段历史了，曾经是[互联网](http://www.kokojia.com/s1791/ "互联网")中最重要的应用之一，但是也是存在众多的不足，从而导致逐步的走向灭亡，下面我们来看看它主要是有哪些不足的缺点。

![原来FTP传输已经是逐步走向灭亡_FTP协议_文件传输_系统运维_课课家](http://www.kokojia.com/Public/images/upload/article/2016-08/57ad7d01e3f1a.jpg)

　**　1.数据传输模式不合理**

首先不考虑文件本身的内容，它总是一味的去使用ASCII模式传输数据这显然是不合理的。文件传输协议（FTP）应该具有自动检测功能，当然用户也是可以进行一个自定义，虽然现在许多的[Linux](http://www.kokojia.com/list/273.html "Linux")和[Windows](http://www.kokojia.com/list/274.html "Windows")客户端它是已经支持自动传输模式，但多达数代的UNIX和Windows客户端都默认使用ASCII传输模式，这种传输模式的话，那么将会是造成文件损坏。

**　　2.工作方式设计不合理**

　　FTP可以在主动模式（PORT）或被动模式（PASV）下工作，这也就是决定了数据连接建立的方式，在主动模式下，客户端首先向[服务器](http://www.kokojia.com/s1029/ "服务器")端发送IP地址和端口号，然后等待服务器端建立TCP链接。在被动模式下，客户端同样首先建立到服务器的链接，但服务器端会开启一个端口（1024到5000之间），等待客户端传输数据，文件传输协议（FTP）中最让人觉得非常奇怪的是，客户端将会是可以侦听服务器端！

**　　3.FTP与防火墙工作不协调**

　　在FTP诞生在网络地址转换（NAT）和防火墙之前，在当时的网络还不存在恶意的一个攻击，而在今天大多数最终用户的IPv4地址已不可路由，最基本的原因就是防火墙的使用和IPv4地址出现的短缺，这一点我们应该是非常的清楚的。

　　与防火墙工作的不协调，那么这样会对FTP意味着什么呢？其实这就意味着如果FTP客户端IP地址不可路由的话，又或者位于防火墙之后，那么就只能使用被动传输模式进行数据传输，如果服务器端的IP地址也是不可路由的话，或者位于防火墙之后呢？那么FTP将会是无法进行数据的传输！

　　而到了现在，许多防火墙它只适用于NAT的环境，可以使用一些特殊的技巧（hacks）允许FTP在防火墙之后正常工作。但是必须是要对防火墙进行一个配置才可以使用。

**　　4.密码[安全](http://www.kokojia.com/s2525/ "安全")策略不完善**

　　其实在互联网早期的时候，文件传输协议（FTP）并没有对密码安全作出规定。在FTP客户端和服务器端，数据以明文的形式传输，任何对通讯路径上的路由具有很强控制能力的人，那么都是可以探取你的密码和数据，这样的数据传输也是非常的不安全的。

　　当然我们也是可以使用SSL封装FTP，但是因为FTP是通过建立多次链接进行数据传输的，即使我们保护了密码安全，这也是很难保护数据传输的安全性，数据的传输还是有一定的风险的，这一点我们是要知道的，因此我们是推荐使用SCP取代FTP进行文件传输，这样将会是更加的安全。

![ftp服务器](http://www.kokojia.com/Public/images/upload/article/2016-08/57ad7d395ed3a.jpg)

**　　5.为什么说FTP协议效率低下**

　　因为我们可以发现如果是要从FTP服务器上检索一个文件的时候，包含反复的交换握手步骤：

　　1.客户端建立到FTP服务器端控制端口的TCPSocket链接，并等待TCP握手完成

　　2.客户端等待服务器端发送回执

　　3.客户端向服务器端发送用户名并等待响应

　　4.客户端向服务器端发送密码并等待响应

　　5.客户端向服务器端发送SYST命令并等待响应

　　6.客户端向服务器端发送TYPEI命令并等待响应

　　我们会发现如果用户需要在服务器端切换目录，那么客户端仍然发送命令并等待响应

　　一般在主动模式下，客户端需要发送PORT命令到服务器端，然后等待响应（如果是被动模式则与主动模式相反）

　　建立数据传输链接（这就需要经过三次握手，建立一条TCPSocket连接）

　　通过链接传输数据

　　客户端等待服务器端从控制连接发送2xx指令，以确保数据传输成功

　　客户端发送QUIT命令，并等待服务器响应

　　**针对上面同样的情形，我们来看看HTTP协议：**

　　1.HTTP客户端向HTTP服务器端建立一条TCPSocket连接

　　2.HTTP客户端向HTTP服务器端发送GET命令，包含URL、HTTP协议版本、虚拟主机名等等，并等待响应

　　3.HTTP服务器端的响应包含了所有想要的数据，完成！

　　通过上面的解析我们就知道，如果是要去传输一个文件，用FTP需要往复10次，而使用HTTP只需要2次！如果传输多个文件的话，那么FTP可以省略发送用户名和密码的的一个步骤，而HTTP则可以使用固定的套接字（Socket），在相同的TCP连接中传输文件。

　　结语：从上面的例子我们可以看出，FTP的使用曾经是风靡一时，但是综合上面所说的缺点，在现在来说已经是过时的表现了，在安全方面也是有众多的不足，而且效率也是比较低的，因此这些传输协议势必会被淘汰掉。


