#### 一、项目介绍
基于区块链Hyperledger Fabric V2.5 的农产品/商品等的通用溯源系统，部署简单，附压测工具、区块链浏览器，文档详细。可以快速使用本系统搭建自己的溯源系统，帮助想法快速落地。

#### 二、版权声明
本项目基于Apache License 2.0开源协议，在个人的科研、学习范围内可以自由使用，请附上项目链接。如有商业需求或合作需求，需要联系作者购买授权。

#### 三、项目特点
本项目采用Hyperledger Fabric V2.5，属于目前最新的Fabric版本，具有更好的性能和稳定性，调用链码使用Fabric-gateway模式，是当前版本的推荐方式。内置了tape压测工具，可以方便的对区块链网络进行压测；内置了区块链浏览器，可以方便地查询交易信息。
项目结构清晰，代码注释详细，方便二次开发。结合了mysql实现账户注册登录功能，更符合真实业务场景。
#### 四、项目背景
区块链技术的出现，为溯源系统的建设提供了新的思路。区块链技术的不可篡改性、去中心化、可追溯等特点，使得区块链技术成为溯源系统的理想选择。本项目基于Hyperledger Fabric V2.5，实现了一个农产品溯源系统。 在本区块链系统中，有5个内置的角色：种植户、工厂、驾驶员、商店、消费者。其中种植户、工厂、驾驶员、商店可以将信息上链，消费者有信息溯源权限。上述可以上链信息的角色各可以输入5个农产品的属性，方便二次开发。本项目的目标是作为Fabric V2.5下的一个通用溯源模板。

#### 五、搭建步骤
 强烈推荐：使用云服务器搭建本系统，虚拟机问题较多。点击此链接购买腾讯云服务器：[https://curl.qcloud.com/Sjy0zKjy](https://curl.qcloud.com/Sjy0zKjy) 点击首单特惠，购买2核4G或以上的服务器，218/年，如果后续准备做程序开发可以用新用户优惠买三年的。

严格按照以下步骤操作：

1. 安装docker 

	```bash
	#下载docker 
	curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun 
	#添加当前用户到docker用户组 
	sudo usermod -aG docker $USER 
	newgrp docker 
	#向/etc/docker/daemon.json写入docker 镜像源
	{
	    "registry-mirrors": ["https://punulfd2.mirror.aliyuncs.com"]
	}
	  #重启docker 
	sudo systemctl restart docker
	```

2. 安装开发使用的go、node、jq

	```bash
	#下载二进制包
	wget https://golang.google.cn/dl/go1.19.linux-amd64.tar.gz
	#将下载的二进制包解压至 /usr/local目录
	sudo tar -C /usr/local -xzf go1.19.linux-amd64.tar.gz
	mkdir $HOME/go
	#将以下内容添加至环境变量 ~/.bashrc
	export GOPATH=$HOME/go
	export GOROOT=/usr/local/go
	export PATH=$GOROOT/bin:$PATH
	export PATH=$GOPATH/bin:$PATH
	#更新环境变量
	source  ~/.bashrc 
	#设置代理
	go env -w GO111MODULE=on
	go env -w GOPROXY=https://goproxy.cn,direct
	
	#下载nvm安装脚本
	wget https://gitee.com/real__cool/fabric_install/raw/main/nvminstall.sh
	#安装nvm；屏幕输出内容添加环境变量
	chmod +x nvminstall.sh
	./nvminstall.sh
	# 将环境变量写入.bashrc
	export NVM_DIR="$HOME/.nvm"
	[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
	[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
	# 更新环境变量
	source  ~/.bashrc
	# 安装node16
	nvm install 16
	#换源
	npm config set registry https://registry.npmmirror.com
	
	#安装jq 
	sudo apt install jq
	```



3. 克隆本项目 

	```bash
	git clone https://gitee.com/real__cool/fabric-trace
	```

4. 启动区块链部分。在fabric-trace/blockchain/network目录下:

	```bash
	// 【仅在首次使用执行】下载Fabric Docker镜像
	./install-fabric.sh -f 2.5.6 d 
	```

	```bash
	// 启动区块链网络
	./start.sh
	```


5. 启动后端 在fabric-trace/application/backend目录下： 执行： `go run main.go`

6. 新开一个窗口，启动前端 在fabric-trace/application/web目录下： 执行： 

	```bash
	npm install 
	npm run dev
	```

#### 六、本项目相关的后续计划：

1. 本项目将持续维护，欢迎给项目点亮Star与B站三连，非常感谢！TrueTechLabs公众号（TrueTechLabs公众号将提供更多的AI与Web3的内容。）将更新此项目的区块链部分、前后端说明文档。
此外：本项目简易的二次开发流程将在[《Fabric项目学习笔记 》](https://blog.csdn.net/qq_41575489/category_12075943.html)发布,欢迎订阅支持！
2. 本系统的讲解课程（约2小时）、Fabric V2.5应用的课程（约10小时），将在B站上架，敬请关注！

#### 七、目前已知存在的BUG：
1. 区块链浏览器镜像拉取较慢，因为不是Docker官方的仓库，需要耐心等待。
2. 区块链浏览器有时候会出现无法访问的情况，可以尝试重启浏览器容器。