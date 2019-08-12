---
title: Raneto配置中文搜索
date: 2018-09-20 20:09:50
category: 后台
tags: nodejs
---

![](http://raneto.com/images/raneto.png)

<!-- more -->

因工作需要，准备给公司搞个软件说明文档。但是这种东西我们又不想自己搭，于是就想找个开源的文档库。找啊找，于是就找到[Raneto](http://raneto.com)这么个东西。感觉还还不错，就决定用了。

官方文档还是相当清晰的。直接去github克隆，再直接npm start一下，就跑起来了。

然后我就把exampale文夹给换掉了。package.json文件夹的启动目录也换掉了。一切貌似都很顺利啊，但最后我居然发现不支持中文搜索。心里一惊，这还怎么玩！

我就不信了，Google搜起来。于是我就看到了这篇文章[Raneto部署](https://blog.csdn.net/xinhui88/article/details/52495156)。在文章的最后，作者给出了如下写道：

> ### 解决不能搜索中文的BUG
>>先下载一个lunr.js，链接：[https://github.com/codepiano/lunr.js](https://github.com/codepiano/lunr.js)，解压并将文件夹命名为lunr，接下来进入node_modules目录，找到里面的lunr重命名为lunr2（不建议删除），然后再将刚下载的lunr复制进去。
还要改一个地方，进入node_modules/raneto-core/node_modules目录，刚上面那个是一样的，将这里的lunr重命名为lunr2，再将刚下载的lunr复制进去。
再次重启，这里重启的时候可能会出现error，原因是node.js少了一些库，仔细看缺了哪些库，然后npm install 

然后我很兴奋啊，跟着步骤来啊。但是，我居然发现我找不到node_modules/raneto-core这个文件夹！

后来我在github才发现[gilbitron/Raneto-Core](https://github.com/gilbitron/Raneto-Core)这个库已经被直接合并到[gilbitron/Raneto](https://github.com/gilbitron/Raneto)

> ## Raneto Core
>> ## LOCKED
>> This codebase has been merged into the main Raneto project
>> No new issues or PRs will be accepted
>> Please visit the main Raneto repository

后来我就直接去Raneto的仓库上搜issues了。看到这一条[Can't search in Chinese. #41](https://github.com/gilbitron/Raneto/issues/41)。其中有人提到[codepiano/lunr.js](https://github.com/codepiano/lunr.js)很有用。

但是它的readme也没具体讲我怎么植入Raneto啊。

于是我又去[lunr官网](https://lunrjs.com/)，想看看到底是个什么东西。文档说的很简单。我当时就把其思路简单理了下。我觉得大概思路就是，把要搜索的内容以一个数据结构来表示，然后把他们一个个放进个队列里，搜索的时候就直接遍历这个数组就好了。

比如，每篇文章都大概能分成三部分，标题，时间跟内容。我以这样的结构来表示`{title:'标题', dateTime:'2018-01-01',conent:'文章内容'}`.每篇文章都是这么个结构，把它们全部放到一个数组里面，搜索就是遍历这个数组里的每一项。但是既然要搜，我肯定要指定匹配数组里的每个对象的哪些个字段。于是lunr里文档里，有这么个说明。

```javascript
var idx = lunr(function () {
    this.field('title')
    this.field('body')

    this.add({
        "title": "Twelfth-Night",
        "body": "If music be the food of love, play on: Give me excess of it…",
        "author": "William Shakespeare",
        "id": "1"
    })
}) 

```
这里的 `this.field('title'); this.field('body')` 就是指定要搜索匹配内容的字段名。上述意思就是，指定匹配标题跟内容字段，但是不匹配时间跟作者名。

我觉得大概就这么个意思，具体就没深究了。

说了上面一大堆，支持中文的lunr的库也找到，那怎么改Raneto呢？

首先，我先找到了Raneto文件里的`app/core/search.js`文件。看到这行方法：

```javascript
    function getLunr (config) {
        if (instance === null) {
            instance = require('lunr');
            require('lunr-languages/lunr.stemmer.support')(instance);
            require('lunr-languages/lunr.multi')(instance);
            config.searchExtraLanguages.forEach(lang =>
            require('lunr-languages/lunr.' + lang)(instance)
            );
        }
        return instance;
    }
```

而这个方法又在search.js暴露出去的handler方法里调用。

```javascript
    function handler (query, config) {
        const contentDir = utils.normalizeDir(path.normalize(config.content_dir));
        const documents = glob
            .sync(contentDir + '**/*.md')
            .map(filePath => contentProcessors.extractDocument(
            contentDir, filePath, config.debug
            ))
            .filter(doc => doc !== null);

        const lunrInstance = getLunr(config);
        const idx = lunrInstance(function () {
            this.use(getStemmers(config));
            this.field('title');
            this.field('body');
            this.ref('id');
            documents.forEach((doc) => this.add(doc), this);
        });

        const results       = idx.search(query);
        const searchResults = [];

        results.forEach(result => {
            const p = pageHandler(contentDir + result.ref, config);
            p.excerpt = p.excerpt.replace(new RegExp('(' + query + ')', 'gim'), '<span class="search-query">$1</span>');
            searchResults.push(p);
        });

        return searchResults;
    }
```

显然`getLunr`这个方法是根据`config.default.js`的` searchExtraLanguages: ['ru']`配置，加载支持特定语言搜索的lunr文件的。但是`lunr-languages\`里面没有一个支持中文的。于是我干脆就都给注释掉了，并把我下载好的`codepiano/lunr.js`文件，复制到了其同级目录，并引入。

```javascript
    function getLunr (config) {
        if (instance === null) {
            instance = require('./lunr.js');//该文件lunr支持中文搜索
            // require('lunr-languages/lunr.stemmer.support')(instance);
            // require('lunr-languages/lunr.multi')(instance);
            // config.searchExtraLanguages.forEach(lang =>
            //   require('lunr-languages/lunr.' + lang)(instance)
            // );
        }
        return instance;
    }
```

同时注释掉了，`handler`里的`this.use(getStemmers(config))`调用。因为我看getStemmers也是加载`config.searchExtraLanguages`里的语言配置。感觉没什么用。

```javascript
function handler (query, config) {
    const contentDir = utils.normalizeDir(path.normalize(config.content_dir));
    const documents = glob
        .sync(contentDir + '**/*.md')
        .map(filePath => contentProcessors.extractDocument(
        contentDir, filePath, config.debug
        ))
        .filter(doc => doc !== null);

    const lunrInstance = getLunr(config);
    const idx = lunrInstance(function () {
    //注释掉默认设置加载的lunr配置。因为上面的 lunr 文件支持中文。如果又配置config里的东西，将又还原成默认的语言搜素配置。将使中文失效。
    //this.use(getStemmers(config)); 
    this.field('title');
    this.field('body');
    this.ref('id');
    documents.forEach((doc) => this.add(doc), this);
});
```

重启，直接报错！缺少`nodejieba`模块。那就装一下，`npm install --save nodejieba`。

最后再次重启，感觉成了！！！就这么个东西，也是折腾了我很多时间。
