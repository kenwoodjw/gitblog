# [如何搭建一套内部的devops流程](https://github.com/kenwoodjw/gitblog/issues/4)

# 工具选型
- 代码仓库: gitlab
- 持续集成: jenkins
- 镜像仓库: harbor
- maven仓库，npm仓库: nexus
- 堡垒机: jumpserver
- 日志查询: grafana
- 文件存储: minio
- http网关: apisix
- 文档中心: wiki
- 文档归档: nextcloud
- 代码质量: sonarqube
- 需求bug流程： 禅道社区版
- 监控: prometheus, uptime kuma

# gitflow 制定
![GitFlow工作流](https://github.com/kenwoodjw/gitblog/assets/10386710/dea138e9-aade-4e2f-9bb7-4031fd1e36ba)

# CI/CD
jenkins开启gitlab账号认证，使用jenkins 构建jar包，systemd启动jar包，profile文件注入中间件等环境变量信息
```
[Unit]
Description=$directory.service
After=syslog.target

[Service]
User=root
Type=simple
EnvironmentFile=$server_dir/profile
ExecStart=/bin/sh -c "java \$JAVA_OPTS -Xmx1024m -Xms1024m -Dfile.encoding=utf-8 -Dfastjson.parser.safeMode=true -Dfastjson.parser.autoTypeSupport=false -jar $server_dir/$directory/xx.jar"
ExecReload=/bin/kill -s HUP
SuccessExitStatus=143
Restart=on-failure
RestartSec=10
TimeoutStopSec=10

[Install]
WantedBy=multi-user.target
EOF
```
# 配置中心
使用nacos，和开发约定中间件的变量名名称，包含url，username，password,
```sh
## profile文件内容
NACOS_URL=xx
NACOS_NAMESPACE=xx
NACOS_USERNAME=xx
NACOS_PASSWORD=xx
```
# 数据库
使用数据库迁移工具 https://github.com/golang-migrate/migrate， 这个工具可以让sql有版本管理，进行升级和回滚

# 日志
使用promtail采集systemd日志，发送到loki，grafana使用gitlab认证，这样研发只需要一套gitlab账号就可以

# 代码扫描
