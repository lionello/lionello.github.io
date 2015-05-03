---
layout: post
title: Modern COM Programming in D
categories:
- Dlang
permalink: /archives/126-Modern-COM-Programming-in-D.html
s9y_link: http://www.lunesu.com/index.php?/archives/126-Modern-COM-Programming-in-D.html
date: 2012-01-25 00:53:25.000000000 +08:00
---
I finally got around to asking for permission to share the slides of a tech talk I did more than a year ago: <a href="http://www.lunesu.com/uploads/ModernCOMProgramminginD.pdf" title="Modern COM Programming in D" target="_blank">Modern COM Programming in D</a>

In the slides I explain how I've made a projection for COM into D, which allows you to use COM objects without the usual hassle of refcount/QueryInterface/HRESULT/BSTR/etc... Sharing the code will be hard (mostly because of the shape it's in) but the slides basically contain all you need. Actually, D has progressed a lot since, so a rewrite might be in order anyway.

<strong>UPDATE:</strong> Add your comments below or on <a href="http://www.reddit.com/r/programming/comments/ow7qc/modern_com_programming_in_d/" title="Reddit">Reddit</a>.

{% highlight c %}
import std.stdio, comxml;

void main()
{
    // Create a new empty document
    auto doc = XmlDocument();

    // Load an XML document from an inline string
    doc.LoadXml("<root>世界你好<a>hello</a><a>world</a></root>");

    // Get the first node's value
    auto text = doc.DocumentElement.FirstChild;
    // Write the node's value to the console
    writeln(text.NodeValue);

    // Perform a simple XPath query
    auto nodes = doc.SelectNodes("/root/a/text()");

    // Do iteration through the child nodes
    foreach (node; nodes.First)
        writeln(node.NodeValue);

    // Access the child nodes by index
    for (uint t = 0; t < nodes.length; ++t)
        writeln(nodes[t].NodeValue);
}
{% endhighlight %}
