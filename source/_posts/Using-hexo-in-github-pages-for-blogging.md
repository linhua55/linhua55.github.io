# Use Hexo for blogging in github pages

## Install

### NPM

    sudo pacman -S npm

#### China mirror

    sudo npm config set registry https://registry.npm.taobao.org


### Hexo

As pacman install the npm in the `/usr` path.[^fn1] We need root priveledge to install Hexo. But this will make running npm and Hexo need root previledge.


    ##npm config get prefix
  
    sudo npm install hexo-cli -g[^fn2]
     


## Setup a blog

    $ sudo hexo init blog

Need root priveledge, and the current directory need to not be in NTFS or FAT filesystem. As it will make symbolic link.  And then you should better change the ownership.

    ```bash
    $ sudo chown -R vagrant:vagrant blog
    ```

I use `vagrant:vagrant` here, you should set yourself `userName:groupName`.

## Hexo plugin
### git[^fn3]

    ```bash
    $ npm install hexo-deployer-git --save
    ```
    
    
This will install it in the local directory, the `blog` directory.

Edit settings. `_config.yml`

    ```yaml
    deploy:
      type: git
      repo: <repository url>
      branch: [branch]
    ```

### mathjax[^fn4][^fn5]

    ```bash
    $ npm install hexo-math --save
    ```

### footnote
Hexo's default markdown render don't support footnote. use `hexo-renderer-markdown-it`[^fn6] instead.


#### install

    ```bash
    $ npm un hexo-renderer-marked --save
    $ npm i hexo-renderer-markdown-it --save
    ```

#### configure[^fn7]

    ```yaml
    markdown:
      render:
        html: true
        xhtmlOut: false
        breaks: false
        linkify: true
        typographer: true
        quotes: '“”‘’'
      plugins:
	        - markdown-it-footnote
	        - markdown-it-sup
	        - markdown-it-sub
	        - markdown-it-abbr
	    anchors:
	        level: 1
	        collisionSuffix: 'v'
	        permalink: true
	        permalinkClass: header-anchor
	        permalinkSymbol: ' '
    ```

If under the footnote, there has some block, such as `code block`. The code block will be act as footnote. You should put the footnote to the end of the article Or use the `Horizontal rule/line` to cut them.

**Attention:** the `Horizontal rule/line break` is `***` here. Not `---`.

### comment [Todo]

## Use
### Create a new post
    $ sudo hexo new "Hello Hexo"
### Generate static files
    $ sudo hexo generate
  
  or
        
    $ sudo hexo g
### Deploy

    $ sudo hexo deploy
or

    $ sudo hexo d
or with generate

    $ sudo hexo d -g

If it didn't pop up the input hint to input name/password. You should manually git push.

    $ cd blog/.deploy_git
    $ git push

## Backup (Use hexo-git-backup)
Use git and .gitignore to backup other source directory to another branch.
Make a shell script.

Or  use https://github.com/coneycode/hexo-git-backup

    $ npm install hexo-git-backup --save

You should configure this plugin in `_config.yml`.

    backup:
        type: git
        repository:
           github: git@github.com:xxx/xxx.git,branchName
           gitcafe: git@github.com:xxx/xxx.git,branchName

***

    $ hexo backup
or

    $ hexo b
    

## 删除文章
https://joilence.github.io/2016/07/14/improve-blog/
在使用 hexo new 生成新文章之后，想要删除，如果只是直接在 /source/_post 中将相关 md 文件删除，可能在生成网页后还会重现。删除文章比较完备的步骤如下：

在 /source/_post 中删除相关 md 文件
在博客根目录下使用 sudo hexo clean
删除博客根目录下的 db.json 文件
再重新生成网页就可以了。

http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more


[^fn1]:  https://docs.npmjs.com/getting-started/fixing-npm-permissions. 

[^fn2]: https://github.com/hexojs/hexo/

[^fn3]:  https://hexo.io/docs/deployment.html#Git

[^fn4]: https://github.com/akfish/hexo-math

[^fn5]: https://github.com/akfish/hexo-math/issues/26

[^fn6]:  https://github.com/celsomiranda/hexo-renderer-markdown-it

[^fn7]: https://joilence.github.io/2016/07/14/improve-blog/



