
## Build Xmrig for Raspberry Pi 4 64bit with Huge Pages

1. Prerequisites

    Use custom RPi 64bit Kernel from here (go in *raspberry* folder)
    ```
    https://github.com/kraoc/raspberry-linux-64
    ```

    Install dependencies
    ```
    sudo apt-get install git build-essential cmake automake libtool autoconf git build-essential cmake libuv1-dev libmicrohttpd-dev libssl-dev gcc-8 g++-8
    ```

2. Grab sources

    *If you already done this step and only want to update the kernel, then skip to step 3*
    
    Make a dedicated folder inside the opt path and get the sources
    ```
    cd /opt
    git clone --depth=1 https://github.com/xmrig/xmrig
    cd xmrig
    ```

3. Refresh sources

    Only when you already compiled and needed update previous builds
    ```
    git pull
    ```

4. Building

    Build dependencies
    ```
    mkdir build
    cd scripts
    ./build_deps.sh
    ```
   
    Build the app 
    ```
    cd ../build
    cmake .. -DCMAKE_BUILD_TYPE=Release -DWITH_OPENCL=OFF -DWITH_CUDA=OFF -DXMRIG_DEPS=scripts/deps -DWITH_SSE4_1=OFF -DCMAKE_C_COMPILER=gcc-8 -DCMAKE_CXX_COMPILER=g++-8
    make -j4
    ```

5. Use specific config file

    Prepare run folder
    ```
    mkdir -p /opt/xmrig/worker
    cd /opt/xmrig/worker
    ```

    Backup config file
    ```
    mv config.json config.json.backup
    ```

    Copy fonctionnal config file
    ```
    cp rpi_xmrig.config.txt config.json
    ```

    Adjust specific settings
    ```
    user: put here your user token
    rig-id : you can specify worker name
    ```


6. Copy/link files

    ```
    cd /opt/xmrig/worker
    ln -s ../build/xmrig xmrig
    ```

7. Add SystemD unit

    ```
    sudo nano /etc/systemd/system/xmrig.service
    ```

    ```
    [Unit]
    Description=XMRig Daemon
    After=network.target

    [Service]
    User=root
    Group=root
    Type=simple
    Nice=+10
    ExecStart=/opt/xmrig/worker/xmrig --config=/opt/xmrig/worker/config.json
    Restart=always

    [Install]
    WantedBy=multi-user.target    
    ```

8. Start mining

    Edit */opt/xmrig/worker/config.json* to add *user* (your user hash) and *url* (of the pool you choose).
    
    Enable xmrig.service then run it !
