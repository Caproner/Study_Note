# Unix环境高级编程（APUE）

大概掂了一下重量。嗯。跟算法导论差不多。

算导我大学整整三年都没看完，这本估计也是路漫漫了。

查了一下貌似并不是什么萌新向的书。好吧有得我看了orz

## 第1章 UNIX基础知识

### 登录

用户在键入登录名和口令之后，系统在其口令文件（通常为`/etc/passwd`）中查看登录名。

在口令文件中，一行登录项代表一个用户，其格式为：

```shell
登录名:加密口令:数字用户ID:数字组ID:注释字段:起始目录:shell程序
```

例如：

```
sar:x:205:105:Stephen Rago:/home/sar:/bin/ksh
```

其中，加密口令储存在另一个文件

### 文件和目录

+ 文件名只能使用字母、数字、句点（.）、短横线（-）和下划线（_）

+ 创建新目录时会自动创建两个文件名：`.`和`..`。其中`.`指向当前目录，`..`指向父级目录

  + 当当前目录为根时，`..`指向当前目录

+ 查看某个命令的命令手册页（以ls为例）：

  ```shell
  man 1 ls
  man -s1 ls
  ```

+ 对`ls`的一个简单实现

  ```c
  #include "apue.h"
  #include <dirent.h>
  
  int main(int argc, char *argv[])
  {
      DIR				*dp;
      struct dirent	*dirp;
      
      if(argc != 2)
      {
          err_quit("usage: ls directory_name");
      }
      if((dp = opendir(argv[1])) == NULL)
      {
          err_sys("can't open %s", argv[1]);
      }
      
      while((dirp = readdir(dp)) != NULL)
      {
          printf("%s\n", dirp->d_name);
      }
      
      closedir(dp);
      exit(0);
  }
  ```

  + 头文件`apue.h`包括一些通用的头文件，之后的示例都会出现这个头文件
  + `dirent.h`头文件包含了`opedir`和`readdir`等，用于对文件目录进行操作
  + 