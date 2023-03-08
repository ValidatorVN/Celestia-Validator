# Celestia - Validator- Full Node - Light Node

Cấu hình khuyến cáo:

Memory: 8 GB RAM
CPU: 6 cores
Disk: 500 GB SSD Storage
Bandwidth: 1 Gbps for Download/100 Mbps for Upload

# Bộ cài đặt cần thiết:

- Cập nhật hệ thống

    sudo apt update && sudo apt upgrade -y
    
- Cài đặt Package bổ sung:

    sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
 
- Cài đặt Golang:

    ver="1.19.1" 
    cd $HOME 
    wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" 
    sudo rm -rf /usr/local/go 
    sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" 
    rm "go$ver.linux-amd64.tar.gz"

- Lưu vào thư mục $PATH

    echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
    source $HOME/.bash_profile
    
- Lệnh kiểm tra phiên bản Go: phiên bản 1.19.1 là bạn đã cài đặt đúng.

    go version
    
# Dành cho chạy Validator:

- Cài đặt App

    cd $HOME 
    rm -rf celestia-app 
    git clone https://github.com/celestiaorg/celestia-app.git 
    cd celestia-app/ 
    APP_VERSION=v0.12.0 
    git checkout tags/$APP_VERSION -b $APP_VERSION 
    make install
    
- Thêm kết nối P2P:

    cd $HOME
    rm -rf networks
    git clone https://github.com/celestiaorg/networks.git
    
- Chọn tên thay cho node-name, chọn chain thay cho mocha:

    celestia-appd init "node-name" --chain-id mocha
  

 Update soon
 
 # Dành cho chạy Light - Node:
 
 - Tải về bộ mã nguồn và cài đặt: Nên copy từng dòng cho đỡ mắc sai sót.
 
    cd $HOME 
    rm -rf celestia-node 
    git clone https://github.com/celestiaorg/celestia-node.git 
    cd celestia-node/ 
    git checkout tags/v0.7.0 
    make build 
    make install 
    make cel-key
  
 - Kiểm tra phiên bản, khớp với thông tin này là bạn đã cài đúng:
 
    celestia version
    
Semantic version: v0.7.0 
Commit: 7226f02794bdf11a91116d0d4cd88399f05149ad 
Build Date: Thu Dec 15 10:19:22 PM UTC 2022 
System version: amd64/linux 
Golang version: go1.19.1 

- Khởi tạo mạng Light - Node:

    celestia light init --p2p.network blockspacerace
    
-> Hiểm thị thông tin như dưới đây là chính xác:

$ celestia light init
2022-12-19T21:45:24.591Z        INFO    node    nodebuilder/init.go:19  Initializing Light Node Store over '/root/.celestia-light-blockspacerace'
2022-12-19T21:45:24.591Z        INFO    node    nodebuilder/init.go:50  Saving config   {"path": "/root/.celestia-light-blockspacerace/config.toml"}
2022-12-19T21:45:24.592Z        INFO    node    nodebuilder/init.go:51  Node Store initialized

- Khởi chạy mạng Celestia Light Node: Thay thế IP address = IP VPS; thay thế port = 26659

    celestia light start --core.ip <ip-address> --gateway --gateway.addr <ip-address> --gateway.port <port> --p2p.network blockspacerace
  
Mở thêm một cửa sổ Terminal truy cập vào VPS:

- Tạo Cel-Key:

    git clone https://github.com/celestiaorg/celestia-node.git
    cd celestia-node/
    make cel-key
    
- Tạo Node Key: Đổi kay-name -> tên bạn muốn đặt

    ./cel-key add <key-name> --keyring-backend test --node.type light --p2p.network blockspacerace
    
- Xuất mã key để lưu trữ:

    ./cel-key export <key-name> --keyring-backend test --node.type light --p2p.network blockspacerace
    
- Trường hợp chạy lại node mới, muốn chép key cũ vào node:

    ./cel-key add <key-name> --recover --keyring-backend test --node.type light --p2p.network blockspacerace
    
- Khởi chạy mạng light node với Key bạn vừa tạo: Thay thế IP address = IP VPS; Port = 26659

    celestia light start --core.ip <ip-address> --keyring.accname <key_name> --gateway --gateway.addr <ip-address> --gateway.port <port> --p2p.network blockspacerace

    ./cel-key list --node.type light --keyring-backend test --p2p.network blockspacerace


    
  
  
