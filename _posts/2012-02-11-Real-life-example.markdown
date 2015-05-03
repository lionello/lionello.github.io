---
layout: post
title: Real life example
categories:
- Knowledge Base
permalink: /archives/127-Real-life-example.html
s9y_link: http://www.lunesu.com/index.php?/archives/127-Real-life-example.html
date: 2012-02-11 17:37:27.000000000 +08:00
---
I never found the need to study those common unix utilities like <strong>sed</strong>, <strong>awk</strong>, <strong>xargs</strong>. Even my knowledge of grep didn't go further than <strong>blah | grep foo</strong>. Until yesterday, when I needed to accomplish a task that just happened to involve all of these utilities:
{% highlight sh %}
grep ppp /proc/net/dev | sed "s/:/ /" | awk '($2+$10>1000000000){print $1}' | xargs --max-args=1 --no-run-if-empty ppp-off
{% endhighlight %}
First, <strong>grep </strong>filters all lines from <em>/proc/net/dev</em> that contain <em>ppp</em>.
Then, <strong>sed </strong>changes the colon into a space using the regex <em>s/:/ /</em>.
Next, <strong>awk </strong>parses the values of the 2nd and 10th column [bytes sent and received] and if the sum exceeds 1GB it prints the value in the first column [the interface].
Finally, <strong>xargs </strong>invokes ppp-off for every interface returned.

Why I needed this is left as an exercise to the reader.
