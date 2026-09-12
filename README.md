# 领券干饭小组

一个外卖领券微信小程序：聚合美团、饿了么的外卖红包券，不知道吃什么的时候还可以随机抽一个。基于 **uni-app（Vue 3）+ uniCloud（阿里云）** 开发，通过 clientDB/JQL 直连云数据库。

产品已在微信小程序上线，可在微信中搜索「领券干饭小组」访问（**注：后端服务现已停用，应用仍可访问，但无数据加载**）。

![海报图](https://cziemihwekwsjimvxnwj.supabase.co/storage/v1/object/public/main/images/coupon_shot.jpeg)

## 功能

### 领券首页（pages/index/index）

- 美团 / 饿了么双 Tab，展示进行中的领券活动海报，按 `sort` 倒序排列，仅展示已开启（`status != 0`）且未过期（`end_date` 晚于当前时间）的活动
- 点击活动海报领取红包，有两种形式：
  - 配置了 `app_id` 的活动：直接跳转到对应平台小程序领券
  - 配置了 `token` 的活动：弹出底部弹窗展示口令，一键复制后手动打开对应平台 App 领取
- 温馨提示栏说明红包领取规则（新老用户每天均可领一次、红包金额随机等）
- 功能区：随机选餐入口、「看我家猫猫」照片轮播（点击可预览大图）
- 支持分享给好友、分享到朋友圈

### 随机选餐（pages/random-food/random-food）

- 不知道吃什么时，从食物库（`co-foods`）随机抽样 100 条
- 点击「选一个」触发摇奖机动画，名字滚动 2.5～6.5 秒后随机定格
- 附带短震动反馈（`uni.vibrateShort`）

## 技术栈

| 项       | 说明                                                                                                            |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| 框架     | uni-app，Vue 3（见 `manifest.json`）                                                                            |
| 后端     | uniCloud 阿里云版，前端直连数据库（clientDB / JQL），无云函数业务代码                                           |
| 目标平台 | 微信小程序（`mp-weixin`，使用了 `uni.navigateToMiniProgram`、朋友圈分享等小程序能力）                           |
| 静态资源 | logo / 图标在 `static/`；海报、猫图等大图托管在 uniCloud CDN（地址配置在 `App.vue` 的 `globalData.uniBaseUrl`） |

## 目录结构

```text
├── App.vue                    # 全局配置：systemInfo、静态资源 CDN 地址
├── main.js                    # 入口
├── pages.json                 # 页面路由
├── pages/
│   ├── index/index.vue        # 领券首页
│   └── random-food/random-food.vue  # 随机选餐
├── components/
│   ├── func-area/             # 首页功能区（选餐入口、猫猫轮播）
│   ├── nav-bar/               # 自定义导航栏
│   ├── tab/                   # 美团 / 饿了么切换 Tab
│   ├── tip-bar/               # 温馨提示
│   ├── popup/                 # 口令复制弹窗
│   ├── load-view/             # 加载动画
│   └── safe-area/             # 安全区占位
├── uniCloud-aliyun/
│   └── database/              # DB Schema：co-events、co-foods
└── static/                    # 本地静态资源（logo、图标）
```

## 数据集合

### co-events（领券活动）

| 字段          | 类型   | 说明                                           |
| ------------- | ------ | ---------------------------------------------- |
| platform      | string | 平台：`meituan` / `eleme`                      |
| description   | string | 活动描述                                       |
| rule          | string | 活动规则                                       |
| commission    | string | 佣金信息                                       |
| end_date      | string | 截止日期，过期不再展示                         |
| poster_path   | string | 海报图地址                                     |
| app_id        | string | 跳转目标小程序 appId（配置后点击直接跳小程序） |
| navigate_path | string | 跳转目标小程序的页面路径                       |
| token         | string | 复制口令（配置后点击弹窗复制口令）             |
| sort          | int    | 排序权重，倒序                                 |
| status        | int    | 0 关闭 / 1 开启                                |

两条互斥：`app_id` 优先跳小程序，否则有 `token` 时弹口令弹窗。

### co-foods（食物库）

| 字段 | 类型   | 说明     |
| ---- | ------ | -------- |
| name | string | 食物名称 |

两个集合的权限均为只读（`read: true`，增删改 `false`），数据通过 uniCloud 控制台或 `JQL查询.jql` 维护。

## 快速开始

1. 使用 [HBuilderX](https://www.dcloud.io/hbuilderx.html) 导入本项目
2. 右键 `uniCloud-aliyun` 目录，关联自己的 uniCloud 服务空间（阿里云）
3. 右键 `uniCloud-aliyun/database`，上传全部 DB Schema
4. 在 uniCloud 控制台（或 `JQL查询.jql`）向 `co-events`、`co-foods` 添加数据
5. 如需发布，将 `manifest.json` 中 `mp-weixin.appid` 换成自己的微信小程序 appId
6. 运行 → 运行到小程序模拟器 → 微信开发者工具
