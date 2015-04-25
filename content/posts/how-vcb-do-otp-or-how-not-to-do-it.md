+++
Categories = []
Tags = []
author = "Cong Nguyen"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
authorlink = "http://ntcong.it"
date = "2015-04-24T17:16:23+07:00"
thumbnail = "/img/avatar.png"
title = "How VCB OTP work and How not to do it"

+++

# The App
So recently VCB released a Smart OTP app for Android (not so recently anymore since I'm holding up the post for a while). They seem to be the first bank to release a app-based OTP, so let's take a look at how they did it.

I'm not an Android programmer by any mean, so it takes a while to be able to understand the code and to collect the tools, you may have better results if you are. And also I'm a terrible programmer, so just take my words with a grain of salt.

Anyway, the process of decompiler is pretty easy:

* Use apktool to extract the dex file and Android data files
* Use a dex2jar decompiler to convert classes.dex to a jar file
* Use a general jar decompiler to convert it back to java files
* Open in Eclipse and setup debug environment

If you want to decompiler yourself, you might need to do a little homework.

# The Problem
First, you have to register your device. In the a/a.java file, they call some webservice from smartotp.vietcombank.com.vn. You will send your number, Mac address and time, and also your confirmation number. Then they will send you back an otp master key. I will trust them to do the right thing and generate that key properly. So let's jump right at the smartotp website.

![VCB Web](/img/vcb_web.png)

Woa, a big red certificate icon: not a good sign. And they list all the web service methods and HOW to call them. **Classic .Net Service1.asmx :)**. You can study further to know how they encrypt and send the request, or you can have fun just by repeatedly sending confirmation and lock your friends from register.

After that I register with the service, and then setup the passcode. Pretty good practice right? You have OTP combined with passcode.
Not that simple, turned out they save app's data in /data/data/vietcombank.../shared_prefs. The file has some pretty suspicious variables: **pHashPassCode** and **pX_FACTOR**.

Look for the pHashPassCode variable and see how they use it: they merge it with device's Mac address, and md5 the result. Luckily, it's just a 4 numbers passcode, we can brute force it. Or better, set it to "1234".

It's pretty sad to see how a big company can't even learn to use the passcode right. What the point of a passcode when people can just replace it with another? Generally we encrypt all the variable or the preference file itself with a key derived from that passcode. Technically someone can still bruteforce if they know the algorithm, but there are methods to prevent it, and even then it's so much better.

And last, let's take a look at the OTP itself. It seems to be a *complicated* algorithm:


{{< highlight python >}}
gg = a(a(y+L+str1+key)+a(y+D+str1+key)+a(L+D+str1+key)) 
{{< /highlight >}}

* str1: just a floor of time/80 (which means the otp will change each 80s)
* D: your phone number
* key: the transaction key in VCB's e-banking
* L: phone's mac address
* y: the secret

You might think I forgot the pX_FACTOR above, but no, the pX_FACTOR is y-the-secret here.

And a: it's just a md5 function

So, you don't even need the passcode, just get pX_FACTOR and use it at your PC. Android even have a helpful command: run-as package so you can easily extract the code:

    adb shell run-as vietcombank... cat /data/data/vietcombank.../shared_prefs/...

Convenience, right?

# Closing

I did this nearly two months ago, so something might be missing here. Also I did not know anything better so don't mock me if I'm wrong:)

But anyway, here the problem with VCB:

* Don't use half-assed security implementation. Passcode is nice, but if you decided to implement passcode, do it right. Don't just add passcode for the sake of passcode, let's the user use a better alternative (like the OS's screen lock).
* Next time secure your web API, it's not a good thing to expose your web service like that.
* I think the secret (or the OTP master key) is a md5 string. Why use a broken algorithm instead of a better implementation? The secret is using just as a string, you can use any hash function for it.
* And the OTP: well, it's your choice to do it yourself (or because I'm stupid and I don't recognize the standard here :) ). But still, don't use md5. No one charged you for any other methods.

I should invest more time into this app, but it's just an OTP generator (and there's standard out there), so why bother :)
