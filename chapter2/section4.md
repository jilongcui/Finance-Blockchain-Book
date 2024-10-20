# 分布式存储技术IPFS

## 本地部署客户端（公共网络）

在本地部署IPFS（InterPlanetary File System）非常简单。以下是一个基本的指南来帮助你在本地进行设置：

### 1. 安装IPFS

首先，你需要下载并安装IPFS客户端。目前，最常用的实现是Go语言编写的`go-ipfs`。

#### 对于Linux和MacOS:

- 打开终端。

- 运行以下命令下载并解压最新版本的go-ipfs

  ```bash
  wget https://dist.ipfs.tech/go-ipfs/v0.12.2/go-ipfs_v0.12.2_linux-amd64.tar.gz
  tar -xvzf go-ipfs_v0.12.2_linux-amd64.tar.gz
  cd go-ipfs
  sudo bash install.sh
  ```

#### 对于Windows:

- 下载最新的Windows版本的`go-ipfs`压缩包。
- 解压文件，并将可执行文件所在目录添加到系统的PATH环境变量中。

### 2. 初始化IPFS节点

在安装完成之后，你需要初始化你的IPFS节点：

```bash
ipfs init
```

这会创建一个新的配置文件（默认在你的用户主目录下的`.ipfs`目录中）。

### 3. 启动IPFS守护进程

通过运行以下命令来启动IPFS守护进程，以便可以开始与IPFS网络互动：

```bash
ipfs daemon
```

启动守护进程后，你的节点将连接到IPFS网络，并开始发现其他对等节点。

### 4. 添加文件到IPFS

当IPFS守护进程正在运行时，你可以使用以下命令将文件添加到网络中：

```bash
ipfs add <filename>
```

这个命令将返回一个唯一的内容标识符 (CID)，你可以使用它来访问该文件。

### 5. 获取文件

如果你有另一个CID，可以使用以下命令从网络获取相应的数据：

```bash
ipfs cat <CID>
```

或下载该文件：

```bash
ipfs get <CID>
```

### 注意事项

- **防火墙设置**: 确保你的计算机防火墙允许所需端口进行通信，通常是4001端口用于传入连接。
- **存储空间**: IPFS会占用一定量的硬盘空间来缓存数据，请确保有足够的空间。
- **安全性**: 如果要进行生产部署，需要考虑数据加密和访问控制策略。

完成上述步骤后，你已经成功地在本地部署了一个IPFS节点，并且能够上传和下载文件。

## 部署私有IPFS网络

构建你自己本地的IPFS网络（也称为私有IPFS网络）是完全可行的，并且在某些情况下，尤其是在需要对数据访问进行严格控制或处理敏感信息时，非常有用。以下是如何实现这一目的的一些步骤和注意事项：

### 构建本地/私有IPFS网络

1. **安装IPFS软件**

   确保在每个参与节点上安装了IPFS客户端（例如go-ipfs）。可以参考之前提供的安装指导完成这一步。

2. **初始化IPFS节点**

   在每台机器上初始化一个新的IPFS节点：

   ```bash
   ipfs init
   ```

3. **配置Swarm密钥**

   为了建立一个私有网络，你需要设置一个共享的`swarm.key`文件，这会限制只有拥有相同密钥的节点才能互连。

   - 生成一个新密钥文件:

     ```bash
     git clone https://github.com/Kubuxu/go-ipfs-swarm-key-gen
     cd go-ipfs-swarm-key-gen/
     go run ipfs-swarm-key-gen/main.go > $HOME/.ipfs/swarm.key
     ```

   - 将生成的`swarm.key`文件复制到所有希望加入该私有网络的节点。

4. **禁用默认引导服务器**

   先查看当前的引导服务器配置：

   ```
   ~/ipfs/go-ipfs$ ipfs bootstrap
   /dnsaddr/bootstrap.libp2p.io/p2p/QmNnooDu7bfjPFoTZYxMNLWUQJyrVwtbZg5gBMjTezGAJN
   /dnsaddr/bootstrap.libp2p.io/p2p/QmQCU2EcMqAqQPR2i9bChDtGNJchTbq5TbXJJ16u19uLTa
   /dnsaddr/bootstrap.libp2p.io/p2p/QmbLHAnMoJPWSCR5Zhtx6BHJX9KiKNN6tpvbUcqanj75Nb
   /dnsaddr/bootstrap.libp2p.io/p2p/QmcZf59bWwK5XFi76CZX8cbJ4BhTzzA3gU1ZjYZcYW3dwt
   /ip4/104.131.131.82/tcp/4001/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
   /ip4/104.131.131.82/udp/4001/quic/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
   ```

   

   编辑IPFS配置文件来移除默认引导节点，确保你的节点不连接到公共IPFS网络。找到并删除相关引导节点条目：

   ```bash
   ipfs bootstrap rm --all
   ```

5. **设置自定义引导节点**

   选择一台机器作为你的初始引导节点，将其多地址添加到其他所有机器中。

   - 检查该节点的多地址：

     ```bash
     ipfs id
     ```

     ```
     {
     	"ID": "12D3KooWMPLJxDGUEf3Lq2BFVbix6BLzBuqFgAenLGucThuVWq2H",
     	"PublicKey": "CAESIKvinxq+m27Fbdnl9W7bvg0j861WJYgObbKP9toR3vnS",
     	"Addresses": [
     		"/ip4/10.0.4.13/tcp/4001/p2p/12D3KooWMPLJxDGUEf3Lq2BFVbix6BLzBuqFgAenLGucThuVWq2H",
     		"/ip4/127.0.0.1/tcp/4001/p2p/12D3KooWMPLJxDGUEf3Lq2BFVbix6BLzBuqFgAenLGucThuVWq2H",
     		"/ip6/::1/tcp/4001/p2p/12D3KooWMPLJxDGUEf3Lq2BFVbix6BLzBuqFgAenLGucThuVWq2H"
     	],
     	"AgentVersion": "go-ipfs/0.12.2/",
     	"ProtocolVersion": "ipfs/0.1.0",
     	"Protocols": [
     		"/ipfs/bitswap",
     		"/ipfs/bitswap/1.0.0",
     		"/ipfs/bitswap/1.1.0",
     		"/ipfs/bitswap/1.2.0",
     		"/ipfs/id/1.0.0",
     		"/ipfs/id/push/1.0.0",
     		"/ipfs/lan/kad/1.0.0",
     		"/ipfs/ping/1.0.0",
     		"/libp2p/autonat/1.0.0",
     		"/libp2p/circuit/relay/0.1.0",
     		"/libp2p/circuit/relay/0.2.0/stop",
     		"/p2p/id/delta/1.0.0",
     		"/x/"
     	]
     }
     ```
     
     
     
   - 在其他节点上将其添加为引导点：
   
     ```bash
     ipfs bootstrap add /ip4/<bootstrap_node_ip>/tcp/<port>/p2p/<peer_id>
     ```
   
6. **启动daemon进程**

   在每个节点上运行：

   ```bash
   ipfs daemon
   
   Initializing daemon...
   go-ipfs version: 0.12.2
   Repo version: 12
   System version: amd64/linux
   Golang version: go1.16.15
   Swarm is limited to private network of peers with the swarm key
   Swarm key fingerprint: 3650f742f8bf2f368bbd517ffe188aa6
   Swarm listening on /ip4/10.0.4.13/tcp/4001
   Swarm listening on /ip4/127.0.0.1/tcp/4001
   Swarm listening on /ip4/172.17.0.1/tcp/4001
   Swarm listening on /ip4/172.18.0.1/tcp/4001
   Swarm listening on /ip4/172.19.0.1/tcp/4001
   Swarm listening on /ip4/172.20.0.1/tcp/4001
   Swarm listening on /ip4/172.21.0.1/tcp/4001
   Swarm listening on /ip4/172.22.0.1/tcp/4001
   Swarm listening on /ip6/::1/tcp/4001
   Swarm listening on /p2p-circuit
   Swarm announcing /ip4/10.0.4.13/tcp/4001
   Swarm announcing /ip4/127.0.0.1/tcp/4001
   Swarm announcing /ip6/::1/tcp/4001
   API server listening on /ip4/127.0.0.1/tcp/5001
   WebUI: http://127.0.0.1:5001/webui
   Gateway (readonly) server listening on /ip4/127.0.0.1/tcp/8080
   Daemon is ready
   ```



### 7. 添加文件到IPFS

当IPFS守护进程正在运行时，你可以使用以下命令将文件添加到网络中：

```bash
$ echo "hello world." > hello.txt
$ ipfs add hello.txt
added QmeS7dohqD5GZagY54KQvKgaoUFoGj8CrYCUjLevVXMrSm hello.txt
```

这个命令将返回一个唯一的内容标识符 (CID)，你可以使用它来访问该文件。

### 8. 获取文件

如果你有另一个CID，可以使用以下命令从网络获取相应的数据：

```bash
$ ipfs cat QmeS7dohqD5GZagY54KQvKgaoUFoGj8CrYCUjLevVXMrSm
hello world.
```

或下载该文件：

```bash
$ ipfs get QmeS7dohqD5GZagY54KQvKgaoUFoGj8CrYCUjLevVXMrSm
```



### 注意事项

- **安全性**: 虽然你的数据不会通过公共互联网传播，但仍要确保传输中的数据经过加密处理，以避免潜在风险。
- **网络设置**: 确保各个计算机之间能够通过指定端口通讯，否则需按照防火墙及路由策略进行适当配置。
- **性能与规模**: 尽管本地或私有部署能有效管理数据权限与安全，但它可能影响到系统扩展能力。如果需求量大，需仔细规划资源分配和管理机制。

通过这些步骤，你可以成功地构建自己的本地或私有IPFS环境，为特定用途提供去中心化的数据存储和共享功能，同时保持较大的控制权。

## 金融机构里的数据适合存储在IPFS

在考虑将IPFS用于金融机构的数据存储时，有几个关键因素需要仔细评估。虽然IPFS在许多方面提供了创新的解决方案，但也存在一些挑战和潜在问题，特别是在高度敏感和合规性要求严格的环境中，如金融行业。

### 优势

1. **分布式存储**: IPFS通过分布式网络共享数据，可以显著提高数据的可用性和冗余性。这种特性有助于防范单点故障，提高系统的弹性。
2. **去中心化**: 去中心化存储可以降低对某一个集中式服务器的依赖，减少被攻击或失效的风险。
3. **内容寻址**: 通过使用加密哈希生成唯一标识符（CID），确保任何试图篡改文件内容的行为都会导致CID的变化，从而提供了一种内置的数据完整性验证方式。

### 挑战

1. **安全和隐私**: 金融机构通常处理高度敏感的数据，IPFS本身并不具备加密功能。因此，需要附加措施来确保数据保密性，比如使用额外的加密层来保护数据传输和存储。
2. **合规性**: 金融数据需要满足严格的合规标准（如GDPR、PCI-DSS等）。由于IPFS是公开网络的一部分，必须慎重评估其是否能够符合相关法律法规。例如，如何确保用户权利（如删除权）在去中心化系统中得到保障可能是一个难题。
3. **访问控制**: 确保只有授权用户才能访问特定数据，这在去中心化网络中可能比传统系统更复杂，需要实施额外机制，以确保数据访问管理到位。
4. **持久性与责任制约**: 在IPFS上，若结点停止共享某个文件，该文件可能变得不可用。尽管Filecoin等项目尝试解决这一问题，但明确的数据持久性和责任归属仍然需要仔细规划。

### 适用场景

- **非敏感或公开数据**: IPFS非常适合作为金融研究报告、市场分析等非机密信息的发布渠道，因为其高可用性和低成本优势。
- **备份与灾备方案**: 可以作为现有集中式系统之外的一种补充手段，用于实现更高水平的数据冗余和灾备能力。

综合考虑这些因素后，如果决定将IPFS应用于金融环境，建议谨慎设计包括加密协议、权限管理以及与传统IT基础设施集成等多个环节，以确保满足业务需求及合规要求。

