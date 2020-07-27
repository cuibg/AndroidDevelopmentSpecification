## 摘要

- [1 前言](#1-前言)
- [2 Android Studio 规范](#2-Android-Studio-规范)
- [3 资源文件命名](#3-资源文件命名)
- [4 命名规范](#4-命名规范)
- [5 组件开发规范](#5-组件开发规范)
- [6 代码样式规范](#6-代码样式规范)
- [7 提交日志规范](#7-提交日志规范)
- [8 注释规范](#8-注释规范)
- [9 三方库](#9-三方库推荐使用)
- [10 常见英文单词缩写](#10-常见英文单词缩写)

### 1 前言

为了更好的进行项目维护、增强代码可读性、提升 Code Review 效率以及规范团队安卓开发，所以提出 Android 开发规范，该规范结合本人多年的开发经验以及《Clean Code》，阿里开发规范凝聚而成。如有更好建议，欢迎到 GitHub 提 issue。此规范仅限于 Android 基本规范，如想要查阅更详细的规范，请查看我的另外一篇文章原文地址和源码：**[代码整洁规范通用版][clean code(gitbook)]** 和 **[源码][clean code源码]**

### 2 Android Studio 规范

1. 编码统一使用 **UTF-8** 编码。
2. 代码采用格式化操作，可以使用 CheckStyle,导入团队模板或者个人模板，团队 style 永远高于个人。
3. 采用 AS 自动导包功能，删除无用的导包。 使用 Optimize Imports（Settings -> Keymap -> Optimize Imports）快捷键。
4. 采用 Google 发布的最稳定版本的 AS，确保尽量不会因为编译器而出现开发的问题。

### 3 资源文件命名

资源建议采用分包的方式进行使用

![分包示例][分包示例图]

新增了 res_assess 资源包

在 gradle 中配置

```
android {
    ...
    sourceSets {
        main {
            res.srcDirs('src/main/res', 'src/main/res_assess')
        }
    }
}
```

然后同步一下就可以使用啦！！

#### 3.1 布局文件 layout

使用举例

```
Activity 的layout 以module_activity 开头
Fragment 的layout 以module_fragment 开头
Dialog 的layout 以module_dialog 开头
include 的layout 以module_include 开头
ListView 的行layout 以module_list_item 开头
RecyclerView 的item layout 以module_recycle_item 开头
GridView 的item layout 以module_grid_item 开头
```

简单规则：

| 名称                     | 说明                                                        |
| ------------------------ | ----------------------------------------------------------- |
| `activity_buy_car.xml`   | 主窗体 `类型_模块名`                                        |
| `activity_main_head.xml` | 主窗体头部 `类型_模块名_逻辑名称`                           |
| `fragment_car.xml`       | 车辆片段 `类型_模块名`                                      |
| `dialog_loading.xml`     | 加载对话框 `类型_逻辑名称`                                  |
| `recycler_item_car_info` | RecyclerView item 布局 `recycler_item_类型_模块名_逻辑名称` |

#### 3.2 drawable(selector/shape/layer-list/图片 webp,png 等)

drawable 资源名称以小写单词+下划线的方式命名，根据分辨率不同存放在不同的 drawable 目录下，如果介意包大小建议只使用一套，系统去进行缩放。

- 命名规则

  ```
  模块名_业务功能描述_控件描述_控件状态限定词
  //例如
  module_login_btn_pressed,module_tabs_icon_home_normal
  ```

#### 3.3 动画资源 (anim/ animator/)

1. 资源名称以小写单词+下划线的方式命名
2. 规则
   ```
   模块名_逻辑名称_[方向|序号]
   ```
3. 例子

   - Tween 动画（使用简单图像变换的动画，例如缩放、平移）资源：尽可能以通用的动画名称命名，如 module_fade_in , module_fade_out , module_push_down_in (动画+方向)。
   - Frame 动画（按帧顺序播放图像的动画）资源：尽可能以模块+功能命名+序号。module_loading_grey_001。

4. 举例

   | 名称                | 说明         |
   | ------------------- | ------------ |
   | `fade_in`           | 淡入         |
   | `fade_out`          | 淡出         |
   | `push_down_in`      | 下方推入     |
   | `push_down_out`     | 下方推出     |
   | `push_left`         | 推向左方     |
   | `slide_in_from_top` | 头部滑动进入 |
   | `zoom_enter`        | 缩放变形进入 |
   | `slide_in`          | 滑动进入     |
   | `shrink_to_middle`  | 中间缩小     |

#### 3.4 颜色资源 (color/)

1. color 资源使用#AARRGGBB 格式，写入 module_colors.xml 文件中
2. 规则

   ```
   模块名_逻辑名称_颜色
   ```

3. 举例

   ```
   <color name="module_btn_bg_color">#33b5e5e5</color>
   ```

#### 3.5 dimen

1. dimen 资源以小写单词+下划线方式命名，写入 module_dimens.xml 文件中
2. 规则

   ```
   模块名_描述信息
   ```

3. 举例

   ```
   <dimen name="module_horizontal_line_height">1dp</dimen>
   ```

#### 3.6 style

1. style 资源采用“ 父 style 名称.当前 style 名称”方式命名，写入 module_styles.xml 文件中，首字母大写.
2. 举例

   ```
   <style name="ParentTheme.ThisActivityTheme">
   …
   </style>
   ```

#### 3.7 string

1. string 资源文件或者文本用到字符需要全部写入 module_strings.xml 文件中，字符串以小写单词+下划线的方式命名.
2. 规则

   ```
   模块名_逻辑名称
   ```

3. 举例

   ```
   moudule_login_tips,module_homepage_notice_desc
   ```

#### 3.8 id

1. Id 资源原则上以驼峰法命名，View 组件的资源 id 建议以 View 的缩写作为前缀.
2. 举例

   | 控件             | 缩写 |
   | ---------------- | ---: |
   | LinearLayout     |   ll |
   | RelativeLayout   |   rl |
   | ConstraintLayout |   cl |
   | ListView         |   lv |
   | ScollView        |   sv |
   | TextView         |   tv |
   | Button           |  btn |
   | ImageView        |   iv |
   | CheckBox         |   cb |
   | RadioButton      |   rb |
   | EditText         |   et |

3. 其它控件的缩写推荐使用小写字母并用下划线进行分割，例如：ProgressBar 对应的缩写为 progress_bar；DatePicker 对应的缩写为 date_picker。

#### 3.9 bimap or drawable 资源

1. 图片根据其分辨率，放在不同屏幕密度的 drawable 目录下管理
2. 为了支持多种屏幕尺寸和密度，Android 提供了多种通用屏幕密度来适配
3. 但是通常我们只用一种或者两种图片就 OK 了
4. 例子

   | 名称                      | 说明                                        |
   | ------------------------- | ------------------------------------------- |
   | `btn_main_about.png`      | 主页关于按键 `类型_模块名_逻辑名称`         |
   | `btn_back.png`            | 返回按键 `类型_逻辑名称`                    |
   | `divider_maket_white.png` | 商城白色分割线 `类型_模块名_颜色`           |
   | `ic_edit.png`             | 编辑图标 `类型_逻辑名称`                    |
   | `bg_main.png`             | 主页背景 `类型_逻辑名称`                    |
   | `btn_red.png`             | 红色按键 `类型_颜色`                        |
   | `btn_red_big.png`         | 红色大按键 `类型_颜色`                      |
   | `ic_head_small.png`       | 小头像图标 `类型_逻辑名称`                  |
   | `bg_input.png`            | 输入框背景 `类型_逻辑名称`                  |
   | `divider_white.png`       | 白色分割线 `类型_颜色`                      |
   | `bg_main_head.png`        | 主页头部背景 `类型_模块名_逻辑名称`         |
   | `def_search_cell.png`     | 搜索页面默认单元图片 `类型_模块名_逻辑名称` |
   | `ic_more_help.png`        | 更多帮助图标 `类型_逻辑名称`                |
   | `divider_list_line.png`   | 列表分割线 `类型_逻辑名称`                  |
   | `sel_search_ok.xml`       | 搜索界面确认选择器 `类型_模块名_逻辑名称`   |
   | `shape_music_ring.xml`    | 音乐界面环形形状 `类型_模块名_逻辑名称`     |

   如果有多种形态，如按钮选择器：`sel_btn_xx.xml`，采用如下命名：

   | 名称                    | 说明                                |
   | ----------------------- | ----------------------------------- |
   | `sel_btn_xx`            | 作用在 `btn_xx` 上的 `selector`     |
   | `btn_xx_normal`         | 默认状态效果                        |
   | `btn_xx_pressed`        | `state_pressed` 点击效果            |
   | `btn_xx_focused`        | `state_focused` 聚焦效果            |
   | `btn_xx_disabled`       | `state_enabled` 不可用效果          |
   | `btn_xx_checked`        | `state_checked` 选中效果            |
   | `btn_xx_selected`       | `state_selected` 选中效果           |
   | `btn_xx_hovered`        | `state_hovered` 悬停效果            |
   | `btn_xx_checkable`      | `state_checkable` 可选效果          |
   | `btn_xx_activated`      | `state_activated` 激活效果          |
   | `btn_xx_window_focused` | `state_window_focused` 窗口聚焦效果 |

   > 注意：使用 Android Studio 的插件 SelectorChapek 可以生成 selector，前提是命名要规范。

### 4 命名规范

这一部分内容(引用来自 **[android standard develop][android standard develop]**)

代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。正确的英文拼写和语法可以让阅读者易于理解，避免歧义。

> 注意：即使纯拼音命名方式也要避免采用。但 `alibaba`、`taobao`、`youku`、`hangzhou` 等国际通用的名称，可视同英文。

#### 4.1 包名

包名全部小写，连续的单词只是简单地连接起来，不使用下划线，采用反域名命名规则，全部使用小写字母。一级包名是顶级域名，通常为 `com`、`edu`、`gov`、`net`、`org` 等，二级包名为公司名，三级包名根据应用进行命名，后面就是对包名的划分了，关于包名的划分，推荐采用 PBF（按功能分包 Package By Feature），一开始我们采用的也是 PBL（按层分包 Package By Layer），很坑爹。PBF 可能不是很好区分在哪个功能中，不过也比 PBL 要好找很多，且 PBF 与 PBL 相比较有如下优势：

- package 内高内聚，package 间低耦合

  哪块要添新功能，只改某一个 package 下的东西。

  PBL 降低了代码耦合，但带来了 package 耦合，要添新功能，需要改 model、dbHelper、view、service 等等，需要改动好几个 package 下的代码，改动的地方越多，越容易产生新问题，不是吗？

  PBF 的话 featureA 相关的所有东西都在 featureA 包，feature 内高内聚、高度模块化，不同 feature 之间低耦合，相关的东西都放在一起，还好找。

- package 有私有作用域（package-private scope）

  你负责开发这块功能，这个目录下所有东西都是你的。

  PBL 的方式是把所有工具方法都放在 util 包下，小张开发新功能时候发现需要一个 xxUtil，但它又不是通用的，那应该放在哪里？没办法，按照分层原则，我们还得放在 util 包下，好像不太合适，但放在其它包更不合适，功能越来越多，util 类也越定义越多。后来小李负责开发一块功能时发现需要一个 xxUtil，同样不通用，去 util 包一看，怎么已经有了，而且还没法复用，只好放弃 xx 这个名字，改为 xxxUtil……，因为 PBL 的 package 没有私有作用域，每一个包都是 public（跨包方法调用是很平常的事情，每一个包对其它包来说都是可访问的）；如果是 PBF，小张的 xxUtil 自然放在 featureA 下，小李的 xxUtil 在 featureB 下，如果觉得 util 好像是通用的，就去 util 包看看要不要把工具方法添进 xxUtil, class 命名冲突没有了。

  PBF 的 package 有私有作用域，featureA 不应该访问 featureB 下的任何东西（如果非访问不可，那就说明接口定义有问题）。

- 很容易删除功能

  统计发现新功能没人用，这个版本那块功能得去掉。

  如果是 PBL，得从功能入口到整个业务流程把受到牵连的所有能删的代码和 class 都揪出来删掉，一不小心就完蛋。

  如果是 PBF，好说，先删掉对应包，再删掉功能入口（删掉包后入口肯定报错了），完事。

- 高度抽象

  解决问题的一般方法是从抽象到具体，PBF 包名是对功能模块的抽象，包内的 class 是实现细节，符合从抽象到具体，而 PBL 弄反了。

  PBF 从确定 AppName 开始，根据功能模块划分 package，再考虑每块的具体实现细节，而 PBL 从一开始就要考虑要不要 dao 层，要不要 com 层等等。

- 只通过 class 来分离逻辑代码

  PBL 既分离 class 又分离 package，而 PBF 只通过 class 来分离逻辑代码。

  没有必要通过 package 分离，因为 PBL 中也可能出现尴尬的情况：

  ```
  ├── service
          ├── MainService.java
  ```

  按照 PBL, service 包下的所有东西都是 Service，应该不需要 Service 后缀，但实际上通常为了方便，直接 import service 包，Service 后缀是为了避免引入的 class 和当前包下的 class 命名冲突，当然，不用后缀也可以，得写清楚包路径，比如 `new com.domain.service.MainService()`，麻烦；而 PBF 就很方便，无需 import，直接 `new MainService()` 即可。

- package 的大小有意义了

  PBL 中包的大小无限增长是合理的，因为功能越添越多，而 PBF 中包太大（包里 class 太多）表示这块需要重构（划分子包）。

如要知道更多好处，可以查看这篇博文：**[Package by features, not layers][package by features, not layers]**，当然，我们大谷歌也有相应的 Sample：**[todo-mvp][todo-mvp]**，其结构如下所示，很值得学习。

```
com
└── example
    └── android
        └── architecture
            └── blueprints
                └── todoapp
                    ├── BasePresenter.java
                    ├── BaseView.java
                    ├── addedittask
                    │   ├── AddEditTaskActivity.java
                    │   ├── AddEditTaskContract.java
                    │   ├── AddEditTaskFragment.java
                    │   └── AddEditTaskPresenter.java
                    ├── data
                    │   ├── Task.java
                    │   └── source
                    │       ├── TasksDataSource.java
                    │       ├── TasksRepository.java
                    │       ├── local
                    │       │   ├── TasksDbHelper.java
                    │       │   ├── TasksLocalDataSource.java
                    │       │   └── TasksPersistenceContract.java
                    │       └── remote
                    │           └── TasksRemoteDataSource.java
                    ├── statistics
                    │   ├── StatisticsActivity.java
                    │   ├── StatisticsContract.java
                    │   ├── StatisticsFragment.java
                    │   └── StatisticsPresenter.java
                    ├── taskdetail
                    │   ├── TaskDetailActivity.java
                    │   ├── TaskDetailContract.java
                    │   ├── TaskDetailFragment.java
                    │   └── TaskDetailPresenter.java
                    ├── tasks
                    │   ├── ScrollChildSwipeRefreshLayout.java
                    │   ├── TasksActivity.java
                    │   ├── TasksContract.java
                    │   ├── TasksFilterType.java
                    │   ├── TasksFragment.java
                    │   └── TasksPresenter.java
                    └── util
                        ├── ActivityUtils.java
                        ├── EspressoIdlingResource.java
                        └── SimpleCountingIdlingResource.java
```

参考以上的代码结构，按功能分包具体可以这样做：

```
com
└── domain
    └── app
        ├── App.java 定义 Application 类
        ├── Config.java 定义配置数据（常量）
        ├── base 基础组件
        ├── custom_view 自定义视图
        ├── data 数据处理
        │   ├── DataManager.java 数据管理器，
        │   ├── local 来源于本地的数据，比如 SP，Database，File
        │   ├── model 定义 model（数据结构以及 getter/setter、compareTo、equals 等等，不含复杂操作）
        │   └── remote 来源于远端的数据
        ├── feature 功能
        │   ├── feature0 功能 0
        │   │   ├── feature0Activity.java
        │   │   ├── feature0Fragment.java
        │   │   ├── xxAdapter.java
        │   │   └── ... 其他 class
        │   └── ...其他功能
        ├── injection 依赖注入
        ├── util 工具类
        └── widget 小部件
```

#### 4.2 类名

类名都以 `UpperCamelCase` 风格编写。

类名通常是名词或名词短语，接口名称有时可能是形容词或形容词短语。现在还没有特定的规则或行之有效的约定来命名注解类型。

名词，采用大驼峰命名法，尽量避免缩写，除非该缩写是众所周知的， 比如 HTML、URL，如果类名称中包含单词缩写，则单词缩写的每个字母均应大写。

| 类                     | 描述                            | 例如                                                                                                       |
| :--------------------- | :------------------------------ | :--------------------------------------------------------------------------------------------------------- |
| `Activity` 类          | `Activity` 为后缀标识           | 欢迎页面类 `WelcomeActivity`                                                                               |
| `Adapter` 类           | `Adapter` 为后缀标识            | 新闻详情适配器 `NewsDetailAdapter`                                                                         |
| 解析类                 | `Parser` 为后缀标识             | 首页解析类 `HomePosterParser`                                                                              |
| 工具方法类             | `Utils` 或 `Manager` 为后缀标识 | 线程池管理类：`ThreadPoolManager`<br>日志工具类：`LogUtils`（`Logger` 也可）<br>打印工具类：`PrinterUtils` |
| 数据库类               | 以 `DBHelper` 后缀标识          | 新闻数据库：`NewsDBHelper`                                                                                 |
| `Service` 类           | 以 `Service` 为后缀标识         | 时间服务 `TimeService`                                                                                     |
| `BroadcastReceiver` 类 | 以 `Receiver` 为后缀标识        | 推送接收 `JPushReceiver`                                                                                   |
| `ContentProvider` 类   | 以 `Provider` 为后缀标识        | `ShareProvider`                                                                                            |
| 自定义的共享基础类     | 以 `Base` 开头                  | `BaseActivity`, `BaseFragment`                                                                             |

测试类的命名以它要测试的类的名称开始，以 Test 结束。例如：`HashTest` 或 `HashIntegrationTest`。

接口（interface）：命名规则与类一样采用大驼峰命名法，多以 able 或 ible 结尾，如 `interface Runnable`、`interface Accessible`。

> 注意：如果项目采用 MVP，所有 Model、View、Presenter 的接口都以 I 为前缀，不加后缀，其他的接口采用上述命名规则。

#### 4.3 方法名

方法名都以 `lowerCamelCase` 风格编写。

方法名通常是动词或动词短语。

| 方法                        | 说明                                                            |
| :-------------------------- | --------------------------------------------------------------- |
| `initXX()`                  | 初始化相关方法，使用 init 为前缀标识，如初始化布局 `initView()` |
| `isXX()`, `checkXX()`       | 方法返回值为 boolean 型的请使用 is/check 为前缀标识             |
| `getXX()`                   | 返回某个值的方法，使用 get 为前缀标识                           |
| `setXX()`                   | 设置某个属性值                                                  |
| `handleXX()`, `processXX()` | 对数据进行处理的方法                                            |
| `displayXX()`, `showXX()`   | 弹出提示框和提示信息，使用 display/show 为前缀标识              |
| `updateXX()`                | 更新数据                                                        |
| `saveXX()`, `insertXX()`    | 保存或插入数据                                                  |
| `resetXX()`                 | 重置数据                                                        |
| `clearXX()`                 | 清除数据                                                        |
| `removeXX()`, `deleteXX()`  | 移除数据或者视图等，如 `removeView()`                           |
| `drawXX()`                  | 绘制数据或效果相关的，使用 draw 前缀标识                        |

#### 4.4 常量名

常量名命名模式为 `CONSTANT_CASE`，全部字母大写，用下划线分隔单词。那到底什么算是一个常量？

每个常量都是一个 `static final` 字段，但不是所有 `static final` 字段都是常量。在决定一个字段是否是一个常量时，得考虑它是否真的感觉像是一个常量。例如，如果观测任何一个该实例的状态是可变的，则它几乎肯定不会是一个常量。只是永远不打算改变的对象一般是不够的，它要真的一直不变才能将它示为常量。

```java
// Constants
static final int NUMBER = 5;
static final ImmutableListNAMES = ImmutableList.of("Ed", "Ann");
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final SetmutableCollection = new HashSet();
static final ImmutableSetmutableElements = ImmutableSet.of(mutable);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```

#### 4.5 非常量字段名

非常量字段名以 `lowerCamelCase` 风格的基础上改造为如下风格：基本结构为 `scope{Type0}VariableName{Type1}`、`type0VariableName{Type1}`、`variableName{Type1}`。

说明：`{}` 中的内容为可选。

> 注意：所有的 VO（值对象）统一采用标准的 lowerCamelCase 风格编写，所有的 DTO（数据传输对象）就按照接口文档中定义的字段名编写。

##### 4.5.1 scope（范围）

非公有，非静态字段命名以 `m` 开头(《Clean Code》不建议这么做，但是在实际开发中加上`m`开头便与识别)。

静态字段命名以 `s` 开头。

其他字段以小写字母开头。

例如：

```java
public class MyClass {
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

使用 1 个字符前缀来表示作用范围，1 个字符的前缀必须小写，前缀后面是由表意性强的一个单词或多个单词组成的名字，而且每个单词的首写字母大写，其它字母小写，这样保证了对变量名能够进行正确的断句。

##### 4.5.2 Type0（控件类型）

考虑到 Android 众多的 UI 控件，为避免控件和普通成员变量混淆以及更好地表达意思，所有用来表示控件的成员变量统一加上控件缩写作为前缀.

例如：`mIvAvatar`、`rvBooks`、`flContainer`。

##### 4.5.3 VariableName（变量名）

变量名中可能会出现量词，我们需要创建统一的量词，它们更容易理解，也更容易搜索。

例如：`mFirstBook`、`mPreBook`、`curBook`。

| 量词列表 | 量词后缀说明         |
| -------- | -------------------- |
| `First`  | 一组变量中的第一个   |
| `Last`   | 一组变量中的最后一个 |
| `Next`   | 一组变量中的下一个   |
| `Pre`    | 一组变量中的上一个   |
| `Cur`    | 一组变量中的当前变量 |

##### 4.5.4 Type1（数据类型）

对于表示集合或者数组的非常量字段名，我们可以添加后缀来增强字段的可读性，比如：

集合添加如下后缀：List、Map、Set。

数组添加如下后缀：Arr。

例如：`mIvAvatarList`、`userArr`、`firstNameSet`。

> 注意：如果数据类型不确定的话，比如表示的是很多书，那么使用其复数形式来表示也可，例如 `mBooks`。

#### 4.6 参数名

参数名以 `lowerCamelCase` 风格编写，参数应该避免用单个字符命名。

#### 4.7 局部变量名

局部变量名以 `lowerCamelCase` 风格编写，比起其它类型的名称，局部变量名可以有更为宽松的缩写。

虽然缩写更宽松，但还是要避免用单字符进行命名，除了临时变量和循环变量。

即使局部变量是 `final` 和不可改变的，也不应该把它示为常量，自然也不能用常量的规则去命名它。

#### 4.8 临时变量

临时变量通常被取名为 `i`、`j`、`k`、`m` 和 `n`，它们一般用于整型；`c`、`d`、`e`，它们一般用于字符型。 如：`for (int i = 0; i < len; i++)`。

#### 4.9 类型变量名

类型变量可用以下两种风格之一进行命名：

1. 单个的大写字母，后面可以跟一个数字（如：`E`, `T`, `X`, `T2`）。
2. 以类命名方式（参考[4.2 类名](#42-类名)），后面加个大写的 T（如：`RequestT`, `FooBarT`）。

### 5 组件开发规范

名字要见名知意，不能采用拼音或者是英汉结合的方式，尽量按照以下规则来命名。

> 这种情况就必须是中文了，比如说 alibaba，beijing 等。

类名都以 UpperCamelCase 风格编写。

类名通常是名词或名词短语，接口名称有时可能是形容词或形容词短语。现在还没有特定的规则或行之有效的约定来命名注解类型。

名词，采用大驼峰命名法，尽量避免缩写，除非该缩写是众所周知的， 比如 HTML、URL，如果类名称中包含单词缩写，则单词缩写的每个字母均应大写。

Android 基本组件指 Activity 、Fragment（不属于四大组件，但是属于基本经常使用的） 、Service 、BroadcastReceiver 、ContentProvider 等

#### 5.1 Activity

- Activity 间的数据通信，对于数据量比较大的，避免使用 Intent + Parcelable 的方式，可以考虑 EventBus 等替代方案，以免造成 TransactionTooLargeException。

- Activity#onSaveInstanceState()方法不是 Activity 生命周期方法，也不保证一定会被调用。它是用来在 Activity 被意外销毁时保存 UI 状态的，只能用于保存临时性数据，例如 UI 控件的属性等，不能跟数据的持久化存储混为一谈。持久化存储应该在 Activity#onPause()/onStop()中实行。

- Activity 间通过隐式 Intent 的跳转，在发出 Intent 之前必须通过 resolveActivity 检查，避免找不到合适的调用组件，造成 ActivityNotFoundException 的异常。

  ```
  if (getPackageManager().resolveActivity(intent, PackageManager.          MATCH_DEFAULT_ONLY) != null) {
        startActivity(intent);
  }else {
        // 找不到指定的Activity
  }
  ```

- 不要在 Activity#onDestroy()内执行释放资源的工作，例如一些工作线程的销毁和停止，因为 onDestroy()执行的时机可能较晚。可根据实际需要，在 Activity#onPause()/onStop()中结合 isFinishing()的判断来执行。

#### 5.2 Service

- 避免在 Service#onStartCommand()/onBind()方法中执行耗时操作，如果确实有需求，应改用 IntentService 或采用其他异步机制完成。

- Service 需要以多线程来并发处理多个启动请求，建议使用 IntentService，可避免各种复杂的设置。

- Service 组件一般运行主线程，应当避免耗时操作，如果有耗时操作应该在 Worker 线程执行。可以使用 IntentService 执行后台任务。

- 总是使用显式 Intent 启动或者绑定 Service，且不要为服务声明 Intent Filter，保证应用的安全性。如果确实需要使用隐式调用，则可为 Service 提供 Intent Filter 并从 Intent 中排除相应的组件名称，但必须搭配使用 Intent#setPackage()方法设置 Intent 的指定包名，这样可以充分消除目标服务的不确定性。

#### 5.3 BroadCastReceiver

- 避免在 BroadcastReceiver#onReceive()中执行耗时操作，如果有耗时工作，应该创建 IntentService 完成，而不应该在 BroadcastReceiver 内创建子线程去做。

- 由于该方法是在主线程执行，如果执行耗时操作会导致 UI 不流畅。可以使用\*\*\*\*IntentService 、创建 HandlerThread 或者调 Context#registerReceiver
  (BroadcastReceiver, IntentFilter, String, Handler)方法等方式，在其他 Wroker 线程执行 onReceive 方法。BroadcastReceiver#onReceive()方法耗时超过 10 秒钟，可能会被系统杀死。

- 避免使用隐式 Intent 广播敏感信息，信息可能被其他注册了对应 BroadcastReceiver 的 App 接收

- 如果广播仅限于应用内，则可以使用 LocalBroadcastManager#sendBroadcast()实现，避免敏感信息外泄和 Intent 拦截的风险。

* 对于只用于应用内的广播，优先使用 LocalBroadcastManager 来进行注册和发送，LocalBroadcastManager 安全性更好，同时拥有更高的运行效率。

* Activity 或者 Fragment 中动态注册 BroadCastReceiver 时，registerReceiver()和 unregisterReceiver()要成对出现。

* Android 基础组件如果使用隐式调用，应在 AndroidManifest.xml 中使用<intent-filter> 或在代码中使用 IntentFilter 增加过滤。

#### 5.4 Fragment

- 添加 Fragment 时， 确保 FragmentTransaction#commit() 在 Activity#onPostResume()或者 FragmentActivity#onResumeFragments()内调用.不要随意使用 FragmentTransaction#commitAllowingStateLoss() 来代替，任何 commitAllowingStateLoss()的使用必须经过 code review，确保无负面影响

- Activity 可能因为各种原因被销毁， Android 支持页面被销毁前通过 Activity#onSaveInstanceState() 保存自己的状态。但如果 FragmentTransaction.commit()发生 在 Activity 状态保存之后，就会导致 Activity 重建、恢复状态时无法还原页面状态，从而可能出错。为了避免给用户造成不好的体验，系统会抛出 IllegalStateExceptionStateLoss 异常。推荐的做法是在 Activity 的 onPostResume() 或 onResumeFragments() （ 对 FragmentActivity ） 里执行 FragmentTransaction.commit()，如有必要也可在 onCreate()里执行。不要随意改用 FragmentTransaction.commitAllowingStateLoss() 或者直接使用 try-catch 避免 crash，这不是问题的根本解决之道，当且仅当你确认 Activity 重建、恢复状态时，本次 commit 丢失不会造成影响时才可这么做。

- 如非必须，避免使用嵌套的 Fragment。

  - 嵌套 Fragment 是在 Android API 17 添加到 SDK 以及 Support 库中的功能，Fragment 嵌套使用会有一些坑，容易出现 bug
  - onActivityResult()方法的处理错乱，内嵌的 Fragment 可能收不到该方法的回调，需要由宿主 Fragment 进行转发处理；
  - 突变动画效果；
  - 被继承的 setRetainInstance()，导致在 Fragment 重建时多次触发不必要的逻辑。

- 当 Activity 或 Fragment 传递数据通过 Intent 或 Bundle 时，不同值的键须遵循上一条所提及到的。

- 当 Activity 或 Fragment 启动需要传递参数时，那么它需要提供一个 public static 的函数来帮助启动或创建它。

### 6 代码样式规范

1. 布局中不得不使用 ViewGroup 多重嵌套时，不要使用 LinearLayout 嵌套，改用 RelativeLayout，可以有效降低嵌套数。

   - Android 应用页面上任何一个 View 都需要经过 measure、layout、draw 三个步骤才能被正确的渲染

   - 页面拥上的 View 越多，measure、layout、draw 所花费的时间就越久。要缩短这个时间，关键是保持 View 的树形结构尽量扁平，而且要移除所有不需要渲染的 View。理想情况下，总共的 measure，layout，draw 时间应该被很好的控制在 16ms 以内

   - 要找到那些多余的 View（增加渲染延迟的 view），可以用 Android Studio Monitor 里的 Hierarchy Viewer 工具

2. 在 Activity 中显示对话框或弹出浮层时，尽量使用 DialogFragment，而非 Dialog/AlertDialog，这样便于随 Activity 生命周期管理对话框/弹出浮层的生命周期。

3. 源文件统一采用 UTF-8 的形式进行编码。

4. 禁止在非 UI 线程进行 View 相关操作。

5. 文本大小使用单位 sp，View 大小使用单位 dp。对于 TextView，如果在文字大小确定的情况下推荐使用 wrap_content 布局避免出现文字显示不全的适配问题。

6. 禁止在设计布局时多次为子 View 和父 View 设置同样背景进而造成页面过度绘制，推荐将不需要显示的布局进行及时隐藏。

7. 灵活使用布局，推荐 merge、ViewStub 来优化布局，尽可能多的减少 UI 布局层级，推荐使用 FrameLayout，LinearLayout、RelativeLayout 次之。

8. 在需要时刻刷新某一区域的组件时，建议通过以下方式避免引发全局 layout 刷新

   - 设置固定的 View 大小的宽高，如倒计时组件等；

   - 调用 View 的 layout 方法修改位置，如弹幕组件等；

   - 通过修改 Canvas 位置并且调用 invalidate(int l, int t, int r, int b)等方式限定刷新区域；

   - 通过设置一个是否允许 requestLayout 的变量，然后重写控件的 requestlayout、onSizeChanged 方法， 判断控件的大小没有改变的情况下， 当进入 requestLayout 的时候，直接返回而不调用 super 的 requestLayout 方法。

9. 不能在 Activity 没有完全显示时显示 PopupWindow 和 Dialog.

   - Android Activity 创建时的生命周期，按照 onCreate() -> onStart() -> onResume() ->onAttachedToWindow() -> onWindowFocusChanged() 的顺序， 其中在 Activity#onAttachedToWindow() 时，Activity 会与它的 Window 关联，这时 UI 才会开始绘制，在 Activity#onWindowFocusChanged() 时，UI 才变成可交互状态.

   - 如果在 Window 未关联时就创建对话框，UI 可能显示异常。推荐的做法是在 Activity#onAttachedToWindow() 之后（ 其实最好是 Activity#onWindowFocusChanged() 之后）才创建对话框。

10. 尽量不要使用 AnimationDrawable，它在初始化的时候就将所有图片加载到内存中，特别占内存，并且还不能释放，释放之后下次进入再次加载时会报错。但是数量少的 drawable 还是可以接受的。
11. 不能使用 ScrollView 包裹 ListView/GridView/ExpandableListVIew;因为这样会把 ListView 的所有 Item 都加载到内存中，要消耗巨大的内存和 cpu 去绘制图面

    - 推荐使用 NestedScrollView

12. 不要在 Android 的 Application 对象中缓存数据。基础组件之间的数据共享请使用 Intent 等机制，也可使用 SharedPreferences 等数据持久化机制。

13. 使用 Toast 时，建议定义一个全局的 Toast 对象，这样可以避免连续显示 Toast 时不能取消上一次 Toast 消息的情况。即使需要连续弹出 Toast，也应避免直接调用 Toast#makeText。

14. 使用 Adapter 的时候，如果你使用了 ViewHolder 做缓存，在 getView()的方法中无论这项 convertView 的每个子控件是否需要设置属性(比如某个 TextView 设置的文本可能为 null，某个按钮的背景色为透明，某控件的颜色为透明等)，都需要为其显式设置属性(Textview 的文本为空也需要设置 setText(null)，背景透明也需要设置)，否则在滑动的过程中，因为 adapter item 复用的原因，会出现内容的显示错乱。

### 7 提交日志规范

| 上传日志功能 | 具体操作                      | 举例                                       |
| ------------ | ----------------------------- | ------------------------------------------ |
| 新增功能     | `[版本][new feature][新增……]` | [1.0.0][new feature][新增登录功能]         |
| 优化功能     | `[版本][refactor][优化……]`    | [1.0.0][refactor][优化登录功能]            |
| 修复 Bug     | `[版本][bug fix][修复……Bug]`  | [1.0.0][bug fix][修复登录失败不提示的 Bug] |

### 8 注释规范

良好的代码是不需要有逻辑注释的，在代码中注释是垃圾，业务代码或者是复杂代码可以有一定的注释，具体请查看我的另外一篇文章原文地址和源码：**[代码整洁规范通用版][clean code(gitbook)]** 和 **[源码][clean code源码]**

### 9 三方库推荐使用

优秀的三方库推荐

- **[Retrofit][retrofit]** 网络请求
- **[OkHttp][okhttp]** 网络请求
- **[RxAndroid][rxandroid]**
- **[Glide][glide]**/**[Fresco][fresco]** 图片加载
- **[Gson][gson]**/**[Fastjson][fastjson]** 数据解析
- **[EventBus][eventbus]**/**[AndroidEventBus][androideventbus]** 事件传递
- **[GreenDao][greendao]**/**[LitePal][litepal]** Sqlite 操作
- **[PermissionsDispatcher][permissionsdispatcher]**/**[RxPermissions][rxpermissions]** 权限申请库
- **[Dagger2][dagger2]**（选用）
- **[Tinker][tinker]**（选用）

### 10 常见英文单词缩写

其实我是不建议缩写的，如果不是太长建议还是不缩写单词

| 名称                 | 缩写 |
| -------------------- | ---- |
| average              | avg  |
| background           | bg   |
| buffer               | buf  |
| control              | ctrl |
| current              | cur  |
| default              | def  |
| delete               | del  |
| document             | doc  |
| error                | err  |
| escape               | esc  |
| icon                 | ic   |
| increment            | inc  |
| information          | info |
| initial              | init |
| image                | img  |
| Internationalization | I18N |
| length               | len  |
| library              | lib  |
| message              | msg  |
| password             | pwd  |
| position             | pos  |
| previous             | pre  |
| server               | srv  |
| string               | str  |
| temporary            | tmp  |
| window               | win  |

程序中使用单词缩写原则：不要用缩写，除非该缩写是约定俗成的。

## 参考

[阿里巴巴 Java 开发手册][阿里巴巴 java 开发手册]

[Github Android Standard Develop 项目][android standard develop]

[分包示例图]: https://raw.githubusercontent.com/cuibg/AndroidDevelopmentSpecification/master/image/res_sub_constract.png
[clean code(gitbook)]: https://cuibg.gitbooks.io/cleancode/content/
[clean code源码]: https://github.com/cuibg/CleanCode
[阿里巴巴 java 开发手册]: https://github.com/alibaba/p3c/blob/master/
[android standard develop]: https://github.com/Blankj/AndroidStandardDevelop
[package by features, not layers]: https://medium.com/hackernoon/package-by-features-not-layers-2d076df1964d#.mp782izhh
[todo-mvp]: https://github.com/android/architecture-samples/tree/todo-mvp
[retrofit]: https://github.com/square/retrofit
[rxandroid]: https://github.com/ReactiveX/RxAndroid
[okhttp]: https://github.com/square/okhttp
[glide]: https://github.com/bumptech/glide
[fresco]: https://github.com/facebook/fresco
[gson]: https://github.com/google/gson
[fastjson]: https://github.com/alibaba/fastjson
[eventbus]: https://github.com/greenrobot/EventBus
[androideventbus]: https://github.com/bboyfeiyu/AndroidEventBus
[greendao]: https://github.com/greenrobot/greenDAO
[litepal]: https://github.com/guolindev/LitePal
[permissionsdispatcher]: https://github.com/permissions-dispatcher/PermissionsDispatcher
[rxpermissions]: https://github.com/tbruyelle/RxPermissions
[dagger2]: https://github.com/google/dagger
[tinker]: https://github.com/Tencent/tinker
