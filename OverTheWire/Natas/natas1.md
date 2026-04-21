# Natas Level 1 Writeup

[Previous Level](natas0.md) | [Index](README.md) | [Next Level](natas2.md)

## Level Info

- **URL:** `http://natas1.natas.labs.overthewire.org`
- **Username:** `natas1`

## Enumeration

For initial access, I used `curl`:

```bash
curl -u 'natas1:{redacted}' http://natas1.natas.labs.overthewire.org
```

The output is an HTML page. If you use a **standard web browser** like Chrome, Firefox, etc. solving this level would be just a bit more difficult than the previous one, because it contains JavaScript code that disables access to the context menu.

```html
<body oncontextmenu="javascript:alert('right clicking has been blocked!');return false;">
```

## Solution

Disabling the context menu may seem to be an effective choice, but there are several other ways to view the page source, either in the browser or using other methods. Some examples:

- Using the `view-source` URI scheme

    Most browsers support this scheme, so simply adding the `view-source:` prefix to any URL in the browser's address bar opens its source code:

    ```
    http://www.example.com/ -> view-source:http://www.example.com/
    ```

- Using keyboard shortcuts

    Depending on the platform and browser, there is usually a keyboard shortcut available that is equivalent to the **View Page Source** context menu command. On my machine, this shortcut is `Ctrl-U`.

Using any of the above methods (or others), it is possible to view the original HTML source code of the page.

The password for the next level can be found in a **"leftover" HTML comment** in the page source:

```html
<!--The password for natas2 is {redacted} -->
```

### Real World Lessons

The vulnerability exploited in this level is very similar to [Natas Level 0](#natas-level-0), so the same lesson applies: always review code for potential information exposure issues, see [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html).

There is one subtle difference, though, and it holds an important lesson: **any data that is sent in an HTTP response can and will be read, parsed, and analyzed**. Solutions like **disabling the context menu**, **disabling keyboard shortcuts**, or **hiding paywalled content behind a blurred `div` element**, are not enough. **Never send sensitive data** and then try to hide it on the front end.
