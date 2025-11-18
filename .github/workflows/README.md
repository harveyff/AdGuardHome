# GitHub Actions Workflows

## Docker 镜像自动构建和推送

当你在 GitHub 上发布一个**以 "v" 开头的 Release** 时（例如：v1.0.0），`docker-release.yml` workflow 会自动：

1. 构建所有 Linux 平台的二进制文件（386, amd64, arm, arm64, ppc64le）
2. 构建多架构 Docker 镜像
3. 推送镜像到 GitHub Container Registry (ghcr.io)

**重要**：
- ✅ 只有发布以 "v" 开头的 Release 才会触发（如：v1.0.0, v2.1.3）
- ❌ 提交代码不会触发此 workflow
- ❌ 不以 "v" 开头的 Release 不会触发（如：1.0.0, release-1.0.0）

### 配置说明

#### 无需额外配置

此 workflow 使用 GitHub 内置的 `GITHUB_TOKEN` 自动推送到 GitHub Container Registry，**无需设置任何额外的 secrets**。

#### 镜像位置

镜像会自动推送到 GitHub Container Registry，格式为：
- `ghcr.io/<你的用户名>/adguardhome:<版本号>`
- `ghcr.io/<你的用户名>/adguardhome:latest`

例如：`ghcr.io/yourusername/adguardhome:v1.0.0` 和 `ghcr.io/yourusername/adguardhome:latest`

#### 查看镜像

1. 在 GitHub 仓库页面，点击右侧的 "Packages"
2. 或者访问：`https://github.com/users/<你的用户名>/packages/container/adguardhome`

#### 使用镜像

```bash
docker pull ghcr.io/<你的用户名>/adguardhome:latest
```

**注意**：如果镜像设置为私有，需要先登录：
```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u <你的用户名> --password-stdin
```

### 注意事项

- 镜像默认是私有的，可以在 GitHub Packages 页面修改为公开
- workflow 会构建多架构镜像（linux/386, linux/amd64, linux/arm/v6, linux/arm/v7, linux/arm64, linux/ppc64le）
- 确保 workflow 有 `packages: write` 权限（已在 workflow 中配置）

