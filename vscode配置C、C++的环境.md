## 配置方法(python直接三角形运行,C/C++按F5运行)

### 最新更改(都能用三角形,但是得选择run code,C还是不行,怀疑跟之前安装的其他编译器有关,因此仍然使用F5)

---

#### 1.在.vscode目录下创建launch.json并将以下内容粘贴进去

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(Windows) 启动",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "C:\\Windows\\System32\\cmd.exe",
            "args": ["/C","${fileDirname}\\${fileBasenameNoExtension}.exe"],//${fileDirname}为生成的exe文件路径
            "cwd": "${fileDirname}",
            "console": "externalTerminal",
            "preLaunchTask": "complie"
        }
    ]
}
```

---

#### 2.在.vscode目录下创建tasks.json并将以下内容粘贴进去

```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "complie",
			"command": "g++",
			"args": [
          "-fdiagnostics-color=always",
          "-g",
          "${fileDirname}\\*.cpp",//源文件名称
          //"${fileDirname}\\*.c",//源文件名称
          "-o",
          "${fileDirname}\\${fileBasenameNoExtension}.exe"//生成的文件名称
			],
		}
	]
}
```

---

**注意**`"${fileDirname}\\*.cpp"`与`"${fileDirname}\\*.c"`(在上面注释掉了)

- 使用`"${fileDirname}\\*.cpp"`时,系统会自动编译文件夹里面的所有.cpp文件(当你include自己写的东西的时候很有用),但需要确保所有文件中的函数名不能重复,包括main()
- 使用`"${fileDirname}\\*.c"`时,系统会自动编译文件夹里面的所有.c文件(当你include自己写的东西的时候很有用),但需要确保所有文件中的函数名不能重复,包括main()
- 一般这两个不同时使用,建议C和C++的程序不要混在同一文件夹,这样tasks.json就不会起冲突

