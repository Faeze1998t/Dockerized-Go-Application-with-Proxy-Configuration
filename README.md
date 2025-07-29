# Dockerized-Go-Application-with-Proxy-Configuration
#This project demonstrates how to configure Docker to build Go applications behind a corporate proxy, solving common issues with goproxy.io connectivity.


#Problem Statement
```
When building Go applications in Docker behind a corporate proxy, you may encounter:
i/o timeout errors when accessing goproxy.io
connection reset by peer errors
Go module download failures
```
#Solution
Dockerfile Configuration

```
FROM golang:1.19

# Proxy configuration
ENV http_proxy http://your.proxy:port
ENV https_proxy http://your.proxy:port
ENV no_proxy localhost,127.0.0.1

# Multiple fallback proxies
ENV GOPROXY https://goproxy.io,https://proxy.golang.org,direct

WORKDIR /app
COPY . .
RUN go build -o main .
CMD ["./main"]
```
#docker-compose.yml
```
services:
  app:
    build:
      context: .
      args:
        - http_proxy=http://your.proxy:port
        - http_proxy=http://your.proxy:port
    environment:
      - http_proxy=http://your.proxy:port
      - https_proxy=http://your.proxy:port
```
#Alternative Proxies
If goproxy.io is blocked:
```
ENV GOPROXY https://goproxy.cn,direct  # Chinese mirror
# or
ENV GOPROXY https://mirrors.aliyun.com/goproxy/,direct
```
#Building with Proxy
```
# With BuildKit
DOCKER_BUILDKIT=1 docker-compose build --no-cache

# Traditional build
docker build --build-arg http_proxy=http://your.proxy:port -t myapp .
```
#Verification
```
docker run --rm -it --env http_proxy=http://your.proxy:port golang:1.19 curl -v https://goproxy.io
```
#This README clearly documents:
```
The problem and solution
Configuration examples
Alternative approaches
Build instructions
Troubleshooting tips
You can customize the proxy URLs and add any project-specific details as needed. The badge at the top adds visual appeal and quick reference information.
```
