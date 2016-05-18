---
layout: post
title: AlphaSSL使用注意事项和碰到的坑
categories: [SSL]
tags: [HTTPS, SSL]
---

全民HTTPS的时代已经到来,先介绍几个免费取得HTTPS的地儿

* StartSSL,最早开放免费申请的。**一年**, **不限个数**
* 阿里云, 用的好像是沃通的。**大陆货,无安全节操**，　**一年**, **每个账号5个**
* Let's Encrypt,开源项目，支持脚本定期续证书。**开源有保障**, **90天**


其实今天要讲的是我现在在用的AlphaSSL的坑,两个

1. 坑一: 关键字*Intermediate certificate*， mobile chrome不兼容
    >以下是同样配置，在PC端生效在移动端报错
        ![](https://66.media.tumblr.com/fc221233cfa491f9115a3aacee0e017f/tumblr_o7ct9nQCqu1r68ev5o1_500.png)
        ![](https://65.media.tumblr.com/6f859d4cd4d93e4a96517687ba725a67/tumblr_o7ct9nQCqu1r68ev5o2_1280.png)




    解决方案

    需要把签发给你的证书然后再加上*AlphaSSL / Wildcard Intermediate certificate*比如：

        -----BEGIN CERTIFICATE-----
        MIIETTCCAzWgAwIBAgILBAAAAAABRE7wNjEwDQYJKoZIhvcNAQELBQAwVzELMAkG
        A1UEBhMCQkUxGTAXBgNVBAoTEEdsb2JhbFNpZ24gbnYtc2ExEDAOBgNVBAsTB1Jv
        b3QgQ0ExGzAZBgNVBAMTEkdsb2JhbFNpZ24gUm9vdCBDQTAeFw0xNDAyMjAxMDAw
        MDBaFw0yNDAyMjAxMDAwMDBaMEwxCzAJBgNVBAYTAkJFMRkwFwYDVQQKExBHbG9i
        YWxTaWduIG52LXNhMSIwIAYDVQQDExlBbHBoYVNTTCBDQSAtIFNIQTI1NiAtIEcy
        MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2gHs5OxzYPt+j2q3xhfj
        kmQy1KwA2aIPue3ua4qGypJn2XTXXUcCPI9A1p5tFM3D2ik5pw8FCmiiZhoexLKL
        dljlq10dj0CzOYvvHoN9ItDjqQAu7FPPYhmFRChMwCfLew7sEGQAEKQFzKByvkFs
        MVtI5LHsuSPrVU3QfWJKpbSlpFmFxSWRpv6mCZ8GEG2PgQxkQF5zAJrgLmWYVBAA
        cJjI4e00X9icxw3A1iNZRfz+VXqG7pRgIvGu0eZVRvaZxRsIdF+ssGSEj4k4HKGn
        kCFPAm694GFn1PhChw8K98kEbSqpL+9Cpd/do1PbmB6B+Zpye1reTz5/olig4het
        ZwIDAQABo4IBIzCCAR8wDgYDVR0PAQH/BAQDAgEGMBIGA1UdEwEB/wQIMAYBAf8C
        AQAwHQYDVR0OBBYEFPXN1TwIUPlqTzq3l9pWg+Zp0mj3MEUGA1UdIAQ+MDwwOgYE
        VR0gADAyMDAGCCsGAQUFBwIBFiRodHRwczovL3d3dy5hbHBoYXNzbC5jb20vcmVw
        b3NpdG9yeS8wMwYDVR0fBCwwKjAooCagJIYiaHR0cDovL2NybC5nbG9iYWxzaWdu
        Lm5ldC9yb290LmNybDA9BggrBgEFBQcBAQQxMC8wLQYIKwYBBQUHMAGGIWh0dHA6
        Ly9vY3NwLmdsb2JhbHNpZ24uY29tL3Jvb3RyMTAfBgNVHSMEGDAWgBRge2YaRQ2X
        yolQL30EzTSo//z9SzANBgkqhkiG9w0BAQsFAAOCAQEAYEBoFkfnFo3bXKFWKsv0
        XJuwHqJL9csCP/gLofKnQtS3TOvjZoDzJUN4LhsXVgdSGMvRqOzm+3M+pGKMgLTS
        xRJzo9P6Aji+Yz2EuJnB8br3n8NA0VgYU8Fi3a8YQn80TsVD1XGwMADH45CuP1eG
        l87qDBKOInDjZqdUfy4oy9RU0LMeYmcI+Sfhy+NmuCQbiWqJRGXy2UzSWByMTsCV
        odTvZy84IOgu/5ZR8LrYPZJwR2UcnnNytGAMXOLRc3bgr07i5TelRS+KIz6HxzDm
        MTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRknl7OedSyps9AsUSoPocZXun4IRZZUw==
        -----END CERTIFICATE-----
        -----BEGIN CERTIFICATE-----
        你自己的
        -----END CERTIFICATE-----
    

        [参考](https://www.ssl2buy.com/wiki/alphassl-intermediate-root-ca-certificates/)


2. 坑二: 证书在Nginx下好用, 但在某些云服务商下不能正常添加
    >这是因为云服务商对证书格式有严格要求

* 把END BEGIN的空行去掉

* 由于证书的最后一行MTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRknl7OedSyps9AsUSoPocZXun4IRZZ*Uw==*
    太长了，在U处换行就行了。

        ...
        MTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRknl7OedSyps9AsUSoPocZXun4IRZZ
        Uw==
        -----END CERTIFICATE-----
        ...
