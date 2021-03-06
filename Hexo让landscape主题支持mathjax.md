## 步骤

#### 修改after-footer.ejs文件  
修改`themes/landscape/layout/_partial/after-footer.ejs`文件，添加下面的内容：  
```html
<!--如果"本theme的_config.yml"配置了mathjax的enable为真
    并且"page has `mathjax: true` in Front-matter"
    那么使用"mathjax.ejs"-->
<% if (theme.mathjax && theme.mathjax.enable && page.mathjax){ %>
<%- partial('mathjax') %>
<% } %>
```
可以将该内容添加到`<%- js('js/script') %>`之前。  

#### 创建mathjax.ejs文件  
创建`themes/landscape/layout/_partial/mathjax.ejs`文件，其内容如下：  
```html
<!-- https://github.com/xiangming/landscape-plus/blob/master/layout/_partial/mathjax.ejs -->
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: { inlineMath: [['$','$'], ["\\(","\\)"]], processEscapes: true }
});
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: { skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'] }
});
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Queue(function() {
  var all = MathJax.Hub.getAllJax(), i;
  for(i=0; i < all.length; i += 1) {
    all[i].SourceElement().parentNode.className += ' has-jax';
  }
});
</script>
<% if (theme.mathjax && theme.mathjax.local){ %>
  <!-- 链接 https://hexo.io/zh-cn/docs/helpers#url-for 解释了 url_for 的用法 -->
                                          <!-- ./source/MathJax-2.7.9/MathJax.js -->
  <script type="text/javascript" src= "<%- url_for() %>/MathJax-2.7.9/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<% } else { %>
  <script type="text/javascript" src="//cdn.bootcss.com/mathjax/2.7.9/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<% } %>
```

#### 修改_config.yml文件  
修改`themes/landscape/_config.yml`文件，添加下面的内容：  
```yml
# mathjax
## https://github.com/xiangming/landscape-plus
## 针对中国大陆地区对hexo官方主题landscape进行优化.
## Hexo让landscape支持mathjax
## enable: (bool,我自己定义的)启用mathjax
##  local: (bool,我自己定义的)从本地加载MathJax(预留字段,尚未实现)
mathjax:
  enable: true
#  local: true
```
如果"本theme的_config.yml"配置了mathjax的enable为真，并且"page has `mathjax: true` in Front-matter"，那么便启用mathjax了。  


## 思路  
无意间看到了主题`landscape-plus`：针对中国大陆的hexo用户，优化hexo官方主题landscape。新增mathjax模块。  
[针对中国大陆地区对hexo官方主题landscape进行优化](https://github.com/xiangming/landscape-plus)。  
我们可以将`landscape-plus`中的`mathjax`模块取出来，人工并入`landscape`中。  

#### 找出mathjax相关的代码  
在`landscape-plus`的根目录执行`grep -irl 'mathjax' ./.`可以找到相关文件。  
用`Beyond Compare`对比`landscape-plus`和`landscape`的文件，可以找到相关的差异。  

#### 让landscape支持mathjax  
将`./themes/landscape-plus/layout/_partial/mathjax.ejs`拷贝到`landscape`的对应目录下。  
将`./themes/landscape-plus/layout/_partial/after-footer.ejs`里面涉及`mathjax`的部分合并到`landscape`的对应文件中。  
将`./themes/landscape-plus/_config.yml`的`mathjax: true`配置合并到`landscape`的对应文件中。  
其中，修改`after-footer.ejs`添加：
```html
<% if (((config.mathjax || theme.mathjax) && page.mathjax)){ %>
<%- partial('mathjax') %>
<% } %>
```

## 其他  
可能用到的一些东西。  

#### 源码和配置相关  
保持默认的`hexo-renderer-marked`就行，无需更换到`hexo-renderer-kramed`。  
执行`npm install mathjax --save`将会生成`./node_modules/mathjax`文件夹。  
[mathjax - npm](https://www.npmjs.com/package/mathjax)。  
[mathjax/MathJax-src: MathJax source code for version 3 and beyond](https://github.com/mathjax/MathJax-src)。  
[MathJax Documentation — MathJax 3.0 documentation](http://docs.mathjax.org/en/latest/index.html)。  
[MathJax-2.7.6.zip](https://github.com/mathjax/MathJax/archive/2.7.6.zip)。  
[mathjax/MathJax: Beautiful math in all browsers](https://github.com/mathjax/MathJax)。  
[MathJax 中文文档 — MathJax Chinese Doc 2.0 documentation](https://mathjax-chinese-doc.readthedocs.io/en/latest/)。  
[MathJax-docs/start.rst at v2.7-latest](https://github.com/mathjax/MathJax-docs/blob/v2.7-latest/start.rst)。  

#### 公式编写指南  
[在线 Markdown MathJax 编辑器](https://kerzol.github.io/markdown-mathjax/editor.html)。  
[CSDN-markdown语法之如何使用LaTeX语法编写数学公式](https://blog.csdn.net/lanxuezaipiao/article/details/44341645)。  
[在Hexo中渲染MathJax数学公式](https://www.cnblogs.com/wangxin37/p/8185688.html)。  
[Markdown LaTeX 数学符号速查表](https://www.rdtoc.com/tutorial/markdown-latex-tutorial.html)。  
[Markdown MathJax 公式](https://www.rdtoc.com/tutorial/markdown-mathjax-tutorial.html)。  
[CSDN Markdown简明教程3-表格和公式](https://blog.csdn.net/whqet/article/details/44277965)。  
[maths-symbols.pdf](http://mirrors.sjtug.sjtu.edu.cn/ctan/info/symbols/math/maths-symbols.pdf)。  

#### 数学公式示例  
如果不能正常显示公式，可以参考下面的公式的写法，用空格分隔开各个变量/元素，这样能解决大多数显示问题。  
$ E = mc^2 $  
$ V = \frac{4}{3} \pi R^3 $  
$ log _a (M \bullet N) = log _a M + log _a N $  
$ log _a \frac{M}{N} = log _a M - log _a N $  
$ log _{a^n} M = \frac{1}{n} log _a M $  
$ log _a M^n = n log _a M $  
$ log _a b = \frac{log _c b}{log _c a} $  
$ log _a b = \frac{1}{log _b a} $  
