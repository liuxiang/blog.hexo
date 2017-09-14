title: https证书的双向验证
date: 2017-6-22 00:00:00
tags: [ https ]


---
# 介绍
- ssl双向认证和单向认证的区别

单向认证：只要求站点部署了ssl证书就行，任何用户都可以去访问（IP被限制除外等），只是服务端提供了身份认证。
双向认证：则是需要是服务端需要客户端提供身份认证，只能是服务端允许的客户能去访问

https://zhidao.baidu.com/question/176742669957802364.html


`Https单向认证和双向认证`
http://blog.csdn.net/duanbokan/article/details/50847612



`HTTPS服务器配置`
https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=10_4


`openssl基本原理 + 生成证书 + 使用实例 - oldmtn的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/oldmtn/article/details/52208747


`Https单向认证和双向认证 - duanbokan的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/duanbokan/article/details/50847612  


![](http://img.blog.csdn.net/20160310160503593)

![](http://img.blog.csdn.net/20160310160519781 )
 
---
# 证书合法性检查
`浏览器如何验证HTTPS证书的合法性？`

https://www.zhihu.com/question/37370216?sort=created



`HTTPS 客户端验证 服务端证书流程 - 简书`
http://www.jianshu.com/p/0d59d2216c64


---
# 微信支付-示例


申请退款

https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_4&index=6

```
是否需要证书 
请求需要双向证书。 详见证书使用


https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=4_3

```
```
（2）使用商户证书


◆ apiclient_cert.p12是商户证书文件，除PHP外的开发均使用此证书文件。 
◆ 商户如果使用.NET环境开发，请确认Framework版本大于2.0，必须在操作系统上双击安装证书apiclient_cert.p12后才能被正常调用。 
◆ 商户证书调用或安装都需要使用到密码，该密码的值为微信商户号（mch_id） 
◆ PHP开发环境请使用商户证书文件apiclient_cert.pem和apiclient_key.pem ，rootca.pem是CA证书。 
各版本的调用实例请参考微信支付提供的Demo外链。 


https://pay.weixin.qq.com/wiki/doc/api/download/cert.zip

```


# 客户端带证书访问
- `cert.zip-JAVA版-ClientCustomSSL.java`
```
/*
 * ====================================================================
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing,
 * software distributed under the License is distributed on an
 * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 * KIND, either express or implied.  See the License for the
 * specific language governing permissions and limitations
 * under the License.
 * ====================================================================
 *
 * This software consists of voluntary contributions made by many
 * individuals on behalf of the Apache Software Foundation.  For more
 * information on the Apache Software Foundation, please see
 * <http://www.apache.org/>.
 *
 */
package httpstest;


import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.security.KeyStore;


import javax.net.ssl.SSLContext;


import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.conn.ssl.SSLContexts;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;


/**
 * This example demonstrates how to create secure connections with a custom SSL
 * context.
 */
public class ClientCustomSSL {


    public final static void main(String[] args) throws Exception {
        KeyStore keyStore  = KeyStore.getInstance("PKCS12");
        FileInputStream instream = new FileInputStream(new File("D:/10016225.p12"));
        try {
            keyStore.load(instream, "10016225".toCharArray());
        } finally {
            instream.close();
        }


        // Trust own CA and all self-signed certs
        SSLContext sslcontext = SSLContexts.custom()
                .loadKeyMaterial(keyStore, "10016225".toCharArray())
                .build();
        // Allow TLSv1 protocol only
        SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
                sslcontext,
                new String[] { "TLSv1" },
                null,
                SSLConnectionSocketFactory.BROWSER_COMPATIBLE_HOSTNAME_VERIFIER);
        CloseableHttpClient httpclient = HttpClients.custom()
                .setSSLSocketFactory(sslsf)
                .build();
        try {


            HttpGet httpget = new HttpGet("https://api.mch.weixin.qq.com/secapi/pay/refund");


            System.out.println("executing request" + httpget.getRequestLine());


            CloseableHttpResponse response = httpclient.execute(httpget);
            try {
                HttpEntity entity = response.getEntity();


                System.out.println("----------------------------------------");
                System.out.println(response.getStatusLine());
                if (entity != null) {
                    System.out.println("Response content length: " + entity.getContentLength());
                    BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(entity.getContent()));
                    String text;
                    while ((text = bufferedReader.readLine()) != null) {
                        System.out.println(text);
                    }
                   
                }
                EntityUtils.consume(entity);
            } finally {
                response.close();
            }
        } finally {
            httpclient.close();
        }
    }
}
```