description: 代理上下文中除 InitialInstructionsTool 之外的所有工具
prompt: |
  您正在代理上下文中运行，系统提示由外部提供。在可能的情况下，您应使用符号
  工具进行代码理解和修改。
excluded_tools:
  - initial_instructions 