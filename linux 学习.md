+ ldd
	`ldd [选项] 文件名`
	`[选项]`：`ldd` 支持的一些选项，比如 `-v`（详细模式）等。
	`文件名`：你想要检查的可执行文件或共享库文件的名称。
	
	`ldd` 命令的输出通常包括每个共享库的路径和地址。例如，如果你运行 `ldd /bin/ls`，你会看到 `/bin/ls` 命令依赖的所有共享库及其路径