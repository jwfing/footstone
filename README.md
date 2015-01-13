# footstone

这是一个有关代码检查、单元测试的 repo，记录了我们开发过程中的工程标准和相应工具。我们使用了如下开源组件来保证我们的工程质量：

* uncrustify
* maven
* checkstyle
* findbugs
* junit
* cobertura maven plugin
* gradle
* blablabla

## 我们为什么要做这些

因为我们公司做的是移动云服务 [LeanCloud](http://leancloud.cn)，为了提供更高质量的服务，我们对 sdk source/docs/sample 都制定了严格的规范，而如何让这些规范切实执行起来，则需要我们寻找一些工具，让规范与日常的开发流程无缝结合起来。

我们首先关注编码规范和单元测试，因为我们使用 git 来做代码管理，使用 jenkins 来自动编译、发布，所以希望做到：

* 代码提交之前即进行编码规范检查，不符合规范的代码不允许提交。
* 模块发布之前，进行系统的单元测试，如果单元测试出现问题，或者最终测试覆盖率达不到要求，都不允许发布。

blablablabla

## 对于 objective-c 代码，我们是如何要求编码规范的

对于 objective-c 代码，我们直接套用了 [Google Objective-C 编码规范](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)。

为了在 git commit 的时候进行代码检查，我们使用了新的 hook 和 [uncrustify](https://github.com/bengardner/uncrustify) 这一检查工具。关于 uncrustify 的使用，大家可以访问 http://uncrustify.sourceforge.net/ 获取更多信息，我已经编译好了一个可以在 Mac OS X 上运行的可执行文件，放到了 scripts/uncrustify。

### 如何配置 pre-commit hook

首先明确一点，我们需要在项目根目录下 .git/hooks/ 目录下，新建一个名为「pre-commit」的脚本文件。可以直接把 scripts/objc/ 目录下的 pre-commit/canonicalize_filename.sh 两个脚本拷贝到项目根目录下 .git/hooks/ 目录下。

> 注意：请按照实际部署结构修改 pre-commit 脚本中有关 uncrustify 命令和配置文件的路径。
> 我们假设你把本工程放到了个人 HOME 目录下，所以 scripts/objc/pre-commit 脚本中对于 UNCRUSTIFY 和 CONFIG 两个变量设置了默认值。

最后部署完毕之后你可能得到如下的目录结构：
<pre>
$HOME
  |- footstone
  |      |- scripts
  |      |    |- uncrustify
  |      |- config
  |           |- objc.cfg
  |- $YOURREPO
         |- .git
         |    |- hooks
         |        |- pre-commit
         |        |- canonicalize_filename.sh
         |- YourProject
</pre>

### 如何使用 uncrustify 进行 codestyle 检查

好了，配置好上面的 hook 之后，我们就可以使用测试一下了。
我们先随意修改一个文件，然后

```
$ git add yourtestfile.h
$ git commit -m 'test checkstyle'
```

如果代码规范检查不通过，可能会出现如下提示消息：

```
You can apply these changes with:
 git apply /tmp/pre-commit-uncrustify-1421132126.patch
(may need to be called from the root directory of your repository)
Aborting commit. Apply changes and commit again or skip checking with --no-verify (not recommended).
```

uncrustify 会自动生成一个 patch 文件，你只需要 apply 这个 patch，即可以得到一份干净漂亮的 source 了，这也是 uncrustify 使用上比较方便的一点。

## 对于 android 代码，我们是如何要求编码规范的

对于 objective-c 代码，我们直接套用了 [Google Java 编码规范](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html)。

为了在 git commit 的时候进行代码检查，我们使用了 pre-commit hook 和 [CheckStyle](http://checkstyle.sourceforge.net/)。

### 如何配置 pre-commit hook

首先，请把 scripts/java/pre-commit 脚本拷贝到项目根目录下 .git/hooks/ 目录下。
接下来，请增加两个 git config 项：

```
$ git config --add checkstyle.jar ~/footstone/libs/checkstyle-6.2-all.jar
$ git config --add checkstyle.checkfile ~/footstone/config/google_checks.xml
```
> 注意：请根据实际部署结构设置 checkstyle jar 文件路径和配置文件路径。
> 我们假设你把本工程放到了个人 HOME 目录下，所以 scripts/objc/pre-commit 脚本中对于 UNCRUSTIFY 和 CONFIG 两个变量设置了默认值

### 如何进行 codestyle 检查

现在，你可以试试修改一下工程中的 java 代码，然后尝试提交：

```
$ git add yourtestfile.java
$ git commit -m 'just for test checkstyle'
```

如果代码规范检查不过，可能会出现如下提示消息：
<pre>
/Userscloud/AndroidSDK/cloudpush/src/com/cloudpush/push/BUConnectivityListener.java:8: warning: Abbreviation in name must contain no more than '1' capital letters.
/Userscloud/AndroidSDK/cloudpush/src/com/cloudpush/push/BUConnectivityListener.java:9: warning: 'method def modifier' have incorrect indentation level 18, expected level should be 2.
/Userscloud/AndroidSDK/cloudpush/src/com/cloudpush/push/BUConnectivityListener.java:9:1: warning: Line contains a tab character.
Audit done.
Commit aborted.
</pre>
这时候你只能根据提示修改相应部分，然后再次提交了。

## 对于 objective-c 代码，我们是如何进行单元测试的

blablablabla

## 对于 android 代码，我们是如何进行单元测试的

blablablabla

