# SlimefunVoid-Fixed

SlimefunVoid 的修复版 —— 修复了打开带战利品表的箱子时必炸的 `ArrayIndexOutOfBoundsException`。

> 本仓库是 [BigBadE/SlimefunVoid](https://github.com/BigBadE/SlimefunVoid)（原仓库，2020 年起停止维护）经 [LobbyTech-MC/SlimefunVoid](https://github.com/LobbyTech-MC/SlimefunVoid) fork 适配 1.21.1 后的继续修复版，遵循原项目 GPL-3.0 协议开源。
>
> 本项目由 AI 辅助完成定位与修复，发布者 mowenqwq。

## 修复了什么

原版 `VoidResearchNotePopulateListener` 在向箱子写入虚空研究纸时写死了槽位上界：

```java
// 修复前：箱子只有 27 格（0~26），nextInt(28) 会随机抽到 27 → 越界
inventory.setItem(random.nextInt(28), item);
```

```java
// 修复后：按实际容器大小取随机槽位，任何容器类型都不会越界
inventory.setItem(random.nextInt(inventory.getSize()), item);
```

症状（修复前的报错）：

```
java.lang.ArrayIndexOutOfBoundsException: Index 27 out of bounds for length 27
    at ...VoidResearchNotePopulateListener.addResearch(VoidResearchNotePopulateListener.java:55)
```

触发条件：玩家右键打开任何带 loot table 的自然生成箱子（研究纸填充机制只对这类箱子生效）。每次抽中越界槽位就抛一次异常并中断本次填充，不影响服务器运行但会刷屏且研究纸丢失。

## 使用

- 依赖：Slimefun4（同原版），API 版本 1.18+
- 下载：见 [Releases](https://github.com/mowenQWQ/SlimefunVoid-Fixed/releases)，或自行构建
- 替换掉原来的 `SlimefunVoid-Build 3 (git bf391f5).jar` 即可；仅改了一处取槽位逻辑，配置与数据完全兼容

## 自行构建

```bash
mvn clean package
# 产物在 target/[sf]SlimeVoid v1.0.jar
```

要求：JDK 21、Maven 3.9+（国内网络建议配置阿里云 Maven 镜像；上游 pom 里已失效的 `repo.destroystokyo.com` 仓库 403 不影响构建）。

## 功能列表（同原版）

- Void Research Table：随时间解锁研究，而非消耗等级
- Research Notes：在带 loot table 的箱子中获取，或猎杀特定生物掉落（修复点就在这条链路）
- Void Bag：可绑定箱子，放入的物品自动传送（虚空会吞噬一部分）
- Wands：在虚空传送门合成，可注入元素素材强化法术；有概率施法反噬
- Void Portal：进入虚空，随机文本事件（收益或惩罚）；用于虚空仪式合成物品与注魔法杖
- Spells：潜行右键法杖切换法术，含 Fireball / Teleport / Light Bending 等

## 许可证

GPL-3.0（继承自上游 BigBadE/SlimefunVoid）
