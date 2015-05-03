---
layout: post
title: Show current GIT branch in TCC
categories:
- Knowledge Base
permalink: /archives/133-Show-current-GIT-branch-in-TCC.html
s9y_link: http://www.lunesu.com/index.php?/archives/133-Show-current-GIT-branch-in-TCC.html
date: 2013-05-03 15:02:39.000000000 +08:00
---
I'm getting more and more familiar with the GIT workflow, which goes kinda like this:<blockquote>git checkout -b topicbranchX<br />
git add somefile<br />
git commit -m"commit message"<br />
git pull<br />
git rebase master<br />
git push</blockquote><br />
Unfortunately this means that you'll end up with a bunch of branches (which you can delete once they get pulled into origin/master) but I keep forgetting what branch I currently have checked out. I've seen bash prompts that show the current branch and I decided to do something similar for <a href="http://jpsoft.com" title="JP Software">TCC/LE</a>.<br />
<pre name="code" class="c">/&#42;
@cl "/Tp%~f0" /nologo /GS- /link /SUBSYSTEM:console /nodefaultlib /entry:_main kernel32.lib
@goto :EOF
*/
#define WIN32_LEAN_AND_MEAN
#include &lt;windows.h>
const WCHAR root[] = L"..\\..\\..\\..\\..\\..\\..\\..\\..\\..\\..\\..\\.git\\HEAD";
int __stdcall _main() {
  int offset = sizeof(root)/2 - 10;
  while (offset >= 0) {
    HANDLE h = CreateFileW(root + offset, GENERIC_READ, FILE_SHARE_READ, NULL, 
      OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);
    if (h != INVALID_HANDLE_VALUE) {
      char buf[64];
      DWORD read = 0;
      if (ReadFile(h, buf, sizeof(buf), &read, NULL) &&amp; read > 16) {
        DWORD off = 0;
        DWORD len = 7;            // show 7 hex digits
        if ((int&)buf[0] == ':fer') {
          off = 16;               // skip ref: refs/heads/
          len = read - off ;     // keep LF
        }
        WriteFile(GetStdHandle(STD_OUTPUT_HANDLE), buf + off, len, NULL, NULL);
      }
      return 13 - offset / 3;
    }
    offset -= 3;
  }
  return 0;
}</pre><br />
What's fun about this file is that you can save it as "gb.cmd". When you then enter "gb" on the command line, it will actually invoke the C compiler (remember to run vcvars32) to generate the gb.exe. Next time, the exe will be invoked instead.<br />
<br />
This is the final prompt:<br />
<blockquote>prompt %%@exec[@gb.exe]$e[1m$P$e[0m$_$+$g</blockquote>
