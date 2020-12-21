#一、SSL/TLS
SSL(Secure Sockets Layer 安全套接层)及其继任者TLS(Transport Layer Security, 传输层安全)是为网络通信提供安全及数据完整性的一种安全协议。SSL/TLS在传输层与应用层之间对网络链接进行加密。
> HTTPS = HTTP + SSL
> 早期的版本SSL(3.0之后叫做TLS)  
> 现在：TLS  
> 1.0TLS = 3.0 SSL  
> 1.1TLS = 3.1SSL  

![alt image](http://chuantu.xyz/t6/712/1578973566x1033347913.png)

##TLS分析
> SSL两层协议：  
> 1.握手相关(TLS握手协议):
> >    - 握手协议  
> >    - 密码规格变更协议  
> >    - 警告协议  
> >    - 应用数据协议  
> 
> 2.数据记录

![alt image](http://chuantu.xyz/t6/712/1578980794x1033347913.png)