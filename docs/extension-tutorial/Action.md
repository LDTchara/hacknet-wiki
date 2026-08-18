# Action（行为）

在本章节中，我们将介绍 Hacknet Extension 中的 Action（行为），以及使用 Action 的两种方法：ConditionalActions 和 Faction。

Action 可以实时改变游戏中的部分内容，例如向玩家的电脑添加一个文件、向 IRC 服务器发送一条消息、切换游戏的主题等。

> [!NOTE]
> ConditionalActions 与 Action 有时都会被简称为 "Action"。为了区分，在本站中 Action 特指一个个具体的行为，不代指 ConditionalActions。

## 认识 Action

Action 的种类非常多，按功能大致可以分为：

- 文件操作：`AddAsset`、`DeleteFile`、`CopyAsset`、`AppendToFile` 等
- Node 相关：`ShowNode`、`HideNode`、`ChangeIP`、`CrashComputer` 等
- 消息与界面：`AddIRCMessage`、`ChangeAlertIcon`、`StartScreenBleedEffect` 等
- 流程控制：`LoadMission`、`AddConditionalActions`、`RunFunction` 等

本教程不会对 Action 逐一详述。你可以把本章当作 Action 的"使用说明"，具体的 Action 列表和属性请查阅 [参考：Action](../reference/Action.md)。

Action 不能独立存在，它总是依附于某种"容器"被游戏执行。在 Hacknet Extension 中有两种方法使用 Action：

- 通过 ConditionalActions
- 通过 Faction

下面分别介绍。

## ConditionalActions（条件行为）

ConditionalActions 是使用 Action 的一种方法。它会使用 Condition（条件）给 Action 设置触发条件：ConditionalActions 被游戏加载后，Condition 满足时就会执行其中的 Action。

描述 ConditionalActions 的 XML 结构如下：

```xml
<ConditionalActions>

    <!-- Condition -->
    <Instantly needsMissionComplete="false">

        <!-- Action -->
        <SaveGame DelayHost="delayNode" Delay="0"/>
        ...
    </Instantly>

    ...
</ConditionalActions>
```

ConditionalActions 可以在游戏开始、Mission 中或者 Action 中加载，非常方便：

- 游戏开始时加载的 ConditionalActions 就是 `Actions/StartingActions.xml`。
- Mission 可以通过 [MissionFunction](./MissionFunction.md) `loadConditionalActions` 加载 ConditionalActions。
- Action 可以通过 `AddConditionalActions` 加载另一个 ConditionalActions，实现连锁加载。

常见的 Condition 有以下几种，你可以在 [参考：ConditionalActions](../reference/ConditionalActions.md) 中查看所有的 Condition：

| Condition       | 触发时机                                    |
| --------------- | ------------------------------------------- |
| `Instantly`     | ConditionalActions 被加载后立即触发         |
| `OnConnect`     | 连接到目标 Node 后触发                      |
| `OnDisconnect`  | 从目标 Node 断开连接后触发                  |
| `OnAdminGained` | 获取目标 Node 的管理员权限后触发            |
| `HasFlags`      | 玩家拥有所有指定的 [Flag](./Flag.md) 时触发 |

### 动手试试

在[准备工作](./preparing.md)中，我们已经见过 `BlankExtension/Actions/StartingActions.xml`，并修复了它的一个 bug。它是 Extension 启动时最先被加载的 ConditionalActions。打开它：

```xml
<ConditionalActions>
  
  <Instantly>
    <AddAsset FileName="RTSPCrack.exe" FileContents="#RTSP_EXE#" TargetComp="playerComp" TargetFolderpath="bin" />
    <RunFunction FunctionName="setFaction:startingfac" FunctionValue="0" />
    
  </Instantly>

</ConditionalActions>
```

`Instantly` 是 Condition，表示在 ConditionalActions 被加载后立即触发。也就是说，Extension 一启动，其中的两个 Action 就会执行：

- `AddAsset` 向玩家电脑的 `bin` 文件夹添加了 `RTSPCrack.exe`。
- `RunFunction` 运行了 Function `setFaction:startingfac`。

我们尝试在 `<Instantly>` 中添加一个新的 Action，让 Extension 启动时给玩家的电脑留下一段欢迎信息。

```xml
<Instantly>
  ...
  <AddAsset FileName="Welcome.txt" FileContents="Welcome to MyFirstExtension!" TargetComp="playerComp" TargetFolderpath="home" /> <!-- [!code add] -->
</Instantly>
```

保存后启动 Extension，在终端中使用 `ls` 查看 `home` 文件夹，再用 `cat Welcome.txt` 查看内容。如果能看到我们写入的文字，说明这个 Action 已经成功执行。

> [!NOTE]
> `StartingActions` 在 Extension 启动时加载。如果你已经有一个进行中的存档，建议删除存档并重新开始 Extension，以便观察 `Instantly` 中的 Action 的执行效果。

### 延迟执行（Delay）

部分 Action 支持通过 `Delay` 和 `DelayHost` 延迟执行。Delay 的细节以及哪些 Action 支持延迟（标有 <Badge type="info" text="Delayable" />），见 [参考：Action#Delay](../reference/Action.md#delay)。

## Faction（阵营）

Faction 是使用 Action 的另一种方法。与 ConditionalActions 的区别主要在于**条件不同**。

Faction 会使用 FactionAction 给 Action 设置条件。FactionAction 可以根据玩家在该 Faction 中的 Rank（排名，游戏内又叫做 Point（积分））来决定是否执行它的 Action。

描述 Faction 的 XML 结构如下：

```xml
<CustomFaction name="Faction Name" id="Faction_ID" playerVal="0">

    <!-- FactionAction -->
    <Action ValueRequired="1">

        <!-- Action -->
        <SaveGame DelayHost="delayNode" Delay="0"/>
        ...
    </Action>
    ...

</CustomFaction>
```

FactionAction 类似一个特殊的 Condition：

```xml
<Action ValueRequired="1" Flags="flag1,flag2"></Action>
```

- `ValueRequired`：`int`，Rank > `ValueRequired` 时会执行 Action。默认值为 `10`。
- *`Flags`*?：`string`，描述需要满足的 flags。多个 flags 之间用逗号 `,` 分隔。

在 BlankExtension 中出现的 `<RunFunction FunctionName="setFaction:startingfac" FunctionValue="0" />` 执行了一个用于指定当前 Faction 的 Function。如果 Faction 没有被指定，Mission 的 `requiredRank` 功能会无法正常工作。

### 与 ConditionalActions 的区别

- ConditionalActions 可以随时加载（游戏开始、Mission 中、Action 中）；Faction 不可以随时加载，而只能在游戏开始的时候加载。
- ConditionalActions 的触发条件是 Condition（连接节点、获得管理员权限、拥有 Flag 等）；FactionAction 的触发条件是玩家在该 Faction 中的 Rank。

Rank 可以通过 Function 改变，例如 `addRank` / `addRankSlient` 会给玩家当前的 Faction 增加 Rank。玩家当前属于哪个 Faction，则可以用 Function `setFaction` 切换——`StartingActions.xml` 中的 `setFaction:startingfac` 就是在设置玩家的初始 Faction。

Faction 的完整说明见 [参考：Faction](../reference/Faction.md)。

## 参考

- [参考：Action](../reference/Action.md)
- [参考：ConditionalActions](../reference/ConditionalActions.md)
- [参考：Faction](../reference/Faction.md)
