# Understanding nginx

Notes on the design and working principles of [nginx](https://nginx.org).

## Configuration File's Structure

nginx consists of modules which are controlled by directives specified in the configuration file.

### Directives

Types of Directives:
1. Simple directive
1. Block directive

#### Simple Directive
Name and parameters separated by spaces and ends with a semicolon (;).

<details>
    <summary>Example</summary>

    worker_processes 1;

    user nobody nogroup;
    error_log /var/log/nginx/error.log warn;
    pid /var/run/nginx.pid;
</details>

#### Block Directive
Name and parameters separated by spaces and ends with a set of additional instructions surrounded by braces (`{` and `}`).

<details>
    <summary>Example</summary>

    location / {
        try_files $uri @proxy_to_app;
    }
</details>

If a block directive can have other directives (both simple and block) inside braces, it is called a __context__. Examples of contexts are `events`, `http`, `server`, and `location`.

<details>
    <summary>Example</summary>

    events {
        worker_connections 768;
        multi_accept on;
    }
</details>

#### Main Context

Directives placed in the configuration file outside of any contexts are considered to be in the __main context__. The `events` and `http` directives reside in the __main context__, `server` directive in `http`, and `location` directive in `server`.

<details>
    <summary>Example</summary>

    worker_processes 1;                             # main context

    http {                                          # main context
        server {                                    # context
            listen 80 default_server;
            return 444;
        }
    }
</details>
