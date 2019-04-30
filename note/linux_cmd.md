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


