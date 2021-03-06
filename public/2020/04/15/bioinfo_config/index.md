拿到一台新的服务器生信分析环境的配置

# 1 终端美化（Oh My Zsh）[^1]

## 1.1 安装 Zsh

```bash
sudo apt install zsh
chsh -s /bin/zsh
```

## 1.2 安装 Oh My Zsh

```bash
sudo apt install git
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

## 1.3 插件配置

- zsh-autosuggestions(命令行命令键入时的历史命令建议插件)
  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```

- zsh-syntax-highlighting(命令行语法高亮插件)
  ```bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

## 1.4 `.zshrc`配置

```vim
# 设置命令行的主题
ZSH_THEME="gianu"

# 启动错误命令自动更正
ENABLE_CORRECTION="true"

# 在命令执行的过程中，使用小红点进行提示
COMPLETION_WAITING_DOTS="true"

# 启用已安装的插件
plugins=(
  git zsh-autosuggestions zsh-syntax-highlighting
)
```
修改完`.zshrc`后记得：
```bash
source .zshrc
```
# 2 R及Rstudio的安装

## 2.1 R安装

默认安装的不是最新版本的，所以为来了获取最新版本的按照如下操作：

- 添加密钥
  ```bash
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys   E298A3A825C0D65DFD57CBB651716619E084DAB9
  ```
  
- 添加源，下面这个源是3.6的，如果需要获取其它版本请访问[UBUNTU PACKAGES FOR R](https://cran.r-project.org/bin/linux/ubuntu/README.html)
  ```bash
  sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran35/"
  ```
  
- 安装
  ```bash
  sudo apt update
  sudo apt install r-base
  ```

## 2.2 安装Rstudio[^2]

**注意：**注意不同Ubuntu系统版本下载的Rstudio Server不同。

现在我常用Ubuntu 18.04，因此下面参考流程以此版本为例：

```bash
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.2.5042-amd64.deb
sudo gdebi rstudio-server-1.2.5042-amd64.deb
```

Rstudio Server默认端口`8787`

安装完成后如果是国内服务器编辑`vim ~/.Rprofile`：

```vim
options("repos" = c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
options(BioC_mirror="https://mirrors.tuna.tsinghua.edu.cn/bioconductor")
```

为了在后续安装R包过程中能够顺利，还需要安装一下依赖库：

```bash
sudo apt-get -y install libcurl4-gnutls-dev
sudo apt-get -y install libxml2-dev
sudo apt-get -y install libssl-dev
sudo apt-get -y install  libmariadb-client-lgpl-dev
```

# 3 conda安装及bioconda配置

## 3.1 miniconda安装

在[Miniconda](https://docs.conda.io/en/latest/miniconda.html)获取最新安装包链接

```bash
# 国外服务器
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# 国内服务器
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh
bash Miniconda3-py38_4.8.2-Linux-x86_64.sh
```

在完成后配置环境变量

```vim
export PATH=/home/duansq/miniconda3/bin:$PATH
```
由于我是安装在我的用户目录下，所以路径为`home/duansq`，这里根据自己实际情况进行替换。

## 3.2 bioconda源添加[^3]

```bash
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

如果是国内服务器，可以换用[Anaconda 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/anaconda/)。按照以下方法添加清华源[^4]：

```bash
#Anaconda Python 免费仓库
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ #Conda Forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/ #msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
#bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/ #menpo
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/ #pytorch
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/ # for legacy win-64
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/peterjc123/
conda config --set show_channel_urls yes
```

## 3.3 常用生信工具安装

首先创建一个生信分析环境并命名为`rna`，目前大多数工具支持的python版本为3.7

```bash
conda create -n rna python=3.7
```

创建好后切换环境

```bash
# 激活环境
source activate rna
# 退出环境
conda deactivate
```

工具安装：

```bash
conda install -y sra-tools fastqc multiqc trim-galore subread bwa hisat2 bowtie2 star samtools htseq 
```
如果需要安装`tophat`，需要新创建一个python2的环境，因为现在它只支持python2

```bash
conda create -n tophat python=2
conda activate tophat
conda install -y tophat
conda deactivate
```

[^1]: [Ubuntu 下 Oh My Zsh 的最佳实践「安装及配置」](https://segmentfault.com/a/1190000015283092)
[^2]: [Download RStudio Server for Debian & Ubuntu](https://rstudio.com/products/rstudio/download-server/debian-ubuntu/)
[^3]: [Set up channels](https://bioconda.github.io/user/install.html)
[^4]: [[转载]bioconda中国镜像(清华已恢复，中科大暂时没恢复)](http://blog.sciencenet.cn/home.php?mod=space&uid=623545&do=blog&id=1187896)
[^5]: [如果有一个云服务器你会做什么？](https://vip.biotrainee.com/d/133--)
