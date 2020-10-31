# `docker-yarp`

A simple Docker container to run a lightweight configurable instance of the [YARP reverse proxy server](https://github.com/microsoft/reverse-proxy). Essentially this is just a pre-built, pre-packaged image of the basic example server (similar to the [Getting Started guide](https://microsoft.github.io/reverse-proxy/articles/getting_started.html)).

## Usage

```bash
docker run -d -p 8000:80 -v /path/to/config.json:/app/reverseProxy.json quay.io/agc93/yarp:latest
```

## Configuration

To configure the proxy, just bind a valid configuration file to the `/app/reverseProxy.json` file. As an example:

```json
{
    "ReverseProxy": {
        "Routes": [{
            "RouteId": "apiv1",
            "ClusterId": "backend",
            "Match": {
                "Path": "{**catch-all}"
            }
        }],
        "Clusters": {
            "backend": {
                "Destinations": {
                    "backend1/server": {
                        "Address": "https://api.example.com/v1/"
                    }
                }
            }
        }
    }
}
```

The configuration file is loaded directly into ASP.NET Core so you can also add any other server configuration (such as logging configs) here as well.

> To make it easier, you can actually add your proxy configuration using either JSON or YAML (using `/app/reverseProxy.yml`)