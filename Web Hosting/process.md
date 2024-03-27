## Web Server

```
Installation on the server: Server Manager > Manage > Add roles and features > Add Web Server role.
```

```
We'll also download another browser, for example, https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US.
```

For management, use the **Internet Information Services Manager console. The basic interface shows 2 components of IIS:**

- **Application Pools** — system resources allocated to individual web applications. Separate application pools run their own processes, thus isolating each web application at the system level. This is important for security (an attack on one application cannot easily affect another application in a different application pool), as well as for system resources (a poorly written web application consuming all allocated resources on the server would disrupt other applications' operation).
- **Sites** — individual web applications ("websites") operated in IIS. Each website has its own data structure (e.g., web pages in HTML) and settings.

After installing IIS, the default page is visible either directly from the server via [http://localhost](http://localhost/) or from anywhere in the domain via [http://server.ecbwin.local](http://server.ecbwin.local/) (applies to VM **WinServer2019** and snapshot named "**3.**").

### Some Important Settings:

**Basic Settings**

- Physical Path - the data directory of the web application.
- Part of Basic Settings:

```
IISM > Default Web Site (or any other site) > Edit Site (right sidebar) > Basic Settings…
```

- It should have read permission (or write if necessary for webapp operation) for the IIS_IUSRS group.

**Default Document**

- Specifies filenames considered as the application's starting files — it automatically uses it as the start page (often index.*).
- Can be completely turned off, then it's necessary to enter the exact filename into the browser each time we want to display a page.

```
Test: Remove iisstart.html from Default Document and see what changed.
What do we need to enter into the browser's address bar to see the original IIS page?
```

**Directory Browsing**

- Option to browse the directory structure exposed by the application (potentially exploitable).
- Enable **Directory Browsing — Enable**.

**Error Pages**

- Response to HTTP status codes.
- Standard codes informing about page loading status.
- More info at https://developer.mozilla.org/en-US/docs/Web/HTTP/Status.

**Logging**

- Setting access, error logs, etc.
- Option for custom log files or using Event logs (or both).
- Custom log composition, definition of individual elements.

**Request Filtering**

- Option to set whether the server processes or doesn't process various elements (files, requests, headers, etc.).

```
Filter jpg, png, and bmp files and check the IIS default page again.
```

**SSL Settings**

- Enforces communication over HTTPS.
- HTTPS communication must be set up first.

### Creating a Custom Web

```
Download a template, e.g., from https://www.free-css.com/
```

```
In IISM then Sites > Add Website.
```

**Bindings**

- Definition of interfaces and FQDNs on which the application is available.
- Different applications can have different interfaces, different FQDNs.
- CNAME (DNS alias) can also be used for specifying FQDN.

### Web with Authentication

```
Add the role: Web Server > Web Server > Security > Windows Authentication.
```

```
In IISM: Sites > Web > Authentication > Disable Anonymous + Enable Windows. (To display this setting, IISM must be restarted after role installation).
```

Basic and Digest Authentication can also be used, differing in how they send user data over the network — Windows is the most secure (uses Kerberos), then Digest (encrypted), and Basic is rather insecure (sends data in plaintext).

### Web on Custom Domain

Create a new website running on the domain [www.ebwin.local](http://www.ebcwin.local/)[.](http://www.ecbwin.local./) Use any HTML template.

```
Create a new CNAME "www" for the srv1.ecbwin.local machine in DNS management:
Start > Windows Administrative Tools > DNS > Server > Forward lookup zones > ecbwin.local > Right-click > New Alias
We must set the name (www) and also the target it will point to (server.ecbwin.local).
```

### Web over HTTPS

1. Generate a self-signed certificate — IISM > Server > Server Certificates > Create Self-Signed Certificate…
2. Name (CNAME or FQDN) + Store (Web Hosting)
3. Bindings > Add > Type HTTPS > select SSL Certificate.

Option to obtain a certificate for HTTPS via https://letsencrypt.org/.

Self-signed certificates are not "trustworthy" for browsers. It's possible to distribute the certificate within the domain via GPO.

```
First export the cert from IISM:
IISM > Server > Server Certificates > Export...
```

```
Then create a policy:
Computer Configuration\Policies\Windows Settings\Security Settings\Public Key Policies > Trusted Root Certification Authorities > Import.
```

When using PowerShell, we can generate a self-signed certificate for a different hostname:

```
New-SelfSignedCertificate -DnsName www.mydomain.example -CertStoreLocation cert:\LocalMachine\My
```

## FTP

Installation: Add Web Server role > FTP Server > FTP Service.

For management, use the **Internet Information Services Manager** application.

**Sites > Add FTP Site…**

Instructions for installing an FTP server at https://winscp.net/eng/docs/guide_windows_ftps_server.

1. **Downloading a template from free-css.com and saving it to c:\inetpub\wwwroot:**
   - Visit free-css.com and download a template.
   - Unzip the downloaded file and save the folder to c:\inetpub\wwwroot.

2. **Opening the IIS Manager and creating a new website:**
   - Open "Administrative Tools" and launch "IIS Management Console."
   - Click on "Add website" and fill in:
     - Name: web.inc.
     - Path to the folder where you saved the template.
     - Hostname: www.web.inc (or www.webinc.local).
   - Click "Save."

3. **Creating a self-signed certificate for HTTPS:**
   - Click on the server in the IIS Management Console.
   - Click on "Server certificates" and create a self-signed certificate.

4. **Configuring HTTPS binding:**
   - For HTTP binding, it's not necessary if you correctly set the hostname in the previous step (www.webinc.local).
   - For HTTPS binding, assign the created certificate and use the same name as the hostname (www…).

5. **Configuring DNS for accessing the website:**
   - Open DNS tools and find the domain.
   - Right-click on the domain and select "New Host A AAAA."
   - Enter the name: www and the server's IP address.
   - If using CNAME, specify the path to the FQDN name of the server for the local network.

6. **Verifying the correctness of the settings:**
   - Try pinging the hostname www.web.inc.
   - Open a browser and check if the website www.web.inc (or www.webinc.local) is working.

7. **Resolving security errors (if any appear):**
   - Add the websites to the browser's security settings if security errors occur.

8. **Logging in to the web server:**
   - Log in using the format DOMAIN(WEBINC)\Administrator(user), if required.
![alt text](image.png)