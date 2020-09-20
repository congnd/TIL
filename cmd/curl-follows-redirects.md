It's common for HTTP server to return 301 or 302 redirect for a given URL. 
One example of this is Github redirects your browser when you click to a link in the release page to start download.
When used in browser, you will be redicrect to the final destination automatically, 
but when using in Bash scripts or running `cURL` that behavior is not enabled by default because it helps debug eaiser.

```sh
$ curl https://github.com/tmspzz/Rome/releases/download/v0.23.3.64/rome.zip

HTTP/1.1 302 Found
date: Sun, 20 Sep 2020 08:06:08 GMT
content-type: text/html; charset=utf-8
server: GitHub.com
status: 302 Found
...
```

But most of the cases, you wouldn't want to have to handle these redirects manually. 
Fortunately, `cURL` offers a command line flag that tells it to automatically follow 
the redirect and return the resolved endpoint and its data.

```sh
$ curl -L URL
```

Now, downloading binary files from Github release page becomes extremely easy.

```sh
$ curl -L https://github.com/tmspzz/Rome/releases/download/v0.23.3.64/rome.zip --output rome.zip
```

### Limitting redirects

It's not common but you can use the `--max-redirs` options to prevent infinite redirects.

```sh
$ curl -L --max-redirs 1 https://github.com/tmspzz/Rome/releases/download/v0.23.3.64/rome.zip --output rome.zip
```
