# This aims at introduing how to configurate Neovim as the coding IDE, mainly using WSL2

- For windows environment, it's recommended to use "Windows Terminal" which can be found and installed via Microsoft Store, since the classic terminal does't support certain symbols in powerline of neovim. Also, for windows, we should install WSL2 (WSL is ok) which can be found in Microsoft Store. Here we use the Linux distribution of Ubuntu (20.04).
- Before configurating neovim, it's recommended to install/configurate the shell of "zsh"/"Oh-my-zsh", for better apperance and utility.
```
sudo apt update

sudo apt install zsh

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

- For zsh theme, we recommend here POWERLEVEL10K
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```
To activate the theme you need to edit your “~/.zshrc” file in your personal folder and replace **ZSH_THEME=”robbyrussel”** with **ZSH_THEME=”powerlevel10k/powerlevel10k”**. After the change, you need to close and restart your terminal.


- To install and enable plugins in zsh: 
- Here we recommend the *auto-suggestion* plugin for zsh shell :
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
- To enable the auto-suggestion plugin or any other plugins in “zsh”, edit your “~/.zshrc” file in your personal folder. Simply change the default line **plugins=(git)** to **plugins=(git zsh-autosuggestions {optional-other-plugins})**. Of course, replace the {optional-other-plugins} with any other plugins you want to enable.


- (Windows) To use the cool icons, we need to install and set up fonts for Windows Terminal. Nerd fonts are recommended, they can be downloaded via [Nerd fonts](https://www.nerdfonts.com/). We use here *Caskaydia Cove Nerd Font*. Once installed, we need to set up the font in Windows Terminal - Settings - Ubuntu - Appearance - Font face.


- So all is ready, let's install and configurate Neovim. To install it, type in terminal
```
sudo apt install neovim
```

- Before configurating, we install the plugin manager. Here we use *vim-plug*
```
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

- To configurate it, we need to create dirs and the file *init.vim*
```
mkdir .config && cd .config
mkdir nvim && cd nvim
nvim init.vim
```

- In the config file *init.vim*, we paste
```
:set number

:set relativenumber

:set autoindent

:set tabstop=4

:set shiftwidth=4

:set smarttab

:set softtabstop=4

:set mouse=a

call plug#begin('~/.config/nvim/plugged')

" The default plugin directory will be as follows:
"   - Vim (Linux/macOS): '~/.vim/plugged'
"   - Vim (Windows): '~/vimfiles/plugged'
"   - Neovim (Linux/macOS/Windows): stdpath('data') . '/plugged'
" You can specify a custom plugin directory by passing it as the argument
"   - e.g. `call plug#begin('~/.vim/plugged')`
"   - Avoid using standard Vim directory names like 'plugin'

" Make sure you use single quotes

Plug 'http://github.com/tpope/vim-surround' " Surrounding ysw)

Plug 'https://github.com/preservim/nerdtree' " NerdTree

Plug 'https://github.com/tpope/vim-commentary' " For Commenting gcc & gc

Plug 'https://github.com/vim-airline/vim-airline' " Status bar

Plug 'https://github.com/ap/vim-css-color' " CSS Color Preview

Plug 'https://github.com/rafi/awesome-vim-colorschemes' " Retro Scheme

Plug 'https://github.com/ryanoasis/vim-devicons' " Developer Icons

Plug 'https://github.com/tc50cal/vim-terminal' " Vim Terminal

Plug 'https://github.com/terryma/vim-multiple-cursors' " CTRL + N for multiple cursors

Plug 'https://github.com/preservim/tagbar' " Tagbar for code navigation

Plug 'https://github.com/neoclide/coc.nvim' " Auto Completion
" Plug 'https://github.com/ycm-core/YouCompleteMe' 

set encoding=UTF-8

call plug#end()

nnoremap <C-f> :NERDTreeFocus<CR>

nnoremap <C-n> :NERDTree<CR>

nnoremap <F2> :NERDTreeToggle<CR>

nmap <F3> :TagbarToggle<CR>

:set completeopt-=preview " For No Previews

:colorscheme jellybeans

" air-line
let g:airline_powerline_fonts = 1

let g:airline#extensions#tabline#enabled = 1

let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'

" switch buffers sequentially
nnoremap <C-q> :bnext<CR>
" :nnoremap <C-S-q> :bprevious<CR>

" switch between two buffers
nnoremap <C-w> :b#<CR>

" let g:NERDTreeDirArrowExpandable="+"

" let g:NERDTreeDirArrowCollapsible="~"

" --- Just Some Notes ---

" :PlugClean :PlugInstall :UpdateRemotePlugins

"

" :CocInstall coc-python

" :CocInstall coc-clangd

" :CocInstall coc-snippets

" :CocCommand snippets.edit... FOR EACH FILE TYPE

" air-line

let g:airline_powerline_fonts = 1

if !exists('g:airline_symbols')

let g:airline_symbols = {}

endif

" airline symbols

let g:airline_left_sep = ''

let g:airline_left_alt_sep = ''

let g:airline_right_sep = ''

let g:airline_right_alt_sep = ''

let g:airline_symbols.branch = ''

let g:airline_symbols.readonly = ''

let g:airline_symbols.linenr = ''

inoremap <expr> <Tab> pumvisible() ? coc#_select_confirm() : "<Tab>"
```

- Save it by *:wq* and reopen it. Type *:PlugInstall* to install the plugins

- There are two plugins that may need further dependencies. The first is Tagbar. If *:TagbarToggle* does't work, we quit nvim and type:
```
sudo apt install exuberant-ctags
```

- The second is the *Coc* completion plugin. We may need install nodejs, by typing
```
sudo apt install nodejs
```
However, this can install the version not satisfying the following *yarn install* requirement (node>=xxx). To this end, we recommend to install nodejs as presented in this [microsoft post](https://docs.microsoft.com/fr-fr/windows/dev-environment/javascript/nodejs-on-wsl)
In short, we may need install curl by
```
sudo apt install curl
```
and then install nvm by
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```
close terminal and reopen it. Install nodejs by
```
nvm install --lts
```
then verify the installation and version of node by
```
node --version
```
Then install npm by
```
sudo apt install npm
```
Finally we need go to the coc plugin dir and install yarn
```
cd ~/.config/nvim/plugged/coc.nvim
sudo npm install -g yarn
yarn install
yarn build
```

In general, *coc* plugin should be well installed. To use the auto-completion for individual language, we need to install individual coc. Like for python, in nvim, we type *:CocInstall coc-python*
And then quit nvim, type in terminal
```
sudo apt install python3-pip
pip3 install jedi
```
So for the python file ended by *.py*, nvim can give recommendation of completion.

The same for c/c++, type in nvim *:CocInstall coc-clangd*. Then type *:CocCommand clangd.install*

- Or, we can also choose *YouCompleteme* instead of *Coc* for auto-completion.
- BUT IT HAS BEEN TESTED THAT YCM IS NOT EASY TO COMPILE AND BUILD... GIVEN UP
- To this end, refer to [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe). In fact, it seems that *Vim-plug* does not perform well in installing YCM. So we just clone ourselfs by
```
git clone https://github.com/ycm-core/YouCompleteMe ~/.config/nvim/plugged/YouCompleteMe --recursive
```
Then install CMake, Vim and Python
```
sudo apt install build-essential cmake vim-nox python3-dev
```
Then install mono-complete, go, node, java and npm (we can remove nodejs and npm)
```
apt install mono-complete golang nodejs default-jdk npm
```
Compile YCM
```
cd ~/.vim/bundle/YouCompleteMe
python3 install.py --all
```


Enjoy nvim !



**References** :

[WSL/WSL2 config](https://www.the-digital-life.com/awesome-wsl-wsl2-terminal/)
[ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)
[NerdFonts](https://www.nerdfonts.com/font-downloads)
[Neovim](https://github.com/neovim/neovim)
[vim-plug](https://github.com/junegunn/vim-plug)
[nvim config file](https://github.com/NeuralNine/config-files/blob/master/init.vim)
[nodejs in WSL2](https://docs.microsoft.com/fr-fr/windows/dev-environment/javascript/nodejs-on-wsl)
[youtube video](https://www.youtube.com/watch?v=JWReY93Vl6g)
