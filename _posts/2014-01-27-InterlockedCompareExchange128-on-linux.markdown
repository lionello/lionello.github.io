---
layout: post
title: InterlockedCompareExchange128 on linux
categories: []
permalink: /archives/138-InterlockedCompareExchange128-on-linux.html
s9y_link: http://www.lunesu.com/index.php?/archives/138-InterlockedCompareExchange128-on-linux.html
date: 2014-01-27 10:26:31.000000000 +08:00
---
The GCC that comes with my Fedora installation doesn't appear to have a <strong>__sync_val_compare_and_swap</strong> that works with <strong>__uint128_t</strong>, so here it is:<br />
<pre name="code" class="c">#undef NDEBUG
#include &lt;assert.h>

inline __uint128_t InterlockedCompareExchange128( volatile __uint128_t * src, __uint128_t cmp, __uint128_t with )
{
  __asm__ __volatile__
  (
      "lock cmpxchg16b %1"
      : "+A" ( cmp )
      , "+m" ( *src )
      : "b" ( (long long)with )
      , "c" ( (long long)(with>>64) )
      : "cc"
  );
  return cmp;
}

int main(int argc, char* argv[])
{
  __uint128_t a=0, b=0, c=0x0123456789ABCDEFULL;
  c &lt;&lt;= 64;
  c |= 0xFEDCBA9876543210ULL;
  assert(b == InterlockedCompareExchange128(&a, b, c));
  assert(a == c);
  assert(c == InterlockedCompareExchange128(&a, b, b));
  assert(a == c);
  assert(c == InterlockedCompareExchange128(&a, c, b));
  assert(a == b);
  assert(b == InterlockedCompareExchange128(&a, c, c));
  assert(a == b);
  return 0;
}
</pre>
