
默记  
touch  
cat  
tac  
more  
less  
head  
tail  
od  
grep  
cut  
uniq  
tee  
tr  
ps  
pstree  
top  
netstat  
chmod   
ln   
ls  
mkdir  
rmdir  
cp   
mv  
which  
whereis  
locate  
find  
gzip  
pwd  
ifconfig  
ping  


## 常用命令  
- touch：创建文件  
- cat：取得文件内容  
- tac：cat反向操作，从最后一行开始打印  
- more：一页一页查看文件内容，适合大文件查看  
- less：比more多了一个向前翻页的功能  
- head：取文件前几行  
- tail：head反向操作，取最后几行  
- od：以字符或者十六进制的形式显示二进制文件  
- grep：使用正则表示式进行全局查找并打印    

管道指令：  
- cut：对数据进行切分，取出想要的部分    
- uniq：可以将重复的数据只取一个  
- tee：双向输出重定向  
- tr：用来删除一行中的字符，或者对字符进行替换  

进程管理：  
- ps：查看某个时间点的进程信息  
- pstree：查看进程树  
- top：实时显示进程信息  
- netstat：查看占用端口的进程  


- chmod 777 某某：修改目录或文件的权限为777(读4，写2，执行1)  
- ln：链接  
- ls  
- mkdir  
- rmdir：删除目录  
- touch：更新文件时间或建立新文件  
- cp：复制  
- mv：移动文件  
- which：指令搜索  
- whereis：文件搜索  
- locate：文件搜索，可用关键字或正则表达式  
- find：文件搜索，可以使用文件的属性和权限进行搜索  
- gzip/tar：压缩  
- pwd：显示当前所在位置  

网络通信：  
- ifconfig：查看当前系统的网卡信息  
- ping：查看与某台机器的连接情况  
- netstat -an：查看当前系统的端口使用  



## 文件系统  

- inode：一个文件占用一个 inode，记录文件的属性，同时记录此文件的内容所在的 block 编号；  
- block：记录文件的内容，文件太大时，会占用多个 block。   

































