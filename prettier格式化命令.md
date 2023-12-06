prettier 命令行工具来格式化多个文件
你可以使用 prettier 命令行工具来格式化多个文件。以下是一个使用命令行批量格式化文件的示例：

安装 prettier
如果你还没有安装 prettier，你可以使用以下命令安装它：

npm install -g prettier

进入需要格式化的文件夹
在命令行中进入需要格式化的文件夹。

格式化文件
使用以下命令批量格式化所有 JavaScript 文件：

`prettier --write "src/**/*.js"`
`prettier --write "src/**/*.ts"`
`prettier --write "src/**/*.tsx"`

这个命令会递归遍历当前文件夹及其所有子文件夹中的所有 JavaScript 文件，并格式化它们。

你也可以使用类似的命令来格式化其他类型的文件。例如，如果你想要格式化所有 CSS 文件，可以使用以下命令：

`prettier --write "src/**/*.css"`

vue 文件同理
`prettier --write "src/**/*.vue"`

请注意，这个命令会直接修改你的文件，请在运行之前备份你的文件。如果你想要检查文件是否符合 Prettier 的规范而不修改它们，可以将 --write 参数替换为 --check，这将会输出哪些文件不符合规范，但不会修改它们。
