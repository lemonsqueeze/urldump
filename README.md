urldump
=======

`urldump` is a command-line tool to dump http(s) requests originating from local machine
(or local network when running on a router).

- Uses [mitmproxy](http://mitmproxy.org/) so works for https too.
- `iptables` is used to redirect all traffic to proxy, so works transparently.  
  (Your nat tables will be flushed while it's running if you have any)

Must be run as root. See top of script for configuration.

`mitmproxy_wrapper` can be used to run mitmproxy as a transparent proxy on a router, capturing lan traffic.

### Demo

    $ sudo urldump   
    http://docs.mitmproxy.org/en/stable/certinstall.html
    http://docs.mitmproxy.org/en/stable/_static/js/modernizr.min.js
    https://media.readthedocs.org/css/sphinx_rtd_theme.css
    https://media.readthedocs.org/css/readthedocs-doc-embed.css
    https://media.readthedocs.org/javascript/readthedocs-doc-embed.js
    ...


### Installation

- Install recent [mitmproxy](http://mitmproxy.org/) (v7.0.3 right now)

- Download [urldump](https://raw.githubusercontent.com/lemonsqueeze/urldump/master/urldump)

Optional (but recommended)

- Create a user for mitmproxy

        $ sudo adduser mitmproxy

  and set `user=mitmproxy` in `urldump`.


### Certificates

Install mitmproxy certificate on client browsers to get rid of security warnings (https only)  
Use either:

- mitmproxy's [built-in cert install feature](http://docs.mitmproxy.org/en/stable/certinstall.html):  
  in browser just visit:  
    [mitm.it](http://mitm.it)

- Ubuntu system-wide:

        $ sudo mkdir /usr/share/ca-certificates/mitmproxy
        $ sudo cp ~mitmproxy/.mitmproxy/mitmproxy-ca-cert.cer /usr/share/ca-certificates/mitmproxy/mitmproxy-ca-cert.crt
        $ sudo dpkg-reconfigure ca-certificates

- Firefox:

        prefs->advanced->certificates->view certificates
          import  /usr/share/ca-certificates/mitmproxy/mitmproxy-ca-cert.crt
          check "choose to identify websites"

- Chrome:

        $ sudo apt-get install libnss3-tools
        $ certutil -d sql:$HOME/.pki/nssdb -A -t "C,," -n mitmproxy -i /usr/share/ca-certificates/mitmproxy/mitmproxy-ca-cert.crt

For more info see [mitmproxy's docs](http://docs.mitmproxy.org/en/stable/certinstall.html).
