# core 中文文档

<div align="center">

**[中文版] Home Assistant (core) — 开源家庭自动化,本地控制与隐私优先**

[![原项目](https://img.shields.io/badge/原项目-home-assistant--core-blue?style=flat-square&logo=github)](https://github.com/home-assistant/core)
[![GitHub Stars](https://img.shields.io/github/stars/home-assistant/core?style=flat-square&label=原项目Stars)](https://github.com/home-assistant/core/stargazers)
[![License](https://img.shields.io/badge/License-Apache--2.0-green?style=flat-square)](https://github.com/home-assistant/core/blob/dev/LICENSE.md)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 本文档是 [home-assistant/core](https://github.com/home-assistant/core) 官方 README 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/home-assistant/core
>
> **代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 项目简介

Home Assistant 是一个把**本地控制**和**隐私**放在首位的开源家庭自动化项目。它由全球范围的折腾爱好者与 DIY 社区驱动,非常适合运行在树莓派(Raspberry Pi)或本地服务器上。

与依赖厂商云端的智能家居方案不同,Home Assistant 让自动化逻辑和设备数据都留在你自己的网络里:断网也能跑,数据不上传,隐私自己掌控。

原 README 开篇原文:

> Open source home automation that puts local control and privacy first. Powered by a worldwide community of tinkerers and DIY enthusiasts. Perfect to run on a Raspberry Pi or a local server.

中文意思:开源的家庭自动化系统,把本地控制与隐私放在第一位。由全球折腾党和 DIY 爱好者社区驱动。完美适配树莓派或本地服务器运行。

## 快速上手

官方推荐的上手路径如下(链接保留原文,便于直接访问):

1. **先看在线演示**:打开 [demo.home-assistant.io](https://demo.home-assistant.io),在浏览器里直接体验完整的 Home Assistant 界面,无需安装任何东西。
2. **选择安装方式**:前往官方安装指南 [home-assistant.io/getting-started](https://home-assistant.io/getting-started/),按硬件选择安装方案——树莓派、NAS、虚拟机或裸机服务器均可。
3. **动手做第一个自动化**:跟随官方自动化教程 [home-assistant.io/getting-started/automation](https://home-assistant.io/getting-started/automation/),从"人到开灯"这类经典场景开始。
4. **查文档解决问题**:完整文档见 [home-assistant.io/docs](https://home-assistant.io/docs/);使用或开发组件遇到问题时,先看网站的[帮助专区](https://home-assistant.io/help/)。
5. **加入社区**:通过 [Discord 聊天室](https://www.home-assistant.io/join-chat/) 与全球用户实时交流。

## 核心特性

- **本地控制优先**:所有自动化在本地执行,不强制依赖外网与厂商云
- **隐私优先**:设备状态、传感器数据、自动化记录都保存在自己家中
- **社区驱动**:由遍布全球的爱好者与 DIY 社区持续开发、测试和改进
- **模块化架构**:系统以模块化方式构建,为其他设备或动作添加支持非常容易
- **硬件门槛低**:一台树莓派或一台闲置本地服务器即可跑起来
- **生态丰富**:官方集成目录覆盖大量主流设备与服务(见 [integrations 列表](https://home-assistant.io/integrations/))

## 精选集成(Featured Integrations)

原 README 的 "Featured integrations" 章节通过截图展示了一批代表性集成,可在官方集成目录浏览完整列表:[home-assistant.io/integrations](https://home-assistant.io/integrations/)。

系统采用模块化方式构建,因此对其他设备或动作的支持可以很容易地实现。想深入了解的开发者可以阅读:

- [架构说明(Architecture)](https://developers.home-assistant.io/docs/architecture_index/):了解 Home Assistant 内部如何组织与运转
- [自定义组件开发指南(Creating your own components)](https://developers.home-assistant.io/docs/creating_component_index/):从零编写自己的集成组件

## 帮助与支持

如果你在使用 Home Assistant 或开发组件的过程中遇到问题,请查阅官网的 [Home Assistant 帮助专区](https://home-assistant.io/help/),那里提供了进一步的帮助与信息。也可以加入 [Discord 社区聊天](https://www.home-assistant.io/join-chat/) 直接向全球用户提问。

## 关于 Open Home Foundation

Home Assistant 是 [Open Home Foundation](https://www.openhomefoundation.org/) 旗下的项目。该基金会致力于推动开放、尊重隐私的智能家居标准,确保项目长期独立于商业云端锁定。

## 常见问题

**Q:必须联网才能用吗?**
A:不需要。本地控制是核心设计目标,局域网内即可完成绝大部分自动化;仅在集成云端设备或远程访问时才需要外网。

**Q:最低硬件要求是什么?**
A:官方推荐的经典方案是树莓派;任何能跑 Python 的 x86/ARM 本地服务器也都可以。

**Q:如何参与开发?**
A:克隆 [home-assistant/core](https://github.com/home-assistant/core) 仓库,阅读上文架构与组件开发文档,即可开始提交自定义集成。

---

## 相关链接

- 原项目仓库:<https://github.com/home-assistant/core>
- 官网与文档:<https://home-assistant.io>
- 在线演示:<https://demo.home-assistant.io>
- 集成列表:<https://home-assistant.io/integrations/>
- 开发者文档:<https://developers.home-assistant.io>

---

## 版权与致谢

本文档为 [home-assistant/core](https://github.com/home-assistant/core) 官方 README 的中文翻译版本,仅供中文用户学习参考。所有代码与原始文档的版权归原项目作者所有,遵循其原始许可证(Apache 2.0)。

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

**如果觉得项目有用,请给原项目 [home-assistant/core](https://github.com/home-assistant/core) 点个 Star!** ⭐
