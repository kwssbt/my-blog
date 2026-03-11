---
title: "__int128 重载"
date: 2026-02-09
categories: 
  - "other"
  - "template"
tags: 
  - "others"
  - "模板"
coverImage: "010023301e41c000_2025-12-25_18-29-30-045-scaled-e1769866007737.png"
---

```
istream &operator>>(istream &is, __int128 &n) {
    string s;
    is >> s;
    n = 0;
    int sign = 1;
    int i = 0;
    if (s[0] == '-') {
        sign = -1;
        i = 1;
    }
    for (; i < (int)s.size(); i++) {
        n = n * 10 + (s[i] - '0');
    }
    n *= sign;
    return is;
}

ostream &operator<<(ostream &os, __int128 n) {
    if (n == 0)
        return os << 0;
    if (n < 0) {
        os << '-';
        n = -n;
    }
    string s;
    while (n > 0) {
        s += char('0' + n % 10);
        n /= 10;
    }
    reverse(s.begin(), s.end());
    return os << s;
}

i128 toi128(const string &s) {
    i128 n = 0;
    for (auto c : s) {
        n = n * 10 + (c - '0');
    }
    return n;
}
```
