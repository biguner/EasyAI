诸如：git clone git@github.com:biguner/EasyAI.git

你在使用 SSH 方式克隆 GitHub 仓库时遇到了 Permission denied (publickey) 错误，这是因为你的本地 SSH 密钥没有被 GitHub 账户授权。以下是解决步骤：

1. 在机器上生成新的 SSH 密钥，必须使用github上的标识 （文件名建议包含机器标识，方便日后管理，比如 github_machineB，我这里是ed25519_github_lzzm_2GPU）
```bash
ssh-keygen -t ed25519 -C "liuzhehrc@qq.com" -f ~/.ssh/ed25519_github_lzzm_2GPU
```

2. 将公钥添加到 GitHub
复制公钥内容：
```bash
cat ~/.ssh/ed25519_github_lzzm_2GPU.pub
```
登录 GitHub → Settings → SSH and GPG keys → New SSH key，粘贴并保存。

3. （可选）配置 SSH 使用该密钥
编辑 ~/.ssh/config，添加：

```text
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/ed25519_github_lzzm_2GPU
    IdentitiesOnly yes
```
这样机器Lzzm_2GPU 连接 GitHub 时会自动使用这个密钥。

4. 测试连接
```bash
lz@lzzm-2GPU:~/.ssh$ ssh -T git@github.com
Hi biguner! You've successfully authenticated, but GitHub does not provide shell access.

```
看到成功信息即可。


> 管理多个机器的密钥小技巧
给密钥文件命名时加入机器名或用途，比如 github_work_laptop、github_home_pc。
>
>可以在 ~/.ssh/config 中为不同主机（或同一主机）指定不同密钥，方便管理。

> 定期检查 GitHub 账户中的公钥列表，及时删除不再使用的机器对应的公钥。

> 总结
虽然共用同一个密钥看起来很省事，但从长远来看，每台机器使用独立密钥能避免很多潜在的安全风险，而且管理起来也并不复杂。推荐你采用每台机器单独生成密钥的方式。

---

每台机器使用不同的密钥
优点：

安全性更高：每台机器的密钥独立，即使某台机器的私钥泄露，只需在 GitHub 上删除对应的公钥即可，不影响其他机器。

细粒度管理：可以针对不同机器单独添加/删除公钥，灵活控制访问。

每台机器的密钥可以设置不同的密码短语（passphrase），进一步增加安全性。

缺点：需要在 GitHub 账户中添加多个公钥，管理上稍微麻烦一点点（不过 GitHub 支持添加任意多个公钥，完全没问题）。