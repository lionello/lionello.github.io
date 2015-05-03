---
layout: post
title: Even, a missing posix command line utility?
categories: []
permalink: /archives/135-Even,-a-missing-posix-command-line-utility.html
s9y_link: http://www.lunesu.com/index.php?/archives/135-Even,-a-missing-posix-command-line-utility.html
date: 2013-08-11 21:38:33.000000000 +08:00
---
I was looking for a utility that would output every byte at an even offset from a file (e.g. skip bytes at odd offset). I couldn't figure out how to do it with dd (from the looks of it, it might not be possible) and ended up writing my own little utility. I was surprised that there wasn't already a utility called 'even', so here it is: <br />
{% highlight c %}
// even, by Lionello Lunesu, placed in the public domain
#include &lt;stdio.h>

int even(FILE &#42; f)
{
  char buf[4096];
  int r, read, wrote;
  while (1)
  {
    read = fread(buf, 1, sizeof(buf), f);
    for (r=1; r&lt;read/2; ++r)
      buf[r] = buf[r*2];
    wrote = fwrite(buf, read/2, 1, stdout);
    if (wrote != 1)
      return 1;
    if (read != sizeof(buf))
      break;
  }
  return 0;
}

int main(int argc, char&#42;&#42; argv)
{
  int i, r = 0;
  if (argc == 1 || (argc == 2 &&amp; argv[1][0] == '-' &&amp; !argv[1][1]))
    return even(stdin);
  for (i=1; i&lt;argc; ++i)
  {
    FILE &#42; f = fopen(argv[i], "r");
    if (f)
    {
      r = even(f);
      fclose(f);
      if (r)
        break;
    }
    else
    {
      r = 2;
    }
  }
  return r;
}
{% endhighlight %}
<br />
Interestingly, when compiling with GCC on my MBP, this performs better without any optimization flag, getting >440MB/s!<br />
<br />
Oh, this is what I needed it for:<br />
```
$ ./even &#42; | grep -oai "[a-z0-9_]&#42;\.dll"
```
