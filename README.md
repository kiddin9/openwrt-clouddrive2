# OpenWrt CloudDrive2 & luci-app-clouddrive2

适用于 OpenWrt（支持 `x86_64`、`aarch64`、`arm/armv7`）的 [CloudDrive2](https://www.clouddrive2.com/) 核心服务软件包与原生 LuCI 客户端 JS 管理插件。

## 主要特性

- **标准 `USE_PROCD` 守护服务**：
  - 自动配置 FUSE 环境（`user_allow_other` 与 `mount --make-shared` 共享挂载传播）、`nofile` 句柄上限、系统时区与 `ENABLE_RUN_AFTER_START`。
  - 原生对接 `procd` 防火墙数据对象（兼容 `fw3` / `fw4`），按需放行监听端口。
  - 停止服务时自动清理 `Start-Service` 子进程并递归卸载挂载主目录下的所有 FUSE 挂载点。
- **精简直观的 LuCI 客户端 JS 界面**：
  - **基本设置**：仅保留路由端必需的 6 项配置（启用、监听端口、开放防火墙端口、配置目录、挂载目录、缓存目录），其余应用层参数均交由 CloudDrive2 自身后台管理。
  - **实时状态与快捷操作**：展示运行状态、PID、总内存占用、已挂载路径及后台 / WebDAV 快捷入口，支持一键启动、重启与停止。
  - **运行日志**：支持切换应用日志与系统 `logread` 日志，提供级别过滤、关键字检索、最新置顶、自动刷新、下载与清空功能。

## 编译说明

```bash
git clone https://github.com/kiddin9/openwrt-clouddrive2.git package/openwrt-clouddrive2
make package/openwrt-clouddrive2/{clouddrive2,luci-app-clouddrive2}/compile V=s
```
