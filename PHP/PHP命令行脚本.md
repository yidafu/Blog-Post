---
title: PHP命令行脚本
date: '2017-12-18'
tags:
  - php
  - shell
---

## 汤头

遇到一个问题:要把一个 git 库的 commit 重新拆分. 由于 git 的 commit 历史是不可以修改的.所以,可行方案只有 checkout 每个 commit 到一个单独的文件夹再做拆分.

<!--more-->

## 解决方案

本来打算采用最简(fei)单(shi)方法, 手动操作(反正也就十几二十个 commit).

在导师的建议下尝试用 PHP 写一个脚本来批量完成这个任务.

## 菜!菜!!

```php
#!/usr/bin/php

<?php
$log  = [];
$state;
/*
进入工作目录
*/
exec("cd ~/[path/to/git-repo] && git log", $log, $state);

$id = 0; /*编号*/
/*
@NOTE 小坑：linux换行必须使用**双引号**包裹字符串
*/
foreach ($log as $log_item) {
    if ( preg_match("/^commit /", $log_item ) ) {
        $commit_id = substr($log_item, 6 );
        print("checkout ... " . $commit_id ."\n");
        $msg = [];
        ++ $id;
        /*
        拼装命令
        */
        exec('cd ~/[path/to/git-repo] '
                . ' && git reset --hard '
                . $commit_id
                . '&& cd ../ && cp -rf ./[git-repo] ./checkout-'
                . $id . '-' . substr( $commit_id , 1, 6)
            , $msg );
        print_r("[info]:" . $msg[0] . "\n");
    }
}
/*
重置到最新的分支
*/
exec( 'cd ~/[path/to/git-repo] && git reset --hard ' . substr($log[0], 6) );
echo "[info]: 重置回默认版本号 \n";
echo "over! and enjoy!! \n";
 ?>
```
