---
layout: post
section-type: post
title: Scratching Post
category: scratch
tags: [ 'tech' ]
---

This is our 'Scratching Post'. Here we just post up snippets of test code for the blog. If things on this post look weird, it's probably because we're in the process of testing new content here.

<b>Syntax Highlighting Tests:</b>
<br>
highlight.js library: cpp
<pre><code class="cpp">
class testClass
{
  public static void test(String args[])
  {
    String str = "";
    Con::printf(str);
  }
}
</code></pre>
{: #code-example-1}
<br>

highlight.js library: cs
<pre><code class="cs">
// comment test
%var = 0;
</code></pre>
<br>

rouge: cpp
{% highlight cpp %}
void test(bool b, int i);
{% endhighlight %}
<br>

rouge: cs
{% highlight cs %}
// comment test
%var = 0;
function testFunction(%bool, %int);
{% endhighlight %}
<br>
