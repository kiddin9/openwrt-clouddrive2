# OpenWrt CloudDrive2 & luci-app-clouddrive2

适用于 OpenWrt（支持 `x86_64`、`aarch64`、`arm/armv7`）的 [CloudDrive2](https://www.clouddrive2.com/) 核心服务软件包与原生 LuCI 客户端 JS 管理插件。

## 主要特性

- **标准 `USE_PROCD` 守护服务**：
  - 使用 `uci_validate_section` 校验配置项，依托 OpenWrt `procd` 管理进程生命周期、崩溃自动拉起（`respawn`）、`pidfile`、文件句柄上限（`nofile`）与系统日志转发。
  - 原生对接 `procd` 防火墙数据对象（兼容 `fw3` / `fw4`），无需污染 `/etc/config/firewall` 即可按需放行 HTTP / HTTPS 端口。
  - 停止服务时自动扫描 `/proc/mounts` 并按路径深度安全卸载挂载主目录下的全部 FUSE 子挂载点。
  - 支持开机非阻塞延迟启动（等待 USB/SATA 磁盘挂载或 WAN 拨号就绪）。
- **纯客户端 LuCI JS 界面（无 Lua 依赖）**：
  - **实时状态栏**：展示运行状态、PID、内存占用、活动 FUSE 挂载数量与路径、Web 后台及 WebDAV（`/dav`）快捷入口，以及一键启动 / 重启 / 停止控制。
  - **完善配置项**：支持配置目录（`CLOUDDRIVE_HOME`）、本地挂载主目录、上传临时缓冲目录（`temp_dir` / `TMPDIR`）、共享挂载传播（`mount --make-shared`）、`user_allow_other`、HTTP/HTTPS 代理、启动后自动运行脚本（`ENABLE_RUN_AFTER_START`）及自定义环境变量。
  - **实时日志页**：支持切换查看 CloudDrive2 应用日志文件（`$CLOUDDRIVE_HOME/log/`）与系统 `logread` 日志，内置日志级别过滤、关键字搜索、倒序置顶、自动刷新、日志导出下载与一键清空。

## 编译说明

```bash
git clone https://github.com/kiddin9/openwrt-clouddrive2.git package/openwrt-clouddrive2
make menuconfig
# Network -> clouddrive2
# LuCI -> 3. Applications -> luci-app-clouddrive2
make package/openwrt-clouddrive2/clouddrive2/compile V=s
make package/openwrt-clouddrive2/luci-app-clouddrive2/compile V=s
```
