[![Splunk](https://community.splunk.com/html/assets/splunk-logo.svg "Splunk")](https://www.splunk.com/)


[[Zettelkasten/splunk stuff]]
-   [COMMUNITY](https://community.splunk.com/)
 -   [SPLUNK ANSWERS](https://community.splunk.com/t5/Splunk-Answers/ct-p/en-us-splunk-answers)
 -   [DISCUSSIONS](https://community.splunk.com/t5/Discussions/ct-p/splunk-discussion)
 -   [SPLUNKTRUST](https://community.splunk.com/t5/SplunkTrust/ct-p/splunk-trust)
 -   [USER GROUPS](https://community.splunk.com/t5/User-Groups/ct-p/ug-hub)
 -   [SPLUNK LOVE](https://community.splunk.com/t5/Splunk-Love/ct-p/SplunkLove)
 -   [IDEAS](https://ideas.splunk.com/)

[Sign In](https://community.splunk.com/plugins/common/feature/samlss/doauth/post?referer=https%3A%2F%2Fcommunity.splunk.com%2Ft5%2FSplunk-Search%2FHow-to-exclude-the-private-ip-range%2Fm-p%2F506827)

[Ask a Question](https://community.splunk.com/t5/forums/postpage/board-id/splunk-search)

-   [Community](https://community.splunk.com/t5/Community/ct-p/en-us)
 -    [Splunk Answers](https://community.splunk.com/t5/Splunk-Answers/ct-p/en-us-splunk-answers)
 -    [Using Splunk](https://community.splunk.com/t5/Using-Splunk/ct-p/use-splunk)
 -    [Splunk Search](https://community.splunk.com/t5/Splunk-Search/bd-p/splunk-search)
 -    How to exclude the private ip range

 

[Options](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827# "Show option menu")

[Solved! Jump to solution](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506829#M141777)

[](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827# "Show option menu")

## 

How to exclude the private ip range

![nareerat_pr](https://community.splunk.com/legacyfs/online/avatars/576972.jpg "nareerat_pr")

[nareerat_pr](https://community.splunk.com/t5/user/viewprofilepage/user-id/50526)

Explorer

‎07-01-2020 01:06 AM

I try to exclude the private ip range with command | search NOT ( src=10.0.0.0/8 OR src=192.168.0.0/16 OR src=172.16.0.0/12)

but I still found the private ips in my search result

Labels 

-   Labels:
 -   ### [other](https://community.splunk.com/t5/forums/filteredbylabelpage/board-id/splunk-search/label-name/other)
    

0 Karma

[](https://community.splunk.com/t5/forums/v5/forumtopicpage.kudosbuttonv2.kudoentity:kudoentity/kudosable-gid/506827?t:ac=board-id/splunk-search/message-id/141776/thread-id/141776&t:cp=kudos/contributions/tapletcontributionspage "Click here to give karma to this post.")

[Reply](https://community.splunk.com/t5/forums/replypage/board-id/splunk-search/message-id/141776)

1 Solution

 Solution

[](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827# "Show option menu")

![bowesmana](https://community.splunk.com/t5/image/serverpage/image-id/9224iD6E9508B2BC1F7B4/image-dimensions/50x50/image-coordinates/0%2C0%2C400%2C400/constrain-image/false?v=v2 "bowesmana")

[bowesmana](https://community.splunk.com/t5/user/viewprofilepage/user-id/6367) ![SplunkTrust](https://community.splunk.com/html/@9E0E338CF4778DCF581C2431A098A10E/rank_icons/splunk-trust-16.png "SplunkTrust") 

SplunkTrust

‎07-01-2020 01:55 AM

search command is doing simple string searching, so what you want is

```markup
| where !(cidrmatch("10.0.0.0/8", src) OR cidrmatch("192.168.0.0/16", src) OR cidrmatch("172.16.0.0/12", src))
```

[View solution in original post](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506829#M141777)

[1 Karma](https://community.splunk.com/t5/kudos/messagepage/board-id/splunk-search/message-id/141777/tab/all-users "Click here to see who gave karma to this post.")

[](https://community.splunk.com/t5/forums/v5/forumtopicpage.kudosbuttonv2.kudoentity:kudoentity/kudosable-gid/506829?t:ac=board-id/splunk-search/message-id/141776/thread-id/141776&t:cp=kudos/contributions/tapletcontributionspage "Click here to give karma to this post.")

[Reply](https://community.splunk.com/t5/forums/replypage/board-id/splunk-search/message-id/141777)

-   [All forum topics](https://community.splunk.com/t5/Splunk-Search/bd-p/splunk-search/page/4362 "Splunk Search")
-    [Previous Topic](https://community.splunk.com/t5/Splunk-Search/Splunk-savesearch-expires-but-still-running/td-p/506853 "Splunk savesearch expires but still running")
-   [Next Topic](https://community.splunk.com/t5/Splunk-Search/How-to-create-flexible-search-strings/td-p/506866 "How to create flexible search strings?") 

 Solution

[](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827# "Show option menu")

![bowesmana](https://community.splunk.com/t5/image/serverpage/image-id/9224iD6E9508B2BC1F7B4/image-dimensions/50x50/image-coordinates/0%2C0%2C400%2C400/constrain-image/false?v=v2 "bowesmana")

[bowesmana](https://community.splunk.com/t5/user/viewprofilepage/user-id/6367) ![SplunkTrust](https://community.splunk.com/html/@9E0E338CF4778DCF581C2431A098A10E/rank_icons/splunk-trust-16.png "SplunkTrust") 

SplunkTrust

‎07-01-2020 01:55 AM

search command is doing simple string searching, so what you want is

```markup
| where !(cidrmatch("10.0.0.0/8", src) OR cidrmatch("192.168.0.0/16", src) OR cidrmatch("172.16.0.0/12", src))
```

[View solution in original post](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506829#M141777)

[1 Karma](https://community.splunk.com/t5/kudos/messagepage/board-id/splunk-search/message-id/141777/tab/all-users "Click here to see who gave karma to this post.")

[](https://community.splunk.com/t5/forums/v5/forumtopicpage.kudosbuttonv2.kudoentity:kudoentity/kudosable-gid/506829?t:ac=board-id/splunk-search/message-id/141776/thread-id/141776&t:cp=kudos/contributions/tapletcontributionspage "Click here to give karma to this post.")

[Reply](https://community.splunk.com/t5/forums/replypage/board-id/splunk-search/message-id/141777)

[](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827# "Show option menu")

![nareerat_pr](https://community.splunk.com/legacyfs/online/avatars/576972.jpg "nareerat_pr")

[nareerat_pr](https://community.splunk.com/t5/user/viewprofilepage/user-id/50526)

Explorer

‎07-02-2020 12:24 AM

It works, thanks

0 Karma

[](https://community.splunk.com/t5/forums/v5/forumtopicpage.kudosbuttonv2.kudoentity:kudoentity/kudosable-gid/506983?t:ac=board-id/splunk-search/message-id/141776/thread-id/141776&t:cp=kudos/contributions/tapletcontributionspage "Click here to give karma to this post.")

[Reply](https://community.splunk.com/t5/forums/replypage/board-id/splunk-search/message-id/141803)

Did you miss .conf21 Virtual?  

### Good news! The event's keynotes and many of its breakout sessions are now available online, and still totally FREE!  
  
[**Catch Up Now >>**](https://conf.splunk.com/watch/conf-online.html)

Get Updates on the Splunk Community!

[](https://community.splunk.com/t5/Community-Blog/Love-Splunk-New-Ways-to-Share-Your-Success-and-Snag-Cool-Perks/ba-p/578755 "Love Splunk? New Ways to Share Your Success and Snag Cool Perks!")

[Love Splunk? New Ways to Share Your Success and Snag Cool Perks!](https://community.splunk.com/t5/Community-Blog/Love-Splunk-New-Ways-to-Share-Your-Success-and-Snag-Cool-Perks/ba-p/578755)

[](https://community.splunk.com/t5/Community-Blog/BSides-Splunk-2022-The-Call-for-Papers-is-now-Open/ba-p/578252 "BSides Splunk 2022 - The Call for Papers is now Open!")

[BSides Splunk 2022 - The Call for Papers is now Open!](https://community.splunk.com/t5/Community-Blog/BSides-Splunk-2022-The-Call-for-Papers-is-now-Open/ba-p/578252)

[](https://community.splunk.com/t5/Community-Blog/Hey-Splunk-Community-You-ve-Been-Rockin-it/ba-p/576953 "Hey Splunk Community - You've Been Rockin' it!")

[Hey Splunk Community - You've Been Rockin' it!](https://community.splunk.com/t5/Community-Blog/Hey-Splunk-Community-You-ve-Been-Rockin-it/ba-p/576953)

[**Read our Community Blog >**](https://community.splunk.com/t5/Community-Blog/bg-p/Community-Blog)

Related Tags

-   [splunk-enterprise](https://community.splunk.com/t5/tag/splunk-enterprise/tg-p/board-id/splunk-search)
-   [search](https://community.splunk.com/t5/tag/search/tg-p/board-id/splunk-search)
-   [regex](https://community.splunk.com/t5/tag/regex/tg-p/board-id/splunk-search)
-   [stats](https://community.splunk.com/t5/tag/stats/tg-p/board-id/splunk-search)
-   [field-extraction](https://community.splunk.com/t5/tag/field-extraction/tg-p/board-id/splunk-search)
-   [eval](https://community.splunk.com/t5/tag/eval/tg-p/board-id/splunk-search)
-   [timechart](https://community.splunk.com/t5/tag/timechart/tg-p/board-id/splunk-search)
-   [lookup](https://community.splunk.com/t5/tag/lookup/tg-p/board-id/splunk-search)
-   [table](https://community.splunk.com/t5/tag/table/tg-p/board-id/splunk-search)
-   [chart](https://community.splunk.com/t5/tag/chart/tg-p/board-id/splunk-search)
-   [rex](https://community.splunk.com/t5/tag/rex/tg-p/board-id/splunk-search)
-   [count](https://community.splunk.com/t5/tag/count/tg-p/board-id/splunk-search)
-   [time](https://community.splunk.com/t5/tag/time/tg-p/board-id/splunk-search)
-   [subsearch](https://community.splunk.com/t5/tag/subsearch/tg-p/board-id/splunk-search)
-   [join](https://community.splunk.com/t5/tag/join/tg-p/board-id/splunk-search)
-   [splunk-cloud](https://community.splunk.com/t5/tag/splunk-cloud/tg-p/board-id/splunk-search)
-   [field](https://community.splunk.com/t5/tag/field/tg-p/board-id/splunk-search)
-   [fields](https://community.splunk.com/t5/tag/fields/tg-p/board-id/splunk-search)
-   [transaction](https://community.splunk.com/t5/tag/transaction/tg-p/board-id/splunk-search)
-   [lookups](https://community.splunk.com/t5/tag/lookups/tg-p/board-id/splunk-search)
-   [splunk](https://community.splunk.com/t5/tag/splunk/tg-p/board-id/splunk-search)
-   [query](https://community.splunk.com/t5/tag/query/tg-p/board-id/splunk-search)
-   [index](https://community.splunk.com/t5/tag/index/tg-p/board-id/splunk-search)
-   [search-language](https://community.splunk.com/t5/tag/search-language/tg-p/board-id/splunk-search)
-   [date](https://community.splunk.com/t5/tag/date/tg-p/board-id/splunk-search)
-   [csv](https://community.splunk.com/t5/tag/csv/tg-p/board-id/splunk-search)
-   [multivalue](https://community.splunk.com/t5/tag/multivalue/tg-p/board-id/splunk-search)
-   [dashboard](https://community.splunk.com/t5/tag/dashboard/tg-p/board-id/splunk-search)
-   [props.conf](https://community.splunk.com/t5/tag/props.conf/tg-p/board-id/splunk-search)
-   [average](https://community.splunk.com/t5/tag/average/tg-p/board-id/splunk-search)

[View All](https://community.splunk.com/t5/forums/tagdetailpage/tag-cloud-grouping/tag/tag-cloud-style/related/message-scope/core-node/board-id/splunk-search/user-scope/all/tag-scope/all/timerange/all/tag-visibility-scope/public)

[Top](https://community.splunk.com/t5/Splunk-Search/How-to-exclude-the-private-ip-range/m-p/506827)

[![Powered by Khoros](https://community.splunk.com/skins/images/7C2BF2880D9401CB592667D61CE77F12/responsive_peak/images/powered_by_khoros.svg "Social CRM & Community Solutions Powered by Khoros")](https://khoros.com/powered-by-khoros "Social CRM & Community Solutions Powered by Khoros")

[](https://twitter.com/splunk) [](https://www.facebook.com/splunk) [](https://www.linkedin.com/company/splunk) [](https://www.youtube.com/user/splunkvideos) [](https://www.instagram.com/splunk/)

[![Splunk](https://www.splunk.com/content/dam/splunk2/images/logos/splunk-logo.svg "Splunk")](https://www.splunk.com/)

[Sitemap](https://www.splunk.com/en_us/site-map.html) | [Privacy](https://www.splunk.com/en_us/legal/privacy/privacy-policy.html) | [Website Terms of Use](https://www.splunk.com/en_us/legal/terms/terms-of-use.html) | [Splunk Licensing Terms](https://www.splunk.com/en_us/legal/splunk-terms-overview.html) | [Export Control](https://www.splunk.com/en_us/legal/export-controls.html) | [Modern Slavery Statement](https://www.splunk.com/pdfs/legal/splunk-modern-slavery-act-transparancy-statement.pdf) | [Splunk Patents](https://www.splunk.com/en_us/legal/patents.html)

© 2005-2021 Splunk Inc. All rights reserved.

Splunk, Splunk>, Turn Data Into Doing, Data-to-Everything, and D2E are trademarks or registered trademarks of Splunk Inc. in the United States and other countries. All other brand names, product names, or trademarks belong to their respective owners.