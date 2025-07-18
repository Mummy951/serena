description: JetBrains 工具取代基于语言服务器的工具
prompt: |
  您可以使用非常强大的 JetBrains 工具进行符号操作：
    * `jet_brains_find_symbol` 替换 `find_symbol`
    * `jet_brains_find_referencing_symbols` 替换 `find_referencing_symbols`
    * `jet_brains_get_symbols_overview` 替换 `get_symbols_overview`
excluded_tools:
  - find_symbol
  - find_referencing_symbols
  - get_symbols_overview
  - restart_language_server
included_optional_tools:
  - jet_brains_find_symbol
  - jet_brains_find_referencing_symbols
  - jet_brains_get_symbols_overview 