## 设置tmux参数
一般在.tmux.conf中，如果没有这个文件，使用  
`tmux show -g | cat > ~/.tmux.conf`  
可以在这个文件中配置tmux前缀，默认是Ctrl+b  

## 快捷键参考
按下 Ctrl-b 后的快捷键如下:  
### 基础
* ? 获取帮助信息  
### 会话管理
* s 列出所有会话  
* $ 重命名当前的会话  
* d 断开当前的会话  
### 窗口管理
* c 创建一个新窗口  
* , 重命名当前窗口  
* w 列出所有窗口  
* % 水平分割窗口  
* " 竖直分割窗口  
* n 选择下一个窗口  
* p 选择上一个窗口  
* 0~9 选择0~9对应的窗口  
### 窗格管理
* % 创建一个水平窗格  
* " 创建一个竖直窗格
* o 在窗格间切换  
* } 与下一个窗格交换位置  
* { 与上一个窗格交换位置  
* ! 在新窗口中显示当前窗格  
* x 关闭当前窗格  

## 设置前缀
修改~/.tmux.conf文件，输入:  
set -g prefix C-a  
unbind C-b  

然后在tmux中，输入C+b : 进入命令模式，输入:  
source-file ~/.tmux.conf  
也可以在.tmux.conf中用r去bind这个命令，这样每次只要输入r就可以了  


参考链接:  
http://mingxinglai.com/cn/2012/09/tmux/  
http://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/



