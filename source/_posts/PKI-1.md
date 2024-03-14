---
title: PKI 1
date: 2023-01-07 22:55:52
tags:
    - Security
    - Architecture
---

### 简介
PKI（Public Key Infrastructure）是通过使用公钥技术和数据证书来提供信息系统安全服务，并负责验证数字证书持有者身份的一种体系。PKI 基础设施采用证书管理公钥，通过第三方可信任认证中心，把用户的公钥和用户的身份信息捆绑在一起，它是具有通用性的安全基础设施，是一套服务体系。


### 架构
{% asset_img pki_system.png link list %}

- CA(Certificate Authority)
CA是PKI的信任基础，是一个用于签发并管理数字证书的可信实体。其作用包括：发放证书、规定证书的有效期和通过发布CRL确保必要时可以废除证书。

- RA(Registration Authority)
RA是CA的延伸，可作为CA的一部分，也可以独立。RA功能包括个人身份审核、CRL管理、密钥对产生和密钥对备份等。PKI国际标准推荐由一个独立的RA来完成注册管理的任务，这样可以增强应用系统的安全性。

- CRL(Certificate Revocation List)
由于用户姓名的改变、私钥泄漏或业务中止等原因，需要存在一种方法将现行的证书撤消，即撤消公开密钥及相关的用户身份信息的绑定关系。在PKI中，所使用的这种方法为证书废除列表。任何一个证书被废除以后，CA就要发布CRL来声明该证书是无效的，并列出所有被废除的证书的序列号。CRL提供了一种检验证书有效性的方式。

当一个CRL的撤消信息过多时会导致CRL的发布规模变得非常庞大，且随着CRL大小的增加，网络资源的使用性能也会随之下降。为了避免这种情况，允许一个CA的撤消信息通过多个CRL发布出来。CRL片断也称为CRL发布点。

### 云安全框架
{% asset_img pki_use_arc.jpeg link list %}




