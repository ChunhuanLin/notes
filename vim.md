## 配置文件的位置  
   终端中输入`vim`  
   命令模式输入`:echo $VIM`  

## vim安装markdown插件  
   参考链接： http://www.jianshu.com/p/44d31327f953  
   项目链接： https://github.com/suan/vim-instant-markdown      
   安装了vim-instant-markdown插件,输入下面这个命令可以进行预览：  
   `:InstantMarkdownPreview`    

   * 安装新版的`node.js`  
     ```
     sudo add-apt-repository ppa:chris-lea/node.js
     sudo apt-get update
     sudo apt-get install nodejs
     ```
   * 安装instant-markdown-d  
     `sudo npm -g install instant-markdown-d`  
   * 按照参考链接的步骤配置vim配置文件/usr/share/vim/vimrc  
   * 下载项目链接里的文件，放在~/.vim下,修改配置文件，加入`Plugin 文件地址`    
   * 在vim里面运行PluginInstall,安装完成  
   * 在vimrc里面设置InstantMarkdown禁止自动启动，而是通过输入命令启动，并更改其启动的命令的名称：  
	> vimrc中加入`command Preview InstantMarkdownPreview` 简单更改为`Preview`  
