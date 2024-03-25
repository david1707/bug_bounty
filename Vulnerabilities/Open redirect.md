## How Open redirects works

Open redirects occur when a developer trusts inputs to redirect to another site, normally via URL parameter, HTML <meta> refresh tags or the DOM window location properly.

### redirect_to

``https://www.google.com/?redirect_to=https://www.gmail.com``

When you visit this URL, Google receives a **GET** HTTP request, and uses **redirect_to** parameter's to determine where to redirect your browser.

An attacker would use this feature to redirect to their own website:

``https://www.google.com/?redirect_to=https://www.attacker-site.com``

If Google isn't validating the **redirect_to** parameter, it would redirect you to the attackers website without you noticing it.

### meta refresh tags

While a perceptive person would notice this parameter in a normal setting, it can be properly hidden in the HTML as a meta tag. For example:

``<meta http-equiv="refresh" content="0; url=https://www.google.com/">``

Here, the **content** attribute defines how long the browser waits to request the **url** attribute, while the **url** redirects you to the URL parameter website, in this case, www.google.com. Attackers would use this vulnerability to redirect you to their own website.

### DOM window.location property:

An attacker also can use Javascript to redirect users by modifying the window's **location** property through the DOM (Document Object Model):

```
window.location = https://www.atacker-site.com
window.location.href = https://www.atacker-site.com
window.location.replace(https://www.atacker-site.com)
```

If your browser uses JavaScript (99.999% yours are using it), you may be vulnerable to this attack.

## What to look for during bug bounty

* Check URL parameters. Sometimes it is easy to spot (``redirect_to``, ``domain_name``, ``checkout_url``), but sometimes it isn't (``r``, ``u``, and so on).
* ``<meta>`` tags.
* ``window.location`` properties.
