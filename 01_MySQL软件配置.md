# MySQL特点

1. MySQL 数据库是 C/S 软件：Client【客户端】和Server【服务端】。
2. 免费开源，不需要支付额外的费用（MySQL5以上企业版开始收费，提供技术支持）。
3. 支持大型的数据库，可以处理拥有上千万条数据。
4. 使用标准的SQL数据语言形式（增、删、改、查）。
5. 支持不同的操作系统，并提供多种编程语言的操作接口。包括C、C++、Python、Java等等。
6. 支持多种存储引擎。

# 下载

## 小皮统一管理

1. 使用[小皮](https://www.xp.cn)。
2. 修改密码。
3. 修改存储引擎为InnoDB，校验更严格，更安全。
4. 配置环境变量。
5. 测试mysql版本：`mysql -V`或者`mysql --version`。

## 官方下载

1. 官网

   [MySQL](https://www.mysql.com/cn/)

   下载免费的社区版。

2. 调整端口

   ![mysql安装端口配置](./MySQL.assets/mysql安装端口配置.png)

   > 这里我修改为了 3307，以便和小皮做区分。

3. 用户配置

   ![mysql用户配置](./MySQL.assets/mysql用户配置.png)

   这是 MySQL 安装过程中的“账户与角色”配置界面，分为上下两个部分，分别对应**超级管理员密码设置**和**普通用户账户创建**。

   上半部分是 **Root 账户密码设置**，这是 MySQL 的最高权限账户，相当于系统的“管理员”，所有数据库操作都依赖它。需要在“MySQL Root Password”输入框中设置一个强密码（建议8位以上，包含大小写字母、数字和特殊符号），并在“Repeat Password”中再次确认。这个密码务必牢记，一旦遗忘将难以恢复，后续所有数据库管理操作都需要它。

   下半部分是 **MySQL 用户账户管理**，用于为不同应用或用户创建独立的数据库账户，并分配相应权限。可以通过右侧的“Add User”按钮添加新用户，设置用户名、允许连接的主机（如 localhost 或 %）、以及分配的角色（如只读、读写、管理员等）。这部分是可选的，新手可以暂时跳过，仅设置 root 密码即可继续安装。

   > 这个界面的设计目的是确保数据库安全：root 账户用于系统级管理，而普通用户账户用于日常应用访问，避免直接使用 root 操作带来的风险。

   密码：`zjy1234` 

4. 对 windows 的服务配置

   ![mysql系统服务配置](./MySQL.assets/mysql系统服务配置.png)

   这张截图是 MySQL 安装过程中的“Windows 服务配置”界面，它决定了 MySQL 如何作为 Windows 系统服务运行。

   - 将 MySQL Server 配置为 Windows 服务

     这是最顶层的总开关。勾选后，MySQL 会注册为一个 Windows 后台服务，而不是一个需要手动双击启动的程序。这是生产环境和开发环境的推荐做法，因为它能确保数据库在系统重启后自动恢复运行，并且可以被其他应用程序稳定调用。

   - Windows 服务详情

     这部分用于定义服务的“身份证”。

     - **Windows **服务名称

       默认是 `MySQL80`。这个名称是 Windows 系统内部识别该服务的唯一标识。如果在同一台电脑上要安装多个 MySQL 实例（比如一个用于开发，一个用于测试），就必须为每个实例设置不同的服务名（如 `MySQL80_Dev`，`MySQL80_Test`），否则会发生冲突。

     - 在系统启动时启动 MySQL Server

       勾选此项后，MySQL 服务会随 Windows 开机自动启动。对于需要持续运行的数据库来说，这是必须的，可以避免每次开机后都要手动去服务管理器里点“启动”。

   - 以何种身份运行 Windows 服务

     这部分决定了 MySQL 服务在操作系统层面的“权限等级”。

     - 标准系统账户

       这是默认且推荐的选项。它使用 Windows 内置的 `SYSTEM` 账户来运行 MySQL，拥有足够的权限来访问数据文件和执行必要的系统操作，同时避免了使用普通用户账户可能带来的权限不足问题。

     - 自定义用户

       这是一个高级选项，允许指定一个特定的 Windows 用户账户来运行 MySQL。这通常用于有特殊安全策略的环境，比如要求服务必须以最低权限运行，或者需要访问特定网络资源时。对于绝大多数用户，保持默认的“标准系统账户”即可。

5. 配置环境变量

   找到安装的 MySQL 的 bin 目录，将其配置到 `Path` 环境变量中。

# 启动

## 小皮

直接在图形化界面中点击 `启动` 即可。

## 服务系统

1. 在 “运行” 窗口中输入 `service.msc` 进入服务系统。
2. 找到在 `对 windows 服务配置` 中注册的名称。
3. 右键选择 `启动` 或者 `停止`。

## 命令行

```cmd
# net start/stop 注册的服务名称

net start mysql80
met stop mysql80
```

如果没有注册服务，则找到 MySQL 的 bin 目录，打开命令行，输入 `mysqld --console`。

- `mysqld` 是 MySQL 的服务器程序。
- `--console` 参数会让 MySQL 在前台运行，并将日志信息直接输出到当前命令行窗口。、

要保持这个窗口打开，才能一直启动。

# 连接

## 官方连接器

搜索 `mysql`，找到对应的命令行启动器，比如 `MySQL 8.0 Command Line Client`。

## 命令行

`mysql -h IP地址 -u 用户名 -P 端口号 -p 密码`

- `-h`：host，当连接主机时可以不写。
- `-u`：用户名，小p的默认是root。
- `-P`：port，端口号，默认就是数据库的3306，可以不写。
- `-p`：password，密码可以不写，连接成功后会提示输入，但是 `-p` 必须写。

连接成功后，使用`quit`或者`exit`退出。

## 客户端

### DataGrip软件

[JetBrains: 软件开发者和团队的必备工具](https://www.jetbrains.com.cn/) 

### vs code

安装 `MySQL Shell for VS Code` 或 `Data Client` 扩展。

## python

安装mysql-connector-python扩展。

```python
import mysql.connector

# 将下面的信息替换为自己的数据库信息
config = {
    'user': 'your_username',
    'password': 'your_password',
    'host': 'your_host',
    'database': 'your_database',
    'raise_on_warnings': True
}

try:
    # 连接到MySQL服务器
    cnx = mysql.connector.connect(**config)
    print("连接成功！")

    # 执行查询
    cursor = cnx.cursor()
    query = "SELECT * FROM your_table" # 替换为你的表名
    cursor.execute(query)

    # 打印结果
    for row in cursor:
        print(row)

except mysql.connector.Error as err:
    print(f"连接失败: {err}")

finally:
    # 关闭连接
    if 'cnx' in locals() and cnx.is_connected():
        cursor.close()
        cnx.close()
        print("连接已关闭。")
```



```python
# 导入sqlalchemy的数据库引擎
from sqlalchemy import create_engine

# 创建数据库引擎，传入url规则的字符串
engine = create_engine('数据库类型+工具（python包）://用户名:密码@IP地址:端口号/数据库名称?charset=码表')

# 比如
engine = create_engine('mysql+pymysql://root:zjy1234@127.0.0.1:3306/day04?charset=utf8')
```

# DataGrip 配置

## 连接

左边 DataBase 处 `+` 选择连接数据源，选择 MySQL。

配置连接信息并下载驱动。

## 展示

选择 `all schema`。