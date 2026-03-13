# Firebase 配置指南

本指南将帮助你配置 Firebase，使游戏能够将所有学生的成绩保存到云端，实现实时排行榜功能。

## 📋 前置要求

- Google 账号（免费）
- 游戏已部署到 GitHub Pages

## 🚀 配置步骤

### 步骤 1：创建 Firebase 项目

1. 访问 [Firebase Console](https://console.firebase.google.com/)
2. 使用 Google 账号登录
3. 点击 **"创建项目"**
4. 填写项目信息：
   - **项目名称**：`organization-sociology-game`（或自定义名称）
   - **Google Analytics**：暂时不需要，可以取消勾选
5. 点击 **"创建项目"**
6. 等待项目创建完成（约 30 秒）

### 步骤 2：创建 Firestore 数据库

1. 在 Firebase 控制台左侧菜单，找到 **"Firestore Database"**
2. 点击 **"创建数据库"**
3. 选择 **"以测试模式启动"**（生产环境需要设置安全规则）
4. 选择数据库位置（推荐默认的 `nam5 (us-central)` 或选择 `asia-east1` 如果在中国）
5. 点击 **"启用"**

### 步骤 3：设置数据库规则

1. 在 Firestore → **规则** 页面
2. 删除默认规则，粘贴以下内容：

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;
      allow write: if request.time < timestamp.date(2025, 1, 1);
    }
  }
}
```

3. 点击 **"发布"**

> ⚠️ **注意**：这是测试规则，允许在 2025 年 1 月 1 日前读写。如需长期使用，请设置更严格的规则。

### 步骤 4：获取 Firebase 配置信息

1. 在项目概览页面，点击 **"Web"** 图标（</> 符号）
2. 给应用起个名字（如：`game`）
3. 复制配置信息，格式如下：

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  authDomain: "project-id.firebaseapp.com",
  projectId: "project-id",
  storageBucket: "project-id.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef123456"
};
```

**字段说明**：
- `apiKey`：以 `AIzaSy` 开头
- `authDomain`：格式为 `项目ID.firebaseapp.com`
- `projectId`：你的 Firebase 项目 ID
- `storageBucket`：格式为 `项目ID.appspot.com`
- `messagingSenderId`：纯数字
- `appId`：以 `1:` 开头，包含 `web:` 字符串

### 步骤 5：在游戏中配置 Firebase

1. 打开游戏网页
2. 点击排行榜旁边的 **"⚙️ 配置云端"** 按钮
3. 将复制的配置信息填入对应字段
4. 点击 **"✅ 保存配置"**
5. 等待看到 **"✅ Firebase 配置成功"** 提示

### 步骤 6：部署更新后的游戏

1. 将更新后的 `index.html` 提交到 GitHub
2. 推送到 GitHub
3. GitHub Pages 会自动部署新版本

## 📊 数据查看方式

### 方法 1：在游戏中查看

配置 Firebase 后，所有学生都能看到实时排行榜。

### 方法 2：在 Firebase Console 查看

1. 打开 Firebase Console
2. 进入 **Firestore Database**
3. 点击 **"scores"** 集合
4. 可以看到所有学生的成绩记录

### 方法 3：导出数据

1. 在 Firestore Database 中
2. 选择数据或表
3. 点击 **"导出数据"**（三个点菜单）
4. 可以导出为 JSON 格式

## 🔒 安全建议（可选）

如果用于正式课程，建议设置更严格的规则：

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /scores/{scoreId} {
      allow read: if true;  // 所有人可读
      allow write: if request.auth != null;  // 只有认证用户可写
    }
  }
}
```

然后启用 Firebase Authentication（Google 登录）。

## ⚠️ 重要提示

1. **免费额度**：
   - Firestore Spark 计划：每月 50,000 次读取，20,000 次写入
   - 存储：1GB
   - 对于小课程来说完全够用

2. **隐私**：
   - 学生数据存储在 Google 服务器上
   - 确保学生知道他们的成绩会被公开

3. **备份**：
   - 建议定期导出 Firestore 数据作为备份

## 🆘 常见问题

### Q: 配置失败怎么办？
A: 检查以下几点：
1. API 密钥是否正确复制
2. 项目 ID 和 Auth Domain 是否匹配
3. Firestore 数据库是否已创建

### Q: 排行榜不更新？
A: 
1. 刷新页面
2. 检查网络连接
3. 查看浏览器控制台是否有错误

### Q: 可以重置数据吗？
A: 可以：
1. 在 Firebase Console 中手动删除记录
2. 或在游戏中点击"清空本地"

### Q: 需要付费吗？
A: 不需要。免费计划足够几十个学生使用。

## 📞 获取帮助

如果遇到问题：
1. 查看 Firebase 官方文档：https://firebase.google.com/docs
2. 检查浏览器控制台的错误信息
3. 尝试重新配置 Firebase

---

祝你配置成功！🎉
