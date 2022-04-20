---
title: "What is Base64"
date: 2022-04-20T15:47:00+09:00
categories:
  - Memo
tags:
  - Base64
# header:
#   teaser: /assets/images/code.jpg
---
When you have some binary data that you want to ship across a network, you generally don't do it by just streaming the bits and bytes over the wire in a raw format. Some media are made for streaming text and some protocols may interpret your binary data as control characters, or your binary data could be screwed up because the underlying protocol might think that you've entered a special character combination. So to get around this, people encode the binary data into characters. Base64 is one of these types of encodings.

https://stackoverflow.com/questions/201479/what-is-base-64-encoding-used-for