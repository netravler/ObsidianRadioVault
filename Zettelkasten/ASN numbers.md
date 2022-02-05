-   [  
    What Is My IP?](https://www.bigdatacloud.com/ip-geolocation/what-is-my-ip)
-   [API List](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#)
    -   -   [](https://www.bigdatacloud.com/ip-geolocation-apis)
        -   [](https://www.bigdatacloud.com/geocoding-apis)
        -   [](https://www.bigdatacloud.com/client-info-apis)
        -   [](https://www.bigdatacloud.com/time-zone-apis)
        -   [](https://www.bigdatacloud.com/country-info-apis)
        
        -   [](https://www.bigdatacloud.com/autonomous-system-info-apis)
        -   [](https://www.bigdatacloud.com/insights-apis)
        -   [](https://www.bigdatacloud.com/network-apis)
        -   [](https://www.bigdatacloud.com/phone-number-apis)
        -   [](https://www.bigdatacloud.com/email-address-validation-apis)
        
-   [Network](https://www.bigdatacloud.com/network-lookup)
-   [ASN Lookup](https://www.bigdatacloud.com/asn-lookup)
-   [Insights](https://www.bigdatacloud.com/insights/ipv4-monitoring)
    -   -   [](https://www.bigdatacloud.com/insights/ipv4-monitoring)
        -   [](https://www.bigdatacloud.com/insights/bogon-routes)
        -   [](https://www.bigdatacloud.com/insights/as-rank)
        -   [](https://www.bigdatacloud.com/insights/tor-exit-nodes)
        -   [](https://www.bigdatacloud.com/insights/ip-geolocation-service-status)
-   [SDK](https://www.bigdatacloud.com/sdk)
-   [Blog](https://www.bigdatacloud.com/blog)
-   [My Account](https://www.bigdatacloud.com/customer/account)

To help us improve our location accuracy, would you be willing to anonymously share your location with us?Yes, i'd like to help!

# ASN Info API

-   [HOME](https://www.bigdatacloud.com/ "Go to Home Page")
 -    [AS INFO APIS](https://www.bigdatacloud.com/autonomous-system-info-apis)
 -    **ASN INFO API**

## Introduction

This API returns extended and detailed information about an Autonomous System (AS) when provided with an AS number. The information includes the registration, IPv4 address space announcements and ranking.

### Unprecedented Update Rate

-   **BGP data****:** updated every 2 hours
-   **Registry data:** updated at least once a day

## Example Response

The response is only available in JSON format. The below response should be expected for the following request:

  

[https://api.bigdatacloud.net/data/asn-info?asn=AS7018&localityLanguage=en&key=54dd681b0bb8461e89b0d6c5985aedcf](https://api.bigdatacloud.net/data/asn-info?asn=AS7018&localityLanguage=en&key=54dd681b0bb8461e89b0d6c5985aedcf "Copy Request URL")

  

-   [JSON View](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#ipview-json)
-   [JSON Raw](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#ipview-jsonraw)

{

-   "status": 403,
-   "description": "access denied or your quota limit has been exceeded"

}

## API Pricing

1.  ##### [Monthly Subscription](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#%22)
    
    FREE10,000 queries/month
    
    Additional 10,000 queriesUS$1.00/month
    
    [ADD TO MY SUBSCRIPTION](https://www.bigdatacloud.com/bdc/subscription/#10040)
    
2.  ##### [Annual Subscription](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#%22)
    
    FREE10,000 queries/month
    
    Additional 10,000 queriesUS$0.80/month
    
    [ADD TO MY SUBSCRIPTION](https://www.bigdatacloud.com/bdc/subscription/#10040)
    

## Request Format

NEW! Payload compression is supported, simply add **Accept-Encoding: gzip** header.  
Use the form below to try out this API.

Parameter

Your Input Value

Description

asn

Autonomous System Number as numeric or ASN format (e.g. 123 or AS123 or ASN123)

localityLanguage

Preferred language for locality names in ISO 639-1 format, such as 'en' for English, 'es' for Spanish etc. **Please note**: [148 common world languages](https://www.bigdatacloud.com/supported-languages) are supported, but not all languages are available for every location. If requested language is not available for a requested location it will default to English, if no English is available, the native, local names will be provided

key

Your API key

TRY IT OUT!

  

**The live query**

[https://api.bigdatacloud.net/data/asn-info?asn=AS7018&localityLanguage=en&key=54dd681b0bb8461e89b0d6c5985aedcf](https://api.bigdatacloud.net/data/asn-info?asn=AS7018&localityLanguage=en&key=54dd681b0bb8461e89b0d6c5985aedcf "Copy Request URL")

  
**CURL Example**

[curl -X GET --header 'Accept: application/json' 'https://api.bigdatacloud.net/data/asn-info?asn=AS7018&localityLanguage=en&key=54dd681b0bb8461e89b0d6c5985aedcf'](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api# "Copy CURL Command")

  

[Visit our SDK page](https://www.bigdatacloud.com/sdk) to access API clients in specific languages like Javascript, Python + PHP.

  

### Response

-   [JSON View](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#ipview-json)
-   [JSON Raw](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#ipview-jsonraw)

{

-   "status": 403,
-   "description": "access denied or your quota limit has been exceeded"

}

## Response Format

Field

Data type

Description

asn

string

Autonomous System Number string

asnNumeric

32 bit unsigned integer

Autonomous System Number

organisation

string

Registered Organisation

name

string

Registered name

registry

string

The Regional Internet Registry (RIR) the AS is registered with

registeredCountry

string

Registered Country ISO 3166-1 Alpha-2 code

registeredCountryName

string

Registered Country name localised to the language is as defined by ‘localityLanguage’ request parameter

registrationDate

string

Registration date in “yyyy-mm-dd” format

registrationLastChange

string

Registration modification date in “yyyy-mm-dd” format

totalIpv4Addresses

32 bit unsigned integer

Total number of IP addresses announced by the AS

totalIpv4Prefixes

32 bit unsigned integer

Total number of BGP prefixes announced by the AS

totalIpv4BogonPrefixes

32 bit unsigned integer

Total number of bogon prefixes announced by the AS

rank

32 bit unsigned integer

World rank by total number of IP addresses announced

rankText

string

World rank by total number of IP addresses announced including total

## API Endpoint

Endpoint:

[/data/asn-info](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api# "Copy Endpoint")

Full URL:

[https://api.bigdatacloud.net/data/asn-info](https://api.bigdatacloud.net/data/asn-info "Copy Endpoint URL")

## Related APIs

1.  ##### [ASN Extended Info API](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-info-extended-api)
    
    FREE10,000 queries/month
    
    Additional 10,000 queries
    
    US$1.00 
    
    /month
    
    Returns extended information about an Autonomous System (AS).
    
    -   Registration data
    -   IPv4 prefixes announced
    -   IPv4 addresses announced
    -   IPv4 ranking
    -   Connectivity data
    -   Most active operational area geolocated
    
    [READ MORE](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-info-extended-api) [ADD TO MY SUBSCRIPTION](https://www.bigdatacloud.com/bdc/subscription/#10041)
    

-   [Introduction](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-introduction)
-   [Example Response](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#example-response)
-   [Pricing](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-pricing)
-   [Request Format](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-request)
-   [Response Format](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-response)
-   [API Endpoint](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-endpoint)
-   [Related APIs](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#api-related)

[![BigDataCloud Pty Ltd company logo](https://www.bigdatacloud.com/pub/media/wysiwyg/bdc_logo2.svg)](https://www.bigdatacloud.com/)

Adelaide, SA, Australia

Copyright © 2019 BigDataCloud Pty  Ltd

**Connect with us!**

-    [](https://www.facebook.com/bigdatacloud.net) 
 -    [](https://www.linkedin.com/company/bigdatacloud-pty-ltd/) 
 -    [](https://twitter.com/bigdatacloud/) 
 -    [](https://github.com/bigdatacloudapi) 

Navigate

-   [About Us](https://www.bigdatacloud.com/about-us)
-   [My Account](https://www.bigdatacloud.com/customer/account "My Account")
-   [Contact Us](https://support.bigdatacloud.com/customer/login "Contact Us")
-   [Blog](https://www.bigdatacloud.com/blog "Blog")
-   [FAQs](https://support.bigdatacloud.com/support/solutions "FAQs")
-   [SDK](https://www.bigdatacloud.com/sdk "SDK and Client APIs")
-   [What Is My IP?](https://www.bigdatacloud.com/ip-geolocation/what-is-my-ip "What Is My IP?")
-   [IP Geolocation Technology](https://www.bigdatacloud.com/blog/next-generation-ip-geolocation-service "IP Geolocation Technology")
-   [Update My Location](https://www.bigdatacloud.com/update-my-location "Update my location data")

Legal

-   [Privacy and Cookie Policy](https://www.bigdatacloud.com/privacy-and-cookie-policy "Privacy and Cookie Policy")
-   [Terms & Conditions](https://www.bigdatacloud.com/terms-and-conditions "Service Agreement ")

  

Download our Mobile App

[![Download on the App Store](https://developer.apple.com/app-store/marketing/guidelines/images/badge-download-on-the-app-store.svg)](https://itunes.apple.com/us/app/ip-tools-network-insights/id1446355714)  [![Get it on Google Play](https://play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png)](https://play.google.com/store/apps/details?id=net.bigdatacloud.iptools&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1)

[](https://www.bigdatacloud.com/autonomous-system-info-apis/asn-short-info-api#)