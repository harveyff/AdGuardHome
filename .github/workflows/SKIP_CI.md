# 如何跳过 CI 测试检查

## 方法 1：在 Commit Message 中使用 [skip ci]（推荐）

在提交或推送时，在 commit message 中包含 `[skip ci]` 或 `[ci skip]`，GitHub Actions 会自动跳过所有 workflow。

### 示例：

```bash
git commit -m "Update code [skip ci]"
git push
```

或者：

```bash
git commit -m "Fix bug [ci skip]"
```

**注意**：`[skip ci]` 必须出现在 commit message 的开头或任意位置。

## 方法 2：使用环境变量控制

如果你想通过环境变量控制是否跳过测试，可以修改 workflow 文件添加条件判断。

## 方法 3：完全禁用测试

如果你完全不需要测试，可以：
1. 删除或重命名 `.github/workflows/build.yml` 和 `.github/workflows/lint.yml`
2. 或者修改这些文件，注释掉测试相关的 job

## 支持的跳过标记

GitHub Actions 支持以下标记来跳过 CI：
- `[skip ci]`
- `[ci skip]`
- `[no ci]`
- `[skip actions]`
- `[actions skip]`

这些标记不区分大小写，可以出现在 commit message 的任意位置。

