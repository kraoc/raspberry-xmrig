# XMRig

[![Github All Releases](https://img.shields.io/github/downloads/xmrig/xmrig/total.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub release](https://img.shields.io/github/release/xmrig/xmrig/all.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub Release Date](https://img.shields.io/github/release-date/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub license](https://img.shields.io/github/license/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/network)

XMRig is a high performance, open source, cross platform RandomX, KawPow, CryptoNight and AstroBWT unified CPU/GPU miner and [RandomX benchmark](https://xmrig.com/benchmark). Official binaries are available for Windows, Linux, macOS and FreeBSD.

## Mining backends
- **CPU** (x64/ARMv8)
- **OpenCL** for AMD GPUs.
- **CUDA** for NVIDIA GPUs via external [CUDA plugin](https://github.com/xmrig/xmrig-cuda).

## Download
* **[Binary releases](https://github.com/xmrig/xmrig/releases)**
* **[Build from source](https://xmrig.com/docs/miner/build)**

## Usage
The preferred way to configure the miner is the [JSON config file](https://xmrig.com/docs/miner/config) as it is more flexible and human friendly. The [command line interface](https://xmrig.com/docs/miner/command-line-options) does not cover all features, such as mining profiles for different algorithms. Important options can be changed during runtime without miner restart by editing the config file or executing [API](https://xmrig.com/docs/miner/api) calls.

* **[Wizard](https://xmrig.com/wizard)** helps you create initial configuration for the miner.
* **[Workers](http://workers.xmrig.info)** helps manage your miners via HTTP API.

## Donations
* Default donation 1% (1 minute in 100 minutes) can be increased via option `donate-level` or disabled in source code.
* XMR: `48edfHu7V9Z84YzzMa6fUueoELZ9ZRXq9VetWzYGzKt52XU5xvqgzYnDK9URnRoJMk1j8nLwEVsaSWJ4fhdUyZijBGUicoD`

## Developers
* **[xmrig](https://github.com/xmrig)**
* **[sech1](https://github.com/SChernykh)**

## Contacts
* support@xmrig.com
* [reddit](https://www.reddit.com/user/XMRig/)
* [twitter](https://twitter.com/xmrig_dev)

## Build Xmrig for Raspberry Pi 4 64bit with Huge Pages

1. Install prerequisites

    ```
    sudo apt-get install git build-essential cmake automake libtool autoconf
    ```

2. Grab sources

    ```
    cd /opt
    mkdir -p /opt/xmrig
    git clone --depth=1 https://github.com/xmrig/xmrig
    ```

3. Refresh sources (for update previous builds)

    ```
    git pull
    ```

4. Use specific config file

    Prepare run folder:
    ```
    mkdir -p /opt/xmrig/worker
    cd /opt/xmrig/worker
    ```

    Backup config file:
    ```
    mv config.json config.json.backup
    ```

    Copy fonctionnal config file:
    ```
    cp rpi_xmrig.config.txt config.json
    ```

    Adjust specific settings:
    ```
    user: put here your user token
    rig-id : you can specify worker name
    ```

5. Building the tool

    ```
    mkdir xmrig/build
    cd xmrig/scripts
    ./build_deps.sh
    cd ../build
    cmake .. -DCMAKE_BUILD_TYPE=Release -DWITH_OPENCL=OFF -DWITH_CUDA=OFF -DXMRIG_DEPS=scripts/deps -DWITH_SSE4_1=OFF
    make -j$(nproc)
    ```

6. Copy files

    ```
    cd /opt/xmrig/worker
    ln -s ../build/xmrig xmrig
    ```
