# hyperboot

offline webapp bootloader

[Try the demo.](http://demo.hyperboot.org)

Single page applications, where appropriate, have many usability benefits: you
can just give someone a URL and they can immediately start using the
application. Native and desktop applications require more setup but once you've
installed an app it doesn't change or disappear without warning.

`hyperboot` gives your users the benefits of explicit, immutable versioning with
control over upgrades while preserving the simplicity of passing around a URL.
Plus, offline works by default!

# problems

Explicit versioning and upgrades addresses many problems that users currently
experience with webapps:

* an app disappears, leaving users with no recourse
* an app changes to have user-hostile features over time
* underlying infrastructure changes or vanishes
* an app is compromised silently with no mechanism for auditing

Compromising apps by criminal, spy agency, or court order will become an
increasingly important problem to solve as browsers gain crypto primitives.
Malicious code could be silently inserted into any update.

hyperboot solves some of these problems by using immutable hashes and control over
upgrades, but a more complete solution might also involve:

* use peer to peer connections (webrtc, webtorrent, etc) as much as possible
* document and version the external protocols and provide a mechanism to swap
out external endpoints
* let users volunteer server infrastructure with open protocols and service
implementations, like seeding content on bittorrent

# getting started

For a more complete example, look at the
[hyperboot-example-app repository](https://github.com/hyperboot-example-app).

Otherise, to get started do:

```
$ npm install -g hyperboot html-inline
$ echo -n "console.log('beep boop')" > main.js
$ echo -e '<body><script src="main.js"></script></body>' > index.html
$ html-inline index.html > bundle.html
```

now given our `bundle.html` with complete inlined assets:

```
$ cat bundle.html
<body><script>console.log('beep boop')</script>
```

we can generate a release:

```
$ hyperboot release bundle.html -v 1.0.0 -m 'initial release!'
9ef1a61f55fade7724f09f5a8940e2dfcacd371103f6eae3f0ba0286a8ada05e
```

The `hyperboot release` command generates a new `hyperdata` directory:

```
$ ls hyperdata/
9ef1a61f55fade7724f09f5a8940e2dfcacd371103f6eae3f0ba0286a8ada05e.html
versions.json
```

Now we can start a hyperboot server that will read releases from `./hyperdata`:

```
$ hyperboot server -p 5000 -v
http://localhost:5000
```

The `-v` tells the server to print all HTTP requests.

Click the icon in the upper right corner to see the application releases.
Refresh the page, kill the server, and note how the app keeps working just the
same!

# usage

```
hyperboot release HTMLFILE { -m MESSAGE, -v VERSION }

  Create a release from HTMLFILE, a self-contained html payload.
  All of HTMLFILE's assets should be inlined.
  
  Set a VERSION and optionally a MESSAGE for the release.
  These will be visible to the user along with the hash in the user
  interface.

hyperboot server { -p PORT }

  Start an http server for the app.

hyperboot list

  Show the available releases.

```

# todo

* update mechanism for hyperboot itself
* load versions from url
* auditing tools

# in closing

The default mechanics of the web are highly skewed toward service providers and
away from users. To correct this imbalance, we developers should irrevocably
limit our own ability to exert control. A manifesto means nothing without a
mechanism.

# install

With [npm](https://npmjs.org) do:

```
npm install -g hyperboot
```

# license

MIT
