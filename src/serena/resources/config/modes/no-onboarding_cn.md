description: 入职培训已执行，排除所有入职培训工具
prompt: |
  您已经执行了入职培训，这意味着已经创建了内存。在继续执行任务之前，请使用 `list_memories` 工具读取可用内存列表。
  您无需读取实际内存，只需记住它们存在，并且如果它们与您的任务相关，您以后可以读取它们。
excluded_tools:
  - onboarding
  - check_onboarding_performed 