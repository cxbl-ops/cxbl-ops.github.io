## 1.安装nodejs,

* 使用nvm或者系统自带的包管理器进行安装（apt或yum）

* nodejs版本 => 8

  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash 
  或
  wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
  执行
  export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # 添加环境变量
  ```
<!--more-->
## 2.部署Docker

* docker 版本  >= 18.06.01

* 安装指南:

  执行

  ```bash
  apt install docker.io 
  或
  yum install docker
  ```
<!--more-->
## 3.部署Caliper

*  新建工作目录

  ```bash
  mkdir benchmarks && cd benchmarks
  ```

* 初始化npm项目

  ```bash
  npm init
  ```

* 安装`caliper-cli`

  ```bash
  npm install --only=prod @hyperledger/caliper-cli@0.2.0
  ```

* 测试是否安装成功

  ```bash
  npx caliper --version
  ```

* 绑定区块链平台

  * 由于Caliper采用了轻量级的部署方式，因此需要显式的绑定步骤指定要测试的平台及适配器版本，`caliper-cli`会自动进行相应依赖项的安装。使用`npx caliper bind`命令进行绑定，命令所需的各项参数可以通过如下命令查看：

    ```bash
    user@ubuntu:~/benchmarks$ npx caliper bind --help
    Usage:
      caliper bind --caliper-bind-sut fabric --caliper-bind-sdk 1.4.1 --caliper-bind-cwd ./ --caliper-bind-args="-g"
    
    Options:
      --help               Show help  [boolean]
      -v, --version        Show version number  [boolean]
      --caliper-bind-sut   The name of the platform to bind to  [string]
      --caliper-bind-sdk   Version of the platform SDK to bind to  [string]
      --caliper-bind-cwd   The working directory for performing the SDK install  [string]
      --caliper-bind-args  Additional arguments to pass to "npm install". Use the "=" notation when setting this parameter  [string]
    ```

* 对于FISCO BCOS，可以采用如下方式进行绑定：

  ```bash
  npx caliper bind --caliper-bind-sut fisco-bcos --caliper-bind-sdk latest
  ```
<!--more-->
## 3.快速体验FISCO BCOS基准测试

* 在工作目录下下载预定义测试用例

  ```bash
  git clone https://github.com/vita-dounai/caliper-benchmarks.git 
  或
  git clone https://gitee.com/mirrors_hyperledger/caliper-benchmarks.git
  ```

  <!--more-->

:::info{title="注意"}
CentOS系统需关闭selinux
:::
## 4.修改配置

* 修改 `/benchmarks/node_modules/\@hyperledger/caliper-fisco-bcos/lib/channelPromise.js` 第49行为：

  ```javascript
    let emitter = emitters.get(seq);
      if(!emitter) {
          //Stale message receieved
          return;
      }
      emitter = emitter.emitter;
  ```

  

* 修改 `/benchmarks/node_modules/\@hyperledger/caliper-fisco-bcos/lib/fiscoBcos.js` 第25行为：

  ```javascript
  const Color = require('./common').Color;
  ```

* 修改 `/benchmarks/node_modules/\@hyperledger/caliper-fisco-bcos/lib/web3lib/web3sync.js` 第27行，91行，118行分别修改为

  27行：

  ```javascript
  uuid = '0x' + uuid.replace(/-/g, '');
  ```

  91行

  ```javascript
  extraData: '0x0'
  ```

  118行

  ```javascript
   extraData: '0x0'
  ```

 * 修改`/benchmarks/node_modules/\@hyperledger/caliper-fisco-bcos中的package.json`文件，在`dependencies`字段中增加 `"secp256k1": "^3.8.0"`,随后在该目录下执行`npm i`

* 修改`/benchmarks/caliper-benchmarks/networks/fisco-bcos/4nodes1group/fisco-bcos.json`，删除其中的`command`字段

* 修改`authentication`字段中`key`,`cert`,`ca`字段的路径为`ficso/nodes/127.0.0.1/sdk/`目录下的对应文件位置
* 修改`nodes`字段中的`rpcport`和`channelport`为本地链端口

* 修改`/benchmarks/caliper-benchmarks/benchmarks/samples/fisco-bcos/helloworld/config.yaml`,在末尾添加：

  ```yaml
  monitor:
    type:
      - process
    process:
      - command: node0
        arguments: fiscoBcosClientWorker.js
        multiOutput: avg
      - command: node1
        arguments: fiscoBcosClientWorker.js
        multiOutput: avg
      - command: node2
        arguments: fiscoBcosClientWorker.js
        multiOutput: avg
      - command: node3
        arguments: fiscoBcosClientWorker.js
        multiOutput: avg
    interval: 0.5
  ```

#### 以下内容仅以helloworld为例

* 修改`/benchmarks/caliper-benchmarks/benchmarks/samples/fisco-bcos/helloworld/`目录下的`get.js`和`set.js`

  get.js

  ```javascript
  /*
  * Licensed under the Apache License, Version 2.0 (the "License");
  * you may not use this file except in compliance with the License.
  * You may obtain a copy of the License at
  *
  * http://www.apache.org/licenses/LICENSE-2.0
  *
  * Unless required by applicable law or agreed to in writing, software
  * distributed under the License is distributed on an "AS IS" BASIS,
  * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  * See the License for the specific language governing permissions and
  * limitations under the License.
  */
  
  'use strict';
  
  module.exports.info = ' querying name';
  
  let bc, contx;
  
  module.exports.init = function (blockchain, context, args) {
      // Do nothing
      bc = blockchain;
      contx = context;
      return Promise.resolve();
  };
  
  module.exports.run = function () {
      return bc.queryState(contx, 'helloworld', 'v0', null, 'get()');
  };
  
  module.exports.end = function () {
      // Do nothing
      return Promise.resolve();
  };
  
  ```

  set.js

  ```javascript
  /*
  * Licensed under the Apache License, Version 2.0 (the "License");
  * you may not use this file except in compliance with the License.
  * You may obtain a copy of the License at
  *
  * http://www.apache.org/licenses/LICENSE-2.0
  *
  * Unless required by applicable law or agreed to in writing, software
  * distributed under the License is distributed on an "AS IS" BASIS,
  * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  * See the License for the specific language governing permissions and
  * limitations under the License.
  */
  
  'use strict';
  
  module.exports.info = ' setting name';
  
  let bc, contx;
  let txnPerBatch;
  
  module.exports.init = function (blockchain, context, args) {
      txnPerBatch = 1;
      bc = blockchain;
      contx = context;
      return Promise.resolve();
  };
  
  /**
   * Generates simple workload
   * @return {Object} array of json objects
   */
  function generateWorkload() {
      let workload = [];
      for (let i = 0; i < txnPerBatch; i++) {
          let w = {
              'transaction_type': 'set(string)',
              'name': 'hello! - from ' + process.pid.toString(),
          };
          workload.push(w);
      }
      return workload;
  }
  
  module.exports.run = function () {
      let args = generateWorkload();
      return bc.invokeSmartContract(contx, 'helloworld', 'v0', args, null);
  };
  
  module.exports.end = function () {
      // Do nothing
      return Promise.resolve();
  };
  ```
<!--more-->
## 5.执行helloworld测试（在benchmarks文件夹中执行）

```bash
npx caliper benchmark run --caliper-workspace caliper-benchmarks --caliper-benchconfig benchmarks/samples/fisco-bcos/helloworld/config.yaml  --caliper-networkconfig networks/fisco-bcos/4nodes1group/fisco-bcos.json
```

参数说明：

`--caliper-workspace` 指定工作目录

`--caliper-benchconfig` 指定测试配置文件

` --caliper-networkconfig`指定网络配置文件

本文参照：

https://github.com/nvm-sh/nvm#troubleshooting-on-linux

https://fisco-bcos-documentation.readthedocs.io/zh_CN/dev/docs/tutorial/caliper.html

https://github.com/FISCO-BCOS/FISCO-BCOS/issues/1248

https://blog.csdn.net/u013288190/article/details/116304877
