## 启用无 PD 模式

- hgstore 设置 yml 文件 **app.fake-pd: true**
- hugegraph 设置 properties 文件 **pd.fake=true**，设置**hstore.peers=** 指向 hgstore 的 grpc
  地址，多个地址逗号分割

````
    backend=hstore
    serializer=binary
    pd.peers=localhost:9000
    pd.fake=true
    hstore.peers=127.0.0.1:9081,127.0.0.1:9082,127.0.0.1:9083
    store=hugegraph
````

###FakePD 集群配置
如果启用 fakePD 模式的集群部署，需要在 hgstore 的 yml 文件增加 fake-pd 配置

+ **store-list:** store 集群列表
+ **peers-list:** raft 集群列表

````
    fake-pd:
            store-list: 127.0.0.1:9080,127.0.0.1:9081,127.0.0.1:9082
            peers-list: 127.0.0.1:8080,127.0.0.1:8081,127.0.0.1:8082
````

###本地打包

项目根目录下执行：

````
./mvnw -Dmaven.test.skip=true package
````

文件保存在项目根目录下的 dist 文件夹中。

###本地单测

- 在跑单测之前需要启动一个 pd 和 store 服务
- 所以的单测统一在 hg-store-test 模块中撰写，client、common、core 等与项目的模块一一对应，相应模块下的单测写到对应的文件夹下
- 保持目录结构与项目模块对应，文件命名必须包含"Test"字样，把编写好的类加到对应的 SuiteTest 类中，由
  Suite 调度执行
- 单测可以用编译工具单个执行，也可以到代码的根目录下执行：mvn verify -Pjacoco
- 代码覆盖率查看，需要先执行：mvn verify -Pjacoco 后，在到根目录的 target->site->jacoco->index.html