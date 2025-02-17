---
title: Use a plugin
description: HelloWorld plugin tutorial
sidebar_position: 8
---

# Use the `HelloWorld` plugin

This tutorial shows you how to use the `HelloWorld` [plugin](../concepts/plugins.md), which exposes a JSON-RPC endpoint to return a greeting message in Spanish.

The [plugin interface](https://github.com/ConsenSys/quorum-plugin-definitions/blob/master/helloworld.proto) is implemented in Go and Java. The plugin can [reload](../concepts/plugins.md#plugin-reloading) changes from its JSON configuration.

## Prerequisites

- [GoQuorum built from source](../deploy/install/binaries.md#goquorum)
- [Gox](https://github.com/mitchellh/gox) installed

## Build the plugin distribution file

1.  Clone the plugin repository:

    ```bash
    git clone --recursive https://github.com/ConsenSys/quorum-plugin-hello-world.git
    cd quorum-plugin-hello-world
    ```

2.  Use the Go implementation of the plugin:

    ```bash
    cd go
    make
    ```

    `quorum-plugin-hello-world-1.0.0.zip` is now created in the `build` directory. The file `hello-world-plugin-config.json` is the JSON configuration file for the plugin.

3.  Rename `quorum-plugin-hello-world-1.0.0.zip` according to your operating system:

    <!--tabs-->

    # Linux

    ```
    quorum-plugin-hello-world-1.0.0-linux-amd64.zip
    ```

    # Intel MacOS

    ```
    quorum-plugin-hello-world-1.0.0-darwin-amd64.zip
    ```

    <!--/tabs-->

## Start GoQuorum with plugin support

1.  Navigate to your GoQuorum installation directory. Copy the `HelloWorld` plugin distribution file and its JSON configuration `hello-world-plugin-config.json` to `build/bin`.

2.  Create `geth-plugin-settings.json`:

    ```bash
    cat > build/bin/geth-plugin-settings.json <<EOF
    {
     "baseDir": "./build/bin",
     "providers": {
       "helloworld": {
         "name":"quorum-plugin-hello-world",
         "version":"1.0.0",
         "config": "file://./build/bin/hello-world-plugin-config.json"
       }
     }
    }
    EOF
    ```

    :::note

    For the configured locations, `./` means that you start from the working directory, from where the program is launched.

    :::

3.  Run `geth` with the plugin:

    ```bash
    PRIVATE_CONFIG=ignore \
    geth \
        --nodiscover \
        --verbosity 5 \
        --networkid 10 \
        --raft \
        --raftjoinexisting 1 \
        --datadir ./build/_workspace/test \
        --http \
        --http.api eth,debug,admin,net,web3,plugin@helloworld \
        --plugins file://./build/bin/geth-plugin-settings.json \
        --plugins.skipverify
    ```

    Run `ps -ef | grep helloworld` to reveal the `HelloWorld` plugin process.

## Test the plugin

1.  Test the `HelloWorld` plugin using the following command:

    <!--tabs-->

    # curl HTTP request

    ```bash
    curl -X POST http://localhost:8545 \
        -H "Content-type: application/json" \
        --data '{"jsonrpc":"2.0","method":"plugin@helloworld_greeting","params":["Quorum Plugin"],"id":1}'
    ```

    # JSON result

    ```json
    { "jsonrpc": "2.0", "id": 1, "result": "Hello Quorum Plugin!" }
    ```

    <!--/tabs-->

2.  Update the plugin configuration `build/bin/hello-world-plugin-config.json` to support the `es` language (Spanish).

3.  Reload the plugin using the following command:

    <!--tabs-->

    # curl HTTP request

    ```bash
    curl -X POST http://localhost:8545 \
        -H "Content-type: application/json" \
        --data '{"jsonrpc":"2.0","method":"admin_reloadPlugin","params":["helloworld"],"id":1}'
    ```

    # JSON result

    ```json
    { "jsonrpc": "2.0", "id": 1, "result": true }
    ```

    <!--/tabs-->

4.  Re-run the `HelloWorld` plugin:

    <!--tabs-->

    # curl HTTP request

    ```bash
    curl -X POST http://localhost:8545 \
        -H "Content-type: application/json" \
        --data '{"jsonrpc":"2.0","method":"plugin@helloworld_greeting","params":["Quorum Plugin"],"id":1}'
    ```

    # JSON result

    ```json
    { "jsonrpc": "2.0", "id": 1, "result": "Hola Quorum Plugin!" }
    ```

    <!--/tabs-->
