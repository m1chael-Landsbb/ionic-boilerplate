# container-proxy-redirect

A lightweight container for redirecting HTTP traffic to alternative destinations, built on `nginx`

## Resources

- [Docker Hub](https://hub.docker.com/r/webproxy/container-redirect/)

## Configuration

### Environment variables

- `SERVER_REDIRECT` - destination server for redirection, eg. `www.example.com`
- `SERVER_NAME` - optionally specify the server name to listen on eg. `~^www.(?<subdomain>.+).example.com`
   - beneficial for capturing variables to use in server_redirect. 
- `SERVER_REDIRECT_PATH` - optionally specify path for redirecting all requests eg. `/landingpage`
   - if unset nginx var `$request_uri` is utilized
- `SERVER_REDIRECT_SCHEME` - optionally specify scheme for redirection 
   - if unset but X-Forwarded-Proto is sent as request header with value 'https' this will be applied. 
     In all other scenarios nginx var `$scheme` is utilized
- `SERVER_REDIRECT_CODE` - optionally specify the http status code for redirection
   - if unset or not in allowed codes list 301 is applied as default
   - allowed Codes are: 301, 302, 303, 307, 308
 - `SERVER_REDIRECT_POST_CODE` - optionally specify the http code for POST redirection
    - beneficial if client should maintain the request method from POST to GET
    - if unset or not in allowed Codes `SERVER_REDIRECT_CODE` is applied
    - by default all requests will be redirected with identical status code
- `SERVER_ACCESS_LOG` - optionally specify the location where nginx will write its access log
   - if unset /dev/stdout is utilized
- `SERVER_ERROR_LOG` - optionally specify the location where nginx will write its error log
   - if unset /dev/stderr is utilized

Refer to `docker-compose.yml` file for examples.

## Usage

With `docker-compose`

    docker-compose up -d
    
With `docker`    

    docker run -e SERVER_REDIRECT=www.example.com -p 8888:80 webproxy/container-redirect
    docker run -e SERVER_REDIRECT=www.example.com -e SERVER_REDIRECT_PATH=/landingpage -p 8888:80 webproxy/container-redirect
    docker run -e SERVER_REDIRECT=www.example.com -e SERVER_REDIRECT_PATH=/landingpage -e SERVER_REDIRECT_SCHEME=https -p 8888:80 webproxy/container-redirect

---

Built by [devteam](http://development-studio.io)
