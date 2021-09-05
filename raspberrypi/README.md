
## Build Xmrig for Raspberry Pi 4 64bit with Huge Pages

1. Install prerequisites

    Use custom 64bit Kernel from here
    ```
    https://github.com/kraoc/raspberry-linux-64
    ```

    Install depencies
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
