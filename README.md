# PythonMinecraftLauncher

一个功能完整、高度可定制的 Minecraft 版本下载器和启动器库。

## ✨ 核心功能

- **多线程下载** - 并发下载大幅提升下载速度
- **完整版本管理** - 自动下载客户端、资源文件和库文件
- **智能版本过滤** - 按版本类型筛选（正式版、快照版等）
- **实时进度跟踪** - 详细的下载进度和速度显示
- **文件完整性验证** - 自动验证文件 SHA1 校验和
- **内置游戏启动器** - 支持启动命令和脚本生成
- **跨平台支持** - 完美支持 Windows、Linux、macOS 系统

## 📚 核心 API

### 主要类说明

#### MinecraftDownloader - 下载器核心
负责 Minecraft 版本管理和文件下载功能。

```python
# 初始化下载器
downloader = MinecraftDownloader(config)
```

**常用方法：**
- `get_version_list()` - 获取可下载版本列表
- `get_version_info()` - 获取特定版本详细信息  
- `download_version()` - 下载完整游戏版本
- `set_progress_callback()` - 设置下载进度回调

#### MinecraftLauncher - 游戏启动器
负责游戏启动命令生成和游戏进程管理。

```python
# 初始化启动器
launcher = MinecraftLauncher(downloader)
```

**常用方法：**
- `get_installed_versions()` - 查看已安装版本
- `launch_game()` - 启动指定版本游戏
- `create_launch_command()` - 生成启动命令
- `generate_launch_script()` - 创建启动脚本

#### DownloadConfig - 下载配置
自定义下载行为的配置类。

```python
config = DownloadConfig(
    max_workers=10,           # 并发线程数
    download_dir="./minecraft" # 游戏安装目录
)
```

### 数据类型

**版本类型 (VersionType)：**
- `RELEASE` - 稳定正式版
- `SNAPSHOT` - 开发快照版  
- `OLD_BETA` - 历史测试版
- `OLD_ALPHA` - 历史Alpha版
- `ALL` - 所有可用版本

**版本信息 (VersionInfo)：**
包含版本ID、类型、下载地址、发布时间等完整信息。

### 异常处理

库提供详细的异常类型，便于错误处理：
- `VersionNotFoundError` - 版本不存在
- `NetworkError` - 网络连接问题
- `DownloadError` - 下载过程错误
- `LaunchError` - 游戏启动失败

## 🚀 使用示例

### 基础用法

```python
from minecraft_downloader import *

# 创建下载器
config = DownloadConfig(download_dir="./minecraft")
downloader = MinecraftDownloader(config)

# 设置进度回调
def progress_callback(message, current=None, total=None):
    if current and total:
        print(f"{message} - 进度: {current}/{total}")
    else:
        print(message)

downloader.set_progress_callback(progress_callback)

# 获取并下载最新正式版
versions = downloader.get_version_list(VersionType.RELEASE, limit=1)
if versions:
    success = downloader.download_version(versions[0])
    if success:
        print("下载完成！")
```

### 高级用法

```python
# 仅下载客户端文件
version_info = downloader.get_version_info("1.20.1")
downloader.download_version(
    version_info,
    download_assets=False,      # 跳过资源文件
    download_libraries=False    # 跳过库文件
)

# 启动游戏
launcher = MinecraftLauncher(downloader)
launcher.launch_game(
    version_id="1.20.1",
    username="MyPlayer",
    memory="4G"
)
```

### 批量操作

```python
# 批量下载多个版本
versions_to_download = ["1.20.1", "1.19.4", "1.18.2"]

for version_id in versions_to_download:
    try:
        version_info = downloader.get_version_info(version_id)
        print(f"开始下载 {version_id}...")
        downloader.download_version(version_info)
    except Exception as e:
        print(f"下载 {version_id} 失败: {e}")
```

## ❓ 常见问题

**下载速度慢怎么办？**
- 增加 `max_workers` 参数提升并发数
- 检查网络连接稳定性

**启动游戏报错？**
- 确认已安装 Java 8 或更高版本
- 检查内存设置是否合理

**文件验证失败？**
- 库会自动重新下载损坏文件
- 检查磁盘空间是否充足

## ⚠️ 重要说明

- 需要稳定的网络连接下载游戏文件
- 确保系统已安装合适版本的 Java 运行时
- 使用本库需遵守 Minecraft 最终用户许可协议

## 📄 开源协议

Apache-2.0 License

---

**项目主页**: https://github.com/lijiayuapp/PythonMinecraftLauncher/
