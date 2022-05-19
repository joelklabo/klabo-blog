---
layout: post
title:  "Bitcoin's Missing Concept"
date:   2022-05-22
categories: bitcoin hashing 
---

When someone first begins their journey of trying to understand Bitcoin and how it work they will quickly come across the concept of hashing. When I try to explain Bitcoin to someone else this is the part that presents the biggest challenge. Unless you happen to be a programmer (even many programmers don't know what it is) you probably have no idea what it means or why it's needed. I believe understanding hashing is the key to really grokking Bitcoin and how it works. Here I will try to explain what it is. Why it's useful. And why it's important to Bitcoin.

**What is hashing**

When I started learning about Bitcoin I was already working as a programmer. Even for me hashing was an unknown concept, even though I was unwittingly using it all the time. It is one of those things that doesn't really have an analogy to real life as far as I know. You just need to learn about it to know it. Once you learn about it though you will likely see it used all over the place.

I think the best place to start is with an example. Here is the hash of `"a"` and the hash of `"b"`:

```
MD5 ("a") = 0cc175b9c0f1b6a831c399e269772661
MD5 ("b") = 92eb5ffee6ae2fec3ad71c777531578f
```

Here I am using the MD5 hashing algorithm, my mac has a program of the same name that can create a hash for me. The hash is that long string of letters and numbers there. The [MD5](https://en.wikipedia.org/wiki/MD5) hashing algorithm produces a 128-bit hash value. The string of letters and numbers actually represent a 128-bit number, it's just formatted that way to make it easier for humans to read. In short a hash function takes some input, in this case the letter a or b, and gives you back a number in some range. In the case of MD5, becaues it's a 128-bit hash function, that range would be 0 to 2^128 - 1 aka between 0 and 340,282,366,920,938,463,463,374,607,431,768,211,455.

***Hashing is deterministic***

You might think that it doesn't seem hard to randomly pick a 128-bit number for some input. In this case `"a"` or `"b"`. But, what makes hash functions special is that is isn't random, it's deterministic. The same input will alway give you the same output.

So for example, if I had downloaded something like Microsoft Office from the internet and it was a 20GB file, I could hash it and get a specific value. If even one byte changed in that file I would get a completely different value. This is something that you see a lot when downloading software on the internet. There will be a hash on the website so that when you download it you can check for yourself that you are getting what you expected. Not some hacked version. So hashing is deterministic, the same input should always have the same output.

***Hash values are randomly distributed***

The next important feature of a hash is that you cannot predict what input will give what output. In the first example using `"a"` and `"b"` as inputs, the hash values were very different. If someone could predict what input would give a certain output they could trick you into downloading a hacked Microsoft Office by modifying it in a way that looked like it hadn't been modified. It would look just like the un-hacked version when you hashed it. This is also called a "hash collision" which is when two inputs have the same output. This is a big problem for a hash function. If a collision is found to exist people will stop using it and move to a better one. This actually happened with SHA-1, another hashing algorithm: [See Article](https://cryptosense.com/blog/google-announces-full-sha-1-collision-what-it-means)

Interestingly, hash collisions are inevitable. There are *only* 340,282,366,920,938,463,463,374,607,431,768,211,455 possible output values for MD5, and there are definitely more possible inputs. But, it's still sufficiently hard to find them. Even more so to find one that would be useful. Skipping ahead a bit Bitcoin acutally uses a 256-bit hash function so it's even less likely.

***They are fast***

Hashing should be fast, but not too fast. It needs to be fast enough so it is actually useful to check the hash of a large file. But, not so fast that a hacker could just try every input until they found a hash they wanted.

***Why is this useful for Bitcoin?***

Bitcoin uses hashing all over the place. For Bitcoin addresses. Transaction IDs. But the most interesting and important use, in my opinion, is mining.

We have all heard that Bitcoin is a Blockchain. If you take a look at one of the blocks in bitcoin you will see multiple uses of hashing:

