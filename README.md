# 安装并配置Git使用智能体推送代码到GitHub

# 目标：本地 Git 绑定 GitHub 账号，直接推送代码（可以用豆包生成代码，一键推到 GitHub）

> 整体流程：安装 Git → 配置 GitHub 身份 → 配置 SSH 密钥（推荐，免密码）→ 新建 / 拉取仓库 → 豆包生成代码 → git 提交 + 推送到 GitHub

## 一、安装 Git

1. 下载 Git：[https://git-scm.com/downloads](https://git-scm.com/downloads)
2. 安装全部默认选项即可，安装完成后打开 **Git Bash**（Windows）/ 终端（Mac）
3. 验证安装

```Plain Text
git --version
```

## 二、配置 Git 全局账号（绑定你的 GitHub 用户名和邮箱）

> 这里邮箱必须是你 GitHub 账号绑定的邮箱！

```Plain Text
# 设置用户名（你的GitHub昵称）
git config --global user.name "springxian163"
# 设置邮箱（GitHub账号邮箱）
git config --global user.email "spring_xian@163.com"
```

查看配置是否成功：

```Plain Text
git config --global --list
```

## 三、【推荐】SSH 密钥配置，推送代码不用每次输账号密码

> HTTPS 方式每次推送要输 token，SSH 一次性配置好更方便

1. 生成 ssh 密钥（一路回车，不要设置密码也行）

```Plain Text
ssh-keygen -t ed25519 -C "spring_xian@163.com"
```

```Plain Text
#提示
$ ssh-keygen -t ed25519 -C "spring_xian@163.com" 
Generating public/private ed25519 key pair. 
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_ed25519):
```

意思：**询问你密钥文件保存路径** 默认路径：`/c/Users/Administrator/.ssh/id_ed25519` ✅ **直接回车，使用默认路径即可，不要输入任何文字**

回车之后，下一行提示：

```Plain Text
Enter passphrase (empty for no passphrase):
```

> 这是私钥密码（可选）

- 直接回车：**不设置私钥密码**，后续 push 代码不用额外输密码（推荐新手）
- 输入密码：每次使用这个密钥推送代码时，都要输入这个密码（安全性更高）

再回车确认密码：

```Plain Text
Enter same passphrase again:
```

同样直接回车（不设密码的情况下）

## 全部完成后，查看公钥

```Plain Text
cat ~/.ssh/id_ed25519.pub
```

复制输出全部内容（以 `ssh-ed25519` 开头，末尾是你的邮箱 spring_xian@163.com），粘贴到 GitHub 的 SSH keys 里面。

## 测试连通

```Plain Text
ssh -T git@github.com
```

```Plain Text
$ ssh -T git@github.com 
The authenticity of host 'github.com (20.205.243.166)' 
can't be established. ED25519 key fingerprint 
is: SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU 
This key is not known by any other names. 
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

直接输入：

```Plain Text
yes
```

然后回车。

### 解释

这是首次连接 [github.com](https://github.com/) 的安全确认：Git 询问你是否信任这个服务器指纹，**不是报错**。

看到 `Hi xxx! You've successfully authenticated` 代表成功。
