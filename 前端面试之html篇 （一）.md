1 . `doctype(文档类型) 的作用是什么？`   ☆

- 对文档进行有效性验证:
    它告诉用户代理和校验器这个文档是按照什么DTD 写的。这个动作是被动的，
    每次页面加载时，浏览器并不会下载DTD 并检查合法性，只有当手动校验页面时才启用。

- 决定浏览器的呈现模式: 对于实际操作，通知浏览器读取文档时用哪种解析算法。
    如果没有写，则浏览器则根据自身的规则对代码进行解析，可能会严重影响HTML 排版布局。

- 浏览器有三种方式解析HTML文档。
     - 非怪异（标准）模式
     - 怪异模式
     - 部分怪异（近乎标准）模式

***


2 . `HTML 和 XHTML 有什么区别？？ 如果页面使用 'application/xhtml+xml' 会有什么问题吗？`

 > 答：xhtml 语法要求严格，一旦遇到错误，立刻停止解析，并显示错误信息。
    如果页面使用'application/xhtml+xml',一些老的浏览器会不兼容。

- XHTML 元素必须被正确地嵌套。
- XHTML 元素必须被关闭。
- 标签名必须用小写字母。
- XHTML 文档必须拥有根元素。
- 所有属性都必须使用双引号

***

3 . `如果网页内容需要支持多语言，你会怎么做？在设计和开发多语言网站时，有哪些问题你必须要考虑？`

> 答：编码使用UTF-8，空间域名需要支持多浏览地址,准备多套模板。

    在设计和开发多语言网站时，需要考虑:

- 应用字符集的选择
- 语言书写习惯&导航结构
- 数据库驱动型网站
- css 盒子会因为内容尺寸不一样出现不对齐偏移

***

4 . `使用 data- 属性的好处是什么？`

> 答：data-为前端开发者提供自定义属性，这些属性集可以通过对象的dataset属性获取，
    不支持该属性的浏览器可以通过getAttribute方法获取:


```
    <div data-author="david" data-time="2011-06-20" data-comment-num="10">...</div>

    div.dataset.commentNum; // 10
```


 需要注意的是，data-之后的以连字符分割的多个单词组成的属性，获取的时候使用驼峰风格。
 并不是所有的浏览器都支持.dataset属性，测试的浏览器中只有Chrome 和Opera 支持。

***

5 . `请描述 cookies、sessionStorage 和 localStorage 的区别。`  ☆ ☆  ☆

>  答 sessionStorage、localStorage、cookie都是在浏览器端存储的数据 有了本地数据，
    就可以避免数据在浏览器和服务器间不必要地来回传递。
    sessionStorage 和 localStorage 是HTML5 Web Storage API 提供的，可用于web请求之间保存数据。


- cookies会发送到服务器端。其余两个不会。Cookie每个域名存储量比较小（各浏览器不同，大致4K）所有域名的存储量有限制（各浏览器不同，大致4K）有个数限制（各浏览器不同）
会随请求发送到服务器

- LocalStorage 永久存储 单个域名存储量比较大（推荐5MB，各浏览器不同）总体数量无限制

- SessionStorage 只在 Session 内有效 存储量更大（推荐没有限制，但是实际上各浏览器也不同）
    sessionStorage 的概念很特别，引入了一个“浏览器窗口”的概念。
    sessionStorage 是在同源的同窗口（或tab）中，始终存在的数据。
    也就是说只要这个浏览器窗口没有关闭，即使刷新页面或进入同源另一页面，数据仍然存在。
    关闭窗口后，sessionStorage 即被销毁。同时“独立”打开的不同窗口，即使是同一页面，sessionStorage 对象也是不同的

***

6 . `请解释 <script>、<script async> 和 <script defer> 的区别。`

答：
- script
当解析器遇到 script 标签时，文档的解析将停止，并立即下载并执行脚本，脚本执行完毕后将继续解析文档。

- defer script
当解析器遇到 script 标签时，文档的解析不会停止，其他线程将下载脚本，待到文档解析完成，脚本才会执行。

- async script
当解析器遇到 script 标签时，文档的解析不会停止，其他线程将下载脚本，脚本下载完成后开始执行脚本，脚本执行的过程中文档将停止解析，直到脚本执行完毕。

    如果脚本不依赖于任何脚本，并不被任何脚本依赖，那么则使用 defer。
    如果脚本是模块化的，不依赖于任何脚本，那么则使用 async。

    defer 的脚本是按照声明顺序执行的。而 async 的脚本不同，只要脚本下载完成，将会立即执行，未必会按照声明顺序执行。

***



7 . `为什么通常推荐将 CSS <link> 放置在 <head></head> 之间，而将 JS <script> 放置在 </body> 之前？你知道有哪些例外吗？`

    答：
    浏览器从上到下依次解析html文档。将 css 文件放到头部， css 文件可以先加载。
    避免先加载 body 内容，导致页面一开始样式错乱，然后闪烁。将 javascript 文件放到底部是因为：
    若将 javascript 文件放到 head 里面，就意味着必须等到所有的 javascript 代码都被 下载、解析和执行完成
    之后才开始呈现页面内容。这样就会造成呈现页面时出现明显的延迟，窗口一片空白。
    为避免这样的问题一般将全部 javascript 文件放到 body 元素中页面内容的后面。
    页面加载的问题，先把页面加载出来，然后再去加载效果，提高用户体验度

***

8 . `什么是渐进式渲染 (progressive rendering)？`

> 答：对渲染进行分割 从具体的使用的场景, 不同的 Level 实际上对应不同的页面内容,论坛是一个比较清晰的例子, 想象一个论坛:

网页的静态部分, HTML 固定的内容, 比如导航栏和底部
- 页面首屏的内容, 比如一个论坛的话题
- 页面首屏看不到的内容, 比如话题下面多少回复
- 切换路由才会显示的页面, 比如导航的另一个页面
- 对于这样的情况, 显然有若干种可行的渲染分割的方案

全在客户端渲染
- 1, 2, 3 在服务端渲染, 4 等到用户点击从浏览器抓
- 1, 2 在服务器渲染, 评论由客户端加载
- 只有 1 在服务端渲染, 动态的数据全部由客户端抓取.

而这些方案对于服务端来说, 性能的开销各不相同, 形成一个梯度,
而最后一种情况, 服务端预编译页面就好了, 几乎没有渲染负担.
根据实际的场景, 可以有更多 Level 可以设计.. 只是没这么简单罢了.

***

9 . `是否了解其他基于HTML的模板引擎？`

> 答： 现在市场比较火的是jade吧 有兴趣可以移步
[jade-源于 Node.js 的 HTML 模板引擎](https://segmentfault.com/a/1190000000357534)



***

10 . `H5有哪些新特性？`

> 答：drag & drop 、 pattern 、 datalist 、 download 、 prefetch 和 dns-perfetch
    及布局属性  section 、 article、 nav等

**记住：很多面试者仅仅只列举新标签**