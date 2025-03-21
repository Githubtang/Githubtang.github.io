---
title: "Git 不能提交到 Github"
description: "修改 hosts 修复"
date: 2025-02-25
draft: false
tags: ["Git", "GitHub", "GitHub Pages"]
---

## 故事的开始：一个普通的提交日

某个深夜，我正尝试将更新后的博客内容推送到 GitHub Pages。这本应是常规操作，但终端里突然抛出的错误却打破了平静：

```bash
fatal: unable to access 'https://github.com/Githubtang/Githubtang.github.io.git/': Failed to connect to github.com port 443 after 2354 ms: Couldn't connect to server
```

## 问题分析

这个错误通常是由于网络连接问题引起的，可能是因为 DNS 解析失败或者网络被防火墙阻挡。为了确认问题的原因，我尝试了以下步骤：

1. **检查网络连接**：确保我的电脑可以正常访问互联网。
2. **Ping GitHub**：在终端中运行 `ping github.com`，发现无法解析域名。

## 解决方案：修改 hosts 文件

为了绕过 DNS 解析问题，我决定手动修改 `hosts` 文件，将 GitHub 的域名解析到正确的 IP 地址。以下是具体步骤：

1. **获取 GitHub 的 IP 地址**：
   你可以使用在线工具或者命令行工具（如 `nslookup` 或 `ping`）来获取 GitHub 的 IP 地址。例如：
   ```bash
   nslookup github.com
   ```

2. **编辑 hosts 文件**：
   在 Windows 系统中，`hosts` 文件位于 `C:\Windows\System32\drivers\etc\hosts`。使用管理员权限打开该文件，并添加以下内容：
   ```plaintext
   140.82.112.3 github.com 
   ```

3. **保存并关闭 hosts 文件**

## 验证 ##

完成上述步骤后，再次尝试推送代码到 GitHub：
```bash
git push origin master
```
如果一切顺利，代码应该能够成功推送到 GitHub。

## 结论 ##

通过修改 `hosts` 文件，我们可以绕过 DNS 解析问题，直接访问 GitHub 的服务器。这是一种临时解决方案，还是要去检查网络才能治本。

希望这个方法能帮助到遇到类似问题的你。如果有任何疑问，欢迎留言讨论！
