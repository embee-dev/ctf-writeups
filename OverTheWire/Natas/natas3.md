# Natas Level 3 Writeup

[Previous Level](natas2.md) | [Index](README.md) | [Next Level](natas4.md)

## Level Info

- **URL:** `http://natas3.natas.labs.overthewire.org`
- **Username:** `natas3`

## Enumeration

For initial access, I used `curl`:

```bash
curl -u 'natas3:{redacted}' http://natas3.natas.labs.overthewire.org/
```

The response is a simple HTML page with only a single paragraph and an HTML comment:

```html
...
There is nothing on this page
<!-- No more information leaks!! Not even Google will find it this time... -->
...
```

## Solution

The HTML comment `Not even Google will find [any leaks]` might refer to a possible `/robots.txt` file, since this is the de facto standard way to give information to crawlers (like Google).

### Looking for a possible `/robots.txt`

`robots.txt` is a file that is usually placed in the root of a website, so I tried to request it with `curl`:

```bash
curl -u 'natas3:{redacted}' http://natas3.natas.labs.overthewire.org/robots.txt
```

The response looks exactly like a regular `robots.txt` file:

```text
User-agent: *
Disallow: /s3cr3t/
```

The `Disallow: /s3cr3t/` line tells us that the website administrator wants to hide the `/s3cr3t/` URL from crawlers and bots.

### Accessing the "secret" URL

I requested this disallowed URL with `curl`:

```bash
curl -u 'natas3:{redacted}' http://natas3.natas.labs.overthewire.org/s3cr3t/
```

The response is an Apache directory listing:

```text
Index of /s3cr3t
Name	    Last modified	    Size	Description
Parent Directory	 	        - 	 
users.txt	2026-04-03 15:07 	40 	 
```

It shows a `users.txt` file, so I requested it with `curl`:

```bash
curl -u 'natas3:{redacted}' http://natas3.natas.labs.overthewire.org/s3cr3t/users.txt
```

The response is a text file that contains the credentials for the next level:

```text
natas4:{redacted}
```

## Real World Lessons

The most important lesson from this challenge is highlighted in the [robots.txt configuration guide at MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/Robots_txt#solution):

> [robots.txt] should not be used as a way to prevent the disclosure of private information or to hide portions of a website.
> 
> ...
> 
> ...be aware that some robots, such as malware robots and email address harvesters, will ignore your robots.txt file.
