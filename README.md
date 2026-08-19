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


---

## 在线播放功能

管理后台现在支持两种播放方式，二选一（也可以都不填，只留跳转链接）：

**方式一：上传本地音频文件**
在"上传本地音频文件"那一栏选择电脑上的 mp3/m4a/wav 文件，保存歌曲时会自动上传到仓库的 `audio/` 文件夹里，主页会出现一个可以直接点开听的播放条。

- 单个文件建议不超过 18MB（一首普通音质的 mp3 一般 3-8MB，完全够用）
- 如果不是自己拥有版权的歌曲，公开托管音频文件存在一定版权风险，请自行斟酌
- 编辑歌曲时如果不重新选择文件，会保留原来的音频；重新选择则会替换

**方式二：内嵌外部平台播放器**
在"在线播放嵌入链接"那一栏填入外部平台的嵌入式播放器地址，网页里会出现一个▶按钮，点开就能听，不需要跳转。

获取嵌入链接的方法：
- 网易云音乐：打开歌曲页 →「分享」→「生成外链播放器」→ 复制生成代码里 `<iframe>` 的 `src="..."` 地址
- QQ音乐：同理找「生成外链播放器」
- YouTube：分享 →「嵌入」→ 复制代码里 `<iframe>` 的 `src="..."` 地址
- Spotify：分享 →「嵌入」→ 同理取 `src`

两种方式都填了的话，页面优先播放本地音频文件。

## 自定义

- 想改标题、开场文案：直接编辑 `index.html` 里 `<h1 class="hero__title">` 附近的文字
- 想改配色：编辑 `css/style.css` 顶部 `:root` 里的颜色变量
- 歌曲字段说明（`data/songs.json`）：

  | 字段 | 说明 | 是否必填 |
  |---|---|---|
  | title | 歌名 | 必填 |
  | artist | 歌手 | 选填 |
  | cover | 封面图片链接 | 选填，留空会显示默认图标 |
  | url | 点圆形箭头图标后跳转的外部链接 | 选填 |
  | embed | 外部平台的嵌入式播放器地址，用于网页内直接播放 | 选填 |
  | audioFile | 上传到仓库的本地音频文件路径，后台自动填 | 自动生成 |
  | embed | 网页内直接播放用的嵌入链接，填了会出现▶按钮 | 选填 |
  | tag | 标签，如"深夜循环" | 选填 |
  | note | 一句话备注 | 选填 |
  | addedAt | 添加日期，后台会自动填 | 自动生成 |

### 怎么获取"在线播放"的嵌入链接

在管理后台的"在线播放嵌入链接"这一栏，填入下面对应平台生成的地址：

**网易云音乐**
1. 打开想要的歌曲页面（网页版 music.163.com）
2. 点"分享" → "生成外链播放器"
3. 会得到一段 `<iframe>` 代码，把里面 `src="..."` 引号中的那一串网址复制出来，粘贴到后台即可
4. 格式类似：`https://music.163.com/outchain/player?type=2&id=歌曲ID&auto=0&height=66`

**QQ音乐**
1. 打开歌曲页面，找到"分享"里的"生成外链播放器"
2. 同样只需要复制 iframe 的 `src` 地址

**YouTube**
1. 打开视频，点"分享" → "嵌入"
2. 复制那段代码里 `src="https://www.youtube.com/embed/视频ID"` 的地址

**Spotify**
1. 打开歌曲，点"⋯" → "分享" → "嵌入"
2. 复制 `src="https://open.spotify.com/embed/track/..."` 的地址

> 只要是能生成 `<iframe src="...">` 的音乐/视频平台都适用，只需要把 src 里的网址复制到后台的"嵌入链接"字段即可。不同来源的歌曲可以混用，互不影响。

如果不想用管理后台，也可以直接在 GitHub 网页上编辑 `data/songs.json` 这个文件（点文件 → 铅笔图标 → 直接改 JSON → Commit），效果是一样的。
