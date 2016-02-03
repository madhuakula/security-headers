# security-headers

---

### List of headers

- strict-transport-security
- x-xss-protection
- x-frame-options
- public-key-pins
- x-permitted-cross-domain-policies
- x-content-type-options
- x-download-options
- x-powered-by
- server
- content-type
- content-security-policy

##Headers Information

[https://blog.srcclr.com/http-secure-headers-in-plain-english/](https://blog.srcclr.com/http-secure-headers-in-plain-english/)

### Security Headers

> Whenever a browser requests a page from a web server, the server responds with the content along with “headers”. Some headers contain content meta-data, such as when the page was last modified. Others include the web server name and version, cookies, if the browser should cache the content, etc. HTTP security headers are headers that let you tell your customer’s browser how to behave when handling your site’s content. For example, a header might specify “do not open me inside of a frame” or “only talk to me over HTTPS”.


####Strict-Transport-Security

> When a browser sees this header, it always assumes it should talk to the server over HTTPS. Using HTTPS with a proper certificate allows the browser to trust that it’s talking to the correct server and any information between it and the server is encrypted, so it can’t be eavesdropped.

*Example :* `Strict-Transport-Security: max-age=31536000; includeSubDomains`

> This tells the browser to only only use HTTPS when talking to the server and all of sub domains of the site for the next year.

*Recommendation :*

> This header is highly recommended. Always requiring HTTPS protects against users not including `https:` when typing the URL in the address bar and if any link on your site accidentally includes `http:` rather than `https:`. It also may [improve your page ranking](https://googlewebmastercentral.blogspot.com/2014/08/https-as-ranking-signal.html). For most sites, adding it is low risk.


####X-Frame-Options

> This header tells the browser when it is OK to embed the site in a frame or object on another site. If your site is embedded on another page, it may be possible for the owners of the other page to trick your users into doing things without realizing it. This is called Clickjacking.
<br><br>
> An attacker may create a page which embeds your site but makes it invisible. The attacker could then claim a button on the page they control does something, like give free money. But actually the user is clicking a link or button on your site.

*Example :* `X-Frame-Options: deny`

*Recommendation :*

> This header is recommended. There is a good chance your website does not need to be embedded on another site.


####X-XSS-Protection

> This header controls the built in cross site scripting (XSS) protection for Chrome and Internet Explorer 8+. When it detects an attack, it tries to alter the site so it is not vulnerable. This is the default behavior, and it’s possible that this breaks the normal operation of the site. Using this header, one can disable the protection, or change the behavior so a request that looks like an attack is blocked instead of changed.

*Example :* `X-XSS-Protection: 1; mode=block`

*Recommendation :*

> This header is recommended, but do not rely on it heavily. Because of how Internet Explorer’s XSS protection works, it’s possible for an attacker, in some situations, to [make a site vulnerable to XSS that would not otherwise be vulnerable](http://p42.us/ie8xss/Abusing_IE8s_XSS_Filters.pdf). Despite this, this header may still protect against many types of traditional XSS attacks.


####X-Content-Type-Options

> This header tells Chrome and Internet Explorer if they should try to figure out the type of file the server is sending. File types are things like image, HTML, JavaScript, etc, and they are all handled differently. Some sites and web servers may not say what the file type is, or the file type they give is not recognized by the browser. This can cause the site to not work correctly. Because of this, some browsers started trying to guess what the file type was by looking at the beginning of the file. This way, the browser could handle the file according to what it actually contained rather than what the server said it was. This is called MIME Sniffing.
<br><br>
> Letting the browser determine how to handle a file becomes a problem when users are allowed to upload files and other users can to see or download them. Consider a site that allows users to upload images. To prevent non-images from being uploaded, the site only allows file names that end with `.jpeg` or `.gif`. An attacker could create a malicious HTML file, rename the extension from `.html` to `.jpeg` and upload the page. If a user attempts to open the uploaded file, instead of trying to treat it as an image, the user’s browser would treat it as HTML. The result is a cross site scripting attack.

*Example :* `X-Content-Type-Options: nosniff`

*Recommendation :*

> Disabling MIME sniffing may not be necessary if your site doesn’t handle user uploads. Also, this may alter the behavior of your site and should be tested carefully.


####Public-Key-Pins

> This directive tells browsers to only trust certain identities when talking to a web server. The browser caches these identities after first receiving them. This way, if the traffic is intercepted and the identity changes, the browser knows to reject the connection.

*Example :* `Public-Key-Pins: pin-sha256="Z3pGTSOuJeEwV189IJ/cEtXUEmy52zs1TZQrU06KUKg="; pin-sha256="XrJYVuhihUrJcxW7wcqyOISTXIsInzdj3xK8QrZbHec="; max-age=2592000; includeSubdomains;`

> The `max-age` tells the browser to cache the setting for that many seconds. In this case, 2592000 seconds is 30 days.
<br><br> 
> A good guide for setting this up for yourself is here: [HPKP: HTTP Public Key Pinning](https://scotthelme.co.uk/hpkp-http-public-key-pinning/)

*Recommendation :*

> This adds an extra layer of protection on top of HTTPS by protecting against compromised certificate authorities. It requires a little more effort to set up than other headers. Apart from that, there are few other negative implications.

####X-Powered-By

> This says what technology is used to render the page. This could be PHP, ASP.NET, JBoss, and so on.

*Example :* `X-Powered-By: PHP/5.2.17`

*Recommendation :*

> This header is informational and is not necessary. The less information you provide attackers, the harder it is for them to target your systems. This isn’t giving away much, but if you don’t need it for anything, it’s good to remove.


####Server

> This header shows what webserver is being used by the server. This could be Apache, Microsoft IIS, nginx, and so on.

*Example :* `Server: Microsoft-IIS/7.5`

*Recommendation :*

> Similar to X-Powered-By; removing this header prevents unnecessary information about server configuration from being disclosed.


####Content-Type

> This tells the browser what kind of document is being sent. It could be text, json, images, media, etc.

*Example :* `Content-Type: text/html; charset=utf-8`

*Recommendation :*

> The HTML5 specification encourages developers to include charset=utf-8 for text and HTML content types. If the content type is `text/html` and `charset` is not defined, it’s possible some characters might not be displayed properly on the browser.

####Content Security Policy

> The Content Security Policy (CSP) is another HTTP header but it’s complex enough to warrant it’s own section. Creating a good, useful CSP requires a lot of fine tuning and may require a lot of changes in the code. Compared to the other security headers, the CSP is much more powerful and flexible. Some good references for implementing a CSP are [Twitter’s blog](https://blog.twitter.com/2013/csp-to-the-rescue-leveraging-the-browser-for-security) and [Dropbox’s blog](https://blogs.dropbox.com/tech/tag/content-security-policy/).
<br><br>
> The various options are called directives. Each directive takes a list of “sources”. A source is usually a domain name. As with other security header, each one of these should be carefully tested. Luckily, if something breaks, most browsers will tell you exactly what should be changed.

####default-src

> This is the default list of sources. The values here are inherited to all other options that take a list of sources. A common value is `default-src none;` and to explicitly set source lists for each directive.

####script-src

> The browser will only load JavaScripts from sources in this list. A common value is `script-src 'self';` or `script-src scripts.mydomain.com;`. By default, inline JavaScript is disabled and must be explicitly enabled with `unsafe-inline`.

####style-src

> The browser will only load Cascading Style Sheets (CSS) from sources in this list. Similar to `script-src`, inline CSS is disabled by default and must be explicitly enabled with `unsafe-inline`.

####img-src

> Images will only be loaded from these sources.


####connect-src

> The browser will only connect to other sites in this source list for things like WebSockets, EventSource, and Ajax requests.

####font-src

> Fonts will only be loaded from these sources.

####child-src

> Only sites in the source list can be embedded in a frame such as `<frame>` or `<iframe>`.

####object-src

> valid sources of plugins like object, embed and applet This tells the browser where plugins and embedded applications like Java applets may be loaded.

####media-src

> Media will only be loaded from these sources. This includes stuff in HTML5’s `<audio>` and `<video>` tags.

####frame-ancestors

> This is similar to the `X-Frame-Options` header. It allows you to control where your site may be embedded as a frame. Unlike `X-Frame-Options` however, you can control which domains allowed to embed the site, instead of just blocking all of them.

####report-uri

> This is an important directive which tells the browser where to send any failure reports. These reports can be monitored and reviewed periodically to ensure the CSP isn’t breaking anything. An example value is: 

`report-uri https://open.srcclr.com/report-uri/fbbc4e69-facd-40b2-aff6-594d3734567c;`

> If you don’t want to go through the trouble of setting up your own endpoint to handle these reports, you can use a tool we developed called [CSP Reports](https://open.srcclr.com/projects/csp-reports). It will provide you with an URL to configure with your CSP and allow you to view any reports sent to it.

### References 

 - [https://github.com/srcclr/security-headers](https://github.com/srcclr/security-headers)
