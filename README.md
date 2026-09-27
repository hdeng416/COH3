# 英雄连3 英雄单位 Mod

这是一个类似英雄的单人单位，功能如下：

- **单人小队**，血量高
- **无限升星**：原版 3 星满了以后继续靠击杀升星，每星加伤害、射速和减伤
- **切换武器**：点一个能力按钮，在强力突击武器（近距离高伤害）和强力火箭筒之间来回切换
- **脱战回血**：8 秒没受伤也没开火，就开始每秒回血，星级越高回得越快
- 阵亡后重新生产，**星级可以继承**（可在配置里关掉）

## 文件

| 文件 | 内容 |
| --- | --- |
| `scar/hero_unit.scar` | 游戏脚本：升星、切换武器、回血 |
| `docs/attributes.md` | 在 Essence Editor 里怎么配置小队、实体、武器和能力 |

## 使用步骤

1. 按 `docs/attributes.md` 在 Essence Editor 里做出 `hero_sbp` 等蓝图。
2. 在 Essence Editor 里建一个 **Game Mode（游戏模式）** 项目，把 `scar/hero_unit.scar` 加进去，并在游戏模式的主脚本里 `import` 它。
   - 如果属性改动放不进 Game Mode 项目，就另建一个 **Tuning Pack**。开房时同时选这个游戏模式和这个调参包。
3. 打开 `hero_unit.scar`，确认顶部 `HERO_CONFIG` 里的蓝图名、挂点名和你的属性一致。
4. 进一局自定义游戏，生产英雄测试。

## 调数值

所有数值都在 `hero_unit.scar` 顶部的 `HERO_CONFIG` 里：

| 配置项 | 默认值 | 说明 |
| --- | --- | --- |
| `xp_first_star` / `xp_star_increment` | 10 / 5 | 第 n 颗额外星需要 `10 + 5*(n-1)` 经验 |
| `xp_per_100_hp` | 1 | 击杀目标每 100 点最大生命值给 1 经验，单次上限 `xp_max_per_kill` |
| `damage_per_star` | 0.08 | 每星伤害 +8%，线性叠加（10 星就是 +80%） |
| `fire_rate_per_star` | 0.04 | 每星射速 +4% |
| `damage_reduction_per_star` | 0.04 | 每星受到伤害 -4%，最多减到 35% |
| `regen_delay` | 8 | 脱战几秒后开始回血 |
| `regen_pct_per_sec` | 0.03 | 每秒回最大生命值的 3%，每星再多 0.2% |

## 首次测试注意

这个脚本没有在游戏里实际跑过。里面用到的 SCAR 函数名沿用 COH2/COH3 ScarUtil 的惯例，比如 `Modify_WeaponEnabled`、`Modify_WeaponDamage`、`Squad_IsUnderAttack`。

不确定是否存在的函数，脚本都做了保护：函数不存在时只在控制台打印一次
`[HeroUnit] missing SCAR function: xxx`，对应功能跳过，脚本本身不会崩。
所以第一次进游戏请打开控制台看输出。如果出现这条提示，去 Essence Editor 自带的 SCAR 文档里查正确的函数名，改掉对应那一行即可。

还没做的：

- 界面上不会显示 3 星以上的额外星数。现在只在控制台打印，界面显示可以后续再加。
