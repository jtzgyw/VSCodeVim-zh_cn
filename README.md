<h2 align="center"><img src="The_original_En_version/images/icon.png" height="128"><br>VSCodeVim</h2>
<p align="center"><strong>在Visual Studio Code 下的 Vim 模拟器</strong></p>



译者注：本中文版基于[VSCodeVim/Vim](https://github.com/VSCodeVim/Vim)的[README.md](https://github.com/VSCodeVim/Vim/blob/master/README.md)（2022-04-26）进行翻译和更新，基础英文版本保存在 `The_original_En_version` 下，如果后续有更新，将以最新版本与该版本比对后，再进行差异翻译。

VSCodeVim 是一个适用 [Visual Studio Code](https://code.visualstudio.com/)于 Vim 模拟器。

- 🚚 查看所有已支持的Vim功能，请参考我们的 [roadmap](ROADMAP.md)
- 📃 我们的 [change log](CHANGELOG.md) 概括了在两次发布之间突破性的/主要的/次要的更新
- ❓ 如果您想询问任何问题，在 [Slack](https://vscodevim.herokuapp.com/) 加入我们
- 在 [GitHub](https://github.com/VSCodeVim/Vim/issues) 报告缺少的功能/Bug


<details>
 <summary><strong>目录</strong> (单击展开)</summary>

- [💾 安装](#-安装)
  - [Mac](#mac)
  - [Windows](#windows)
- [⚙️ 设置](#️-设置)
  - [简单示例](#简单示例)
  - [VSCodeVim 设置](#vscodevim-设置)
  - [Neovim 集成](#neovim-集成)
  - [Key映射](#key映射)
    - [`"vim.insertModeKeyBindings"`/`"vim.normalModeKeyBindings"`/`"vim.visualModeKeyBindings"`/`"vim.operatorPendingModeKeyBindings"`](#viminsertmodekeybindingsvimnormalmodekeybindingsvimvisualmodekeybindingsvimoperatorpendingmodekeybindings)
    - [`"vim.insertModeKeyBindingsNonRecursive"`/`"normalModeKeyBindingsNonRecursive"`/`"visualModeKeyBindingsNonRecursive"`/`"operatorPendingModeKeyBindingsNonRecursive"`](#viminsertmodekeybindingsnonrecursivenormalmodekeybindingsnonrecursivevisualmodekeybindingsnonrecursiveoperatorpendingmodekeybindingsnonrecursive)
    - [Debugging 映射](#debugging-映射)
    - [映射更复杂的Key组合](#映射更复杂的key组合)
  - [Vim 模式](#vim-模式)
  - [Vim 设置](#vim-设置)
- [.vimrc支持](#vimrc支持)
- [🖱️ 多光标模式](#️-多光标模式)
- [🔌 被模拟的插件](#-被模拟的插件)
  - [vim-airline](#vim-airline)
  - [vim-easymotion](#vim-easymotion)
  - [vim-surround](#vim-surround)
  - [vim-commentary](#vim-commentary)
  - [vim-indent-object](#vim-indent-object)
  - [vim-sneak](#vim-sneak)
  - [CamelCaseMotion](#camelcasemotion)
  - [Input Method](#input-method)
  - [ReplaceWithRegister](#replacewithregister)
  - [vim-textobj-entire](#vim-textobj-entire)
  - [vim-textobj-arguments](#vim-textobj-arguments)
- [🎩 VSCodeVim 小技巧！](#-vscodevim-小技巧)
- [📚 F.A.Q.](#-faq)
- [❤️ 贡献](#️-贡献)
  - [特别鸣谢](#特别鸣谢)

</details>

## 💾 安装

VSCodeVim 在 [安装](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) 和重载 VS Code 后会自动启动。

### Mac

想要启用 key-repeating（译者注：Key映射） 功能，请在终端中执行以下操作，注销并重新登录，然后重新启动 VSCode：

```sh
$ defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false	# For VS Code
$ defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false	# For VS Code Insider
$ defaults write com.visualstudio.code.oss ApplePressAndHoldEnabled -bool false		# For VS Codium
$ defaults write com.microsoft.VSCodeExploration ApplePressAndHoldEnabled -bool false	# For VS Codium Exploration users
$ defaults delete -g ApplePressAndHoldEnabled	# If necessary, reset global default
```

我们建议在 _系统首选项 -> 键盘_ 增加 Key 重复和延迟功能。

### Windows

像真正的 Vim 一样， VSCodeVim 将接管您的 Ctrl 键行为。这个行为可以使用[`useCtrlKeys`](#vscodevim-settings) 和 [`handleKeys`](#vscodevim-settings) 设置进行修改。

## ⚙️ 设置

此处描述的设置只是受支持设置的一部分；完整的设置列表在 VSCodeVim 的[扩展详情页](https://code.visualstudio.com/docs/editor/extension-gallery#_extension-details)的`Contributions`选项卡中（选项卡可以在 VS Code [扩展视图](https://code.visualstudio.com/docs/editor/extension-gallery)可以找到）。

### 简单示例

下面是一个关于VSCodeVim的 [settings.json](https://code.visualstudio.com/Docs/customization/userandworkspace) 配置文件的示例：

```json
{
  "vim.easymotion": true,
  "vim.incsearch": true,
  "vim.useSystemClipboard": true,
  "vim.useCtrlKeys": true,
  "vim.hlsearch": true,
  "vim.insertModeKeyBindings": [
    {
      "before": ["j", "j"],
      "after": ["<Esc>"]
    }
  ],
  "vim.normalModeKeyBindingsNonRecursive": [
    {
      "before": ["<leader>", "d"],
      "after": ["d", "d"]
    },
    {
      "before": ["<C-n>"],
      "commands": [":nohl"]
    },
    {
      "before": ["K"],
      "commands": ["lineBreakInsert"],
      "silent": true
    }
  ],
  "vim.leader": "<space>",
  "vim.handleKeys": {
    "<C-a>": false,
    "<C-f>": false
  }
}
```

### VSCodeVim 设置

这些设置是 VSCodeVim 的特有设置。

| 设置                             | 描述                                                         | 类型    | 默认值                                                |
| -------------------------------- | ------------------------------------------------------------ | ------- | ----------------------------------------------------- |
| vim.changeWordIncludesWhitespace | 当修改单词时包含尾部相连的空格。 这个配置将作用于在 <kbd>cw</kbd> 指令以及同级别的  (<kbd>yw</kbd> and <kbd>dw</kbd>) 指令上，而不是 <kbd>ce</kbd> | Boolean | false                                                 |
| vim.cursorStylePerMode._{Mode}_  | 为每个 _{模式}_ 配置不同的光标样式，每个模式都有 [默认的光标样式](https://github.com/VSCodeVim/Vim/blob/4a6fde6dbd4d1fac1f204c0dc27c32883651ef1a/src/mode/mode.ts#L34)。已经支持的样式有： line, block, underline, line-thin, block-outline, and underline-thin. | String  | None                                                  |
| vim.digraphs._{shorthand}_       | 设置自定义的有向图简写，可以覆盖默认的简写。 使用两个字符去表示一个有向图和一个或多个UTF16代码位。例如: `"R!": ["🚀", [55357, 56960]]` | Object  | `{"R!": ["🚀", [0xD83D, 0xDE80]]`                      |
| vim.debug.silent                 | Boolean indicating whether log messages will be suppressed（译者注：静默输出日志内容？） | Boolean | false                                                 |
| vim.debug.loggingLevelForConsole | 在控制台输出的最高日志级别。日志在[开发者工具](https://code.visualstudio.com/docs/extensions/developing-extensions#_developer-tools-console)中可见。 支持的值有：  'error'、 'warn'、'info'、'verbose'、'debug'。 | String  | error                                                 |
| vim.debug.loggingLevelForAlert   | 在 VSCode 提示框输出的最高日志级别。 支持的值有： 'error'、 'warn'、'info'、'verbose'、'debug'。 | String  | error                                                 |
| vim.disableExtension             | 禁用 VSCodeVim 扩展。也可以在命令面板使用 `toggleVim` 命令切换。 | Boolean | false                                                 |
| vim.handleKeys                   | 被配置的Key将由 VSCode 执行，而不是 VSCodeVim 扩展。在[package.json](https://github.com/VSCodeVim/Vim/blob/master/package.json) 的 `keybindings` 集合中的（ `when` 参数含有 `vim.use<C-...>` ）任何Key都可以通过设置 `"<C-...>": false`由 VS Code 执行。例如：使用 `ctrl+f` 搜索（VS Code 的原生行为）： `"vim.handleKeys": { "<C-f>": false }`。 | String  | `"<C-d>": true`<br /> `"<C-s>": false`                |
| vim.overrideCopy                 | 为了能在 VSCodeVim 中正确执行，我们覆盖 VSCode Copy 命令，如果 cmd-c/ctrl-c 给您带来了麻烦，可以将此值设置为 false 并在[这里](https://github.com/Microsoft/vscode/issues/217)投诉 | Boolean | false                                                 |
| vim.useSystemClipboard           | 将系统剪切板寄存器 (`*`) 作为默认的寄存器                  | Boolean | false                                                 |
| vim.searchHighlightColor         | 当前搜索未匹配的背景色                                       | String  | `findMatchHighlightBackground` ThemeColor（主题颜色） |
| vim.searchHighlightTextColor     | 当前搜索未匹配的文本色                                       | String  | None                                                  |
| vim.searchMatchColor             | 当前搜索匹配的背景色                                         | String  | `findMatchBackground` ThemeColor（主题颜色）          |
| vim.searchMatchTextColor         | 当前搜索匹配的文本色                                         | String  | None                                                  |
| vim.substitutionColor            | 当 `vim.inccommand` 设置为 enable 时，替换文本的背景色           | String  | "#50f01080"                                           |
| vim.substitutionTextColor        | 当 `vim.inccommand` 设置为 enable 时，替换文本的文本色           | String  | None                                                  |
| vim.startInInsertMode            | 以 Insert 模式启动，而不是 Normal 模式（VSCode 启动时的默认模式） | Boolean | false                                                 |
| vim.useCtrlKeys                  | 使用 Vim 的 Ctrl 键行为代替 VSCode 的行为，例如复制、粘贴、查找、等等 | Boolean | true                                                  |
| vim.visualstar                   | 在Visual模式下，使用 `*` 或 `#` 搜索当前光标所在词           | Boolean | false                                                 |
| vim.highlightedyank.enable       | 当复制时高亮内容                                             | Boolean | false                                                 |
| vim.highlightedyank.color        | 设置复制时高亮的颜色                                         | String  | rgba(250, 240, 170, 0.5)                              |
| vim.highlightedyank.duration     | 设置复制时高亮的高亮的持续时长（译者注：毫秒）               | Number  | 200                                                   |

### Neovim 集成

> :warning: 实验性功能。请在[这里](https://github.com/VSCodeVim/Vim/issues/1735)留下您关于Neovim集成的问题

使用neovim执行Ex命令

1.  安装[neovim](https://github.com/neovim/neovim/wiki/Installing-Neovim)
2.  修改下面的配置：

| 设置                    | 描述                                                         | 类型    | 默认值 |
| ----------------------- | ------------------------------------------------------------ | ------- | ------ |
| vim.enableNeovim        | 允许 Neovim                                                   | Boolean | false  |
| vim.neovimPath          | neovim 可执行文件的绝对路径。如果未设置，则会自动从环境变量查找 neovim 的路径。 | String  |        |
| vim.neovimUseConfigFile | 如果设置为 true，Neovim 会从 `vim.neovimConfigPath` 指定的位置加载配置文件。如果您想使用 Neovim 上的扩展，则这个配置是必须为 true | Boolean | false  |
| vim.neovimConfigPath    | Neovim 配置文件的路径。如果未设置，Neovim 将搜索它的默认配置文件路径 | String  |        |

以下是关于集成 neovim 的一些技巧：

- [g命令](http://vim.wikia.com/wiki/Power_of_g)
- [:normal 命令](https://vi.stackexchange.com/questions/4418/execute-normal-command-over-range)
- 快速查找与替换！

### Key映射

自定义的映射只会在相对应的模式下生效的。

#### `"vim.insertModeKeyBindings"`/`"vim.normalModeKeyBindings"`/`"vim.visualModeKeyBindings"`/`"vim.operatorPendingModeKeyBindings"`

- Key映射（译者注：默认是循环Key映射）可用于 Insert、Normal、Operator Pending and Visual 模式。
- Key绑定可能会包含 `"before"`, `"after"`, `"commands"`和`"silent"`。
- 在插入模式下，绑定 `jj` 为 `<Esc>` （译者注：按下 `jj` 相当于按下 `<Esc>` 的效果）：

```json
    "vim.insertModeKeyBindings": [
        {
            "before": ["j", "j"],
            "after": ["<Esc>"]
        }
    ]
```

- 绑定 `£` 为当前光标的前一个完整单词：

```json
    "vim.normalModeKeyBindings": [
        {
            "before": ["£"],
            "after": ["#"]
        }
    ]
```

- 绑定`:` 为打开命令面板，和在状态栏不显示消息：

```json
    "vim.normalModeKeyBindings": [
        {
            "before": [":"],
            "commands": [
                "workbench.action.showCommands",
            ],
            "silent": true
        }
    ]
```

- 绑定 `<leader>m` 为添加书签，和绑定 `<leader>b` 为打开书签列表 (在使用了 [Bookmarks](https://marketplace.visualstudio.com/items?itemName=alefragnani.Bookmarks) 插件的提前下)：

```json
    "vim.normalModeKeyBindings": [
        {
            "before": ["<leader>", "m"],
            "commands": [
                "bookmarks.toggle"
            ]
        },
        {
            "before": ["<leader>", "b"],
            "commands": [
                "bookmarks.list"
            ]
        }
    ]
```

- 绑定 `ctrl+n` 为关闭搜索高亮，和绑定 `<leader>w` 为保存当前文件:

```json
    "vim.normalModeKeyBindings": [
        {
            "before":["<C-n>"],
            "commands": [
                ":nohl",
            ]
        },
        {
            "before": ["leader", "w"],
            "commands": [
                "workbench.action.files.save",
            ]
        }
    ]
```

- 在 Operator Pending 模式下绑定 `{` 为 `w` ， 使 `y{` 和 `d{` 各自像 `yw` 和 `dw` 一样工作：

```json
    "vim.operatorPendingModeKeyBindings": [
        {
            "before": ["{"],
            "after": ["w"]
        }
    ]
```

- 在 Operator Pending 模式下绑定 `L` 为 `$` 和绑定 `H` 为 `^`， 使 `yL` 和 `dH` 各自像 `y$` 和 `d^` 一样工作:

```json
    "vim.operatorPendingModeKeyBindings": [
        {
            "before": ["L"],
            "after": ["$"]
        },
        {
            "before": ["H"],
            "after": ["^"]
        }
    ]
```

- 在 Visual 模式下，绑定 `>` 和 `<` 为 内缩进行/外缩进行（可重复的）:

```json
    "vim.visualModeKeyBindings": [
        {
            "before": [
                ">"
            ],
            "commands": [
                "editor.action.indentLines"
            ]
        },
        {
            "before": [
                "<"
            ],
            "commands": [
                "editor.action.outdentLines"
            ]
        },
    ]
```

- 绑定 `<leader>vim` 为 克隆 Vim 仓库到指定位置:

```json
    "vim.visualModeKeyBindings": [
        {
            "before": [
                "<leader>", "v", "i", "m"
            ],
            "commands": [
                {
                    "command": "git.clone",
                    "args": [ "https://github.com/VSCodeVim/Vim.git" ]
                }
            ]
        }
    ]
```

#### `"vim.insertModeKeyBindingsNonRecursive"`/`"normalModeKeyBindingsNonRecursive"`/`"visualModeKeyBindingsNonRecursive"`/`"operatorPendingModeKeyBindingsNonRecursive"`

- 非循环Key绑定可用于 Insert，Normal 和 Visual 模式
- _例如：_ 调换两个 Key 的功能，像绑定 `j` 为 `k` 作为使光标向上的命令，和绑定 `k` 为 `j` 作为使光标向下的命令。注意，一旦您在正常情况下（译者注：循环Key映射）尝试了这个绑定，那么 `j` 命令将被 `k` 替代，和 `k` 命令将被 `j` 替代。当这种情况超过 `maxmapdepth` 配置的次数时（默认1000次），将抛出 “E223 循环绑定” 异常。使用非循环Key映射可以停止这种循环的扩大。

```json
    "vim.normalModeKeyBindingsNonRecursive": [
        {
            "before": ["j"],
            "after": ["k"]
        },
        {
            "before": ["k"],
            "after": ["j"]
        }
    ]
```

- 在 Operator Pending 模式下绑定 `(` 为 `i(` ，使 `y(` 和 `c(` 各自像 `yi(` 和 `ci(` 一样工作。

```json
    "vim.operatorPendingModeKeyBindingsNonRecursive": [
        {
            "before": ["("],
            "after": ["i("]
        }
    ]
```

- 在 Visual 模式下绑定 p 为粘贴而不覆盖当前寄存器：

```json
    "vim.visualModeKeyBindingsNonRecursive": [
        {
            "before": [
                "p",
            ],
            "after": [
                "p",
                "g",
                "v",
                "y"
            ]
        }
    ],
```

#### Debugging 映射

1.  您的配置正确吗？

    扩展的 [日志级别](#vscodevim-settings) 调整为 `debug`，重启 VSCode。当加载每个重新映射的配置时，都会在控制台输出日志。在 Developer Tools（译者注：开发者工具） 的控制台下，您有看到任何错误吗？

    ```console
    debug: Remapper: normalModeKeyBindingsNonRecursive. before=0. after=^.
    debug: Remapper: insertModeKeyBindings. before=j,j. after=<Esc>.
    error: Remapper: insertModeKeyBindings. Invalid configuration. Missing 'after' key or 'command'. before=j,k.
    ```

    错误的配置将会被忽略（不会生效）。

2.  扩展是否正常处理了您重新映射的Key？

    VSCodeVim 通过 [package.json](https://github.com/VSCodeVim/Vim/blob/9bab33c75d0a53873880a79c5d2de41c8be1bef9/package.json#L62) 明确的告诉 VSCode 我们所关心的 Key 事件。如果您重新映射的 Key 是 vim/vscodevim 未处理，那么扩展将不会从  VSCode 接收到这些Key事件。把[日志级别](#vscodevim-settings) 调整为 `debug` ，当您按下键位时，您应该会看到类似以下的输出：

    ```console
    debug: ModeHandler: handling key=A.
    debug: ModeHandler: handling key=l.
    debug: ModeHandler: handling key=<BS>.
    debug: ModeHandler: handling key=<C-a>.
    ```

    当您按下您重新映射的Key时，您看到日志输出在上面了吗？如果没有，意味着我们不关心这些Key事件。仍然可以使用VSCode的 [keybindings.json](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference) 重新映射这些Key。（参阅下一节：[重新映射更复杂的Key组合](#remapping-more-complex-key-combinations)）

#### 映射更复杂的Key组合

强烈建议您使用像 `"vim.normalModeKeyBindings"` （[参考此处](#key-remapping)）这样的Vim命令重新映射Key。但有时一般的Key映射不可能支持所有的组合键（例如 `Alt+key` 或 `Ctrl+Shift+key`）。在这种情况下，可以在 [keybindings.json](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference) 文件中创建新的Key绑定。可以这样做：`CTRL+SHIFT+P` 打开 VSCode 的 keybindings.json，然后选择 `Open keyboard shortcuts (JSON)`

你可以像下面这样添加一个新的条目到 keybindings.json：

    ```json
    {
      "key": "YOUR_KEY_COMBINATION",
      "command": "vim.remap",
      "when": "inputFocus && vim.mode == 'VIM_MODE_YOU_WANT_TO_REBIND'",
      "args": {
        "after": ["YOUR_VIM_ACTION"]
      }
    }
    ```

例如，在 Normal 模式下绑定 `ctrl+shift+y` 到 VSCodeVim 的 `yy`（复制当前行），请添加以下内容到您的 keybindings.json 文件：

    ```json
    {
      "key": "ctrl+shift+y",
      "command": "vim.remap",
      "when": "inputFocus && vim.mode == 'Normal'",
      "args": {
        "after": ["y", "y"],
      }
    },
    ```

如果您首次打开 keybindings.json，那么它可能是空的。请在文件添加 `[` 和 `]` 用于存储Key绑定的配置。

### Vim 模式

这是 VSCodeVim 已支持的模式

| 模式                  |
| --------------------- |
| Normal                |
| Insert                |
| Visual                |
| VisualBlock           |
| VisualLine            |
| SearchInProgressMode  |
| CommandlineInProgress |
| Replace               |
| EasyMotionMode        |
| EasyMotionInputMode   |
| SurroundInputMode     |
| OperatorPendingMode   |
| Disabled              |

当在 [keybindings.json](https://code.visualstudio.com/docs/getstarted/keybindings) 中重新绑定Key时使用了 [“when 子句上下文”](https://code.visualstudio.com/api/references/when-clause-contexts)，知道Vim当前处于哪种模式可能很有用。例如当编写“when子句”时，检查vim当前是否处于Normal模式或Visual模式可以编写以下内容：

```json
"when": "vim.mode == 'Normal' || vim.mode == 'Visual'",
```

### Vim 设置

从 vim 复制的设置。 Vim 设置将按以下顺序加载：

1.  `:set {setting}`
2.  来自用户/工作空间的 `vim.{setting}` 
3.  VSCode 设置
4.  VSCodeVim 默认值

| 设置             | 描述                                                         | 类型    | 默认值                                                       |
| ---------------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| vim.autoindent   | 开始新行时，从当前行复制缩进                                 | Boolean | true                                                         |
| vim.gdefault     | 当启用时，`:substitute` 命令和标志位 `g` 命令默认开启。这意味着一行中的所有匹配项都将被替换，而不是一个。当 `:substitute` 指定了标志位 `g` 时，所有的匹配项都将被替换。 | Boolean | false                                                        |
| vim.hlsearch     | 高亮显示当前搜索匹配的所有文本                               | Boolean | false                                                        |
| vim.ignorecase   | 在搜索模式下忽略大小写                                       | Boolean | true                                                         |
| vim.incsearch    | 输入搜索时显示下一个匹配项                                   | Boolean | true                                                         |
| vim.inccommand   | 当输入时， `:substitute` 命令的显示效果                      | String  | `replace`                                                    |
| vim.joinspaces   | 当加入或重新格式化时，在 '.', '?', 和'!' 之后添加两个空格    | Boolean | true                                                         |
| vim.leader       | 定义 `<leader>` 的键位，用于在Key绑定时使用                 | String  | `\`                                                          |
| vim.maxmapdepth  | 循环映射的最大次数。例如设置了“:map x y”，又设置了“:map y x”，就会造成循环映射，类型于代码里面的“相互引用”的概念（译者注：为了便于理解，未使用原文翻译） | Number  | 1000                                                         |
| vim.report       | 有些命令会显示有多少行受影响，只有行数超过 ’report’ 控制的阈值才会显示具体行数 | Number  | 2                                                            |
| vim.shell        | `!` and `:!` commands.用于 `!` 和`:!`命令的shell路径         | String  | `/bin/sh` on Unix, `%COMSPEC%` environment variable on Windows |
| vim.showcmd      | 在状态栏显示（部分）命令                                     | Boolean | true                                                         |
| vim.showmodename | 在状态栏显示当前模式的名称                                   | Boolean | true                                                         |
| vim.smartcase    | 如果搜索表达式包含大写字符，则覆写 `ignorecase` 设置           | Boolean | true                                                         |
| vim.textwidth    | 当使用 `gq` 命令时换行时，允许的最大宽度（译者注：类似格式化代码时达到预定最大宽度时会自动换行） | Number  | 80                                                           |
| vim.timeout      | 重新映射命令的超时时长（毫秒）                               | Number  | 1000                                                         |
| vim.whichwrap    | 当光标位于行首或行尾时，允许指定Key进行左移和右移使光标移动到上一行行尾或下一行行首。更多查看 [:help whichwrap](https://vimhelp.org/options.txt.html#%27whichwrap%27)。（译者注：默认的 `b` 指的是 `Backspace` 键，`s` 指的是 `space` 键） | String  | `b,s`                                                        |

## .vimrc支持

> :warning: .vimrc 的支持目前处于试验阶段。 仅支持重新映射，您可以会遇到问题。请 [反馈它们](https://github.com/VSCodeVim/Vim/issues/new?template=bug_report.md)！

设置`vim.vimrc.enable` 为 `true` ，并且设置 `vim.vimrc.path` 的正确路径。

## 🖱️ 多光标模式

> :warning: Multi-Cursor 模式是实验性的功能。请在我们的 [feedback thread](https://github.com/VSCodeVim/Vim/issues/824)上面反馈您的问题

以下是进行Multi-Cursor模式的方法：

- 在OSX下， `cmd-d`， 在Windows下，`ctrl-d`。
- `gb`, 我们添加的新快捷方式，相当于 `cmd-d` (OSX) 或 `ctrl-d` (Windows)。它会在与光标当前所在的单词匹配的下一个单词处添加另一个光标。
- 运行 "Add Cursor Above/Below" 或 在任何平台运行快捷键。

一旦有多个光标，您应该就可以运行您想要的Vim命令了。大部分情况是可以正常工作的，但也会有一些不支持（ref [PR#587](https://github.com/VSCodeVim/Vim/pull/587)）。

- 每个光标都有它自己的剪切板。
- 在Multi-Cursor Visual模式下按下 Esc 键您将进入Multi-Cursor Normal模式。再次按下 Esc 键您将进入 Normal 模式。

## 🔌 被模拟的插件

### vim-airline

> :warning: 使用这个插件会影响性能。为了更改状态栏，我们将覆写您的工作区settings.json的配置，这将导致您的工作目 录对于不断变化的差异加载延迟。

基于当前模式更改状态栏的颜色。启用后，请配置`"vim.statusBarColors"`。为每个模式设置颜色，例如“ `string`”表示背景色，或“`[string, string]`”表示背景色、前景色。

```json
    "vim.statusBarColorControl": true,
    "vim.statusBarColors.normal": ["#8FBCBB", "#434C5E"],
    "vim.statusBarColors.insert": "#BF616A",
    "vim.statusBarColors.visual": "#B48EAD",
    "vim.statusBarColors.visualline": "#B48EAD",
    "vim.statusBarColors.visualblock": "#A3BE8C",
    "vim.statusBarColors.replace": "#D08770",
    "vim.statusBarColors.commandlineinprogress": "#007ACC",
    "vim.statusBarColors.searchinprogressmode": "#007ACC",
    "vim.statusBarColors.easymotionmode": "#007ACC",
    "vim.statusBarColors.easymotioninputmode": "#007ACC",
    "vim.statusBarColors.surroundinputmode": "#007ACC",
```

### vim-easymotion

基于 [vim-easymotion](https://github.com/easymotion/vim-easymotion) 并通过以下设置进行配置：

| 配置                                             | 描述                                                         | 类型           | 默认值                         |
| ------------------------------------------------ | ------------------------------------------------------------ | -------------- | ------------------------------ |
| vim.easymotion                                   | 允许/禁用 easymotion 插件                                    | Boolean        | false                          |
| vim.easymotionMarkerBackgroundColor              | 标记框的背景色                                               | String         | '#0000'                        |
| vim.easymotionMarkerForegroundColorOneChar       | 一个字符标记的字体颜色                                       | String         | '#ff0000'                      |
| vim.easymotionMarkeForegroundColorTwoCharFirst   | 两个字符标记中第一个字符的字体颜色，用于区别于一个字符标记   | String         | '#ffb400'                      |
| vim.easymotionMarkerForegroundColorTwoCharSecond | 两个字符标记中第二个的字体颜色，用于区分连续标记             | String         | '#b98300'                      |
| vim.easymotionIncSearchForegroundColor           | 搜索n个字符命令的字体颜色，用于突出显示匹配项                | String         | '#7fbf00'                      |
| vim.easymotionDimColor                           | 暗显字符的字体颜色，当`#vim.easymotionDimBackground#`被设置为true时使用 | String         | '#777777'                      |
| vim.easymotionDimBackground                      | 标记可见时是否使其他文本变暗                                 | Boolean        | true                           |
| vim.easymotionMarkerFontWeight                   | 被标记文本的字体粗细                                         | String         | 'bold'                         |
| vim.easymotionKeys                               | 用于跳转标记名称的字符                                       | String         | 'hklyuiopnm,qwertzxcvbasdgjf;' |
| vim.easymotionJumpToAnywhereRegex                | 自定义正则表达式，用于匹配JumpToAnywhere移动（类似于`Easymotion_re_anywhere`）。示例设置：^\\s\*（匹配一行以及 Javascript 注释的开头和结尾）（译者注：此处可能翻译不准确） | \\b[A-Za-z0-9] | [A-Za-z0-9]\\b                 |

一旦您激活了easymotion，可以使用以下命令发起移动指令。在您发起移动指令后，文本装饰器/标记将被显示，然后你可以按被显示的键位跳转至您想要的位置。`leader`是可配置的，默认是 `\` 。（译者注：以下移动指令不理解的话，可以实际操作一下）

| 移动命令                            | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `<leader><leader> s <char>`         | 搜索字符                                                     |
| `<leader><leader> f <char>`         | 向后搜索字符                                                 |
| `<leader><leader> F <char>`         | 向前搜索字符                                                 |
| `<leader><leader> t <char>`         | 向后搜索字符，标记位是被搜索字符的前一个字符                 |
| `<leader><leader> T <char>`         | 向前搜索字符，标记位是被搜索字符的后一个字符                 |
| `<leader><leader> w`                | 向后标记所有单词，标记位是第一个字符                         |
| `<leader><leader> b`                | 向前标记所有单词，标记位是第一个字符                         |
| `<leader><leader> l`                | 向后匹配 `_` \\ `#` 之后、驼峰、单词的开关和结尾             |
| `<leader><leader> h`                | 向前匹配`_` \\ `#` 之后、驼峰、单词的开关和结尾              |
| `<leader><leader> e`                | 向后标记所有单词，标记位是最后一个字符                       |
| `<leader><leader> ge`               | 向前标记所有单词，标记位是最后一个字符                       |
| `<leader><leader> j`                | 向后标记所有行，标记位是第一个非空字符                       |
| `<leader><leader> k`                | 向前标记所有行，标记位是第一个非空字符                       |
| `<leader><leader> / <char>... <CR>` | 搜索n个字符                                                  |
| `<leader><leader><leader> bdt`      | Til character                                                |
| `<leader><leader><leader> bdw`      | 单词的开头                                                   |
| `<leader><leader><leader> bde`      | 单词的结尾                                                   |
| `<leader><leader><leader> bdjk`     | 行的开始                                                     |
| `<leader><leader><leader> j`        | JumpToAnywhere 移动；默认行为匹配 `_` \\ `#` 之后、驼峰、单词的开关和结尾 |

`<leader><leader> (2s|2f|2F|2t|2T) <char><char>` 和 `<leader><leader><leader> bd2t <char>char>` 也是可用的。
不同之处在于搜索的字符数。
例如，`<leader><leader> 2s <char><char>` 需要2个字符，并按这2个字符搜索。

这个映射不是标准的映射，因此建议您使用自定义的映射。

### vim-surround

基于 [surround.vim](https://github.com/tpope/vim-surround)，这个插件用于处理范围字符，例如圆括号、方括号、引号和XML标签。

| 配置         | 描述                   | 类型    | 默认值 |
| ------------ | ---------------------- | ------- | ------ |
| vim.surround | 允许/禁用 vim-surround | Boolean | true   |

使用 `t`  或 `<` 代替 `<desired>` 或 `<existing>` 将进入tag输入模式。使用 `<CR>` 代替 `>` 来完成tag的更改，并保留所有已存在的属性。

| 范围命令                   | 描述                                           |
| -------------------------- | ---------------------------------------------- |
| `y s <motion> <desired>`   | 为`<motion>`选中的区域添加`desired`范围字符    |
| `d s <existing>`           | 删除`<existing>`范围字符                       |
| `c s <existing> <desired>` | 把  `existing` 范围字符改成 `desired` 范围字符 |
| `S <desired>`              | 在 Visual 模式下给选中区域添加范围字符         |

示例：

- `"test"` ，将光标移至引号内， 输入 `cs"'` 后会变成 `'test'`
- `"test"` 将光标移至引号内，输入 `ds"` 后会变成 `test`
- `"test"` 将光标移至引号内， 输入 `cs"t` 然后再输入 `123>` 后会变成 `<123>test</123>`

### vim-commentary

类似于 [vim-commentary](https://github.com/tpope/vim-commentary)， 但使用了 VS Code 原生的 _Toggle Line Comment_ 和 _Toggle Block Comment_ 功能。

示例：

- `gc` - 切换行注释。 例如`gcc`（取消）当前行注释，`gc2j`（取消）当前行和下一行注释。
- `gC` - 切换块注释。 例如`gCi`用括号注释选中区域。

### vim-indent-object

基于 [vim-indent-object](https://github.com/michaeljsmith/vim-indent-object)，它允许将处于同一缩进级别的代码块当做一个文本对象。对于不需要使用大括号的语言中非常有用（例如：Python）。

前提是在括号和tag的开头和结尾之间有一个新行，它可以被认为是一个不可知论的`cib`/`ci{`/`ci[`/`cit`。（译者注：不是很理解）

| 命令           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| `<operator>ii` | 当前行缩进级别                                               |
| `<operator>ai` | 当前行缩进级别和上一行（想想Python中的`if` 语句）            |
| `<operator>aI` | 当前行缩进级别，上一行和下一行（想想 C/C++/Java/etc中的`if` 语句） |

### vim-sneak

基于 [vim-sneak](https://github.com/justinmk/vim-sneak)，它允许由两个字符指定的任何位置。

| 配置                               | 描述                                                       | 类型    | 默认值 |
| ---------------------------------- | ---------------------------------------------------------- | ------- | ------ |
| vim.sneak                          | 开启/禁用 vim-sneak                                        | Boolean | false  |
| vim.sneakUseIgnorecaseAndSmartcase | 当使用sneaking时，遵守 `vim.ignorecase` 和 `vim.smartcase` | Boolean | false  |

一旦sneak被激活，可以使用以下命令发起移动指令，对于操作键，sneak 使用`z`代替了`s`，因为`s`已经被surround插件占用。

| 移动命令                  | 描述                                                   |
| ------------------------- | ------------------------------------------------------ |
| `s<char><char>`           | 向前移动到`<char><char>`第一次出现的位置               |
| `S<char><char>`           | `<char><char>`向后移动到`<char><char>`第一次出现的位置 |
| `<operator>z<char><char>` | 向前执行`<operator>` 第一次出现`<char><char>`的位置    |
| `<operator>Z<char><char>` | 向后执行`<operator>` 第一次出现`<char><char>`的位置    |

### CamelCaseMotion

基于 [CamelCaseMotion](https://github.com/bkad/CamelCaseMotion)，但不是完全的模糊。这个插件提供了一种更简单的方法来移动camelCase和snake_case单词

| 配置                       | 描述                      | 类型    | 默认值 |
| -------------------------- | ------------------------- | ------- | ------ |
| vim.camelCaseMotion.enable | 启用/禁用 CamelCaseMotion | Boolean | false  |

一旦CamelCaseMotion被启用，以下命令是可以使用的：

| Motion Command         | Description                                        |
| ---------------------- | -------------------------------------------------- |
| `<leader>w`            | 向前移动到下一个camelCase或snake_case词段的开头    |
| `<leader>e`            | 向前移动到下一个camelCase或snake_case词段的结尾    |
| `<leader>b`            | 向后移动到下一个camelCase或snake_case词段的开头    |
| `<operator>i<leader>w` | 选择/修改/删除/等当前的camelCase or snake_case词段 |

默认情况下，`<leader>` 被映射到`\`，因此例如，`d2i\w` 将删除当前和下一个驼峰字母词段。

### Input Method

退出Insert模式时禁用输入法

| 配置                                    | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| `vim.autoSwitchInputMethod.enable`      | 一个布尔值，表示autoSwitchInputMethod是开还是关              |
| `vim.autoSwitchInputMethod.defaultIM`   | 默认输入法                                                   |
| `vim.autoSwitchInputMethod.obtainIMCmd` | 检索当前输入法键值的命令的完整路径                           |
| `vim.autoSwitchInputMethod.switchIMCmd` | 切换输入法命令的完整路径，使用 `{im}` 作为输入法键值的占位符 |

任何第三方程序都可以用来切换输入法。下面将介绍如何配置 [im-select](https://github.com/daipeihust/im-select)：

1.  安装im-select （查看 [安装向导](https://github.com/daipeihust/im-select#installation)）
1. 查找你默认输入法的键值

    - Mac：

      将你的输入法切换为英文，并在你的终端中运行以下命令：`/<path-to-im-select-installation>/im-select` ，之后将输出您的当前输入法的键值。下表列出了MacOS常用的英文键布局。  

      | 键值                           | 描述       |
      | ------------------------------ | ---------- |
      | com.apple.keylayout.US         | U.S.       |
      | com.apple.keylayout.ABC        | ABC        |
      | com.apple.keylayout.British    | British    |
      | com.apple.keylayout.Irish      | Irish      |
      | com.apple.keylayout.Australian | Australian |
      | com.apple.keylayout.Dvorak     | Dvorak     |
      | com.apple.keylayout.Colemak    | Colemak    |

    - Windows：

      关于如何查看您的输入法键值请查看[im-select向导](https://github.com/daipeihust/im-select#to-get-current-keyboard-locale) 。通常，如果您的键盘布局是en_US，那么输入法键值是1033 (en_US键盘布局的locale ID)。 您也可以从[这个页面](https://www.science.co.il/language/Locale-codes.php)找到您的locale ID，其中`LCID Decimal` 列是locale ID。

1.  配置 `vim.autoSwitchInputMethod`.

    - MacOS:

      假设给出了`com.apple.keylayout.US` 的输入法键值，和`im-select` 的本地路径是`/usr/local/bin`，那么配置下：

      ```json
      "vim.autoSwitchInputMethod.enable": true,
      "vim.autoSwitchInputMethod.defaultIM": "com.apple.keylayout.US",
      "vim.autoSwitchInputMethod.obtainIMCmd": "/usr/local/bin/im-select",
      "vim.autoSwitchInputMethod.switchIMCmd": "/usr/local/bin/im-select {im}"
      ```

    - Windows:

      假设给出了`1033` (en_US)的输入法键值，和`im-select.exe`的本地路径是`D:/bin`，那么配置如下：

      ```json
      "vim.autoSwitchInputMethod.enable": true,
      "vim.autoSwitchInputMethod.defaultIM": "1033",
      "vim.autoSwitchInputMethod.obtainIMCmd": "D:\\bin\\im-select.exe",
      "vim.autoSwitchInputMethod.switchIMCmd": "D:\\bin\\im-select.exe {im}"
      ```

上面的`{im}` 参数是的一个命令行选项，将被传递给 `im-select`用于切换输入法。 如果使用替代程序来切换输入方法，应该在配置中添加类似的选项。例如，如果程序使用的是`my-program -s imKey` 来切换输入法，那么`vim.autoSwitchInputMethod.switchIMCmd` 应该是`/path/to/my-program -s {im}`

### ReplaceWithRegister

基于 [ReplaceWithRegister](https://github.com/vim-scripts/ReplaceWithRegister)，用寄存器的内容替换现有文本的一种简单方法。 

| 配置                    | 描述                               | 类型    | 默认值 |
| ----------------------- | ---------------------------------- | ------- | ------ |
| vim.replaceWithRegister | Enable/disable ReplaceWithRegister | Boolean | false  |

一旦激活，输入 `gr` (全称“go replace”)，然后输入一个动作，来描述您想要使用寄存器内容替换的文本。 

| 移动命令                | 描述                                        |
| ----------------------- | ------------------------------------------- |
| `[count]["a]gr<motion>` | 移动所描述的文本，替换为指定寄存器的内容    |
| `[count]["a]grr`        | \[count\]行或当前行，替换为指定寄存器的内容 |
| `{Visual}["a]gr`        | 所选内容，替换为指定寄存器的内容            |

### vim-textobj-entire

和 [vim-textobj-entire](https://github.com/kana/vim-textobj-entire) 相似

添加两个有用的文本对象：

- `ae` 代表了buffer的全部内容
- `ie` 代表了buffer区不包含开关和结尾空格的全部内容

使用示例：

- `dae` - 删除整个buffer区内容
- `yie` - 复制buffer区不包含开头和结尾空格的全部内容
- `gUae` - 将buffer区全部内容转换为大写

### vim-textobj-arguments

类似于在 [targets.vim](https://github.com/wellle/targets.vim) 中的参数文本对象。在大多数编程语言中，对于处理函数内部参数，使用它会变的非常简单。

| 移动命令       | 描述               |
| -------------- | ------------------ |
| `<operator>ia` | 不包含分隔符的参数 |
| `<operator>aa` | 包含分隔符的参数   |

使用示例：

- `cia` - 更改光标下的参数，同时保留`,`分隔符  
- `daa` - 删除光标下的所有参数，包含分隔符（如果存在的话）

| 配置                                | 描述             | 类型        | 默认值     |
| ----------------------------------- | ---------------- | ----------- | ---------- |
| vim.argumentObjectOpeningDelimiters | 开始分隔符的列表 | String list | ["(", "["] |
| vim.argumentObjectClosingDelimiters | 结束分隔符的列表 | String list | [")", "]"] |
| vim.argumentObjectSeparators        | 对象分隔符列表   | String list | [","]      |

## 🎩 VSCodeVim 小技巧！

VSCode 有很多实用的技巧，我们试着保留其中的一些：

- `gd` - 跳转至定义处
- `gq` - 对选中的文本块进行换行，保持注释风格。非常适合格式化文档注释
- `gb` - 定位与当前光标下单词相同的在下一个位置处，并添加光标
- `af` - Visual 模式下的命令，用于选择更大范围内的文本块。例如，有一段 `blah (foo [bar 'baz'])`，按下 `af` 后首先会选中 `baz`，再次按下会选中 `[bar 'baz']`，第三次按下会选中 `(foo [bar 'baz'])`
- `gh` - 相当于将鼠标悬停在光标所在的位置。 方便查看类型和错误消息，无需使用鼠标!  

## 📚 F.A.Q.

- Q：Visual Studio Code 原生的 `ctrl` 命令没有生效（例如：`ctrl+f`, `ctrl+v`）

  A：设置 [`useCtrlKeys` 配置](#vscodevim-设置) 为 `false`

- Q：移动 `j`/`k` 到折线上面，将打开和关闭折线内容

- A：试着把`vim.foldfix` 为 `true`，这是一处修改；它正常工作，但是有副作用（查看[issue#22276](https://github.com/Microsoft/vscode/issues/22276)）

- Q：替换键不能用了

  A：你是用Mac吗？您可以先看看我们的[mac-setup](#mac)介绍

- Q：有烦人的提示/通知/弹窗，我不能使用 `<esc>` 关闭！或者我在某些情况下我们关闭弹窗
  A：按 `shift+<esc>` 去关闭所有的弹窗

- Q：在 Zen 模式下或状态栏被禁用下，我如何使用命令行？

  A：这个扩展允许重新映射命令以显示 VSCode 风格的快选版本，但此功能有很多限制。可以在 VSCode 的keybindings.json 文件中增加以下内容：

  ```json
  {
    "key": "shift+;",
    "command": "vim.showQuickpickCmdLine",
    "when": "editorTextFocus && vim.mode != 'Insert'"
  }
  ```

  或者仅在 Zen 模式下：

  ```json
  {
    "key": "shift+;",
    "command": "vim.showQuickpickCmdLine",
    "when": "inZenMode && vim.mode != 'Insert'"
  }
  ```

- Q：如何使用换行功能移动光标？

- A：如果你设置了单词自动换行，并且当使用<kbd>j</kbd>, <kbd>k</kbd>, <kbd>↓</kbd> 或 <kbd>↑</kbd>时希望光标进入每个自动换行的行里，在VSCode的keybindings.json文件中添加以下内容：
  <!-- prettier-ignore -->
  ```json
  {
    "key": "up",
    "command": "cursorUp",
    "when": "editorTextFocus && vim.active && !inDebugRepl && !suggestWidgetMultipleSuggestions && !suggestWidgetVisible"
  },
  {
    "key": "down",
    "command": "cursorDown",
    "when": "editorTextFocus && vim.active && !inDebugRepl && !suggestWidgetMultipleSuggestions && !suggestWidgetVisible"
  },
  {
    "key": "k",
    "command": "cursorUp",
    "when": "editorTextFocus && vim.active && !inDebugRepl && vim.mode == 'Normal' && !suggestWidgetMultipleSuggestions && !suggestWidgetVisible"
  },
  {
    "key": "j",
    "command": "cursorDown",
    "when": "editorTextFocus && vim.active && !inDebugRepl && vim.mode == 'Normal' && !suggestWidgetMultipleSuggestions && !suggestWidgetVisible"
  }
  ```

  **警告：** 此解决方案将恢复VSCode中 <kbd>j</kbd> 和 <kbd>k</kbd> 的默认行为，所以像 `10j` [将不起作用](https://github.com/VSCodeVim/Vim/pull/3623#issuecomment-481473981)。如果您需要这些移动指令起作用，您还有[其他性能较差的选择](https://github.com/VSCodeVim/Vim/issues/2924#issuecomment-476121848)。

- 我用 setxkbmap 交换了 Escape 和 Caps Lock 按键，但在 VSCodeVim 中不起作用

  这是一个在 VSCode 中已知的问题，在一个工作区中，您可以设置 `"keyboard.dispatch": "keyCode"` 然后重启VSCode。

## ❤️ 贡献

这个项目是由一组了不起的人在维护，非常欢迎您贡献爱心。如果您也想参与进来，可以查看[贡献向导](https://github.com/VSCodeVim/Vim/blob/master/.github/CONTRIBUTING.md)

<a href="https://www.buymeacoffee.com/jasonpoon" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Us A Coffee" style="height: auto !important;width: auto !important;" ></a>

### 特别鸣谢:

- 感谢 @xconverge 提交了100多次代码。如果您发现您最讨厌的 bug 悄悄消失了，很可能是他修复的。
- 感谢 @Metamist 实现的 EasyMotion ！
- 感谢 @sectioneight 实现的文本对象！
- 强大的工具 [Kevin Coleman](http://kevincoleman.io)，创造我们的LOGO！
- 向 @chillee （又名 Horace He） 致敬，感谢他的贡献和辛勤工作。