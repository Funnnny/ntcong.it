+++
Categories = ["blog", "go"]
Description = ""
Tags = ["blog", "go"]
date = "2014-12-08T11:22:36+07:00"
title = "Goodbye Blogspot, Hello Hugo"
author = "Cong Nguyen"
authorlink = "http://ntcong.it"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
thumbnail = "/img/hugo.png"

+++

# Goodbye (any) traditional blogging system #
I used Wordpress for like 8 years, wrote some plugins myself (some that I already published), eventually got tired of managing my own hosting, and switched to wordpress.com. Then Wordpress.com want 15 dollar a year just for custom domain feature, and I switched to Blogspot. I happily used Blogspot for a while, but even so, the experience is not that great. Some example:

* Bloated: there are so many features I don't use, and the feature I use, I have to code it in manually.
* Editor: I hate the WYSIWYG editor, it's bloated (again) with the non-standard tag, bbcode. It's easy to break with my horrible Internet. And it doesn't behave the way I want it to. I wanted to use Markdown, but write in Markdown, then convert to HTML and copy to the WYSIWYG editor, it's not a pleasant way.
* Ugly Themes: It seems that no one want to use a minialist theme with Blogspot (or even Wordpress). I'm happy to remove away most features for a minimalistic approach, but to no avail. Every minimal templates/themes want to retain the most core-feature (which is not a bad thing). I'm a horrible designer, I know very little about CSS, so in the end, it's still a problem I had to deal with.
* Bloated website: The website bundle with a lot of useless things: some stupid Google's javascript that I don't bother to read, Google+ js, some other Google's one, then some Google+, a lot of default Blogspot CSS, jQuery, api.js, source code highlighting script...Every features I added, I had to add some javascript/library too. 
* Always online: I used some offline clients, but the trust is: you will never know how your post display until you submit the post. Only to find some problem, and have to fire up the online editor. And the downtime, every person in the world tells me Google's uptime is the best, but I occasionally find some problem with the editor, or the admin interface.

I can make myself a static website, I even had one before I use Wordpress (with a lot of marquee). I don't write much anyway, but again I'm a horrible designer, the website would be ugly.

Then I heard about Jekyll, and then Octopress, and some alternatives like Brunch, Hyde. But nothing catches my attention (maybe because I don't really like Ruby). One day, I read about Hugo and decided it's time to switch.

# Hello static website generator (Hugo) #

I install hugo, build some test page with it, and then deploy the page to Github Pages in like 30 minutes. And I really like it:

* Being able to write my post in my editor: I use vim-binding so writing in normal or WYSIWYG editor feels missing something
* I can host my own blog (so I can do whatever I want) and also don't have to manage it, because it integrated nicely with my work flow: Markdown+Git+Github Pages.
* Markdown is amazing, no more weird problem with WYSIWYG.
* No more jQuery, no more google-prettify. Most things that can be done in server, like paginate and syntax highlighting, will be done when you regenerate your post. Less JS dependancy = real minimalistic website
* Go: Go is amazing and you should learn/use it.

The only thing bugs me right now is the lack of themes, and it requires you have some decent knowledge about Go to solve some quirks. But it also give me some reason to tinkering with my setup.
