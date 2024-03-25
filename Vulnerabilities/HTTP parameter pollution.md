## How HTTP parameter pollution works

HTTP parameter pollution, or HPP, is the process of manipulating how a website treats the parameters it receives during HTTP requests. If an attacker can inject extra parameters into a requests and the website trust them, it may lead to unexpected behaviour.

This can happen on the server side or the client side.

### Server-side HPP

Sometimes the server uses the HTTP request parameters to run code, using those parameters as values. For example:

``https://www.bank.com/transfer?from=12345&to=67890&amount=5000``

Let's assume a code uses the **from**, **to**, and **amount** to transfer money. But you can only use one **from** value. Let's append an extra **from** parameter:

``https://www.bank.com/transfer?from=12345&to=67890&amount=5000&from=Your_Account``

Now I'm transferring money from **Your_Account** to mine.

Depending of the server, it responds in different ways: PHP and Apache uses the last occurrence, Tomcat the first one, while ASP and IIS uses all of them.

### Client-side HPP

Client-side (i.e. your browser) let's you add extra parameters. Let's say we have the following URL:

``http://host/page.php?par=123``

We can add extra parameters, like this:

``http://host/page.php?par=123%26action=edit``

This translates to ``http://host/page.php?par=123&action=edit``, as ``%26`` is interpreted as **&**.

Why are we doing it like this, instead of using directly **&**?

If we used ``par=123&action=edit``, the ``&`` would have been interpreted as a two parameters, dropping the ``action=edit``, as it is not expecting the ``action`` parameter.

But by encoding it with ``%26``, it isn't recognised immediately, adding the ``action=edit`` to the URL.

Granted, this one is a bit as we need to know how the code works, making it hard to use it in a **black box** setting, but if we know which parameters the website uses, we can add extra values to the URL.

## What to look for during bug bounty

* Check out which URL parameters are used and try adding a second one with the desired value.
* Try adding new parameters not existing in the current URL that you have seeing in other URLs of the same website.