---
tags: [GitHub]
title: Baidu map URL with coordinates
created: '2019-09-04T08:19:58.201Z'
modified: '2019-09-04T08:20:00.435Z'
---

# [Baidu map URL with coordinates](https://stackoverflow.com/questions/44262343/baidu-map-url-with-coordinates)

[Ask Question](https://stackoverflow.com/questions/ask)

Asked 2 years, 3 months ago

Active [10 months ago](https://stackoverflow.com/questions/44262343/baidu-map-url-with-coordinates?lastactivity "2018-10-11 00:38:13Z")

Viewed 4k times

1

0

i wanted to open a Baidu map with a URL with co\-ordinates (latitude/longitude in some form)

For normal browsers this is working

[http://map.baidu.com/?l=13&tn=B\_NORMAL\_MAP&c=13748138,4889173&s=gibberish](http://map.baidu.com/?l=13&tn=B_NORMAL_MAP&c=13748138,4889173&s=gibberish)

thanks to \- [https://annoyingtechnicaldetails.wordpress.com/2015/01/12/parsing\-baidu\-map\-urls/](https://annoyingtechnicaldetails.wordpress.com/2015/01/12/parsing-baidu-map-urls/)

But this is not working for mobile browsers. Can anyone with knowledge of baidu map URLs help with making a map URL with co\-ordinates with [http://map.baidu.com/mobile](http://map.baidu.com/mobile) ?

Please help.

[google\-maps](https://stackoverflow.com/questions/tagged/google-maps "show questions tagged 'google-maps'") [maps](https://stackoverflow.com/questions/tagged/maps "show questions tagged 'maps'") [baidu](https://stackoverflow.com/questions/tagged/baidu) [baidu\-map](https://stackoverflow.com/questions/tagged/baidu-map)

[share](https://stackoverflow.com/q/44262343 "short permalink to this question")

Share a link to this question

Copy link

|[improve this question](https://stackoverflow.com/posts/44262343/edit)

asked May 30 '17 at 12:29

[

![](https://www.gravatar.com/avatar/7a89a86f9b22660a12ca622559611cf8?s=32&d=identicon&r=PG)

](https://stackoverflow.com/users/592099/ghostcoder)

[ghostCoder](https://stackoverflow.com/users/592099/ghostcoder)ghostCoder

5,44755 gold badges 3838 silver badges56 56 bronze badges

add a comment | [](# "expand to show all comments on this post")

## 1 Answer 1

[active](https://stackoverflow.com/questions/44262343/baidu-map-url-with-coordinates?answertab=active#tab-top "Answers with the latest activity first") [oldest](https://stackoverflow.com/questions/44262343/baidu-map-url-with-coordinates?answertab=oldest#tab-top "Answers in the order they were provided") [votes](https://stackoverflow.com/questions/44262343/baidu-map-url-with-coordinates?answertab=votes#tab-top "Answers with the highest score first")

7

I found the answer. For anyone looking in future:

```
http://api.map.baidu.com/marker?location=39.916979519873,116.41004950566&output=html

```

[share](https://stackoverflow.com/a/44304945 "short permalink to this answer")

Share a link to this answer

Copy link

|[improve this answer](https://stackoverflow.com/posts/44304945/edit)

[edited Oct 11 '18 at 0:38](https://stackoverflow.com/posts/44304945/revisions "show all edits to this post")

[

![](https://www.gravatar.com/avatar/c2ea8790040ee3f38d779c56dd63e45c?s=32&d=identicon&r=PG)

](https://stackoverflow.com/users/584192/samuel-liew)

[Samuel Liew](https://stackoverflow.com/users/584192/samuel-liew)♦

47.2k3636 gold badges 120120 silver badges176 176 bronze badges

