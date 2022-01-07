[Don Irwin](https://medium.com/@viperguynaz?source=post_page-----f8f0bff46ba9-----------------------------------)

[5 Followers](https://medium.com/@viperguynaz/followers?source=post_page-----f8f0bff46ba9-----------------------------------)

[About](https://medium.com/@viperguynaz/about?source=post_page-----f8f0bff46ba9-----------------------------------)

Follow

[Upgrade](https://medium.com/plans?source=upgrade_membership---nav_full-------------------------------------)

[](https://medium.com/?source=post_page-----f8f0bff46ba9-----------------------------------)

[](https://policy.medium.com/medium-rules-30e5502c4eb4?source=responses-----f8f0bff46ba9-----------------------------------)

# Building a Serverless Contact Form

[

![Don Irwin](https://miro.medium.com/fit/c/56/56/0*g-ZOThC_ZsAonZ50.)

](https://medium.com/@viperguynaz?source=post_page-----f8f0bff46ba9-----------------------------------)

[

Don Irwin

](https://medium.com/@viperguynaz?source=post_page-----f8f0bff46ba9-----------------------------------)

[

Apr 22, 2020·3 min read

](https://medium.com/@viperguynaz/building-a-serverless-contact-form-f8f0bff46ba9?source=post_page-----f8f0bff46ba9-----------------------------------)

Using Azure Functions and Proxies as an API Relay

![Serverless Computing](https://miro.medium.com/max/12032/1*Sd50m5VidzQA8wzhJiFHhQ.jpeg)

Photo by [panumas nikhomkhai](https://www.pexels.com/@cookiecutter?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/bandwidth-close-up-computer-connection-1148820/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

On a recent project, the client wanted a simple, single page, portfolio web site with a contact form to notify the client via email when a user submitted the form. The client was “techy” and being all read up on the current state of web development, wanted the web site to be serverless. I’ve been building web sites for 20+ years, so building a site with HTML, CSS and JavaScript was a refreshing challenge given all the options available like Angular, React or Vue. In the end though, most of what these frameworks do is render HTML, CSS and Javascript. So, the challenge for this article: How do we capture user input and email it with just HTML, CSS and JavaScript?

# Serverless Email?

Well, not technically — there has to be an email server somewhere, but we definitely want nothing to do with maintaining that server. So, we’ll use an email service like [SendGrid](https://sendgrid.com/). There is a free tier to SendGrid that (as of this writing) allows you to send 40,000 emails for 30 days, then 100/day forever.I’m not going to cover setting up SendGrid. For this exercise, all you will need is an API key and the email API enpoint — you can create one under settings in your SendGrid account or use any other email service of your choosing.

SendGrid has an Email API, so I can just send requests to their server, right? Well yes, if you are using a relay server, but we are trying to do this without a dedicated server we have to maintain. Can I send directly from a browser? No — SendGrid (and any reputable email service) has [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) restrictions enabled which prevents direct communication between browsers and their email API. When you have a browser-only application that reaches out to APIs, the API key has to be embedded in the application. Anyone with access to a browser-only application can access all of the Javascript source code, including your API keys.

Making your API key publicly accessible could result in anyone authenticating API calls with your API key — this is a significant security concern both for you and SendGrid. That leaves cloud-based serverless computing as a good option.

# Functions and Proxies

For this article, I’ll be using Azure Functions (you could use AWS Lambda or GCP Functions as well). Serverless functions are great for delivering small blocks of functionality through custom code. A couple years back, Azure added proxies to their Functions.

> Functions Proxies let you define a single API surface for multiple function apps. Any function app can now define an endpoint that serves as a reverse proxy to another API — whether that endpoint is another function app, an API app, or anything else.

An Azure Function Proxy allows us to modify requests and responses. Therefore, we can take a request to the proxy from our browser and add SendGrid’s required Authorization header which includes our Email API key.

In order to get started, create an Azure Function, then create a Proxy in the function:

![Azure Function](https://miro.medium.com/max/2000/1*yW-gjYTlDGD_9IcKAtzj1Q.png)

Configure the proxy:

-   Set the route template — this defines the URL your code will call — /send
-   Set the HTTP methods you will accept — POST
-   Set the backend URL (to Email API) — [https://api.sendgrid.com/v3/mail/send](https://api.sendgrid.com/v3/mail/send)
-   Override the request to add the Authorization header

Configure Azure Function CORS:

-   Navigate back the the Azure Function (top level)
-   Navigate to CORS (link on main function tab)
-   Set the domains your site will be calling from (include localhost for testing)

To test the proxy, set the backend URL to a utility like [RequestBin](https://requestbin.com/) and post a request to see if your app is sending data the way your email service expects it.

On the web side, simply make a call to the proxy:

JavaScript example code

QED — now, you can essentially call an email API from your browser by relaying the request through an Azure Function proxy.

[

## Don Irwin

](https://medium.com/@viperguynaz?source=post_sidebar--------------------------post_sidebar--------------)

Live in the future, build what is missing! F-16s and code…hmmm! [https://www.donirwin.dev](https://www.donirwin.dev/)

Follow

DON IRWIN FOLLOWS

-   [![SmartLab AI](https://miro.medium.com/fit/c/20/20/1*4NdAZNIaoL1NX5I_npIHAA.png)](https://smartlabai.medium.com/?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
    [
    
    #### SmartLab AI
    
    
    
    
    
    ](https://smartlabai.medium.com/?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
-   [![James Hamblin](https://miro.medium.com/fit/c/20/20/0*q8LikiWBUIsPl0c-.png)](https://medium.com/@jameshamblin?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
    [
    
    #### James Hamblin
    
    
    
    
    
    ](https://medium.com/@jameshamblin?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
-   [![Roshan Adusumilli](https://miro.medium.com/fit/c/20/20/1*15Ip7Mr5WwjL3ST1eaXhog.png)](https://medium.com/@roshan.adusumilli?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
    [
    
    #### Roshan Adusumilli
    
    
    
    
    
    ](https://medium.com/@roshan.adusumilli?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
-   [![Drew Magary](https://miro.medium.com/fit/c/20/20/2*WQRhQWS33DixPPAOYK6PzA.jpeg)](https://medium.com/@bigdaddydrew?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
    [
    
    #### Drew Magary
    
    
    
    
    
    ](https://medium.com/@bigdaddydrew?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
-   [![Eric Simons](https://miro.medium.com/fit/c/20/20/2*2tCYD-st3YiKRlGSrFgj0w.jpeg)](https://medium.com/@ericsimons?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    
    [
    
    #### Eric Simons
    
    
    
    
    
    ](https://medium.com/@ericsimons?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)
    

[See all (11)](https://medium.com/@viperguynaz/following?source=blogrolls_sidebar-----f8f0bff46ba9-----------------------------------)

33

2

-   [Azure Functions](https://medium.com/tag/azure-functions)
-   [Serverless](https://medium.com/tag/serverless)
-   [JavaScript](https://medium.com/tag/javascript)
-   [Proxy](https://medium.com/tag/proxy)
-   [Email](https://medium.com/tag/email)

33

2

## [More from Don Irwin](https://medium.com/@viperguynaz?source=follow_footer-----f8f0bff46ba9-----------------------------------)

Follow

Live in the future, build what is missing! F-16s and code…hmmm! https://www.donirwin.dev

[](https://medium.com/?source=post_page-----f8f0bff46ba9-----------------------------------)

[About](https://medium.com/about?autoplay=1&source=post_page-----f8f0bff46ba9-----------------------------------)

[Write](https://medium.com/new-story?source=post_page-----f8f0bff46ba9-----------------------------------)

[Help](https://help.medium.com/hc/en-us?source=post_page-----f8f0bff46ba9-----------------------------------)

[Legal](https://policy.medium.com/medium-terms-of-service-9db0094a1e0f?source=post_page-----f8f0bff46ba9-----------------------------------)