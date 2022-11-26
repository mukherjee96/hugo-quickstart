---
title: 'A Web Security Checklist For Creating Secure Websites'
date: Tue, 21 Jan 2020 19:47:50 +0000
draft: false
tags: ['apache', 'api', 'clickjacking', 'content security policy', 'cookies', 'cors', 'cross origin resource sharing', 'cross site request forgery', 'cross site scripting', 'csp', 'csrf', 'http', 'https', 'iframes', 'nginx', 'ssl', 'Web Development', 'web security', 'wordpress', 'xss']
---

Hello there! As a web developer, I always strive to ensure that my websites are as secure as possible. Therefore, in this article, I have put together a checklist of 9 crucial measures that should be implemented by web developers to ensure their websites are optimally defended. The items in this checklist have been mandated by Mozilla in their Web Security [guidelines](https://infosec.mozilla.org/guidelines/web_security) for all websites and/or web applications. Let's get started!

Index
-----

1.  [Use HTTPS over HTTP](#http-https)
2.  [Redirect From HTTP to HTTPS](#redirection)
3.  [Load Resources Over HTTPS](#resources)
4.  [Enforce HTTP Strict Transport Security](#hsts)
5.  [Use Content Security Policy (CSP)](#csp)
6.  [Secure Your Cookies](#cookies)
7.  [Prevent Clickjacking](#clickjacking)
8.  [Cross-origin Resource Sharing (CORS)](#cors)
9.  [Prevent Cross-site Request Forgery (CSRF)](#csrf)

1\. Use HTTPS over HTTP
-----------------------

Hypertext Transfer Protocol Secure (HTTPS) [encrypts data exchanged](https://geekyminds.co.in/http-https-and-ssl-explained/) between the server and the client. It prevents [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks. In order to use HTTPS, you will need to get an [SSL certificate](https://geekyminds.co.in/http-https-and-ssl-explained/) for your domain.

Serving your website over HTTPS also helps greatly with Google search ranking and SEO in general. Google prefers websites that use HTTPS due to which such sites automatically get preference.

![HTTP vs HTTPS - Web Security Checklist - GeekyMinds](https://geekyminds.co.in/wp-content/uploads/2019/09/http_to_https-1.jpg)

HTTP uses Secure Sockets Layer (SSL) to encrypt data - GeekyMinds

It is mandatory for every website or web API endpoint to use HTTPS and a basic necessity in terms of web security. Depending on which hosting provider you choose for your website or API, you may already have access to a free SSL service. Others will charge you a recurring fee.

2\. Redirect From HTTP to HTTPS
-------------------------------

Installing an SSL certificate for your website is the first step, but not the last. In order to complete the procedure, we have one more step - set up a redirection from HTTP to HTTPS.

Your website will be accessible over HTTP after you start serving over HTTPS. While it is recommended to disable HTTP access from API endpoints, we cannot say the same about websites. This is because web browsers still attempt to initially connect to your website over HTTP.

This still leaves your website and services vulnerable. In order to solve this, you will need to set up redirections. HTTP Redirections are used to redirect the client (in this case the browser) to a different path.

There is a little bit of configuration that has to be programmed into your web server software like Nginx, Apache, etc. For [WordPress users](https://www.wpbeginner.com/beginners-guide/beginners-guide-to-creating-redirects-in-wordpress/), this process is made really easy by plugins.

### Example

#### Using Nginx

The following code configures Nginx to first listen on port 80. Port 80 is the default port for HTTP. Finally, the last statement instructs Nginx to redirect to the HTTPS version of your website. The number 301 is an HTTP status code that stands for "Moved Permanently".

```
server {
  listen 80;
  return 301 https://$host$request_uri;
}
```

#### Using Apache

In Apache, you can either use your `.htaccess` file or VirtualHost. The VirtualHost method is shown below as an example. Find an in-depth guide [here](https://www.tecmint.com/redirect-http-to-https-on-apache/).

```
<VirtualHost *:80>
  ServerName geekyminds.co.in
  Redirect permanent / https://geekyminds.co.in/
</VirtualHost>
```

3\. Load Resources Over HTTPS
-----------------------------

Resources are the various kinds of media (images/videos), scripts, and stylesheets that a website requires for its functioning.

Loading your website's resources over HTTP leaves it vulnerable to cyberattacks like [Phishing](https://geekyminds.co.in/phishing-attacks-safety-tips/). Apart from that, browsers like Chrome and Firefox will show [Mixed Content](https://developer.mozilla.org/en-US/docs/Security/MixedContent) warnings to your site's visitors.

![Loading Resources Over HTTPS - Web Security Checklist - GeekyMinds](https://geekyminds.co.in/wp-content/uploads/2020/01/Resources-over-HTTPS.png)

Always load resources over a secure connection - GeekyMinds

Therefore, it is very important for web security to make sure that such resources are fetched over HTTPS.

#### Example

HTTPS is the recommended way to load a JavaScript resource, as shown below.

```
<script src="https://code.jquery.com/jquery-1.12.0.min.js"></script>
```

Similarly, images/videos should also be loaded over HTTPS.

```
<img src="https://geekyminds.co.in/image.jpg">
```

The same applies to CSS too. For example:

```
<link rel="stylesheet" href="https://cdn.domain.com/css/core.min.css">
```

4\. Enforce HTTP Strict Transport Security
------------------------------------------

HTTP Strict Transport Security (HSTS) is an HTTP header that allows user agents like browsers to only connect to a website over HTTPS.

The HSTS header requires a _mandatory parameter_ called **max-age**. It indicates the amount of time for which the policy is to be followed. The recommended time period is 2 years or 63072000 seconds.

Find more information [here](https://infosec.mozilla.org/guidelines/web_security#http-strict-transport-security).

#### Example

```
Strict-Transport-Security: max-age=63072000
```

5\. Use Content Security Policy
-------------------------------

Content Security Policy (CSP) is used to specify the sources from where your website will fetch its resources. This information is used by web browsers to block requests for resources from sources, other than the ones specified by you.

![Content Security Policy - GeekyMinds](https://geekyminds.co.in/wp-content/uploads/2020/01/Content-Security-Policy2.png)

Content Security Policy - GeekyMinds

#### Why is it necessary for web security?

CSP has been mandated by Mozilla for all new websites. This is because setting up a robust CSP protects your website from [cross-site scripting (XSS)](https://geekyminds.co.in/xss-attacks/) attacks. You can check out [our article](https://geekyminds.co.in/xss-attacks/) on how XSS works.

In short, an attacker can use a vulnerability on your website to inject some malicious JavaScript into it. Later on, when that web page is fetched, the browser will execute the injected malicious code, leaving your site open to devastating attacks.

#### How to implement CSP?

In order to implement CSP, first, make a list of sources from where your website fetches resources. Remember that different pages of your website will have different resource requirements.

The `Content-Security-Policy` header supports many sources. Some of the important ones are:

**`font-src`** - Used to state the sources from where fonts will be fetched.

**`img-src`** - Used to state the sources from where images will be fetched.

**`object-src`** - It provides control over Flash and similar plugins.

**`script-src`** - Used to state the sources from where scripts will be fetched.

**`style-src`** - Used to state the sources from where stylesheets will be fetched.

**`default-src`** - The default source for fetching resources. This is used as a default for the sources which you do not mention.

#### Example

The following example defines a CSP policy does the following:

*   Sets the default source for fetching resources to none.
*   Source of fonts is set to Google Fonts.
*   Sets the source of images to the origin server and Imgur.
*   Bans the execution of plugins like Flash
*   Sets the source of scripts to the origin server.
*   Sets the source of stylesheets to the origin server.

```
Content-Security-Policy: default-src 'none'; font-src 'https://fonts.googleapis.com'; img-src 'self' https://i.imgur.com; object-src 'none'; script-src 'self'; style-src 'self';
```

Find a detailed guide on Content Security Policy [here](https://www.html5rocks.com/en/tutorials/security/content-security-policy/).

6\. Secure Your Cookies
-----------------------

A cookie is a small file that stores information on the client's machine. Other than tracking users' web activity, cookies are used for session management. This is why securing your cookies is important for optimal web security. Mozilla has [mandated](https://infosec.mozilla.org/guidelines/web_security#cookies) the creation of HTTP cookies with the secure flag. Here's a gist:

*   **Secure:** All cookies must be set using the `Secure` flag.
*   **Name:** Cookies names may be prepended with either `__Secure` or `__Host`, in order to prevent them from being overwritten. Use `__Host` when the cookie is intended only for a specific domain. User `__Secure` otherwise.
*   **HttpOnly:** Cookies that do not require access via JavaScript should be created using the `HttpOnly` flag.
*   **Expiry:** Ensure your cookies (especially session cookies) expire as soon as they are done. Use `Expires` directive to set an absolute date of expiry. Use `Max-Age` directive to set a relative expiration date in seconds.
*   **Path:** Cookies should be set to the most restrictive path possible. The most commonly used path is root.
*   **SameSite:** Set this directive to `Strict`, in order to prevent the cookie from being sent via cross-origin requests. This is a good first step to [mitigate](https://owasp.org/www-project-cheat-sheets/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) Cross-site Request Forgery (CSRF) risks.

#### Example

```
Set-Cookie: __Host-SESSIONID=YnVnemlsbGE; Max-Age=2592000; Path=/; Secure; HttpOnly; SameSite=Strict
```

7\. Prevent Clickjacking
------------------------

Clickjacking is a type of attack performed on a victim website using iframes.

#### What are iframes?

An HTML document can display another HTML document within itself using the iframe tag. Example [here](https://www.w3schools.com/tags/tryit.asp?filename=tryhtml_iframe). They are both useful and harmful in terms of web security.

#### What is clickjacking?

Consider an evil website, that has been set up with a button with the intention of making users click it anyhow possible. Next, consider a victim website with buttons or links tied to some important action (ex. the Facebook Like button).

![Clickjacking using iFrames - GeekyMinds](https://geekyminds.co.in/wp-content/uploads/2020/01/Clickjacking.png)

An illustration of clickjacking - GeekyMinds

Using iframes, the button/link of the victim website can be placed over the button on the evil website (using CSS z-index). Finally, the iframe's opacity is set to zero. The visitor clicks the button on the evil website but that click instead affects the button/link of the victim website.

Find a cool demo [here](https://javascript.info/clickjacking#the-demo).

#### Solution

Websites can protect themselves from getting framed (literally) by using either CSP `frame-ancestors` directive or [X-Frame-Options](https://infosec.mozilla.org/guidelines/web_security#x-frame-options) header. Most browsers (except Opera Mini, Opera Mobile and UC Browser for Android) [support](https://caniuse.com/#search=frame-ancestors) the former. Therefore, using CSP to prevent clickjacking is **recommended**. Please [use](https://infosec.mozilla.org/guidelines/web_security#x-frame-options) X-Frame-Options for others.

#### Example

This disables a website from being framed.

```
Content-Security-Policy: frame-ancestors 'none'
```

If you want to be able to frame parts of your website in itself, then use 'self'.

```
Content-Security-Policy: frame-ancestors 'self'
```

Similarly, if you would like to allow a particular origin to frame your website, use the following CSP:

```
Content-Security-Policy: frame-ancestors https://example.com
```

8\. Cross-origin Resource Sharing
---------------------------------

⚠️This is exclusively for APIs.

The concept of Cross-origin resource sharing (CORS) is very simple. Imagine you have a website along with an API service hosted on the same server/domain. This server is origin A.

Now suppose another origin named B (a web application), is trying to fetch data from your API using XMLHttpRequest or Fetch. If CORS is not properly configured on your API, then B will also be able to fetch all pages of your website, without authorization. This gives B access to resources it otherwise shouldn't have.

To mitigate this, please make sure to use the header `Access-Control-Allow-Origin` on your API responses. This header can be used to block _unauthorized_ foreign access to resources on your origin/server. By using this header, you can state which origins you want to allow access to a particular resource, which is being requested.

#### Examples

**Public APIs** should return the `Access-Control-Allow-Origin` header set to `*` in their responses. This allows any origin to fetch resources from the origin/server hosting the API.

```
Access-Control-Allow-Origin: *
```

**Private APIs** should return the same header set to the origin(s) that the private API serves:

```
Access-Control-Allow-Origin: https://my-webapp.com
```

* * *

* * *

9\. Prevent Cross-site Request Forgery
--------------------------------------

Cross-site request forgery (CSRF) attacks are quite simple. Websites often use cookies to store session information of a logged-in user. Such cookies preserve the logged-in state of the user.

![Cross-site Request Forgery - Web Security Checklist - GeekyMinds](https://geekyminds.co.in/wp-content/uploads/2020/01/Cross-site-Request-Forgery.png)

Evil site from Server B attempts an unauthorized destructive change on Server A - GeekyMinds

An evil website can send requests to the victim site using the authority of the logged-in user's cookies. It's pretty easy to implement. Consider this `img` tag:

```
<img src="https://example.com/users/delete?confirm=true">
```

The user visiting this evil website is logged-in to `example.com` and the authentication cookie is stored in the user's browser. This cookie will be sent to the victim site. That's how cookies work. As a result, the request will be authorized when the server receives said cookie.

A quick definition: A _destructive change_ is such a change in data that is characterized by the permanent deletion of important data. Verifying such destructive changes before applying them is a big step towards better web security.

#### Solution

The very first solution is to set the `SameSite` directive as `Strict` when placing an authentication cookie on the client machine. This has been explained above.

The second method is to use Anti-CSRF tokens. These are special randomized strings that are generated and sent along with each destructive request for verification. This token is matched on the server and the authenticity of the request is verified.

Here's an example from [MDN](https://infosec.mozilla.org/guidelines/web_security#examples-8):

```
<!-- A secret anti-CSRF token, included in the form to delete an account -->
<input type="hidden" name="csrftoken" value="1df93e1eafa42012f9a8aff062eeb1db0380b">
```

How Anti-CSRF tokens are generated is frankly beyond the scope of this already lengthy article. If you use modern web development frameworks, you will find them using these tokens to verify such requests.

That's all folks! If I missed out any other important points, do mention it in the comments below and I will update the article with due credits! 😃