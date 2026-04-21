# Natas Writeup

## Overview

Natas is a series of wargames hosted at [OverTheWire](https://overthewire.org).

According to the official description:

> Natas teaches the basics of serverside web-security.

Each level is hosted at:

```
http://natasX.natas.labs.overthewire.org
```

Where `X` is the current level number.

Each level uses Basic HTTP Authentication.

- **Username:** `natasX`, where `X` is the number of the current level
- **Password:** the *flag* that must be obtained from the previous level.

OverTheWire also adds:

> **All passwords are also stored in /etc/natas_webpass/**. E.g. the password for natas5 is stored in the file /etc/natas_webpass/natas5 and only readable by natas4 and natas5.

## About this Writeup

This directory contains my notes and solutions for the Natas wargame. I followed these rules:

- recovered password are redacted (displayed as `{redacted}`)
- commands and relevant output are displayed in code blocks instead of screenshots
- each level file links to the previous level, the next level and this index

## Quick Links to Each Level

- [Natas Level 0](natas0.md)
- [Natas Level 1](natas1.md)
