### hexo更新需知

打开Git bash之前一定要使用`git branch`确认分支为hexo后，才能执行SourceDeploy.sh的批处理shell命令文件

不使用shell命令上传，则可以自己手动执行下面代码

```shell
git add .;
git commit -m "source code update"; 
git push origin hexo;
```

否则需要自己手动切换分支`git checkout master`

执行`hexo clean`+`hexo generate`



接着可以手动执行下面的命令或者直接执行deploy.sh批处理shell命令

```shell
hexo generate;
cp -R public/* .deploy/;
cd .deploy/;
git add .;
git commit -m “update”;
git push origin master
```

