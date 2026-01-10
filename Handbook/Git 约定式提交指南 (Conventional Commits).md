# Git 约定式提交指南 (Conventional Commits)

> **核心原则**：让别人（和未来的你自己）一眼看懂这行代码改了什么。

## 1. 标准格式结构

一条标准的提交信息应该包含三个部分：

```text
<类型>(<范围>): <描述>
```

- **类型 (Type)**: *[必填]* 告诉大家这次改动是什么性质。
- **范围 (Scope)**: *[选填]* 告诉大家改了哪个模块（如 `login`, `db`, `ui`）。
- **描述 (Subject)**: *[必填]* 简短描述做了什么（建议使用动词开头）。

------

## 2. 常用类型速查表 (Type)

| **关键词**   | **英文全称**  | **含义**     | **适用场景**                                |
| ------------ | ------------- | ------------ | ------------------------------------------- |
| **feat**     | Features      | **新功能**   | 开发了新的需求、页面或按钮                  |
| **fix**      | Bug Fixes     | **修补 Bug** | 修复了报错、崩溃或逻辑错误                  |
| **docs**     | Documentation | **文档**     | 仅修改了 README、注释或文档                 |
| **style**    | Styles        | **格式**     | 调整空格、缩进、分号（不影响代码运行）      |
| **refactor** | Refactoring   | **重构**     | 优化代码结构，既没加新功能也没修 Bug        |
| **perf**     | Performance   | **性能**     | 提升页面加载速度、优化查询效率              |
| **test**     | Tests         | **测试**     | 增加测试代码或修改测试用例                  |
| **chore**    | Chores        | **杂务**     | 修改构建流程、依赖管理、配置文件            |
| **build**    | Builds        | **构建**     | 影响构建系统或外部依赖的更改 (如 npm, gulp) |
| **ci**       | CI            | **集成**     | 修改 CI 配置文件 (如 GitHub Actions)        |

------

## 3. 实战案例对比

### ✅ 推荐写法 (Clear & Professional)

- **新功能**: `feat(user): add login button on navbar`
- **修 Bug**: `fix(server): handle null pointer exception in API`
- **改文档**: `docs: update install guide in README`
- **做重构**: `refactor(auth): simplify password validation logic`
- **改配置**: `chore: update .gitignore rules`

### ❌ 糟糕写法 (Avoid These)

- `update` (更新了啥？)
- `fix bug` (修了哪个 bug？)
- `wip` (正在做... 这种尽量少提交到公共分支)
- `123` (毫无意义)

------

## 4. 为什么要这样写？

1. **自动生成日志**：自动化工具可以提取 `feat` 和 `fix` 自动生成 `CHANGELOG.md`。
2. **快速定位**：排查 Bug 时，可以直接过滤 `fix` 类型的提交。

3. **显得专业**：这是开源社区（如 Vue, React, Angular）通用的国际标准。
