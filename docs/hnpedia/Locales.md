# 多语言与本地化

Hacknet 支持多语言，通过**本地化词条文件**（Locale Terms）实现。默认语言为 `en-us`（英语），其余语言放在 `Content/Locales/<代码>/` 目录下。

## 支持的语言

语言列表定义在 `Content/Locales/SupportedLanguages.txt`，每行 `名称,语言代码,Steam代码`：

| 语言 | 代码 | Steam 代码 |
|---|---|---|
| English | `en-us` | `english` |
| German | `de-de` | `german` |
| French | `fr-be` | `french` |
| Russian | `ru-ru` | `russian` |
| Spanish | `es-ar` | `spanish` |
| Korean | `ko-kr` | `koreana` |
| Japanese | `ja-jp` | `japanese` |
| Chinese (Simplified) | `zh-cn` | `schinese` |
| Turkish | `tr-tr` | `turkish` |
| Dutch | `nl-nl` | `dutch` |

> 中文为简体（`zh-cn`），代码与 `ExtensionInfo.xml` 的 `<Language>` 字段一致。

## 本地化机制

`LocaleActivator.ActivateLocale(code, content)` 负责激活指定语言：

- **词条加载**：读取 `Content/Locales/<代码>/UI_Terms.txt`（或 `Hacknet_UI_Terms.txt`），有 DLC 时再叠加 `DLC/Hacknet_UI_Terms.txt`。
- **词条格式**：tab 分隔的 `原文→译文` 列表，由 `LocaleTerms.ReadInTerms` 解析；`[%\n%]` 会被替换为换行符。原文词条即英文界面字符串，运行时 `LocaleTerms.Loc()` 查表替换。
- **CJK 字体**：中日韩（`zh`/`ja`/`ko` 开头）走 `LocaleFontLoader` 加载本地化字体（`<代码>_Font12` 等），保证方块字正常渲染。
- **英文特判**：`en-us` 直接 `ClearForEnUS()` 清空词条表（英文即原文，无需查表）。

## 语言选择流程

1. 首次启动且无设置文件：`PlatformAPISettings.GetCodeForActiveLanguage()` 优先匹配 **Steam 语言**，再匹配系统区域，否则回退 `en-us`。
2. 有设置文件时：读取 `Settings.txt` 中 `defaultLocale: <代码>`。
3. `ForceEnglish` 开关为 true 时强制用英文。
4. 激活的语言写入 `Settings.ActiveLocale`，并持久化到 `Settings.txt`。

## 本地化内容分布

- `Content/Locales/<代码>/`：UI 词条（`UI_Terms.txt` / `Hacknet_UI_Terms.txt`）、字体（`Fonts/`）、以及镜像本体 `Content/` 目录结构的**本地化内容**——如 `Missions/`、`DLC/Missions/`、`People/`、`Post/`、`files/` 下的对应文件。
- `LocalizedFileLoader` 在读取文件时先把 `Content/` 替换为 `Content/Locales/<当前语言>/` 查找本地化版本，找不到才回退英文原版。
- 剧情文本（邮件、文件、网页）同样支持本地化，中文版覆盖了主线与 DLC 的全部任务文件。

## 与 Extension 的关系

- 每个 Extension 的 `ExtensionInfo.xml` 可声明 `<Language>`（如 `zh-cn`），启动扩展时 `ExtensionLoader` 会调用 `LocaleActivator.ActivateLocale(info.Language, ...)` 切换语言。
- 扩展不加载额外的词条文件——激活的是**游戏级**词条表；扩展自身的内容（节点、任务、文件）文本是硬编码在扩展的 XML 里的，如需多语言需自行按语言复制内容或用 `<Language>` 指定单一语言。
- 参考：[ExtensionInfo（扩展信息）](../reference/ExtensionInfo.md)

## 扩展多语言词条文件

Extension 的 `Locale` 词条通过 `LocaleTerms` 机制读取，游戏会在扩展目录下查找对应的词条文件并按 `Language` 声明激活。建议在扩展中按语言组织词条，避免硬编码 UI 文本。

> 相关页面：[游戏模式与设置](./Modes.md) · [ExtensionInfo](../reference/ExtensionInfo.md)