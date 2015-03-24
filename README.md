# Minimal Erlang Docker Image

This is a minimal Docker image that contains the `erlang-base` installation from [Erlang Solutions][1]. It weights in at 104.9 Mb  and cointains nothing more than the Debian base image plus the `erlang-base` package. It is ideal for running an Erlang or Elixir release without the need to install any additional dependencies.

# Adding an Erlang and Elixir App

This image can be used to install an Erlang or Elixir release directly. You don't need to include the Erlang Runtime System (ERTS) into the release since it is provided by the `erlang-base` package. For example, using [Relx][2] you can generate the release with the flag `--include-erts false` or the `relx.config` directive `{include_erts, false}`.

Copy over your release into the image and run your start script. This should automatically find the Erlang VM and start your release.

# Dependencies

Most likely your dependencies will be included in your Erlang release archive already. If you need to install any dependencies as Debian packages, you can exclude them from your release and install them manually. The following packages are installed by default:

* `erlang-base` - Erlang/OTP virtual machine and base applications
* `erlang-syntax-tools` - Erlang/OTP modules for handling abstract Erlang syntax trees
* `erlang-crypto` - Erlang/OTP cryptographic modules

Since this images only has `erlang-base` and dependencies installed, you can add extra Erlang libraries by installing the necessary packages if you need to. To add additional packages, install them before adding your application to your image:

```Dockerfile
RUN apt-get update && \
    apt-get install -y erlang-xmerl=VERSION && \
    rm -rf /var/lib/apt/lists/*
```

[1]: https://www.erlang-solutions.com/downloads/download-erlang-otp "Download Erlang OTP | Erlang Solutions"
[2]: https://github.com/erlware/relx "erlware/relx - A release assembler for Erlang"
