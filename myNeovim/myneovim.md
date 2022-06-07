

# 关于我基于lua配置neovim的说明

## nerd font

首先需要将终端字体改成nerd font字体(我在ubuntu终端之前使用的字体是ubuntu Mono，所以对应安装的字体也是ubuntu nerd font)

安装nerdfont之后才能正常显示例如nvim-tree、bufferline上的图标

github下载地址: 
https://github.com/ryanoasis/nerd-fonts/releases/tag/v2.1.0

unicode emoji table: 
https://www.nerdfonts.com/cheat-sheet



## 有关nvim-lspconfig

### clangd

有关clangd的nvim-lspconfig说明可以参考nvim-lspconfig的帮助文档 [server_configurations.md--clangd](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#clangd)




对于复杂的工程项目，例如cmake，头文件引用方面会有语法报错的提示，这时候需要一个`compile_commands.json`文件，clangd会自动的在根目录和根目录下的build目录中寻找这个文件。该文件生成的方式如下：

这里还是以ORB_SLAM2为例

```bash
cd ORB_SLAM2
mkdir build
cd build
cmake .. -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
```
既可在build文件中生成`compile_commands.json`

对于比较简单的项目来说，还可以用`compile_flags.txt`。

对于只有MakeFile的工程，可以使用[Bear](https://github.com/rizsotto/Bear)插件来生成`compile_commands.json`



clangd好像是完全基于c++20的标准，不支持头文件递归调用，比如在ORB_SLAM2的代码中：

`inlude/System.h`等头文件会报头文件引用的错误

所以我现在都是在用ccls(2022-05-21)


### ccls


有关clangd的nvim-lspconfig说明可以参考nvim-lspconfig的帮助文档 [server_configurations.md--ccls](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#ccls)

同样的这里需要一个`compile_commands.json`, 或者是 `compile_flags.txt`, 但是ccls的默认配置中并不会在build文件中寻找，但是可以设置其`comp`


我对于ccls的配置如下：

```lua
    elseif server.name == "ccls" then
		nvim_lsp.ccls.setup({
            cmd = { 'ccls' },
            filetypes = { 'c', 'cpp', 'objc', 'objcpp' },
            -- root_dir = nvim_lsp.util.root_pattern('compile_commands.json', '.ccls', 'compile_flags.txt', '.git', 'build'),
            root_dir = nvim_lsp.util.root_pattern('compile_commands.json', '.ccls', 'compile_flags.txt', '.git'),

			capabilities = capabilities,
			on_attach = custom_attach,
            -- single_file_support = true,
            init_options = {
                compilationDatabaseDirectory = "build";
            },

            -- root_dir = nvim_lsp.root_pattern('build/compile_commands.json', '.git'),
		})
```

解释一下这里的几个设置：

+ root_dir: 语言服务器该怎么判断哪个是你的根目录呢? 其实root_dir就是根目录的标志，有这几个文件其中之一的目录就是根目录

+ compilationDatabaseDirectory: 当ccls在根目录下找不到`compile_commands.json`文件的时候，就从`build`目录下找，默认值是`""`, 这里我设置成了build, 如果在build下找不到，就在根目录找 



注意：

ccls的默认单文件支持(`single_file_support`)是`false`, 而clangd的是`true`。
我来解释一下，假如root_dir中的几个文件/文件夹都没有，就没有找到根目录，那么clangd也是可以开启语言支持的，但是ccls就不行，必须要有`'compile_commands.json', '.ccls', 'compile_flags.txt', '.git')`这几个标志文件/文件夹才行。

比如说，我新建了一个目录`mkdir test && cd test`, 并在该目录中新建了一个文件`nvim test_ccls.cpp`, 这时候，是没有ccls语言服务器语法支持的。这个时候我们可以在`test`文件夹下新建一个文件，例如`mkdir .ccls`, 再进入 `test_ccls.cpp`, 就有语法支持了

### pyright

install nodejs

curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -

install npm


sudo aptitude install npm

## telescope

对于live_grep的搜索(如搜索文档中的字符串), 可以用到一些工具，这里我用的是它的官方README中推荐的，[ripgrep](https://github.com/BurntSushi/ripgrep#installation)

按照网站的说明，Ubuntu中可以通过如下方式安装：

```bash
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb
sudo dpkg -i ripgrep_13.0.0_amd64.deb
```

说明一下， ripgrep并不是telescope必须的，它只是众多查找器中的一个

## other

### 光标回到上一次编辑该文件的位置

快捷键`g;`







