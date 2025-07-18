# 建议的命令

## 开发任务（使用 uv 和 poe）

以下任务通常应该使用 `uv run poe <task_name>` 执行。

- `format`：这是格式化的**唯一**允许命令。运行为 `uv run poe format`。
- `type-check`：这是类型检查的**唯一**允许命令。运行为 `uv run poe type-check`。
- `test`：这是运行测试的首选命令（`uv run poe test [args]`）。您可以使用标记选择测试子集，
   当前的标记有：
   ```toml
    markers = [
        "python: language server running for Python",
        "go: language server running for Go",
        "java: language server running for Java",
        "rust: language server running for Rust",
        "typescript: language server running for TypeScript",
        "php: language server running for PHP",
        "snapshot: snapshot tests for symbolic editing operations",
    ]
   ```
  默认情况下，`uv run poe test` 使用环境变量 `PYTEST_MARKERS` 中设置的标记，或者如果未设置，则使用 `-m "not java and not rust and not isolated process"`。
  您可以通过简单地向 `uv run poe test` 传递 `-m` 选项来覆盖此行为，例如 `uv run poe test -m "python or go"`。

为了完成任务，请确保 format、type-check 和 test 都通过！在任务结束时运行它们，
如果需要，修复出现的任何问题，然后再次运行直到它们通过。 