# ESequencer

Sequencer 是 Hacknet 本体终局阶段使用的特殊机制，其变体 ESequencer 被专门用于扩展中。

让 ESequencer 模仿原版 Sequencer 的效果需要 [ExtensionInfo](./ExtensionInfo.md) 与 [ConditionalActions](./Action.md#conditionalactions条件行为) 搭配一起使用。

## 修改 ExtensionInfo

```xml
<HacknetExtension>
  ...
  <SequencerTargetID>advExamplePC</SequencerTargetID>
  <SequencerSpinUpTime>10.5</SequencerSpinUpTime>
  <SequencerFlagRequiredForStart>testFlag</SequencerFlagRequiredForStart>
  <ActionsToRunOnSequencerStart>Actions/ThemeSwapActions.xml</ActionsToRunOnSequencerStart>
  ...
</HacknetExtension>
```

- `SequencerTargetID`：ESequencer 的目标 Node 的 Computer ID。必填，缺失时激活 ESequencer 会导致游戏崩溃。
- *`SequencerSpinUpTime`*?：ESequencer 的启动时间（秒）。默认值为 `17`。
- *`SequencerFlagRequiredForStart`*?：ESequencer 启动所需的 Flag。缺失时跳过 Flag 检查，ESequencer 始终可用。
- `ActionsToRunOnSequencerStart`：ESequencer 启动时执行的 ConditionalActions 的文件路径。必填，缺失时激活 ESequencer 会导致游戏崩溃。

## 使用 ConditionalActions 添加 ScreenBleed 效果

通过 Action `StartScreenBleedEffect` 添加 ScreenBleed （红屏）效果。

```xml
<ConditionalActions>
    <Instantly>
        <StartScreenBleedEffect 
        AlertTitle="Sequencer Attack"
        CompleteAction="Actions/ScreenBleedFailed.xml" 
        TotalDurationSeconds="200" 
        DelayHost="delayNode" 
        Delay="0">Break into the Moonshine servers
Delete all files and backups
Get out of there!</StartScreenBleedEffect>
    </Instantly>
</ConditionalActions>
```

- `AlertTitle`：提示标题。缺失时会导致游戏黑屏。
- *`CompleteAction`*?：`ScreenBleed` 结束后加载的 ConditionalActions 的文件路径。
- *`TotalDurationSeconds`*?：总持续时间（秒）。默认值为 `200`。
- *`DelayHost`*?：延迟执行的宿主 Node 的 Computer ID。
- *`Delay`*?：延迟执行的时间（秒）。
- 内容：左下角的提示内容。最多三行。

> [!NOTE]
> 需要注意的是，一般玩家是需要玩家在 ScreenBleed 覆盖前完成任务。
> 
> 这种情况下 **CompleteAction 不是指任务完成后加载的ConditionalActions，而是失败后加载的**。
>
> 所以也需要额外通过Action处理取消 ScreenBleed 的逻辑，比如说任务完成后加载的 ConditionalActions。

```xml
<CancelScreenBleedEffect DelayHost="delayNode" Delay="0"/>
```

## 参考

你可以在以下页面中找到更多细节：

- [参考：ExtensionInfo#ESequencer 相关](../reference/ExtensionInfo.md#esequencer-相关)
- [参考：Action](../reference/Action.md)：[StartScreenBleedEffect](../reference/Action.md#startscreenbleedeffect) [CancelScreenBleedEffect](../reference/Action.md#cancelscreenbleedeffect)
