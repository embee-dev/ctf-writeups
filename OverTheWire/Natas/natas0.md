# Natas Level 0 Writeup

[Index](README.md) | [Next Level](natas1.md)

## Level Info

- **URL:** `http://natas0.natas.labs.overthewire.org`
- **Username:** `natas0`
- **Password:** `natas0`

## Enumeration

As this is the starting level, the credentials are provided. I used `curl` for the initial observation.

```bash
curl -u 'natas0:natas0' http://natas0.natas.labs.overthewire.org
```

## Solution

The output is very telling. It is a simple HTML page, but it includes a very important detail inside a **"leftover" HTML comment**:

```html
<!--The password for natas1 is {redacted} -->
```

## Real World Lessons

Although this is clearly an exaggerated scenario, things like **leftover developer comments**, **credentials**, etc. are all examples of [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html). It is good practice to regularly review code to fix such potential information exposure issues.
