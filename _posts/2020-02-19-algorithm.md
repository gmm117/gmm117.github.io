---
layout: post
title: 알고리즘
date: 2020-02-19 00:00:00 +0200
published: 2020-02-19 00:00:00 +0200
categories: development
tags: ['algorithm']
comments: true
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    Algorithm 특징
</pre>
<!--more-->

---

```C++
bool isPrime(int number)
{
    number = abs(number);

    if(number==0 && number==1)
    {
        return false;
    }

    int divisor;
    for(divisor=number/2; number%divisor != 0; --divisor)
    ;

    return divisor==1;
}

int main(int argc, char* argv[])
{
    list<int> coll;

    for(int i=24; i<=30; ++i) {
        coll.push_back(i);
    }

    list<int>::iterator pos;

    pos = find_if(coll.begin(), coll.end(), isPrime);

    if(pos != coll.end()) {
        cout << *pos << " is first prime number found" << endl;
    }
    else {
        cout << "no prime number found" << endl;
    }

    return 0;
}

```