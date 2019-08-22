---
title: https常见错误
date: 2019-08-22 10:41:08
tags:
---

- NET::ERR_CERT_DATE_INVALID 网站的 ssl 证书有效期过期导致的
- NET::ERR_CERT_COMMON_NAME_INVALID 访问的域名和证书绑定的域名不一致导致
- NET::ERR_CERT_AUTHORITY_INVALID 使用了自签证书或者已经被吊销的根证书导致
- NET::ERR_CERT_REVOKED 证书文件已经被吊销导致
- NET::ERR_SSL_PINNED_KEY_NOT_IN_CERT_CHAIN 服务器提供的证书与内置预期证书不匹配
- NET::ERR_CERT_WEAK_SIGNATURE_ALGORITHM 网站使用已经过期的 SHA1 算法的中间证书
- SEC_ERROR_EXPIRED_CERTIFICATE 网站的 SSL 证书有效期过期导致的
- SSL_ERROR_BAD_CERT_DOMAIN 使用了自签证书或者已经被吊销的根证书导致，请在合法的 CA 申请 SSL 证书
- SEC_ERROR_UNKNOWN_ISSUER 使用了自签证书或者已经被吊销的根证书导致，请在合法的 CA 申请 SSL 证书
- SEC_ERROR_REVOKED_CERTIFICATE 证书文件已经被吊销导致
- MOZILLA_PKIX_ERROR_KEY_PINNING_FAILURE 服务器提供的证书与内置预期证书不匹配
- SSL_ERROR_NO_CYPHER_OVERLAP 网站使用了不受支持的协议配置证书的加密套件和加密算法不浏览器支持
