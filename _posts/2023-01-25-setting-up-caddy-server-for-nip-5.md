---
layout: post
title:  "Setting up Caddy Server for NIP-05 Verification"
date:   2023-01-25
categories: nostr damus 
---

![image](/assets/klabo-nip-5-verification.jpeg)

In the last post [Adding Nostr NIP-05 Verification to Your Blog (Jekyll)](https://klabo.blog/nostr/damus/2023/01/04/adding-nip-05-verification-to-your-blog.html) I went over the steps for adding NIP-05 verification to your Jekyll blog. See the actual NIP-05 document [here](https://github.com/nostr-protocol/nips/blob/master/05.md). Since then I've updated my setup to fix this common error you may have seen:


![image](/assets/jack-nip-5-error.png)


The issue here is that CORS wasn't set up correctly. You can learn more about [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) if you're interested but for those of you that just want to get it working I'm going to show you what I had to do in my Caddyfile:

```
(cors) {
  @cors_preflight method OPTIONS
  @cors header Origin {args.0}

  handle @cors_preflight {
    header Access-Control-Allow-Origin "*"
    header Access-Control-Allow-Methods "GET, POST, PUT, PATCH, DELETE"
    header Access-Control-Allow-Headers "Content-Type"
    header Access-Control-Max-Age "3600"
    respond "" 204
  }

  handle @cors {
    header Access-Control-Allow-Origin "{args.0}"
    header Access-Control-Expose-Headers "Link"
  }
}

klabo.blog {
    import logging
    import cors *
    root * /home/azureuser/klabo-blog/_site/
    encode gzip
    file_server
}

satoshis.lol {
    import logging
    route /.well-known/nostr.json {
	    rewrite * /nostrnip5/api/v1/domain/bHkhhSQdNE7ntZuYEPBVuL/nostr.json
	    reverse_proxy localhost:8000
    }
    reverse_proxy 0.0.0.0:17422
}
```

I used a [snippet](https://caddyserver.com/docs/caddyfile/concepts#snippets) to add *support* for CORS to the places I needed it. You'll notice I have two places I've set up NIP-05 verfication. My personal blog, and I have it set up on LNBits so others can set up theirs on the **satoshis.lol** domain. If you want a **you@satoshis.lol** verification you can get one [here](https://lnbits.klabo.blog/nostrnip5/signup/bHkhhSQdNE7ntZuYEPBVuL) for 999 sats 🤙 

**Conclusion**

Thats about it. Let me know if you run into any issues and good luck! Find me on nostr here: `npub19a86gzxctwtz68l8zld2u9y2fjvyyj4juyx8m5geylssrmfj27eqs22ckt`

