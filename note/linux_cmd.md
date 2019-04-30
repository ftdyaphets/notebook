#shell修改配置文件

```shell
#!/bin/bash

CONF=conf.sh    
:<<EOF      #EOF中的内容不执行，等价于多行注释
	#conf.sh
		export aaa = 111
		EOF
		set_key_value() {                                                             
		    local key=${1}                                                           
		        local value=${2}  
			    if [ -n $value ]; then   
			            #echo $value    
				            local current=$(sed -n -e "s/^\($key = \)\([^ ']*\)\(.*\)$/\2/p" $CONF)            
					            if [ -n $current ];then 
						                echo "setting $CONF : $key = $value"
								            value="$(echo "${value}" | sed 's|[&]|\\&|g')"
									                sed -i "s|^export [#]*[ ]*${key}\([ ]*\)=.*|export ${key} = ${value}|" ${CONF} 
											        fi      
												    fi          
												    }                                                                                                  
												    set_key_value "abc" "111"                                         

```

#shell替换文件内容

```shell
sed -i "s/LOCAL_DATA_PATH=\${LOCAL_ROOTPATH}\/data/LOCAL_DATA_PATH=\${LOCAL_ROOTPATH}\/data\/dr/" conf.sh                  #特殊字符用\转义，sed -i "s/$oldstr/$newstr/" conf.sh    替换多次末尾用/g
```

#文本内搜索

vim进入文本，/ 开启搜索，输入内容，回车，n或N跳转到下一个结果

cat part-00000 |grep 13170771 -C 5   查找part-00000文件中匹配内容为13170771的结果及其前后5行，-A 5 后5行，-B 5 前5行

grep -c 统计查找结果行数

#shell切换目录

```shell
#!/bin/sh  
# chdir.sh
cd /home/user/Downloads  
pwd  
```

在shell环境下通过./chdir.sh执行这段脚本是无法进入Downloads目录的； 这是因为shell在执行脚本时，会创建一个子shell，并在子shell中逐个执行脚本中的指令； 而子shell中从父shell中继承了环境变量，但是执行后不会改变父shell的环境变量；如果想要代码中切换目录的操作生效，只需要通过source 命令执行即可：

```shell
source ./chdir.sh  #或者  . ./chdir.sh (source可以用.代替)
```

#cd切换目录

```shell
cd    # 等价于 cd ~，进入主目录
cd -  #返回进入此目录之前所在的目录
cd !$ #把上个命令的参数作为cd参数使用
```

##### #硬链接与软链接

​    硬连接指通过索引节点来进行连接，多个文件名指向同一索引节点。硬连接的作用是允许一个文件拥有多个有效路径名，
这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。只删除一个连接并不影响索引节点本身和其它的连接，
只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，
文件真正删除的条件是与之相关的所有硬连接文件均被删除。
​    另外一种连接称之为符号连接（Symbolic Link），也叫软连接。软链接文件有类似于Windows的快捷方式。可以进行读写。
它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。
​    不能给目录创建硬链接，只有同一文件系统的文件且是超级用户才能建立硬链接，软链接没这种限制。软链接缺点是
源文件的内容可以同步更新，但路径信息不会更新，源文件路径改变时无法访问，并且保存路径信息需要额外空间。
ln srcFile targetFile  硬链接
ln -s srcFile targetFile  软链接
 ls -li     # -i参数显示文件的inode节点信息