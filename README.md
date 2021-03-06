# GoPhast
A simple, customizable backend engine written in [Go][golang] by 
[Lincoln Anders][LACOM]. GoPhast uses the [Gin Framework][gin] and a 
backend pattern developed by Lincoln Anders.

## Quickly Run GoPhast
To quickly run GoPhast, ensure [Go 1.15+][golang] is installed. You can check 
that Go is installed by running `go version` in your terminal. You may then 
install GoPhast by running `go run github.com/lincolnanders5/GoPhast`. This 
will run GoPhast in the current directory.

To change the port to 5000 for example, run
```shell script
$ echo "port: 5000" > config.yml
$ go run github.com/lincolnanders5/gophast
```

## Route Patterns
GoPhast uses common backend route patterns to remove the backend difficulty 
common in rolling custom engines.

The following path rules denote their GoPhast URL and destination:
- `/` -> `./public/html/index.html`
- `/landing` -> `./public/html/landing.html`
- `/a/main_style.css` -> `./public/css/main_style.css`
- `/api/request_stub` -> `localhost:4000/request_stub`

## Static Assets
Assets such as images and PDFs and front-end materials such as JavaScript and 
CSS files are all pulled from the [public directory][config]. Assets will be 
requested through URLs beginning with the `asset_route` value and found in
the public directory based on the folder named by the file extension. For 
example, the route `/a/main_style.css` will find the `main_style.css` file
in the `./public/css` directory.

Synonymous file types (such as `png`, `jpg`, `gif`) can be reduced to a single 
directory with a representative name (e.g. the `img` directory). All assets
will then properly be found in the `./public/img` directory when an asset such 
as `/a/minesweeper.png` is requested.

## Subsites
Subsites may be used when running multiple websites that share 
[static assets][static-assets]. Multiple GoPhast instances may then access 
the shared static assets while presenting different content through dedicated
HTML pages in their subsite directory.

Subsites may be configured with the [`subsite` configuration field][config], 
must be located in the [public directory][config], and must be a directory in 
the format `_[subsite]`.

*Note*: All static assets may be accessed from all subsites, i.e. the images 
used for `subsite1` may be accessed by `subsite2`, given the proper URL is
accessed.

## API Proxying
All requests through routes starting with the `api_route` value will be 
proxied through the provided `api_host` value. Both `api_route` and `api_host` 
must be configured in the `config.yml` file for API proxying to be enabled. 

## Configuration
The following fields are valid in the `config.yml` file:

| Field Name		| Default			| Purpose							                                    |
|-------------------|-------------------|-----------------------------------------------------------------------|
| `name`			| (none)			| If used, displays the configured name for clarity.					|
| `port`			| `4000`			| The port to run GoPhast from 										    |
| `release`			| `false`			| Whether to suppress stdio logging or not 								|
| `public`			| `./public`		| The [static asset directory][static-assets] to pull assets from 		|
| `asset_route`     | `a`               | The prefix for all [static asssets][static-assets]                    |
| `subsite`			| `(none)`			| If used, specifies which [subsite][subsites] directory to pull from 	|
| `api_route`		| `(none)`			| If used, specifies which routes to [proxy to the provided API][api] 	|
| `api_host`        | `(none)`          | If used, specifies the API address to proxy API request through       |

An example of a GoPhast site can be made with the following configuration:
```yaml
name: GoPhast Example
port: 5000
release: true
public: ./here
subsite: subsite
asset_route: static
api_route: api/v1
api_host: localhost:4000
```

This configuration will:
1. Run a GoPhast site on port `5000` named `GoPhast Example`
1. Suppress logging through the `release` flag
1. Pull assets from the `./here` folder (relative to the current directory) 
   when a `/static/asset_name.asset_type` path is requested
1. Proxy all requests to `/api/v1/...` through `localhost:4000/...`

## Installing Globally
To install GoPhast, run
```shell script
$ go install github.com/lincolnanders5/gophast
```
*Note*: Install locations depend on your configuration. Check `go help install` to see
 where the GoPhast executable will be installed. 
 
 You may need to update your `$PATH` environment variable to include the 
 location of the GoPhast executable. For example, to include the `$HOME/go/bin`
 directory, run (or add to your shell configuration file) 
 ```
export PATH=$PATH:$HOME/go/bin
```

## Running Locally
```shell script
export PATH=$PATH:$HOME/go/bin
# Either...
# - Run quickly, in place
go run github.com/lincolnanders5/gophast/cmd/gophast

# - Compile to root level 
go build github.com/lincolnanders5/gophast/cmd/gophast
./gophast
```


[api]: #api-proxying
[config]: #configuration
[static-assets]: #static-assets
[subsites]: #subsites

[gin]: https://github.com/gin-gonic/gin
[golang]: https://golang.org
[LACOM]: https://lincolnanders.com