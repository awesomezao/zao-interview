推荐[2018，开始你的pwa学习之旅]( https://github.com/alienzhou/learning-pwa )

[webpack配置service worker]( https://juejin.im/post/5cde2d24e51d45698161f63f )

[请你实现一个PWA]( https://juejin.im/post/5e26aa785188254c257c462d#heading-2 )



### 问题1：什么是PWA？

> 难度：⭐

`渐进式网络应用（PWA）`是谷歌在2015年底提出的概念。基本上算是web应用程序，但在外观和感觉上与`原生app`类似。支持`PWA`的网站可以提供脱机工作、推送通知和设备硬件访问等功能。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/47055893/what-is-a-progressive-web-app-in-laymans-terms)

### 问题2：PWA有那些优点？

> 难度：⭐⭐

1. **更小更快**: 渐进式的web应用程序比原生应用程序小得多。他们甚至不需要安装。这是他们没有浪费磁盘空间和加载速度非常快。
2. **响应式界面**: `PWA`支持的网页能够自动适应各种屏幕大小。它可以是`手机、平板、台式机或笔记本`。
3. **无需更新**: 大多数移动应用程序需要每周定期更新。与普通网站一样，每当用户交互发生且不需要应用程序或游戏商店批准时，PWA总是加载最新更新版本。
4. **高性价比**：原生移动应用需要分别为`Android和iOS设备`开发，开发成本非常高。另一方面，`PWAs`有着相同的功能，但只是先前价格的一小部分，开发成本低。
5. **SEO优势**：搜索引擎可以发现`PWAs`，并且加载速度非常快。就像其他网站一样，它们的链接也可以共享。提供良好的用户体验和结果，在SEO排名提高。
6. **脱机功能**：由于`service worker API`的支持，可以在脱机或`低internet连接`中访问`PWAs`。
7. **安全性**：`PWAs`通过`HTTPS`连接传递，并在每次交互中保护用户数据。
8. **推送通知**：通过推送通知的支持，`PWAs`轻松地与用户进行交互，提供非常棒的用户体验。
9. **绕过应用商店**：`原生app`如果需要任何新的更新,需要应用商店几天的审批，且有被拒绝或禁止的可能性,对于这方面来说，PWAs有它独特的优势，不需要`App Store`支持。更新版本可以直接从web服务器加载，无需`App Store`批准。
10. **零安装**：在浏览过程中，`PWA`会在手机和平板电脑上有自己的图标，就像移动应用程序一样，但不需要经过冗长的安装过程。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/47055893/what-is-a-progressive-web-app-in-laymans-terms)

### 问题3： PWA有什么缺点？

> 难度：⭐⭐

1. **对系统功能的访问权限较低**：目前`PWA`对本机系统功能的访问权限比原生app有限。而且，所有的浏览器都不支持它的全部功能，但可能在不久的将来，它将成为新的开发标准。
2. **多数Android，少数iOS**：目前更多的支持来自`Android`。`iOS系统`只提供了部分。
3. **没有审查标准**：`PWAs`不需要任何适用于应用商店中本机应用的审查，这可能会加快进程，但缺乏从应用程序商店中获取推广效益。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/47055893/what-is-a-progressive-web-app-in-laymans-terms)

### 问题4：如何让一个web应用成为PWA？

> 难度：⭐⭐

一个`web应用`要成为`PWA`,有需要一些关键原则需要遵守，如下：

- **可发现的**，内容可以通过搜索引擎找到。
- **可安装**，可以在设备的主屏幕上使用。
- **可链接**，可以通过`URL`来分享它。
- **与网络无关**，可以在离线或网络连接不好的情况下工作。
- **渐进式的**，仍可在较老的浏览器上使用，但在最新的浏览器上功能齐全。
- **可重新参与**，能够发送通知，每当有新的内容可用。
- **响应式**，可以在任何有屏幕和浏览器的设备上使用 —— 手机、平板、笔记本、电视、冰箱等等。
- **安全**，用户和应用程序之间的连接是安全的，不受任何第三方试图访问用户敏感数据的影响。

### 问题5：为什么我们在PWA中需要Web manifest？

> 难度：⭐⭐

`Web manifest`以`JSON`格式列出关于网站的所有信息。这个文件是使网站可安装的要求之一。

它通常驻留在一个web程序的根文件夹。其中包含一些有用的信息,如应用程序的标题，路径不同大小的图标，可以用来表示移动操作系统上的应用程序(例如,主屏幕图标)，和一个背景颜色用于加载或闪屏。浏览器需要这些信息才能在安装和主屏幕上正确显示web程序。

**💡 来源：**[MDN](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/Installable_PWAs)

### 问题6：PWA有哪些原生app不具备的特性?

> 难度：⭐⭐⭐

- 可发现性——`PWA`中的内容很容易被搜索引擎找到，但诸如掘金之类的以内容为中心的`原生app`来说不会在其确实提供访问权限的内容的应用商店搜索结果中得到它确切提供访问的内容（也就是无法通过应用商店去搜索到文章嘿嘿😂，我大`PWA`就不一样了）。

- 可链接性——任何页面/屏幕都可以有一个直接链接，可以很容易地共享

- 可收藏性——保存该链接以直接访问应用程序的视图

- 始终新鲜——不需要通过应用商店来推送更新

- 通用访问权限——不受应用商店政策或地理限制（你懂的🤣）

- 节省大量数据——这对于昂贵且/或互联网访问速度缓慢的新兴市场至关重要。例如，`Konga(电子商务网站)`通过迁移到`PWA`减少了92％的首次负载数据使用量。

- 发行摩擦低——如果你的

  ```
  PWA
  ```

  在线，则

  ```
  Android（和其他移动）用户
  ```

  已经可以使用它。 

  - 大部分手机用户没有每月下载任何新的应用程序
  - `PWAs`不需要去`app store`，搜索`app`，点击`Install`，等待下载，然后打开`app`。每一步都会损失20%的潜在用户（懒癌福音，简直了🤣）。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/35504194/what-features-do-progressive-web-apps-have-vs-native-apps-and-vice-versa-on-an)

### 问题7：Service Workers是什么？

> 难度：⭐⭐⭐

`Service Worker`是浏览器在后台独立于网页运行的脚本，它打开了通向不需要网页或用户交互的功能的大门。 现在，它们已包括如推送通知和后台同步等功能。 将来，`Service Worker`将会支持如定期同步或地理围栏等其他功能。 本教程讨论的核心功能是拦截和处理网络请求，包括通过程序来管理缓存中的响应。

这个`API`之所以令人兴奋，是因为它可以支持离线体验，让开发者能够全面控制这一体验。

在`Service Worker`出现前，存在能够在网络上为用户提供离线体验的另一个`API`，称为 `AppCache`。`AppCache API` 存在的许多相关问题，在设计`Service Worker`时已予以避免。

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/primers/service-workers)

### 问题8：PWA与Hybrid之间有什么不同？

> 难度：⭐⭐⭐

`Hybrid`通常是指使用web和原生技术(`native technology`)的组合构建的应用程序，这些程序通过应用商店发布。要经过苹果、谷歌、微软等公司的应用程序商店审查程序。

`PWA`是使用在浏览器中运行的Web技术构建的应用程序，可以添加到主屏幕（就是固定到桌面🐷）。它们不需要通过本地应用程序商店发布，但可以包含在其中，Microsoft自2018年起在其Microsoft Store中包含了`PWA`，并且`TWA（Trusted Web Activities`，是谷歌最新提供的一种将web页面移植到`Android APP`的方法）使向`Google Play Store`提交`PWA`变得更加容易。

一些`Hybrid`平台包括`PhoneGap（又名Cordova）`，`Appcelerator` `Titanium`和`Ionic`。您不需要一个平台来创建一个`Hybrid`，但它们对你很有帮助，因为它们已经在`本地API`和`JavaScript API`之间建立了桥梁。(也就是众所周知的`JSbridge`🐷)

`PWAs`只需在浏览器中运行，因此可以使用基本的`HTML`，`CSS`和`JavaScript`进行构建。（前端福音，嘿嘿😂）

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/40820592/difference-between-a-progressive-web-app-and-a-hybrid-mobile-app)

### 问题9：什么是fetch事件？

> 难度：⭐⭐⭐

在安装`Service Worker`且用户转至其他页面或刷新当前页面后，`Service Worker`将开始接收`fetch`事件

示例：

```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});
复制代码
```

这里我们定义了`fetch`事件，并且在 `event.respondWith()` 中，我们传入来自`caches.match()`的一个`promise`。 此方法检视该请求，并从服务工作线程所创建的任何缓存中查找缓存的结果。

如果发现匹配的响应，则返回缓存的值，否则，将调用`fetch`以发出网络请求，并将从网络检索到的任何数据作为结果返回。 这是一个简单的例子，它使用了在安装步骤中缓存的所有资产。

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/primers/service-workers)

### 问题10：对网站进行PWA安装有哪些要求？

> 难度：⭐⭐⭐

要使网站可安装，它需要具备以下条件：

- `Web Manifest`，其中填写了正确的字段
- 网站的域必须是安全（`HTTPS`）的
- 一个本设备上代表应用的图标
- 一个注册好的`Service Worker`，可以让应用离线工作（这仅对于安卓设备上的`Chrome浏览器`是必需）

**💡 来源：**[MDN](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/Installable_PWAs)

### 问题11：什么是IndexedDB以及如何在PWA中使用它？

> 难度：⭐⭐⭐

`IndexedDB`是大型`NoSQL`存储系统。它允许您在用户浏览器中存储任何内容。除了通常的`search`、`get`和`put`操作之外，`IndexedDB`还支持事务。

`IndexedDB`是用于客户端存储大量结构化数据（包括`文件/blob`）的低级API。该`API`使用索引来对数据的高性能搜索。虽然`DOM存储`对于存储较小数量的数据很有用，但对于存储较大数量的结构化数据则不太有用,所以说，`IndexedDB`提供了一个解决方案。

在`PWA`中，一般建议离线存储数据：

- 对于脱机时加载应用程序所需的网络资源，使用`Cache API`（`Service Workers`的一部分）。
- 对于所有其他数据，使用`IndexedDB`。

**💡 来源：**[developers.google.com](https://developers.google.com/web/ilt/pwa/working-with-indexeddb)

### 问题12：什么是CacheStorage？

> 难度：⭐⭐⭐

`CacheStorage`是浏览器中的一种存储机制，用于存储和检索网络请求和响应。它以`Request` 为`key`，`Response`为`value`去存储请求和响应对象。

`CacheStorage`不是`Service Worker API`，但它使SW能够缓存网络响应，以便在用户断开与网络的连接时提供脱机功能。

`Caches对象`（`CacheStorage`的一个实例）被用来访问`CacheStorage`，检索，存储和删除对象。

**💡 来源：**[blog.bitsrc.io](https://blog.bitsrc.io/introduction-to-the-cache-storage-a-new-browser-cache-pwa-api-a5d7426a2456)

### 问题13： 说说Service Worker生命周期，并画出其大概流程。

> 难度：⭐⭐⭐⭐

`Service Worker`的生命周期完全独立于网页。

要为网站安装服务工作线程，您需要先在页面的`JavaScript`中注册。 注册服务工作线程将会导致浏览器在后台启动服务工作线程安装步骤。

在安装过程中，您通常需要缓存某些静态资产。 如果所有文件均已成功缓存，那么`Service Worker`就安装完毕。 如果任何文件下载失败或缓存失败，那么安装步骤将会失败，`Service Worker`就无法激活（也就是说，不会安装）。 如果发生这种情况，不必担心，它下次会再试一次。 但这意味着，如果安装完成，您可以知道您已在缓存中获得那些静态资产。

安装之后，接下来就是激活步骤，这是管理旧缓存的绝佳机会。

激活之后，`Service Worker`将会对其作用域内的所有页面实施控制，不过，首次注册该`Service Worker` 的页面需要再次加载才会受其控制。 服务工作线程实施控制后，它将处于以下两种状态之一：服务工作线程终止以节省内存，或处理获取和消息事件，从页面发出网络请求或消息后将会出现后一种状态。

以下是`Service Worker`初始安装时的简化生命周期：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="654" height="603"></svg>)



**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/primers/service-workers)

### 问题14：如何更新Service Worker？

> 难度：⭐⭐⭐⭐

在某个时间点，您的`Service Worker`需要更新。 此时，您需要遵循以下步骤：

1. 更新您的服务工作线程`JavaScript`文件。 用户导航至您的站点时，浏览器会尝试在后台重新下载定义`Service Worker` 的脚本文件。 如果`Service Worker`文件与其当前所用文件存在字节差异，则将其视为`新 Service Worker`。
2. 新`Service Worker`将会启动，且将会触发`install`事件。
3. 此时，`旧 Service Worker`仍控制着当前页面，因此`新 Service Worker`将进入`waiting`状态。
4. 当网站上当前打开的页面关闭时，`旧 Service Worker`将会被终止，`新 Service Worker`将会取得控制权。
5. `新 Service Worker`取得控制权后，将会触发其`activate`事件。

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/primers/service-workers)

### 问题15：阐述你所知道的Service Worker缓存策略。

> 难度：⭐⭐⭐⭐

下述三种缓存策略与框架无关：

- 仅缓存（Cache Only）

  ——Service Worker期望在Cache找到请求的assets。 

  ![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="880" height="299"></svg>)

- **仅网络（Network only）**——此策略与上一个策略完全相反：始终访问网络，甚至不查询缓存。



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="880" height="299"></svg>)



- 重新验证时失效(Stale while revalidate)

  ——与

  ```
  仅缓存(Cache Only)策略
  ```

  类似，目标是通过从缓存中传递数据来确保快速响应。但是，在处理客户端请求时，将向服务器触发一个单独的请求，以获取更新的版本（如果有的话😂）并将其存储到缓存中。 

  ![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="880" height="427"></svg>)

接下来说一下Angular框架提供的两种缓存策略：

- 性能优先（默认）

  ：这里的目标是优化响应时间。如果缓存中

  ```
  有可用的资源
  ```

  ，则会传递此版本。否则，将执行网络请求以获取并缓存它。 

  ![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="880" height="435"></svg>)

- 新鲜度优先

  ：当需要从网络传输最新数据时使用。我们可以指定一个

  ```
  超时时间
  ```

  ，在请求超时之后，将退回到缓存并尝试从那里传递所需的数据。 

  ![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="880" height="427"></svg>)

**💡 来源：**[dev.to](https://dev.to/paco_ita/service-workers-and-caching-strategies-explained-step-3-m4f)

### 问题16：什么是App Shell？

> 难度：⭐⭐⭐⭐

`App Shell`架构是构建`PWA`的一种方式，这种应用能可靠且即时地加载到您的用户屏幕上，与原生app相似。

`App Shell`是支持用户界面所需的最小的`HTML`、`CSS`和`JavaScript`，如果离线缓存，可确保在用户重复访问时提供即时、可靠的良好性能。这意味着并不是每次用户访问时都要从网络加载`App Shell`。 **只需要从网络中加载必要的内容**。

`App Shell`架构对具有`相对不变的导航以及一直变化的内容`的应用和网站意义重大。

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/architecture/app-shell)

### 问题17：一个App Shell应该具有什么特性？

> 难度：⭐⭐⭐⭐

`App Shell`应能完美地执行以下操作：

- 快速加载
- 尽可能使用较少的数据
- 使用本机缓存中的静态资产(`static assets`)
- 将内容与导航分离开来
- 检索和显示特定页面的内容（`HTML`、`JSON`等）
- 可选：缓存动态内容

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/architecture/app-shell)

### 问题18：苹果设备对于PWA支持怎样？

> 难度：⭐⭐⭐⭐⭐

`Safari 11.1`中提供了`Service Worker`，该产品于`2018年3月29日`与`iOS 11.3`和`macOS 10.13.4`一起发布。

下面是补充注意点：

- iOS和macOS都支持`Service Worker` + `Cache API`（`Cache API`实际上是`Service Worker`支持的**先决条件**）
- 没有“添加到主屏幕（就是固定到桌面😝）”提示
- 没有推送API
- 没有后台同步
- 其对`Service Worker`实现中还有一些[值得注意的问题](https://medium.com/@firt/pwas-are-coming-to-ios-11-3-cupertino-we-have-a-problem-2ff49fd7d6ea)

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/29895387/service-workers-and-ios-safari)

### 问题19：Service Workers是否能够同时存在多个？

> 难度：⭐⭐⭐⭐⭐

有两点要注意：

- 只要每个`Service Workers`都有自己独有的范围，就可以为给定的来源注册任意数量的`Service Workers`
- `缓存存储(Cache Storage API)`(以及其他存储机制，如`IndexedDB`)在整个源中都是共享的，默认情况下不存在“分片”或命名空间隔离。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/42474908/using-multiple-javascript-service-workers-at-the-same-domain-but-different-folde)

### 问题20：有哪些Service Worker能做但是web worker不能的？

> 难度：⭐⭐⭐⭐⭐

`Web Workers`——为Web内容提供在后台线程中运行脚本的简单方法。工作线程可以在不干扰用户界面的情况下执行任务。此外，它们还可以使用`XMLHttpRequest`执行`I/O`（尽管`responseXML`和`channel`属性始终为空）。创建后，工作人员可以通过将消息发布到该代码指定的事件处理程序中来向创建该代码的`JavaScript代码`发送消息（反之亦然）。

`Service Worker`——本质上是充当位于`Web app`与浏览器和网络（如果可用）之间的代理服务器。它们旨在（除其他外）创建有效的脱机体验，拦截网络请求并根据网络是否可用以及服务器上是否存在更新的资产(`assets`)来采取适当的操作。它们还允许访问`推送通知`和`后台同步API`。

|                      | Web Workers              | Service Workers              |
| -------------------- | ------------------------ | ---------------------------- |
| 实例（Instances）    | 每个（Many per tab）     | 一对所有（One for all tabs） |
| 存活时间（Lifespan） | 与tab一致（Same as tab） | 独立（Independent）          |
| 用途（Intended use） | 并行（Parallelism）      | 离线支持（Offline support）  |

`Web Worker`可以方便地运行高耗能的脚本（`expensive scripts`，我也是看英文的，不知道翻译对不对🤣），而不会造成页面阻塞，而`Service Worker`有助于修改网络请求的响应（例如，构建离线App时😂）。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/38632723/what-can-service-workers-do-that-web-workers-cannot)

### 问题21：使用Service Worker线程的 App Shell 架构有哪些优势？

> 难度：⭐⭐⭐⭐⭐

- **始终快速的可靠性能**。重复访问速度极快。 第一次访问时即可缓存静态资产和 UI（例如`HTML`、`JavaScript`、`图像和 CSS`），以便在重复访问时即时加载。内容可能会在第一次访问时缓存到系统中，但一般会在需要时才进行加载。
- **如同本机一样的交互**。通过采用`App Shell`模型，您可以构建如同本机应用一样的即时导航和交互，包括离线支持。
- **数据的经济使用**。其设计旨在实现最少的数据使用量，并且可以正确判断缓存的内容，因为列出不需要的文件（例如，并不是每个页面都显示的大型图像）会导致浏览器下载的数据超出所必需的量。尽管在西方国家和地区中，数据相对较廉价，但新兴市场并非如此，这些市场中连接和数据费用都非常昂贵。

**💡 来源：**[developers.google.com](https://developers.google.com/web/fundamentals/architecture/app-shell)

### 问题22：有可能在PWA中拥有真正的持久存储吗?为什么需要这样的存储?

> 难度：⭐⭐⭐⭐⭐

答案是否定的。用户可以删除`PWA`（如果安装在`Android`上）并清除浏览器缓存中的所有`PWA`。 `Cache API`是临时的，当用户清除浏览器缓存时，`IndexedDB`数据可以被擦除。 所以要在PWA安装中持久化应用程序的数据，您应该通过一些`API`使用云存储。

**💡 来源：**[stackoverflow.com](https://stackoverflow.com/questions/56708828/is-truly-persistent-storage-possible-in-a-progressive-web-app)


作者：MaoMao链接：https://juejin.im/post/5e26aa785188254c257c462d来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 1.manifest

将网站添加到桌面上

```html
<link rel="manifest" href="/manifest.json">
```

```json
{
    "name": "图书搜索",
    "short_name": "书查",
    "start_url": "/",
    "display": "standalone",
    "background_color": "#333",
    "description": "一个搜索图书的小WebAPP（基于豆瓣开放接口）",
    "orientation": "portrait-primary",
    "theme_color": "#5eace0",
    "icons": [{
        "src": "img/icons/book-32.png",
        "sizes": "32x32",
        "type": "image/png"
    }, {
        "src": "img/icons/book-72.png",
        "sizes": "72x72",
        "type": "image/png"
    }, {
        "src": "img/icons/book-128.png",
        "sizes": "128x128",
        "type": "image/png"
    }, {
        "src": "img/icons/book-144.png",
        "sizes": "144x144",
        "type": "image/png"
    }, {
        "src": "img/icons/book-192.png",
        "sizes": "192x192",
        "type": "image/png"
    }, {
        "src": "img/icons/book-256.png",
        "sizes": "256x256",
        "type": "image/png"
    }, {
        "src": "img/icons/book-512.png",
        "sizes": "512x512",
        "type": "image/png"
    }]
}
```

## 2.Service Worker

### 生命周期

**installing：** 这个状态发生在service worker注册之后，表示开始安装。`在这个过程会触发install事件回调指定一些静态资源进行离线缓存。`

**installed：** sw已经完成了安装，进入了waiting状态，等待其他的Service worker被关闭（在install的事件回调中，可以调用skipWaiting方法来跳过waiting这个阶段）

**activating：** 在这个状态下没有被其他的 Service Worker 控制的客户端，允许当前的 worker 完成安装，并且清除了其他的 worker 以及关联缓存的旧缓存资源，等待新的 Service Worker 线程被激活。

**activated：** 在这个状态会处理activate事件回调，并提供处理功能性事件：fetch、sync、push。（在acitive的事件回调中，可以调用self.clients.claim()）

**redundant：** 废弃状态，这个状态表示一个sw的使命周期结束

### 消息推送

[PWA(Progressive Web App)入门系列：Push]( https://juejin.im/post/5cee3b7ff265da1bc55245e0 )

![](https://gitee.com/ruoyiwen/img/raw/master/blog/16b0298522f25d07)