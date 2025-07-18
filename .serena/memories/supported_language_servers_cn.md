# Solid-LSP 中支持的语言服务器

Serena 目前通过位于 `src/solidlsp/language_servers/` 的 solid-lsp 架构支持多种编程语言。

## 主要语言（直接支持）

### Python
- **语言服务器**：Pyright 语言服务器
- **位置**：`src/solidlsp/language_servers/pyright_language_server/`
- **功能**：完整的语义分析、类型检查、符号导航
- **以前**：也支持 Jedi（现在弃用，推荐使用 Pyright）

### Java
- **语言服务器**：Eclipse JDTLS（Java 开发工具语言服务器）
- **位置**：`src/solidlsp/language_servers/eclipse_jdtls/`
- **功能**：完整的 Java 语言支持、Maven/Gradle 集成
- **注意**：启动可能很慢，特别是在初次启动时

### TypeScript/JavaScript
- **语言服务器**：TypeScript 语言服务器
- **位置**：`src/solidlsp/language_servers/typescript_language_server/`
- **功能**：TypeScript 和 JavaScript 支持、类型检查、IntelliSense

## 其他支持的语言

### C#
- **语言服务器**：OmniSharp
- **位置**：`src/solidlsp/language_servers/omnisharp/`
- **功能**：完整的 C# 语言支持、.NET 集成

### Rust
- **语言服务器**：Rust-Analyzer
- **位置**：`src/solidlsp/language_servers/rust_analyzer/`
- **功能**：Rust 语言支持、cargo 集成

### Go
- **语言服务器**：Gopls
- **位置**：`src/solidlsp/language_servers/gopls/`
- **功能**：Go 语言支持、模块系统集成

### Ruby
- **语言服务器**：Solargraph
- **位置**：`src/solidlsp/language_servers/solargraph/`
- **功能**：Ruby 语言支持、gem 集成

### C++
- **语言服务器**：Clangd
- **位置**：`src/solidlsp/language_servers/clangd_language_server/`
- **功能**：C++ 语言支持、CMake 集成

### Dart
- **语言服务器**：Dart 语言服务器
- **位置**：`src/solidlsp/language_servers/dart_language_server/`
- **功能**：Dart 和 Flutter 支持

### PHP
- **语言服务器**：Intelephense
- **位置**：`src/solidlsp/language_servers/intelephense/`
- **功能**：PHP 语言支持、Composer 集成

### Kotlin
- **语言服务器**：Kotlin 语言服务器
- **位置**：`src/solidlsp/language_servers/kotlin_language_server/`
- **功能**：Kotlin 语言支持、Gradle 集成

## 语言服务器协议（LSP）集成

所有语言服务器都利用语言服务器协议（LSP）提供：
- **语义分析**：符号定义、引用和关系
- **代码导航**：转到定义、查找引用、符号搜索
- **错误检测**：语法和语义错误报告
- **代码完成**：IntelliSense 风格的代码完成
- **文档**：悬停信息和签名帮助

## 架构集成

### Solid-LSP 框架
- **统一接口**：所有语言服务器使用相同的 `SolidLanguageServer` 接口
- **配置**：`LanguageServerConfig` 处理语言特定设置
- **生命周期管理**：所有服务器的启动、停止和重启功能
- **进程管理**：每个语言服务器的直接进程控制

### 项目集成
- **自动检测**：基于项目文件组成的语言选择
- **配置**：通过 `.serena/project.yml` 的项目特定语言服务器设置
- **忽略路径**：尊重 gitignore 和自定义忽略模式
- **超时设置**：可配置的每个语言服务器超时

## 添加新的语言服务器

该架构支持通过以下方式添加新的语言服务器：
1. **创建语言服务器类**：在 `src/solidlsp/language_servers/` 中实现特定于语言的服务器
2. **配置**：添加语言检测和配置规则
3. **集成**：向 solid-lsp 框架注册
4. **测试**：确保语义工具与新语言服务器正常工作

该系统设计为可扩展的，同时在所有支持的语言中保持一致的行为。 