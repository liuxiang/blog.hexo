title: spring设置跨域支持
date: 2017-3-20 00:00:02
tags: [spring ]


---


- Access-Control-Allow-Origin



# 基础处理
` response.setHeader("Access-Control-Allow-Origin", "*"); `

```
String resultOut = JSON.toJSONString(result, SerializerFeature.DisableCircularReferenceDetect);// 原模型响应
response.setHeader("Access-Control-Allow-Origin", "*");
response.setCharacterEncoding("utf-8");
response.setContentType("application/json");
PrintWriter out = null;
try {
    out = response.getWriter();
} catch (IOException e) {
    e.printStackTrace();
}
if (out != null) {
    out.print(resultOut);
    out.close();
}
```


# 过滤器处理`CorsFilter`
见： `从MVC到前后端分离 - 梦想通 - 博客园`


例：
```
<filter>
<filter-name>DomainFilter</filter-name>
<init-param>
<param-name>access.control.allow.origin</param-name>
<param-value>*</param-value>
</init-param>
<filter-class>com.wosai.teach.filter.DomainFilter</filter-class>
</filter>


<filter-mapping>
<filter-name>DomainFilter</filter-name>
<servlet-name>spring servlet</servlet-name>
</filter-mapping>
```
```
@Component
public class DomainFilter implements Filter {


private String AccessControlAllowOrigin;


@Override
public void destroy() {
}


@Override
public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
throws IOException, ServletException {
HttpServletResponse response = (HttpServletResponse) res;
response.setHeader("Access-Control-Allow-Origin", AccessControlAllowOrigin);
response.setHeader("Access-Control-Allow-Methods", "POST, GET, DELETE, PUT, PATCH");
// response.setHeader("Access-Control-Max-Age", "3600");//缓存时间
response.setHeader("Access-Control-Allow-Headers", "x-requested-with,token,client_version,server_version,platform");
chain.doFilter(req, res);
}


@Override
public void init(FilterConfig config) throws ServletException {
// 从web.xml配置
AccessControlAllowOrigin = config.getInitParameter("access.control.allow.origin");
}


}
```
# 信息转换器处理 `HttpMessageConverter`
?


---
**参考**
`从MVC到前后端分离 - 梦想通 - 博客园`
http://www.cnblogs.com/iOS-mt/p/5667428.html
