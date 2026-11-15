# 变更记录

本文件记录本 fork（`wyf1463630616/carvex-android`）相对上游 `iamr0s/carvex-android`
的本地定制，按时间倒序追加。上游原有的发布历史不在本文件内。

## 2026-11-15 — CI 改为「推送 main 即出包」（单品牌）

- **版本**：未改动版本派发规则，仍为 `20000 + git 提交数`（`versionCode` = `versionName`）。
- **修改时间**：2026-11-15 14:50 (GMT+8)
- **修改内容**：
  1. `.github/workflows/android.yml` 触发条件收敛为 `push: branches: [main]` +
     `workflow_dispatch`，移除 `pull_request` 触发与发布步骤里的 PR 判断。
  2. `apk` job 由五品牌矩阵改为单品牌 `xiaomi`，执行 `:app:testXiaomiDebugUnitTest`、
     `:app:lintXiaomiRelease`、`:app:assembleXiaomiRelease`；job 名与失败注释同步改为
     单品牌表述。
  3. `publish` job 只汇总一个 APK，取消 `CarVex_all_flavors_release_<version>.zip`
     汇总包；Release 直接附 APK、`SHA256SUMS`、`build-info.txt`。
  4. `publish` 仍 `needs: [protocol-tests, apk]`：协议与宿主测试不通过时不产出可下载包。
  5. `protocol-tests`、`apk` 的超时分别由 35 分钟收敛为 25 / 30 分钟。
  6. `README.md`、`README.en.md` 的 CI 与签名章节按单品牌重写，构建状态徽章指向本 fork。
  7. `AGENT.md`、`AGENT.en.md` 第 6 节的 CI 描述同步为单品牌口径。
- **涉及文件**：
  - `.github/workflows/android.yml`（重写 job 结构与发布步骤）
  - `README.md` / `README.en.md`（CI 与签名章节、徽章）
  - `AGENT.md` / `AGENT.en.md`（第 6 节「测试与验证」CI 段）
  - `CHANGELOG.md`（本文件，新增）
- **影响的功能与模块**：
  - 仅影响 CI 流水线。Gradle 构建脚本、签名配置、flavor 定义、应用代码与模块依赖均未改动。
  - CI 不再验证 `oppo`、`vivo`、`samsung`、`honor` 四个品牌；需要这四类包时按
    `README.md` 的本地命令构建。
  - Release 产物形态由「5 个 APK + 汇总 zip」变为「1 个 APK + `SHA256SUMS` +
    `build-info.txt`」。
  - 出包前提：fork 仓库需在 Actions 页面启用 workflow，且
    Settings → Actions → General → Workflow permissions 设为 Read and write（发布 Release
    需要写权限）。
