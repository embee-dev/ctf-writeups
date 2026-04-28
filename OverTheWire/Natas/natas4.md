# Natas Level 4 Writeup

[Previous Level](natas3.md) | [Index](README.md) | [Next Level](natas5.md)

## Level Info

- **URL:** `http://natas4.natas.labs.overthewire.org`
- **Username:** `natas4`

## Enumeration

For initial access, I used `curl`:

```bash
curl -u 'natas4:{redacted}' http://natas4.natas.labs.overthewire.org/
```

The response is a simple HTML page with an interesting sentence:

```html
...
Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
...
```

## Solution

The phrase `You are visiting from ""` (note the empty double quotes) suggests that some kind of server logic is involved in generating this text and that server logic expects some kind of data along with our request.

The text also says that `authorized users should come only from "http://natas5.natas.labs.overthewire.org/"`. That immediately servers as a hint.

### Experimenting with the `Referer` HTTP Header

According to [MDN's entry on the HTTP `Referer` request header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Referer):

> The HTTP Referer request header contains the absolute or partial address from which a resource has been requested. The Referer header allows a server to identify referring pages that people are visiting from or where requested resources are being used. This data can be used for analytics, logging, optimized caching, and more.

Let's try an experiment with this header. I sent a new request for the same page as above, but with the `Referer` header set to a random value:

```bash
curl -H "Referer: my_random_referrer" -u 'natas4:{redacted}' http://natas4.natas.labs.overthewire.org/
```

This seems to work, because the response now includes the exact value I have sent along with this request:

```html
Access disallowed. You are visiting from "my_random_referrer" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
```

The server inserts the value of the `Referer` HTTP header inside those previously empty double quotes.

### Getting the flag using the `Referer` HTTP Header

With that in mind, I sent another request, this time setting the `Referer` header to the exact value that was mentioned in the response:

```bash
curl -H "Referer: http://natas5.natas.labs.overthewire.org/" -u 'natas4:{redacted}' http://natas4.natas.labs.overthewire.org/
```

The response now reveals the flag for the next level:

```html
...
Access granted. The password for natas5 is {redacted}
...
```

## Real World Lessons

According to the [MDN entry about the HTTP `Referer` request header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Referer):

> [The Referer Header] has many fairly innocent uses, including analytics, logging, or optimized caching. However, there are more problematic uses such as tracking or stealing information, or even just side effects such as inadvertently leaking sensitive information.

The MDN article suggests some techniques for [mitigating the risks of the `Referer` header](https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Referer_header:_privacy_and_security_concerns#how_can_we_fix_this)
