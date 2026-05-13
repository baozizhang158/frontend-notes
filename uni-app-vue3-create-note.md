# Git 实习标准操作（完整版）

## 一、克隆项目（入职第一天）

```bash
一定得克隆，不能直接下载文件，否则没有关联，无法提交代码。
git clone 公司仓库地址.git
cd 项目名
pnpm install
code . 用vscode打开项目
```

## 二、每天上班（继续昨天的任务）

```bash
git checkout main
git pull
git checkout feature/你的分支名
```

## 三、开始一个新任务

```bash
git checkout main
git pull
git checkout -b feature/新任务分支名
```

## 四、写代码、提交、推送

```bash
git add .
git commit -m "feat: 完成了xxx功能"
git push origin feature/你的分支名
```

提交信息类型：`feat`(新功能)、`fix`(修bug)、`style`(样式)、`refactor`(重构)、`docs`(文档)

## 五、发起合并请求

去 GitHub/Gitee 网页，找到你的分支，点击「发起合并请求」，指定同事 review。

## 六、合并完成后，删除本地分支

```bash
git checkout main
git pull
git branch -d feature/你的分支名
```

## uni-app + Vue3 项目搭建笔记

#### 切换路径到当前文件夹：`cd C:\Users\Administrator\Desktop\前端\笔记`

### 删除不小心创建的.git文件夹：

- `rmdir .git` → 删除空文件夹
- `rmdir /s .git` → 强制删除非空文件夹（包括里面所有内容）
- 输入 `y` 确认删除

---

### 一、环境准备

| 工具           | 检查命令     | 说明                                         |
| -------------- | ------------ | -------------------------------------------- |
| Node.js        | `node -v`    | 需要 18+ 或 20+                              |
| pnpm           | `pnpm -v`    | 包管理器，比 npm 快                          |
| 微信开发者工具 | 官网下载安装 | 必须开启：**设置 → 安全设置 → 开启服务端口** |

---

### 二、创建项目

#### Gitee（国内服务器）

`npx degit gitee:dcloud/uni-preset-vue#vite-ts my-project`

## uni-app 官方模板分支对照表

| 模板类型                  | 分支名           | 命令                                               |
| ------------------------- | ---------------- | -------------------------------------------------- |
| Vue3 + TypeScript（推荐） | `vite-ts`        | `npx degit dcloudio/uni-preset-vue#vite-ts 项目名` |
| Vue3 + JavaScript         | `vite`           | `npx degit dcloudio/uni-preset-vue#vite 项目名`    |
| Vue2 + JavaScript         | 默认（不加 `#`） | `npx degit dcloudio/uni-preset-vue 项目名`         |
| Vue2 + TypeScript         | `ts`             | `npx degit dcloudio/uni-preset-vue#ts 项目名`      |

```bash
# 从官方模板创建 Vue3 + TS 项目

`npx degit dcloudio/uni-preset-vue#vite-ts 项目名`
#项目名要求小写加短斜杠

# 进入目录
cd my-uni-app

# 安装依赖
pnpm install
```

---

### 三、项目结构（重点文件）

```
my-uni-app/
├── src/
│   ├── pages/           # 页面文件（主要开发目录）
│   ├── components/      # 公共组件
│   ├── store/           # Pinia 状态管理
│   ├── utils/           # 工具函数
│   ├── App.vue          # 根组件
│   ├── main.ts          # 入口文件
│   └── pages.json       # 路由配置（新增页面需注册）
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

---

### 四、核心文件说明

#### `src/main.ts`

```ts
import { createSSRApp } from "vue";
import App from "./App.vue";

export function createApp() {
  const app = createSSRApp(App);
  return { app };
}
```

> uni-app 标准入口，和普通 Vue 的 `createApp(App).mount('#app')` 不同。

#### `src/pages.json`（路由配置）

```json
{
  "pages": [
    {
      "path": "pages/index/index",
      "style": { "navigationBarTitleText": "首页" }
    }
  ],
  "globalStyle": {
    "navigationBarTextStyle": "black",
    "navigationBarTitleText": "我的应用",
    "navigationBarBackgroundColor": "#F8F8F8"
  }
}
```

#### `src/App.vue`

```vue
<script setup lang="ts">
import { onLaunch, onShow, onHide } from "@dcloudio/uni-app";

onLaunch(() => {
  console.log("App Launch");
});
onShow(() => {
  console.log("App Show");
});
onHide(() => {
  console.log("App Hide");
});
</script>
```

> 这里的生命周期是小程序级别的，不是 Vue 的 `onMounted`。

---

### 五、H5 练习 → uni-app 写法对照表

| H5 练习        | uni-app 写法         | 原因             |
| -------------- | -------------------- | ---------------- |
| `div`          | `view`               | uni-app 内置组件 |
| `span`         | `text`               | 同上             |
| `img`          | `image`              | 同上             |
| `@click`       | `@tap`               | 移动端事件       |
| `px`           | `rpx`                | 自适应单位       |
| `localStorage` | `uni.setStorageSync` | 跨端 API         |
| `ul/li`        | `view` + `v-for`     | 无列表语义标签   |

---

### 七、运行到微信开发者工具

```bash
# 编译微信小程序
pnpm run dev:mp-weixin
```

编译完成后，根目录下生成 `dist/dev/mp-weixin` 文件夹。

**微信开发者工具操作：**

- 打开工具 → **导入项目**
- 目录选择 `dist/dev/mp-weixin`
- AppID 使用测试号或自己的

> 热更新：VSCode 改代码保存后自动重新编译，微信开发者工具自动刷新。

---

### 八、VS Code 插件推荐

| 插件                   | 作用                   |
| ---------------------- | ---------------------- |
| Vue - Official (Volar) | Vue 3 语法支持（必装） |
| uni-create-view        | 快速新建 uni-app 页面  |
| uni-helper             | uni-app 代码提示       |
| TypeScript Vue Plugin  | TS 支持                |

---

### 九、常用命令速查

| 命令                       | 作用             |
| -------------------------- | ---------------- |
| `pnpm run dev:mp-weixin`   | 运行到微信小程序 |
| `pnpm run dev:h5`          | 运行到 H5        |
| `pnpm run build:mp-weixin` | 打包小程序       |
| `pnpm run build:h5`        | 打包 H5          |

---

---

**个人学习笔记推荐做法：**

```bash
# 创建笔记仓库
mkdir frontend-notes
cd frontend-notes
git init

# 添加笔记文件 先在本地创建git仓库
git add .  把文件放到暂存区
git commit -m "first commit"  把暂存区里的文件确认打包，给一个版本说明 first commit。执行完这一步后，代码才真正记录到本地 Git 仓库
git commit -m "docs: 初始化前端笔记"

# 关联 GitHub 远程仓库（先网页上建好空仓库）https://github.com/new
git remote add origin https://github.com/baozizhang158/frontend-notes.git
git push -u origin main
本地默认名字是master
git push -u origin master:main 意思是：把你的本地 master 分支推送到远程 main 分支
```

#### 以后上传三步走

```bash
git add .
git commit -m "更新笔记"
git push

(git push -u origin master:main
-u 会记住关联，以后直接 git push 就行。)

```

## 工作中用git

### 克隆项目 入职第一天：克隆项目（只一次）

`git clone https://github.com/公司仓库地址.git`
cd 项目文件夹

### 1. 每天上班：拉取最新代码（避免冲突）

`git pull`

### 2. 切换到自己的开发分支（比如 feature-login）

`git checkout -b feature-login`

### 3. 写代码... 改完文件后

### 4. 添加

`git add .`

### 5. 提交

`git commit -m "feat: 完成登录功能"`

## Git 提交备注规范（精简版）

一定空格！
| 类型 | 含义 | 示例 |
| ------- | ------------- | ------------------------------ |
| `docs` | 文档/笔记更新 | `docs: 补充 uni-app 搭建步骤` |
| `fix` | 修复错误 | `fix: 修改笔记里的命令写错` |
| `feat` | 新增内容 | `feat: 添加 Vue3 生命周期笔记` |
| `style` | 格式调整 | `style: 调整表格排版` |

### 6. 推送远程（第一次要用 -u）

`git push -u origin feature-login`

# Git 冲突解决步骤

## 一、冲突发生的场景

执行 `git pull` 或 `git merge` 时，两人改了同一文件的同一行。

## 二、解决流程

### 1. 查看冲突文件

```bash
git status
# 冲突文件会显示 "both modified" 或 "unmerged"
```

### 2. 打开冲突文件

文件内会有冲突标记：

```
<<<<<<< HEAD
你的代码
=======
别人的代码
>>>>>>> branch-name
```

### 3. 手动解决冲突

| 情况         | 做法                           |
| ------------ | ------------------------------ |
| 要你的代码   | 删掉 `=======` 和别人的代码    |
| 要别人的代码 | 删掉 `<<<<<<< HEAD` 和你的代码 |
| 都要         | 删掉标记，合并两段代码         |

### 4. 标记已解决

```bash
git add <冲突文件名>
```

### 5. 完成合并

```bash
git commit -m "解决冲突"
```

## 三、放弃合并（回到合并前）

```bash
git merge --abort
```

## 四、VSCode 快捷操作

- 点击 `Accept Current Change`（保留你的）
- 点击 `Accept Incoming Change`（保留别人的）

## 五、避免冲突的建议

- 经常 `git pull`，减少差异积累
- 改代码前先拉最新
- 分工明确，少改同一文件
- 提交前再拉一次
