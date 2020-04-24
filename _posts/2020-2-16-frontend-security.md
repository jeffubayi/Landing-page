---
layout: post
title: Security tips for front-end Apps
author: jeff
category: web security
date: 2020-2-16
excerpt: "This is excerpt."
abstract: "Lock your app down and make it less subceptible to hackers."
---

Whether you’re a React.js, Angular, Vue.js, or simply a front-end developer, your code
can be an inviting door for hackers.

As a front-end developer, we’re mostly concern about performance, SEO, and UI/UX — security is often overlooked

You might be surprised to know how big frameworks let you open yourself up to crosssite scripting (XSS) attacks. 
There are risky operations names like dangerouslySetInnerHTML in React or the bypassSecurityTrust APIs in Angular.

We should keep in mind that the front end now shares equal responsibilities as the back
end or DevOps in terms of security. There are thousands of malicious attacks that can
happen from a front end.

Let’s understand the most common — these will cover a great portion of these types of
attacks.
![](https://miro.medium.com/max/1400/0*QOkqygancKoG8HQ-)

## 1.Unrestricted File Upload

This is an attack in which malicious files are uploaded to the server and then executed,
to attack the system. The attack may include: an overloaded file system or database,
complete system takeover, client-side attacks, forwarding attacks to back-end systems,
or simple defacement.

## 2.Clickjacking

This is is an attack where the user is trick to click on a webpage or an element that does
not belong to the site. This attack may cause users to unwittingly provide credentials or
sensitive information, download malware, visit malicious web pages, purchase products
online, or transfer money.

## 3. XSS Attack

This is an attack in which malicious scripts are injected in the form of a browser-side
script into the webpage. Flaws on the websites allow these attacks to succeed and
become widespread.

## 4. SQL injection

This is an attack in which malicious code in SQL statements are injected to destroy your
database via input fields.

## 5. Denial-of-service attack (DoS attack)

This is an attack in which the server or its resources are made unavailable to the
intended user by bombarding your server with traffic.

## 6. Man in the middle attack or session hijacking

This is an attack in which communications between client and server are intercepted to
steal passwords, account numbers, or any personal details.

The attacker will always try to find some loophole in the frontend to reach the server
and do his work.

---
In this article, we will see some of the common best practices to always
keep in mind while coding the frontend.

## 1. Strict UserInput(the First Point of Attack)

User input should always be strict in nature, to avoid vulnerabilities like SQL injection,
clickjacking, etc. So it’s important to validate or sanitize user input before sending it to
the back end.

Sanitizing data can be done by removing or replacing contextually-dangerous
characters, such as by using a whitelist and escaping the input data.
However, I realize that sanitizing and encoding is not an easy task for all existing
possibilities, so we may use the following open-source libraries:

`DOMPurify`-- This is the most simple to use and has one method to sanitize the user’s
input. It has an option to customize the rules and it supports HTML5, SVG, and
MathML.

`secure-filters`-- A Salesforce library that provides methods to sanitize HTML,
JavaScript, inline CSS styles, and other contexts. It’s especially useful if you want to
make use of user input in other places, for example generating CSS or JavaScript.

In the case of file upload always check the file type and use a file filter function and
allow only certain file types to get upload. Refer to this for more.
. . .

## 2. Beware of Hidden Fields or Data Stored in Browser Memory

If we add input `type="hidden"` to hide sensitive data in pages or add them in the browser
localStorage , sessionStorage , cookies and think that’s safe, we need to think again.
Everything added to the browser can be accessed by the attacker easily. 

An attacker can open the dev tools and change all the in-memory variables. And what if you had hidden
the auth page depending upon the localStorage , sessionStorage , and cookies values?
There are tools like ZapProxy and even inspection tools in the browser that can expose
those values to attackers if they find a way to inject a script, and they can then can use
them to attack further.

Hence avoid using `type="hidden"` and avoid storing keys, auth tokens, etc, in the
browser in-memory storage as much as possible.
. . .

## 3.Use a Strong Content Security Policy (CSP)

Never trust everything that the server sends — always define a strong Content-SecurityPolicy HTTP header which only allows certain trusted content to be executed on the
browser or render more resources.
It’s good practice to have a whitelist — a list of allowed sources. Now, even if an attacker
injects script, the script won’t match the whitelist and won’t be executed.
For example:

`content-security-policy: script-src ‘self’ https://apis.xyz.com`

Here the application trusts only the scripts coming from `apis.xyz.com` and ourselves
( `self `). For the rest of the sources, an error in the console is thrown.

>Note: A strong content security policy does not solve the problem of inline scripts
>execution, hence XSS attack is still valid.
You can read a full list of CSP directives on the [MDN website.](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)
. . .

## 4. Enable XSS Protection Mode

If somehow an attacker injects malicious code from the user input, we can instruct the
browser to block the response by supplying the `"X-XSS-Protection": "1; mode=block"`
header.
Most modern browsers have XSS protection mode enabled by default but it’s still
recommended to include the X-XSS-Protection header. This helps to ensure better
security for older browsers that don't support CSP headers.
. . .

## 5.Avoid Typical XSS Mistakes

An XSS attack is usually traced to the DOM API’s `innerHTML`. For instance:

`document.querySelector('.tagline').innerHTML = nameFromQueryString`

Any attacker is able to inject malicious code with the line above.

Consider using textContent instead of innerHTML to prevent generating HTML output
altogether. If you don’t generate HTML, there’s no way to insert JavaScript — you may
see the content but nothing will happen.

Keep an eye out for a new [Trusted Types specification](https://developers.google.com/web/updates/2019/02/trusted-types) which aims to prevent all DOMbased cross-site scripting attacks made by googlers.
In the case of react.js,` dangerouslySetInnerHTML` is unambiguous and cautionary and can
have a similar impact as innerHTML .

>Note: Don't Set the innerHTML value-based on user input and use `textContent` instead of
>`innerHTML` as much as possible.
>Also, HTTP responses headers` Content-Type`and `X-Content-Type-Options` should be set
>properly, with their intended behavior. For example, JSON data should never be
>encoded as text/HTML, to prevent accidental execution.
. . .

## 6.Disable iframe Embedding

The disabling iframe can project us from a clickjacking attack. We should always use
the `"X-Frame-Options": "DENY"` header in the request that prohibits the rendering of the
website in a frame.
Also, we can use `frame-ancestors` CSP directive, which provides more control over
which parents can embed the page in an iframe.
. . .

## 7.Keep Errors Generic

An error like “Your password is incorrect,” may be helpful to the user but also to the
attackers. They may figure out information from these errors that helps them to plan
their next action.
When dealing with accounts, emails, and PII, we should try to use ambiguous errors like
“Incorrect login information.”
. . .

## 8.Use Captcha

Using Captcha at public-facing endpoints (login, registration, contact). A Captcha is a
computer program or system intended to distinguish humans from bots and can help
stop DoS (Denial Of Service) attacks.
. . .

## 9.Always Set `Referrer-Policy`

Whenever we use anchor tag or a link that navigates away from the website, make sure
you use a header policy `"Referrer-Policy": "no-referrer"` or, in case of the anchor tag,
`set rel = noopener or noreferrer` .
When we don’t set these headers and `rel`, the destination website can obtain data like
session tokens and database IDs.

## 10.Use Limited Browser Features & APIs

As in CSP, the limited domain can connect to the website, the same principle can also be
applied to browser features & APIs. We can add a `Feature-Policy` header to deny access
to certain features and APIs. Read more.

Tip: Set `none` for all features that you don't use.
. . .

## 11.Audit Dependencies Regularly

Run `npm audit` regularly to get a list of vulnerable packages and upgrade them to avoid
security issues.
GitHub now flags vulnerable dependencies. We can also use [Snyk](https://snyk.io/) that checks your
source code automatically and opens pull requests to bump versions.
. . .

## 12.Compartmentalize Your Application

As in the back end, we have a microservice architecture where monolithic applications
are split into smaller self-contained components, each running individually.
The same principle can be applied to the frontend. For instance, an application can be
split into the public, authenticated, and admin parts, each hosted on separate
subdomains, like `https://public.example.com , https://users.example.com` and
`https://admin.example.com` .
This will ensure fewer client-side vulnerabilities.

>_Note: Proper compartmentalization would also prevent an XSS vulnerability in the public
>part of the application, preventing it from automatically compromising user information._
. . .

## 13.Avoid Third-Party Services

One line of code third-party services like Google Analytics, Google Tag Manager,
Intercom, Mixpanel, can make your web app vulnerable. Think of a situation when these
third party services are compromised.
It’s important to have a strong CSP policy. Most third-party services have a defined CSP
directive, so always add them.
Also, be sure to include `integrity `attribute when possible when adding a script tag.
Subresource Integrity feature can validate the cryptographic hash of the script and make
sure it hasn’t been tampered with.



```javascript
<script src= "https://example.com/example-framework.js" 
       integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/ux..."
       crossorigin= "anonymous" >
</script>
```

. . .

Carefully Consider Autofill Fields
Personal identification information stored in the autofill of a browser can be convenient
for both users and attackers.
Attackers add third-party scripts to exploit browsers’ built-in autofill to extract email
addresses for building tracking identifiers. They can use these to build user browsing
history profiles which they can sell to the bad guys. Read more on this here.
Many of us aren’t even aware of what information their browser’s autofill has stored.

Tip: Disable auto-filled forms for sensitive data.
. . .

Happy Hacking
