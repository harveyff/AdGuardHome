# GitHub Actions Workflows

## Docker 镜像自动构建和推送

当你在 GitHub 上创建一个新的 Release 时，`docker-release.yml` workflow 会自动：

1. 构建所有 Linux 平台的二进制文件（386, amd64, arm, arm64, ppc64le）
2. 构建多架构 Docker 镜像
3. 推送镜像到 Docker Hub

### 配置步骤

#### 1. 在 GitHub 仓库中设置 Secrets

在 GitHub 仓库的 Settings → Secrets and variables → Actions 中添加以下 secrets：

- **DOCKER_USERNAME**: 你的 Docker Hub 用户名
- **DOCKER_PASSWORD**: 你的 Docker Hub 密码或访问令牌（Access Token）

#### 2. 获取 Docker Hub Token（推荐使用 Token 而不是密码）

1. 登录 [Docker Hub](https://hub.docker.com/)
2. 点击右上角头像 → Account Settings
3. 选择 Security → New Access Token
4. 创建具有读写权限的 token
5. 复制 token 并保存到 GitHub Secrets 中作为 `DOCKER_PASSWORD`

**注意**：你也可以直接使用 Docker Hub 密码，但使用 Access Token 更安全。

#### 3. 修改 Docker 镜像名称（可选）

如果你想使用不同的 Docker 镜像名称，可以修改 `.github/workflows/docker-release.yml` 文件中的 `DOCKER_IMAGE_NAME` 环境变量。

默认格式为：`${{ secrets.DOCKER_USERNAME }}/adguardhome`

#### 4. 创建 Release

当你创建一个新的 GitHub Release 时，workflow 会自动触发：

- 镜像标签：`<你的用户名>/adguardhome:<版本号>` 和 `<你的用户名>/adguardhome:latest`
- 例如：`yourusername/adguardhome:v1.0.0` 和 `yourusername/adguardhome:latest`

### 注意事项

- 确保 Docker Hub 仓库名称与 `DOCKER_IMAGE_NAME` 中设置的一致
- 首次推送前，建议在 Docker Hub 上手动创建对应的仓库
- workflow 会构建多架构镜像（linux/386, linux/amd64, linux/arm/v6, linux/arm/v7, linux/arm64, linux/ppc64le）

