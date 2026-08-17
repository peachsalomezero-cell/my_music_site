# 我的歌单网站

一个零编程基础也能上线的个人歌单网站：
- `index.html` — 公开的歌单展示页，任何人访问你的域名都能看到
- `admin.html` — 只有你自己用的管理后台，可以在网页上直接**添加 / 编辑 / 删除**歌曲
- `data/songs.json` — 存放歌曲数据的文件，管理后台会通过 GitHub 自动更新它

不需要服务器、不需要数据库，数据就存在你的 GitHub 仓库里。

---

## 第一步：新建 GitHub 仓库并上传文件

1. 登录 [github.com](https://github.com)，点右上角 `+` → `New repository`
2. 仓库名随便取，比如 `my-music-site`，设为 **Public**，创建
3. 把这个文件夹里的所有文件（`index.html`、`admin.html`、`css/`、`data/`）上传上去
   - 最简单的办法：打开仓库页面 → `Add file` → `Upload files`，把文件拖进去 → `Commit changes`

## 第二步：开启 GitHub Pages

1. 进入仓库的 `Settings` → 左侧 `Pages`
2. `Source` 选择 `Deploy from a branch`，分支选 `main`，目录选 `/ (root)`，保存
3. 等 1-2 分钟，页面顶部会出现一个网址，形如：
   `https://你的用户名.github.io/my-music-site/`
   打开它就能看到你的歌单主页了

## 第三步：生成一个 GitHub Token（用于后台管理）

管理后台需要一个"令牌"才能帮你修改仓库里的文件，操作如下：

1. 访问 [github.com/settings/tokens?type=beta](https://github.com/settings/tokens?type=beta)
2. 点 `Generate new token`
3. **Repository access** 选择 `Only select repositories`，选中你刚建的仓库（重要：不要选"所有仓库"，把权限限制到最小）
4. **Permissions** → 展开 `Repository permissions` → 找到 `Contents`，设置为 `Read and write`
5. 生成后会显示一串以 `github_pat_` 开头的字符串，**立刻复制保存好**（关闭页面后就再也看不到了）

> ⚠️ 这个 Token 相当于你仓库的钥匙，不要发给别人、不要贴到公开的地方。如果不小心泄露了，回到这个页面把对应 Token 删除（Revoke）即可。

## 第四步：使用管理后台

1. 打开 `https://你的用户名.github.io/my-music-site/admin.html`
2. 填入：GitHub 用户名、仓库名、分支（默认 `main`）、数据文件路径（默认 `data/songs.json`）、以及第三步生成的 Token
3. 点"连接仓库"
4. 之后就能在网页上添加、编辑、删除歌曲了，每次保存都会自动提交到你的 GitHub 仓库，主页会实时更新

> 💡 `admin.html` 这个链接本身不需要密码就能打开页面，但没有正确的 Token 就无法修改数据 —— 所以请不要把这个后台链接分享出去，也建议不要在公用电脑上使用。

## 第五步（可选）：绑定自己的域名

1. 在阿里云 / 腾讯云 / Namecheap / GoDaddy 等平台购买一个域名
2. 在域名的 DNS 解析设置里：
   - 如果用二级域名（如 `music.yourdomain.com`）：添加一条 **CNAME** 记录，指向 `你的用户名.github.io`
   - 如果用根域名（如 `yourdomain.com`）：添加 4 条 **A** 记录，分别指向：
     `185.199.108.153` `185.199.109.153` `185.199.110.153` `185.199.111.153`
3. 回到 GitHub 仓库 `Settings → Pages`，在 `Custom domain` 填入你的域名并保存
4. 勾选 `Enforce HTTPS`（DNS 生效后才能勾选，可能需要等待几分钟到几小时）

---

## 自定义

- 想改标题、开场文案：直接编辑 `index.html` 里 `<h1 class="hero__title">` 附近的文字
- 想改配色：编辑 `css/style.css` 顶部 `:root` 里的颜色变量
- 歌曲字段说明（`data/songs.json`）：

  | 字段 | 说明 | 是否必填 |
  |---|---|---|
  | title | 歌名 | 必填 |
  | artist | 歌手 | 选填 |
  | cover | 封面图片链接 | 选填，留空会显示默认图标 |
  | url | 点击后跳转的播放链接（网易云/Spotify/YouTube等） | 选填 |
  | tag | 标签，如"深夜循环" | 选填 |
  | note | 一句话备注 | 选填 |
  | addedAt | 添加日期，后台会自动填 | 自动生成 |

如果不想用管理后台，也可以直接在 GitHub 网页上编辑 `data/songs.json` 这个文件（点文件 → 铅笔图标 → 直接改 JSON → Commit），效果是一样的。
