# Po: Password organizer

Po is a secure password organizer. It is built by the Mailboxly's developer team, in their spare time; especially on weekends. [This blog post explains our motivation](http://blog.mailboxly.com/2016/05/introducing-po-an-open-source-password-organizer.html) behind this project.

## Security

Po is a **zero-knowledge system**. We don't store a user's passwords, not even the master passphrase.

### Signing Up:

- While signing up, the user chooses a master passphrase (MP).
- The sha256 hash of the MP is sent to the server. The MP itself never leaves the client.
- The server stores the user's:
    1. Username
    2. Hashed MP (sha256)
    3. Email address (optional)
    4. Recovery hint (optional)

### Organizing Passwords:

The client maintains an array of account information objects. Each account information object corresponds to one account, and includes the account's username, password, email etc. This array is never sent to the serve. It remains on the client, in a variable. It is not stored in `localStorage` or `sessionStorage`.

When a password is added, deleted or edited, the underlying array changes. The changed array is mapped to a JSON string, which is then encrypted using AES-256-CCM (SJCL). The encryption password is the MP. Finally the encrypted JSON string is sent to the server.

## Running Po Locally

All you need is Python 2.7 &  MongoDB 3.2. As yet, Po has not been tested with other versions of Python or MongoDB. None the less, we expect it to *usually* work with other combinations too.

Having installed Python and MongoDB:

```
$ git clone https://github.com/mailboxly/po.git
$ cd po
$ python wsgi.py
```

That should do the trick. Go to to [localhost:8080](http://localhost:8080) and check!

--- THE END ---
