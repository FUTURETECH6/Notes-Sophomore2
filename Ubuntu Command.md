```bash
ps
kill -9 {PID}

zip -r {filename}.zip dirName

screen -S {screenName}
screen -ls
screen -r shadowflow
screen -S shadowflow -X quit
ctrl+a+d

served {PortName}
simplefileserver ({PortName})

axel -n 100 http://ip/takeout.zip
aria2c http://ip/takeout.zip
aria2c -x 2 http://ip/takeout.zip
aria2c -s 2 -x 2 -j 10 http://ip/takeout.zip # -s这个参数的意思是使用几个线程进行下载，-x是最大使用几个线程下载，-j就是同时下载几个文件。
aria2c -c http://ip/takeout.zip	# 使用 c 选项可以断点续传文件

ls -l ({filename})
ls -sh ({filename})
du -sh

tar -zcf calculator.tar.gz calculator
tar -zxf calculator.tar.gz

usermod {user} -G GROUP1[,GROUP2,...[,GROUPN]]]

cp -r
rm -r

# mv 不能 "/*"否则会丢掉隐藏文件如".git"
mv /home/vivek/data/ /nas/home/vivek/archived/	# dest的最后'/'少了则会变成吧data下所有文件移到archived下而不是data自己移过去

fg

# 改完".gitmodules"后需执行
git submodule init --recursive	# 等效于 git submodule update --init --recursive
git submodule sync --recursive

locate *.swp | xargs rm

$ git clone http://github.com/large-repository --depth 1
$ cd large-repository
$ git fetch --unshallow

diskutil unmount /Volumes/ExtremeSSD 
```

