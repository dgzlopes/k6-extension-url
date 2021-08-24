# xk6-url

This is a [k6](https://go.k6.io/k6) extension using the [xk6](https://github.com/grafana/xk6) system.

| :exclamation: This is a proof of concept, isn't supported by the k6 team, and may break in the future. USE AT YOUR OWN RISK! |
|------|

## Build

To build a `k6` binary with this extension, first ensure you have the prerequisites:

- [Go toolchain](https://go101.org/article/go-toolchain.html)
- Git

Then:

1. Install `xk6`:
  ```shell
  $ go install go.k6.io/xk6/cmd/xk6@latest
  ```

2. Build the binary:
  ```shell
  $ xk6 build --with github.com/dgzlopes/xk6-url@latest
  ```

## Example

```javascript
import url from 'k6/x/url';

export default function () {
  var parsed = url.parse("user.local:8000/path")
  console.log(`Scheme: ${parsed.scheme} `)
  console.log(`Host: ${parsed.host}`)
  console.log(`Path: ${parsed.path}`)

  var normalized = url.normalize("localhost:80///x///y/z/../././index.html?b=y&a=x#t=20")
  console.log(normalized)

  var split = url.splitHostPort("localhost:80")
  console.log(`Host: ${split.host}`)
  console.log(`Port: ${split.port}`)

  console.log(url.resolve("example.com"))
}
```

Result output:

```
$ ./k6 run script.js

          /\      |‾‾| /‾‾/   /‾‾/   
     /\  /  \     |  |/  /   /  /    
    /  \/    \    |     (   /   ‾‾\  
   /          \   |  |\  \ |  (‾)  | 
  / __________ \  |__| \__\ \_____/ .io

  execution: local
     script: ../example.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 10m30s max duration (incl. graceful stop):
           * default: 1 iterations for each of 1 VUs (maxDuration: 10m0s, gracefulStop: 30s)

INFO[0000] Scheme: http                                  source=console
INFO[0000] Host: user.local:8000                         source=console
INFO[0000] Path: /path                                   source=console
INFO[0000] http://localhost/x/y/index.html?a=x&b=y#t=20  source=console
INFO[0000] Host: localhost                               source=console
INFO[0000] Port: 80                                      source=console
INFO[0000] 93.184.216.34                                 source=console

running (00m00.0s), 0/1 VUs, 1 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  00m00.0s/10m0s  1/1 iters, 1 per VU

    data_received........: 0 B 0 B/s
    data_sent............: 0 B 0 B/s
    iteration_duration...: avg=906.98µs min=906.98µs med=906.98µs max=906.98µs p(90)=906.98µs p(95)=906.98µs
    iterations...........: 1   44.168084/s
```
