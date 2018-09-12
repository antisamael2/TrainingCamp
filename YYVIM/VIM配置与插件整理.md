[TOC]

## 编码

Content [^1]

* 查看文件编码
    ```viml
    :set fileencoding
    ```

即可显示文件编码格式

VIM默认实用latin-1(ASCII)编码打开，如果要解决其他编码格式的乱码问题，可以在vimrc中添加以下内容：
    ```viml
    set encoding=utf-8 "默认编码
    set fileencodings=ucs-bom,utf-8,cp936 “自动识别编码列表
    ```

* 转换文件编码
  比如将一个文件转换成utf-8格式
    ```viml
    :set fileencoding=utf-8
    ```

* gvim下乱码问题解决方案

将编码设置为utf8之后，这时候可能出现很多中文乱码。如：文件显示乱码、菜单乱码、右键菜单乱码、控制台输出乱码等等。
在vimrc中加入如下配置可以解决这些问题。
    ```viml
    set encoding=utf-8
    set fileencodings=utf-8,chinese,latin-1
    if has("win32")
        set fileencoding=chinese
    else
        set fileencoding=utf-8
    endif
    "解决菜单乱码
    source $VIMRUNTIME/delmenu.vim
    source $VIMRUNTIME/menu.vim
    "解决consle输出乱码
    language messages zh_CN.utf-8
    ```

## 缓冲区

| 命令         | 功能                  |
| ------------ | --------------------- |
| ___:ls___    | 查看缓冲区内容        |
| ___:bp___    | 跳转到上一个缓冲区    |
| ___:bn___    | 跳转到下一个缓冲区    |
| ___:b num___ | 跳转到num对应的缓冲区 |
| ___:bdel___  | 删除当前缓冲区        |


## 高亮方案

[Molokai](https://github.com/tomasr/molokai)
[dersetEx](https://github.com/vim-scripts/desertEx)
[solarized](https://github.com/altercation/vim-colors-solarized)

## 插件

### 通用

#### 插件管理

- [pathogen](https://github.com/tpope/vim-pathogen)

  __基本用法：__
  下载好zip后解压，将autoload/中的pathogen.vim拷贝到vimfiles/autoload/中，然后在vimrc中添加如下配置：

  ```viml
  call pathogen#infect()
  ```

  再在vimfiles/下新建一个bundle文件夹，后面再安装其他插件的时候，只需要将下载下来的插件zip解压到bundle/下即可。

  很多插件在doc目录中带有自己的说明文档，只要执行一下命令：

  ```viml
  :call pathogen#helptags()
  ```

  pathogen就可以自动为bundle目录下所有的doc目录中的txt文件生成帮助文档标签。

- [Vundle.vim](https://github.com/VundleVim/Vundle.vim)
  老牌的vim插件管理器，缺点是无法并行更新和安装，在Windows环境下安装稍微麻烦点。

  __安装：__

  Linux环境下的安装比较简单，主要说说windows下的安装方法，参考[这篇文章](https://github.com/VundleVim/Vundle.vim/wiki/Vundle-for-Windows)
  安装[msysgit](https://code.google.com/p/msysgit/downloads/list)，并将Git的安装路径加入到Path环境变量
  再安装[curl](https://curl.haxx.se/download.html#Win64)，同样将curl.exe的路径加入到Path环境变量
  从官网Clone Vundle到Vim的安装路径下，如：

  ```bash
  git clone https://github.com/gmarik/vundle D:\Vim\vimfiles\bundle\vundle
  ```

  配置vimrc：

  ```viml
  set rtp+=$VIM/vimfiles/bundle/vundle/
  call vundle#rc('$VIM/vimfiles/bundle/')
  call vundle#begin()

  Plugin 'VundleVim/Vundle.vim'
  "Plugin 'git上的vim插件项目名称'

  call vundle#end()
  filetype plugin indent on    " required
  ```

  __基本用法__：

| 命令           | 作用         |
| -------------- | ------------ |
| :PluginList    | 显示插件列表 |
| :PluginInstall | 安装插件     |
| :PluginUpdate  | 升级插件     |
| :PluginClean   | 删除插件     |

- [vim-plug](https://github.com/junegunn/vim-plug) 目前最快的vim插件管理器，看上去很像SulimeText的插件管理器，还没有试过

#### 符号管理

- [tagbar](https://github.com/majutsushi/tagbar)

  宏定义、变量名、结构名、函数名这些东西我们将其称之为符号(symbols)，vim中可以使用taglist插件来显示和定位符号（taglist中管符号叫tag）；taglist插件只能解析和显示tag文件，tag文件的扫描及生成需要使用[Exuberant Ctags](http://ctags.sourceforge.net)或[Universal Ctags](https://ctags.io/)工具完成，所以想要正常使用taglist，必须配合ctags。

  如果默认的tagbar不能支持你需要的语言，如Markdown等等，可以参考[这篇文章](https://github.com/majutsushi/tagbar/wiki)提供额外的支持。

  __vimrc配置：__

 ```viml
 "打开/关闭tagbar界面
 nmap <leader>t :TagbarToggle<CR>
 ```

#### 文件管理

- [NerdTree](https://github.com/scrooloose/nerdtree)

  在VIM中浏览文件系统，在vimrc下加入如下配置，实现按下 ``Ctrl``+``N`` 时呼出侧边栏：

  ```viml
  nmap <C-n> :NERDTreeToggle<CR>
  ```

  NerdTree支持收藏夹，按下``Shift`` + ``B`` 显示/隐藏收藏夹，键入 ``:Bookmark`` 命令来将鼠标所指向的文件或文件夹添加到收藏夹，按下 ``Shift`` + ``D`` 将项目从收藏夹中删除。

  __推荐配置：__

  ```viml
  let NERDTreeShowBookmarks=1 “显示收藏夹
  autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif "当只有NERDTree窗口时关闭vim
  autocmd vimenter * NERDTree "当vim启动时自动打开NERDTree
  let g:NERDTreeDirArrowExpandable = '▸' "修改标记样式
  let g:NERDTreeDirArrowCollapsible = '▾' "修改标记样式
  ```

#### 状态栏增强

- [vim-airline](https://github.com/vim-airline/vim-airline)

- [vim-airline主题](https://github.com/vim-airline/vim-airline-themes)

- [bufferline](https://github.com/bling/vim-bufferline)

  轻量级的PowerLine，而且比它还更强大，支持tab和buffer显示，并集成了一些常用插件
  在vimrc中加入如下配置：

  ```viml
  set laststatus=2 "总是显示状态栏
  set noshowmode
  set t_Co=256 "显示颜色
  if !exists('g:airline_symbols')
  let g:airline_symbols = {}
  endif
  let g:airline_powerline_fonts = 1 "使用powerline打过补丁的字体
  let g:airline#extensions#tabline#enabled = 1 "开启tabline
  let g:airline#extensions#tabline#left_sep = ' ' "tabline中当前buffer两端的分隔字符
  let g:airline#extensions#tabline#left_alt_sep = '|' "tabline中未激活buffer两端的分隔字符
  let g:airline#extensions#tabline#buffer_nr_show = 1 "tabline中buffer显示编号
  ```

  重启vim后发现有些符号显示异常或不显示，是因为插件内置了一些字体，需要额外[下载](https://github.com/powerline/fonts)安装。
  字体下载安装完成后，在vimrc中将字体设置成需要打补丁的字体即可，如：

  ```viml
  set guifont=DejaVu_Sans_Mono_for_Powerline:h10:cANSI
  ```

#### 文件搜索

- [Ctrlp](https://github.com/ctrlpvim/ctrlp.vim)

  老牌的vim fuzzy 搜索工具，默认使用grep进行搜索，速度会比较慢，建议使用[ag](https://github.com/ggreer/the_silver_searcher)替换默认的搜索

  为了集成ag，需在vimrc中添加如下配置：

  ```viml
  if executable('ag')
    " Use Ag over Grep
    set grepprg=ag\ --nogroup\ --nocolor
    " Use ag in CtrlP for listing files.
    let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
    " Ag is fast enough that CtrlP doesn't need to cache
    let g:ctrlp_use_caching = 0
  endif
  ```

  __vimrc推荐配置：__

  ```viml
  "set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " MacOSX/Linux
  set wildignore+=*\\tmp\\*,*.swp,*.zip,*.exe  " Windows
  let g:ctrlp_custom_ignore = '\v[\/]\.(git|hg|svn)$'
  let g:ctrlp_custom_ignore = {
    \ 'dir':  '\v[\/]\.(git|hg|svn)$',
    \ 'file': '\v\.(exe|so|dll)$',
    \ 'link': 'some_bad_symbolic_links',
    \ }
  ```

  __基本用法：__

  在vim中，键入 ``Ctrl`` + ``P`` 开启搜索界面，在界面中：

  ``F5`` 刷新缓存
  ``Ctrl`` + ``d`` 切换搜索路径和仅搜索文件名
  ``Ctrl`` + ``r`` 切换字符串和正则表达式搜索方式
  ``Ctrl`` + ``j`` / ``Ctrl`` + ``k`` 在搜索列表中上下选择

#### 搜索增强

- [vim-easygrep](https://github.com/dkprice/vim-easygrep)

  __常用用法：__
  如果在vimrc中将mapleader设置为 ``,``

| 快捷键 | 作用                                     |
| ------ | ---------------------------------------- |
| ,vv    | 在文件中搜索当前光标下的单词             |
| ,vV    | 在文件中搜索当前光标下的单词（全词匹配） |

#### 代码语法静态检查

- for vim8.0 or neovim [ALE](https://github.com/w0rp/ale)
- for older vims [Syntastic](https://github.com/vim-syntastic/syntastic)

  以使用ALE检查Python语法为例；在ALE的git网页中查找Python的linters，有[autopep8](https://github.com/hhatto/autopep8), [flake8](http://flake8.pycqa.org/en/latest/), [isort](https://github.com/timothycrosley/isort), [mypy](http://mypy-lang.org/), [pycodestyle](https://github.com/PyCQA/pycodestyle), [pyls](https://github.com/palantir/python-language-server), [pylint](https://www.pylint.org/) !!, [yapf](https://github.com/google/yapf) 这么些，随便选一个比如[flake8](http://flake8.pycqa.org/en/latest/), 在命令行中使用下面的命令来安装（需要先安装Python环境）：

  ```bash
  python<version> -m pip install flake8
  ```

  然后编辑vimrc文件，加入以下配置：

  ```viml
  let g:ale_enabled = 0 "默认关闭ALE
  let g:ale_linters = {
                \ 'python' : ['flake8'],
                \}
  let g:ale_python_flake8_executable='python'   " or 'python3' for Python 3
  let g:ale_python_flake8_options='-m flake8'
  let g:ale_set_highlights = 1 "高亮出错的位置
  let g:ale_sign_error = 'E:'
  let g:ale_sign_warning = 'W:'
  "let g:airline#extensions#ale#enabled = 1 ”需要配置airline插件
  let g:ale_echo_msg_error_str = 'Error'
  let g:ale_echo_msg_warning_str = 'Warning'
  let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'
  let g:ale_lint_on_text_changed = 'never'
  "跳转到上一处错误
  nmap <silent> <C-k> <Plug>(ale_previous_wrap)
  "跳转到下一处错误
  nmap <silent> <C-j> <Plug>(ale_next_wrap)
  "开启ALE
  nnoremap <Leader>ck :ALEToggle<CR>
  ```

#### 代码注释
- [NerdCommiter](https://github.com/scrooloose/nerdcommenter)
- [tcomment_vim](https://github.com/tomtom/tcomment_vim)
- [vim-commentary](https:/github.com/tpope/vim-commentary)

#### 代码补全

- [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
- Neocomplete

#### 代码对齐

- [tabular](http://link.zhihu.com/?target=https%3A//github.com/godlygeek/tabular)

  __使用方法：__
  如需要对齐 ``=`` 时：

  ```viml
  :Tab /=
  ```

#### 显示缩进对齐线

- [indentLine](https://github.com/Yggdroot/indentLine)

  安装插件后默认显示对齐线，可以自定义键盘映射切换是否显示：

  ```
  map <leader>il :IndentLinesToggle<CR>

  ```
#### 快速标记跳转

- [vim-signature](https://github.com/kshenoy/vim-signature)

#### 代码段生成

- [UltiSnips](https://github.com/SirVer/ultisnips)

  比snipmate更强大，但是它已经停止维护了，开发小组后来加入到了ultisnips中，之所以换个名字是因为ultisnips需要python支持

#### 多光标输入

- [vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors)

  类似SublimeText的多光标输入，需要的时候只要只要记得用 ``Ctrl`` + ``N`` 就行了

### C/C++

#### C++源文件/头文件快速切换 [a.vim](http://www.vim.org/scripts/script.php?script_id=31)

多年没有维护了

#### C++代码自动补全 [omnicppcomplete](http://www.vim.org/scripts/script.php?script_id=1520)

多年没有维护了

#### C/C++快速开发 [c-support](https://github.com/WolfgangMehner/c-support)

### Python

#### Python文档查找 [pydoc](http://www.vim.org/scripts/script.php?script_id=910)

#### Python代码提示 [jedi-vim](https://github.com/davidhalter/jedi-vim)

#### 增强型Python高亮方案 [python.vim](http://www.vim.org/scripts/script.php?script_id=790)

多年没有维护了

### Markdown

#### Markdown语法高亮

- [vim-markdown](https://github.com/gabrielelana/vim-markdown)

#### Markdown实时预览

- [markdown-preview.vim](https://github.com/iamcco/markdown-preview.vim)

注意：需要vim内置python

#### Markdown大纲
前提是安装了tagbar插件，然后下载[markdown2ctags.py](https://raw.githubusercontent.com/tamlok/vimconf/master/markdown2ctags.py)放到.vim文件下
在vimrc中加入如下配置：
    ```viml
    "在tagbar中添加markdown支持
    let g:tagbar_type_markdown = {
        \ 'ctagstype': 'markdown',
        \ 'ctagsbin' : 'd:/Program Files/Vim/markdown2ctags.py',
        \ 'ctagsargs' : '-f - --sort=yes',
        \ 'kinds' : [
            \ 's:sections',
            \ 'i:images'
        \ ],
        \ 'sro' : '|',
        \ 'kind2scope' : {
            \ 's' : 'section',
        \ },
        \ 'sort': 0,
    \ }
    ```

```flow
st=>start: Start:>http://www.google.com[blank]
e=end:>http://www.google.com
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?:>http://www.google.com
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```

```c++ {.line-numbers}
void main()
{
  return;
}
```

```c++
void main()
{
  return;
}
```

39^th^
H~2~O


[^1]: Hi! This is a footnote

*[HTML]: Hyper Text Markup Language
*[W3C]:  World Wide Web Consortium
The HTML specification
is maintained by the W3C.

==marked==
