# Natas Level 2 Writeup

[Previous Level](natas1.md) | [Index](README.md) | [Next Level](natas3.md)

## Level Info

- **URL:** `http://natas2.natas.labs.overthewire.org`
- **Username:** `natas2`

## Enumeration

For initial access, I used `curl`:

```bash
curl -u 'natas2:{redacted}' http://natas2.natas.labs.overthewire.org/
```

The response is a simple HTML page. At first glance, there is nothing immediately suspicious in the source. It contains only a short note and an `img` element:

```html
...
There is nothing on this page
<img src="files/pixel.png">
...
```

## Solution

### Dead End #1: Inspecting the `img` element

I decided to analyze the `pixel.png` file more closely for possible clues. First, I downloaded it with `curl`:

```bash
curl -u 'natas2:{redacted}' -O http://natas2.natas.labs.overthewire.org/files/pixel.png
```

Then I examined it with `exiftool`:

```bash
exiftool pixel.png
```

However, the results show that **this is just a normal PNG file** containing a single pixel.

### Inspecting the `/files` path

Next, I checked the `/files` path with `curl`:

```bash
curl -u 'natas2:{redacted}' -O http://natas2.natas.labs.overthewire.org/files/
```

The response is an HTML page with the telltale signs of an **Apache directory listing**. You could also open the directory in a browser for easier reading, but I include it here as plain text for simplicity:

```text
Index of /files
Name        Last modified       Size	Description
Parent Directory	 	        -
pixel.png   2026-04-03 15:07 	303
users.txt	2026-04-03 15:07 	145
```

### Getting the `users.txt` file

Besides the previously analyzed `pixel.png` image, the directory listing reveals another file: `users.txt`. I requested it with `curl`:

```bash
curl -u 'natas2:{redacted}' http://natas2.natas.labs.overthewire.org/files/users.txt
```

The response is a text file containing credentials in the format `username:password`. I omitted the unrelated lines and included only the entry relevant to the challenge: the credentials for the next level.

```text
# username:password
...
natas3:{redacted}
...
```

## Real World Lessons

This challenge is a good example of the [CWE-548: Exposure of Information Through Directory Listing](https://cwe.mitre.org/data/definitions/548.html) vulnerability.

As a general rule: unless it is an explicit requirement, **directory indexing should be disabled**.
