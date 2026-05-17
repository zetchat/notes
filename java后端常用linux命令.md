# 💡 Java 后端开发必备 Linux 操作肌肉记忆手册

> 📌 **核心目标**：形成肌肉记忆，告别“临时查百度”，提升线上排查效率与独立搭建环境能力。

---

## 1. 基础生存与文件管理（每日高频肌肉记忆）

*本模块是与服务器交互的基础，必须做到盲打。*

### 1.1 目录与文件基础操作

* `pwd`：查看当前所在绝对路径（防止迷路）。
* `ls -lah`：查看当前目录下的所有文件（`-a` 显示隐藏文件如 `.bashrc`，`-l` 显示详细信息，`-h` 以易读的 KB/MB 显示大小）。
* `cd -`：切换回上一次所在的目录（在两个长路径间切换时极度好用）。
* `mkdir -p /path/to/dir`：递归创建多级目录（如直接创建 `com/java/app`）。
* `cp -r src_dir/ dest_dir/`：递归复制整个目录。
* `mv file.txt /new/path/`：移动文件或重命名。
* `rm -rf`：**💥 千万慎用！**
* *⚠️ Java后端避坑指南*：绝对不要在脚本里写 `rm -rf $DIR/` 而没有对 `$DIR` 做非空校验，否则一旦变量为空，直接变成 `rm -rf /`。



### 1.2 进阶文件查找：`find`

线上常用于寻找遗忘的配置文件、生成的 `dump` 文件或日志。

```bash
# 按文件名精准查找
find / -name "application.yml"

# 在当前目录下查找 3 天内修改过的 .log 文件（排查最近的变更）
find . -mtime -3 -name "*.log"

# 查找大于 100M 的文件（常用于找异常暴涨的日志或堆转储文件）
find / -type f -size +100M

```

### 1.3 打包与解压（发布部署必备）

```bash
# 解压 tar.gz 包（后端发布常用）
tar -zxvf app.tar.gz

# 将应用打包成 tar.gz
tar -zcvf app.tar.gz ./my-application-dir

# Zip 家族
unzip app.zip -d ./target_dir/   # 解压到指定目录
zip -r app.zip ./my-app          # 递归压缩目录

```

---

## 2. 日志查看与文本处理（💡 重中之重：排查 Bug 的利器）

*线上出了 Bug，第一件事就是看日志。这是后端开发在 Linux 上花时间最多的地方。*

### 2.1 动态实时看日志

```bash
# 实时滚动查看日志（最常用）
tail -f spring.log

# 实时查看最后 200 行日志（防止刷屏太快，先定位当前状态）
tail -200f spring.log

```

### 2.2 静态翻页看日志：`less`

遇到大文件（几百M或几个G）时，**严禁使用 `cat**`，会导致终端卡死。必须使用 `less`：

```bash
less spring.log

```

* **`less` 交互快捷键：**
* `G`：直接跳转到日志**文件末尾**（看最新的日志）。
* `g`：跳转到日志**文件开头**。
* `/关键字`：**向下搜索**关键字（例如 `/NullPointerException`）。
* `?关键字`：**向上搜索**关键字。
* `n`：跳转到**下一个**搜索匹配项。
* `N`：跳转到**上一个**搜索匹配项。
* `q`：退出 `less` 模式。



### 2.3 精准过滤：`grep`（神级命令）

看异常不能只看一行，必须要看**上下文（Context）**。

```bash
# 查看包含 "Exception" 的行及后 10 行 (After)
grep -A 10 "Exception" spring.log

# 查看包含 "Exception" 的行及前 5 行 (Before)
grep -B 5 "Exception" spring.log

# 查看包含 "OrderNo:12345" 的行及前后 20 行 (Context，最常用，看完整业务流)
grep -C 20 "12345" spring.log

# 多条件过滤（统计错误日志数）
grep "ERROR" spring.log | wc -l

```

### 2.4 进阶文本三剑客（了解即用）

* **`awk`（按列提取）**：
```bash
# 假设日志格式中第5列是接口耗时，提取所有耗时并打印
cat access.log | awk '{print $5}'

```


* **`sed`（流编辑器）**：
```bash
# 临时将日志或配置中的 dev 替换为 prod（仅输出，不修改原文件）
sed 's/dev/prod/g` config.properties

```



---

## 3. 进程与网络排查（服务挂了？接口不通？）

*应用启动失败或前端报 502/接口不通时的定位套路。*

### 3.1 找进程与杀进程

```bash
# 通用查 Java 进程
ps -ef | grep java

# 💡 专为 Java 设计：JDK 自带的 jps（清晰直观显示 PID 和类名）
jps -l

# 优雅停机（触发 Spring 销毁钩子，释放资源）
kill -15 <PID>

# 💥 强制击杀（优雅停机无效、进程卡死时使用）
kill -9 <PID>

```

### 3.2 查端口占用（解决 "Port already in use"）

```bash
# 方式一：netstat (注意可能需要 root 权限才能看全进程名)
netstat -tunlp | grep 8080

# 方式二：lsof (精准定位具体端口)
lsof -i:8080

```

### 3.3 网络连通性测试三板斧

1. **第一步：机器通不通？**

```bash
    ping 192.168.1.100
    ```
2.  **第二步：目标端口/防火墙放开没有？（连不上 Redis/MySQL 必用）**
    
```bash
    # 方式一：telnet
    telnet 192.168.1.100 3306
    
    # 方式二：nc (Netcat，更现代高效)
    nc -vz 192.168.1.100 3306
    ```
3.  **第三步：服务器上直接模拟 HTTP 请求测试接口**
    
```bash
    # 发送 GET 请求
    curl http://localhost:8080/api/v1/health
    
    # 发送 POST 请求（带 JSON 参数）
    curl -X POST -H "Content-Type: application/json" -d '{"id":123}' http://localhost:8080/api/user
    ```

---

## 4. 系统性能监控基础（进阶：OOM 与卡顿分析）
*当服务响应变慢、频繁 Full GC 或直接内存溢出时的高级排查。*

### 4.1 CPU 与系统负载排查
```bash
# 查看整体 CPU 和内存占用（按 1 键可以看多核 CPU 详情）
top

# 💡 Java 高级技巧：找出占用 CPU 最高的 Java 线程
top -Hp <Java_PID>
# 1. 找到该 Java 进程中 CPU 飙高的线程 ID（假设是 2011）
# 2. 将 2011 转换为十六进制：printf "%x\n" 2011 -> 得到 7db
# 3. 使用 jstack 抓取堆栈，利用 7db 过滤定位具体哪行 Java 代码在死循环
jstack <Java_PID> | grep -A 30 "7db"

```

### 4.2 内存监控

```bash
# 以 MB/GB 为单位查看系统剩余内存
free -m
free -h
# 📌 注意：关注 free 和 available 的值，如果 available 见底，代表系统随时可能触发 OOM Killer 杀掉 Java 进程。

```

### 4.3 磁盘空间排查（防止日志写满磁盘导致服务崩溃）

```bash
# 查看整体磁盘空间挂载点和使用率
df -h

# 查看当前目录下各个文件/目录分别占用了多少空间（从大到小排序）
du -sh * | sort -hr

```

---

## 5. 权限与环境变量（环境搭建）

### 5.1 权限管理

```bash
# 给 shell 启停脚本赋予执行权限
chmod +x startup.sh

# 变更文件/目录所属的用户和用户组（通常从 root 切换到专门运行 Java 的 app 用户）
chown -R app:app /usr/local/springboot-app/

```

### 5.2 环境变量配置

线上或测试环境通常通过配置 `/etc/profile`（全局）或 `~/.bashrc`（当前用户）来固定 `JAVA_HOME`。

```bash
# 编辑配置文件
vim ~/.bashrc

# 在文件末尾添加：
export JAVA_HOME=/usr/local/jdk-17
export PATH=$PATH:$JAVA_HOME/bin

# 💥 重要：使配置立即生效（不执行这步，当前终端不会生效）
source ~/.bashrc

```

---

## 6. Shell 脚本基础（解放双手）

*不需要成为运维专家，但要能手写并看懂基本的 Spring Boot 启停脚本。*

### 6.1 简易 Spring Boot 启动脚本模板 (`startup.sh`)

```bash
#!/bin/bash

# 1. 定义变量
APP_NAME=my-app-1.0.0.jar
LOG_FILE=stdout.log

# 2. 判断进程是否已存在
PID=$(ps -ef | grep $APP_NAME | grep -v grep | awk '{print $2}')

if [ -n "$PID" ]; then
    echo "⚠️ 错误: $APP_NAME 已经在运行中 (PID = $PID)."
    exit 1
fi

# 3. 后台启动 Java 应用并将日志重定向
echo "🚀 正在启动 $APP_NAME..."
nohup java -Xms512m -Xmx1024m -jar $APP_NAME > $LOG_FILE 2>&1 &

echo "$APP_NAME 启动指令已发送，请通过 'tail -f $LOG_FILE' 查看日志."

```

### 6.2 实用小脚本：定期清理 30 天前的旧日志

```bash
#!/bin/bash
# 查找指定目录下 30 天前修改过的 .log 文件并将其删除
LOG_DIR="/usr/local/app/logs"
find $LOG_DIR -mtime +30 -name "*.log" -exec rm -f {} \;
echo "🧹 旧日志清理完毕！"

```
