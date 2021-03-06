"glob" --- Unix 风格路径名模式扩展
**********************************

**源代码:** Lib/glob.py

======================================================================

"glob" 模块可根据 Unix 终端所用规则找出所有匹配特定模式的路径名，但会
按不确定的顺序返回结果。 波浪号扩展不会生效，但 "*", "?" 以及表示为
"[]" 的字符范围将被正确地匹配。 这是通过配合使用 "os.scandir()" 和
"fnmatch.fnmatch()" 函数来实现的，而不是通过实际发起调用子终端。 请注
意不同于 "fnmatch.fnmatch()"，"glob" 会将以点号 (".") 开头的文件名作为
特殊情况来处理。 （对于波浪号和终端变量扩展，请使用
"os.path.expanduser()" 和 "os.path.expandvars()"。）

对于字面值匹配，请将原字符用方括号括起来。 例如，"'[?]'" 将匹配字符
"'?'"。

参见: "pathlib" 模块提供高级路径对象。

glob.glob(pathname, *, recursive=False)

   返回匹配 *pathname* 的可能为空的路径名列表，其中的元素必须为包含一
   个路径信息的字符串。 *pathname* 可以是绝对路径 (如
   "/usr/src/Python-1.5/Makefile") 或相对路径 (如
   "../../Tools/*/*.gif")，并且可包含 shell 风格的通配符。 结果也将包
   含无效的符号链接 (与在 shell 中一致)。 结果是否排序取决于具体文件系
   统。

   如果 *recursive* 为真值，则模式 ""**"" 将匹配目录中的任何文件以及零
   个或多个目录、子目录和符号链接。 如果模式加了一个 "os.sep" 或
   "os.altsep" 则将不匹配文件。

   引发一个 审计事件 "glob.glob" 附带参数 "pathname", "recursive"。

   注解:

     在一个较大的目录树中使用 ""**"" 模式可能会消耗非常多的时间。

   在 3.5 版更改: 支持使用 ""**"" 的递归 glob。

glob.iglob(pathname, *, recursive=False)

   返回一个 *iterator*，它会产生与 "glob()" 相同的结果，但不会实际地同
   时保存它们。

   引发一个 审计事件 "glob.glob" 附带参数 "pathname", "recursive"。

glob.escape(pathname)

   转义所有特殊字符 ("'?'", "'*'" 和 "'['")。 这适用于当你想要匹配可能
   带有特殊字符的任意字符串字面值的情况。 在 drive/UNC 共享点中的特殊
   字符不会被转义，例如在 Windows 上 "escape('//?/c:/Quo vadis?.txt')"
   将返回 "'//?/c:/Quo vadis[?].txt'"。

   3.4 新版功能.

例如，考虑一个包含以下内容的目录：文件 "1.gif", "2.txt", "card.gif" 以
及一个子目录 "sub" 其中只包含一个文件 "3.txt".  "glob()" 将产生如下结
果。 请注意路径的任何开头部分都将被保留。:

   >>> import glob
   >>> glob.glob('./[0-9].*')
   ['./1.gif', './2.txt']
   >>> glob.glob('*.gif')
   ['1.gif', 'card.gif']
   >>> glob.glob('?.gif')
   ['1.gif']
   >>> glob.glob('**/*.txt', recursive=True)
   ['2.txt', 'sub/3.txt']
   >>> glob.glob('./**/', recursive=True)
   ['./', './sub/']

如果目录包含以 "." 打头的文件，它们默认将不会被匹配。 例如，考虑一个包
含 "card.gif" 和 ".card.gif" 的目录:

   >>> import glob
   >>> glob.glob('*.gif')
   ['card.gif']
   >>> glob.glob('.c*')
   ['.card.gif']

参见:

  模块 "fnmatch"
     Shell 风格文件名（而非路径）扩展
