---
layout: post
comments: true
title:  "FreeBSD: QuickCheck (tf-random) and `invalid byte sequence'"
date:   2014-06-25 00:46:12
categories: Haskell QuickCheck FreeBSD
---

Have you ran into this while installing QuickCheck on FreeBSD?
{% highlight bash %}
QuickCheck-2.7.5 depends on tf-random-0.5 which failed to install.
tf-random-0.5 failed during the final install step. The exception was:
/tmp/pkgConf-tf-random-043046.5: hGetContents: invalid argument (invalid byte
sequence)
{% endhighlight %}

I wasted a lot of time trying to debug this, but today I was fighting with a
similar problem with Jekyll and finally found an [answer][jekyll-post]. In
short, the issue is that tf-random (as well as [Liquid][jekyll-post]) needs
UTF-8 input. And FreeBSD does not use that encoding by default.

## The solution
Add appropriate language and encoding tag. to the `default:` section of your
`/etc/login.conf`

If you your language is `en_US` then you `lang` tag would be
`:lang=en_US.UTF-8:`

Before:
{% highlight bash %}
default:\
  :passwd_format=md5:\

  *many lines...*

  :umask=022:
{% endhighlight %}

After:
{% highlight bash %}
default:\
  :passwd_format=md5:\

  *many lines...*

  :umask=022:\
  :lang=en_US.UTF-8:
{% endhighlight %}

To reload the changes, run (as root):
{% highlight bash %}
cap_mkdb /etc/login.conf
{% endhighlight %}

After that relogin to your environment. More details in the mentioned
[post][jekyll-post]. Thank to [Patrick Gibson][pgib] for sharing the solution.

[jekyll-post]: https://pgib.me/blog/2013/09/25/jekyll-freebsd-byte-sequence/
[pgib]: https://pgib.me/
