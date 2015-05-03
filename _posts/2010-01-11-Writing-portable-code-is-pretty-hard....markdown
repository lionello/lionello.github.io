---
layout: post
title: Writing portable code is pretty hard...
categories: []
permalink: /archives/97-Writing-portable-code-is-pretty-hard....html
s9y_link: http://www.lunesu.com/index.php?/archives/97-Writing-portable-code-is-pretty-hard....html
date: 2010-01-11 11:26:16.000000000 +08:00
---
I've decided to make a small Wake-On-Lan program that I can register as a scheduled task in order to wake up my NAS. This task must be executed <em>every minute</em> or else the NAS will shutdown again, so to minimize system resources I decided to write this thingy in plain old C, without using any functions that would use the CRT. <br />
<br />
As if that wasn't bad enough, I tried to make it portable: buildable using different compilers on different platforms. Here's the result:

{% highlight c %}
// Portable Wake-On-Lan by Lionello Lunesu, placed in the public domain
#ifdef _WIN32
#define WIN32_LEAN_AND_MEAN
#include &lt;windows.h&gt;
#include &lt;winsock2.h&gt;
#ifdef _MSC_VER
#pragma comment(lib, "ws2_32.lib")
#endif
#else
#include &lt;sys/socket.h&gt;
#include &lt;netinet/in.h&gt;
typedef unsigned char BYTE;
typedef char TCHAR;
typedef int BOOL;
typedef int SOCKET;
#endif
//#include &lt;stdio.h&gt;

#define MACLEN (6)
#define PACKLEN (17*MACLEN)

int H2I(TCHAR m) {
	return m &lt;= '9' ? m - '0' : (m | 32) - 'a' + 10;
}

BOOL ValidHex(TCHAR t) {
	return (t &gt;= '0' &&amp; t &lt;= '9') || (t &gt;= 'a' &&amp; t &lt;= 'f') || (t &gt;= 'A' &&amp; t &lt;= 'F');
}

#ifdef _WIN32
int WINAPI WinMain(
    HINSTANCE hInstance,
    HINSTANCE hPrevInstance,
    LPSTR lpCmdLine,
    int nCmdShow
)
{
	WSADATA wsaData;
#else
int main(int argc, char* argv[])
{
	const char* lpCmdLine = (argc == 2 ? argv[1] : NULL);
#endif
	struct sockaddr_in addr;
	int t = 0, i;
	SOCKET sock;
	TCHAR nibble1, nibble2;
	BOOL BroadCast = 1;
	char pack[PACKLEN];

	if (lpCmdLine == NULL) {
		//printf("Usage: wol &lt;MAC&gt;\n");
		return -3;
	}

#ifdef _WIN32
	// Initialize Winsock
	i = WSAStartup(MAKEWORD(2,2), &wsaData);
	if (i != 0) {
		//printf("WSAStartup failed: %d\n", i);
		return i;
	}
#endif

	// FFFFFFFFFFFF
	while (t &lt; MACLEN)
		pack[t++] = 0xFF;

	// first MAC
	for (i=0; t &lt; MACLEN+MACLEN; ++i) {
		nibble1 = lpCmdLine[i];
		if (nibble1 == 0)
			return -1;
		if (!ValidHex(nibble1))
			continue;
		nibble2 = lpCmdLine[++i];
		if (!ValidHex(nibble2))
			return -2;
		pack[t++] = (H2I(nibble1) &lt;&lt; 4) | H2I(nibble2);
	}

	// repeat 15 more times
	for (;t &lt; PACKLEN; ++t)
		pack[t] = pack[t - MACLEN];

	sock = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP);
#ifdef _WIN32
	if (sock == INVALID_SOCKET) {
		i = WSAGetLastError();
	}
#else
	if (sock &lt; 0) {
		//i = errno;
	}
#endif
	else {
		i = setsockopt(sock, SOL_SOCKET, SO_BROADCAST, (const char*)&BroadCast, sizeof(BroadCast));

		addr.sin_family = AF_INET;
		addr.sin_addr.s_addr = 0xFFFFFFFF;
		addr.sin_port = htons(9);	// network byte order
		t = sendto(sock, pack, PACKLEN, 0, (struct sockaddr*)&addr, sizeof(addr));
#ifdef _WIN32
		if (t != PACKLEN)
			i = WSAGetLastError();
		closesocket(sock);
#else
		close(sock);
#endif
	}

#ifdef _WIN32
	WSACleanup();
#endif
	return i;
}
{% endhighlight %}

<br />
I've build this using GCC on Ubuntu and MSVC and DMC on Windows. Actually, on Windows this application still needs the CRT, since that WinMain isn't exactly the entry-point called by the OS. The OS calls an entry point without any arguments and then the CRT will extract the HINSTANCE and command line etc using the Windows API. So to get the above code free of all CRT use I'll have to use GetCommandLineA and skip the first argument, taking double-quotes into account. Easy to do, for sure, but I'm lazy and the program does not load the MSVCRT DLL which is what I basically wanted.<br />
<br />
<strong>UPDATE:</strong> I have since then added a native Windows service wrapper tol my WakeOnLan program. Let me tell you: parsing command line parameters without CRT is not funny! My WOL service's private working set is now around 132KB. Not sure what else I can do to minimize that. (IIRC, there are some EXE headers that can be tuned, like stack size and such. Haven't tried that yet.)
