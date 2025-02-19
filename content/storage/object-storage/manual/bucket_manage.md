---
title: "Bucket 管理"
date: 2020-02-28T10:08:56+09:00
description:
draft: false
weight: 1
---



## Bucket 列表

对象存储的主视图是 Bucket 列表，展示当前区域下用户所有的 Bucket 信息，包括名称、当前文件数、空间大小、权限以及可访问的 url。

- 名称：该 Bucket 在山河对象存储服务中全局唯一的标记。
- 当前文件数：Bucket 中所有 Object 的数量，包括文件夹在内。
- 空间大小：Bucket 中所有 Object 大小的总和，是 Bucket 的一个监控项和计费项。
- 权限：该 Bucket 的可访问性，控制台中可以设为私有、公开只读、公开读写。
- url：Bucket 通过 RESTful API 访问时的 uri，调用 Object 相关 API 时的 uri 前缀。

![bucket_1](bucket_listview.png)

> Bucket 列表和文件页提供“列表”和“文件”两种视图，缺省为列表视图，您可以根据习惯选择浏览方式。

### 创建 Bucket

用户可以在 Bucket 列表页点击“新建 Bucket”按钮，创建一个新的 Bucket。由于 Bucket 名称是 url 域名的一部分，因此需要遵循标准 url 域名规范：只包含字母、数字、中划线，以字母开始，以字母或数字结尾。创建时请注意长度范围是 3 - 63，另外 Bucket 是全局唯一的，因此如果提交已存在的名称，则提示创建失败。

![bucket_2](/storage/object-storage/_images/bucket_2.png)

> 新建的 Bucket 是私有的，如果想公开这个 Bucket 的权限，可以在创建后设置。

### 修改 Bucket 权限

对象存储的 Bucket 信息、文件访问受到权限控制。在控制台中可以给 Bucket 设置 3 种级别的权限：

- 私有：该 Bucket 及其文件只能在控制台或者通过 API 或 SDK（经用户的密钥签名）访问。
- 公开只读：所有人都可以访问 Bucket 及其文件相关的 GET/HEAD 等类型 API 接口。
- 公开读写：所有人都可以访问 Bucket 及其文件相关的所有 API 接口。

![bucket_3](/storage/object-storage/_images/bucket_3.png)

> 如果想指定特定用户的访问权限，可以调用 acl 的 POST API 来设置。

### 删除 Bucket

如果一个 Bucket 不再需要，用户可以选择删除 Bucket。请注意，如需删除 Bucket，须提前将Bucket 内的数据删除，操作前需要谨慎。

![bucket_4](/storage/object-storage/_images/bucket_4.png)

### Bucket 文件视图

设置、删除 Bucket 等操作在文件视图中，可以在 Bucket 信息卡右键进入。

![bucket_5](/storage/object-storage/_images/bucket_5.png)

## Bucket 详情

从 Bucket 列表（列表视图或文件视图）点击可进入 Bucket 详情页。Bucket 是独立的文件存储、分段管理、数据处理、监控、计费单元以及设置，详情页分为 6 个 标签页，分别提供按文件查看和操作、监控和消费记录的查看。

![bucket_6](/storage/object-storage/_images/bucket_6.png)

## 分段管理

分段管理标签提供了查看、完成和取消分段上传的功能。

### 分段列表

在分段列表中可以查看分段上传文件的名称，上传 ID 和创建时间等信息。

![bucket_7](bucket_multipart_uploads.png)

### 完成分段上传

点击分段列表右侧的查看分段按钮，在弹出的对话框中可以看到此次上传包含的分段，选择此文件需要的分段，点击下方的完成上传按钮就可以完成此次分段上传，未被选中的分段将在完成上传后删除。

![bucket_8](bucket_multipart_complete.png)

### 取消上传

在分段列表勾选需要取消上传的文件，点击上方工具栏的取消上传按钮，在弹出的对话框中点击确认按钮就可以取消选中的分段上传。右键点击分段列表也可以看到取消上传的快捷方式。

![bucket_9](bucket_multipart_abort.png)

## Bucket 设置

### 设置 CORS

跨源资源共享 (CORS) 定义了在某个域名下的 web applications 对另一个域名下资源的访问规则。通过配置 CORS 规则，你不但可以创建直接与对象存储进行富交互的 web application，也可以指定哪些请求源有权限访问你在对象存储中的资源。

#### 添加请求源

点击 “添加请求源” 按钮，在弹出的模态对话框中输入 CORS 的具体规则。

- 允许请求源：设置跨域请求来源，以 `http://` 或 `https://` 开头，可以用 `*` 来进行通配。
- 允许 HTTP 方法： 设置请求源可以使用何种动作操作 Bucket 中的资源。
- 允许 HTTP 请求头： 设置源所允许的 HTTP header，多个 headers 之间使用逗号分隔，可以用 `*` 来进行通配。
- 允许访问 HTTP 响应头： 设置客户能够从其应用程序（例如，从 JavaScript XMLHttpRequest 对象）进行访问的 HTTP 响应头，多个 headers 之间使用逗号分隔。
- 最大响应缓存时间：设置在预检请求（Options）被资源、HTTP 方法和源识别之后，浏览器将为预检请求缓存响应的时间，最大值为 86400, 单位秒。

其中 “允许请求源” 和 “允许 HTTP 方法” 必填，其他项选填。

![bucket_10](/storage/object-storage/_images/bucket_10.png)

#### 导入请求源

通过导入其他 Bucket 的 CORS 规则可以很方便的完成当前 Bucket 的规则设置，避免了重复输入的麻烦。点击 “导入请求源” 按钮，在 “源 Bucket” 下拉框中选择导入 CORS 规则所属的 Bucket。如果要导入的规则中有相同的请求源，可以选择覆盖或者保留当前 CORS 规则中的请求源。

![bucket_11](/storage/object-storage/_images/bucket_11.png)

### 设置存储空间策略

存储空间策略 (Bucket Policy) 允许用户更细粒度的控制存储空间的访问。 其语义主要由用户、 资源、动作及条件运算符组合定义。例如仅允许指定用户以指定站点为 Referer 以下载存储空间中的某单个文件，或者防止外链等。

#### 添加存储空间策略

点击 “添加规则” 按钮，在弹出的对话框中输入存储空间策略的具体规则。

- ID： 策略的标识符,可用来描述策略的用途。可为任意 ASCII 字符，长度不能超过100个字符。
- 操作： 资源所支持操作。
- 资源： 允许或拒绝访问的资源，以 ‘/’ 为前缀，当操作全为 Bucket 级别时需缺省，多个值使用英文逗号分隔。可使用 ‘*’ 进行通配，以表示 Object 资源。
- 响应动作： 当策略成功匹配用户的请求时，是否允许该请求。
- 用户： 策略所应用到的用户，必须为山河用户 ID 或邮件地址。长度不能超过300个字符，多个值使用英文逗号分隔。如果要匹配所有用户，可设置值为 ‘*’。
- Referer： 存储空间策略生效的条件。

![bucket_12](/storage/object-storage/_images/bucket_12.png)

### 自定义域名和静态网站托管

用户可以将静态资源存放在 OIS Bucket 中提供浏览或下载，首先需要为 Bucket 绑定一个已经备案过的自定义域名。

#### 设置自定域名并开启CDN

建议用户选择启用 CDN，提高访问速度并且节省流量成本。CDN 的回源地址将指向 Bucket 的地址。开启 CDN 的 Bucket 必须公开可读。

下图示例：为 jn1 区的 appcenter-docs Bucket 设置自定义域名 appcenter-docs.shanhe.com 并且启用 CDN 加速。

输入 Bucket CDN 的具体规则。

- 节点类型： CDN 加速的服务类型，可以选择”网页”，“下载”或者“点播”。
- 服务区域： CDN 加速的服务区域

![bucket_13](bucket_cdn_add.png)

然后按照界面上提示的, 到域名注册商修改域名记录，为 appcenter-docs.shanhe.com 添加指向我们分配的 CNAME 域名的记录。

查看域名修改是否生效, linux 平台下可以在终端下使用 dig 命令

![bucket_14](bucket_cdn_check.png)

查看域名修改是否生效, Windows 平台下可以打开命令提示符界面，使用 nslookup 命令

![bucket_15](bucket_cdn_check_win.png)

>
> 假如您的网站已经搭建在别处需要迁移到对象存储上， 不希望中断访问， 希望先等待CDN 生效， 验证效果再修改域名记录。
> 可以先添加域名，获取分配的"xxxxxx.cname.qingcache.com"域名，
> 通过"dig xxxxxx.cname.qingcache.com"查询到 CDN 边缘节点
> 的 A 记录， 配置本地 Host 文件将域名解析到CDN节点上， 进行测试。

#### 设置自定域名但是不开启 CDN

要绑定的域名必须已完成备案，并且该域名到存储空间域名的 CNAME 记录必须已于域名服务商处注册且生效。

以 download.hashdata.cn 绑定到 hashdata-download.jn1.is.shanhe.com 为例, 首先添加域名 CNAME记录，
这个步骤用于检查用户是否拥有真实的域名。配置成功后，Linux 平台的 dig 命令效果如下图:

![bucket_16](bucket_cname_check.png)

添加域名绑定，与上边启用 CDN 的界面类似，区别仅为不勾选 "启用CDN" 选框。提交后理论上立即能访问。

> NOTE:
>
> 对于已经按上述CDN流程配置过自定义域名的用户， 可以随时修改域名的 CNAME 记录指向 Bucket 域名， 绕过CDN加速。
>
> 请务必按照上述的文档使用 CNAME 方式， 而不要直接将域名解析到对象存储 IP 上，
> 因为我们可能会在不通知您的情况下切换对象存储的域名解析到不同的IP。

#### 设置静态网站托管

默认情况下，绑定了自定域名的 Bucket 不开放根目录访问，仅允许直接访问存在的文件路径。
静态网站托管功能，在 Bucket 绑定了自定义域名的基础上，允许用户设置默认访问的首页以及404错误页。

使用流程

1. 上传文件至 Bucket

	在开启静态网站托管服务之前，需要将 Bucket 设置为 “公开可读” （参考 [修改权限](#修改-bucket-权限) ），
	或者使用[存储空间策略](#设置存储空间策略) 来进行访问控制。
	之后将网站内容上传至 Bucket，推荐使用 QingStor 的命令行工具 qsctl 进行上传，
	使用方法可以参考 [qsctl 文档](/storage/object-storage/manual/developer-tools/qsctl)。

2. 绑定自定义域名

	参见前边的文档说明

3. 设置并开启静态网站托管

	在 “静态网站托管” 设置页面，填写希望使用的索引页面和错误页面， 点击 “开启静态网站托管” 即可使用自定义域名访问网站内容。 此时访问静态网站域名的根路径，或者访问的路径以 “/” 结尾，将返回索引页面。 若访问发生错误，如对象不存在，将返回错误页面。

	例如，设置索引页面为 “index.html”，设置错误页面为 “error.html”。 访问根路径时，将返回 “index.html”，访问 “about/” 时，将返回 “about/index.html”。 访问 “test/hello.mp4” 时，如果 “test/hello.mp4” 对象不存在，将返回 “error.html”。

	![bucket_17](bucket_web_hosting_start.png)

	开启静态网站托管后，支持修改网站所使用的索引页面和错误页面，点击 “停止静态网站托管” 可关闭托管服务。

	![bucket_18](bucket_web_hosting_stop.png)

### 设置外部镜像源站

为存储空间设置外部镜像源站，当请求的对象在 Bucket 中不存在时，服务端把对象名称拼接在外部镜像源站后作为抓取的源链接，然后自动从源站抓取（回源），并写入到 Bucket 当中。

#### 开启镜像功能

在下拉框选择 “http” 或 “https”协议，在其后文本框中输入外部镜像源站，然后点击 “开启镜像功能” 按钮。

- 源站点：外部镜像回源的源站，其形式为 “://[:port]/[path]”。

![bucket_19](bucket_external_mirror_source_site_add.png)

### 设置生命周期

定义了一个或多个将应用于一组对象的规则，该规则会定期对所匹配的对象执行相应的操作。目前支持的操作有：删除对象、取消未完成的分段上传和变更存储级别。

#### 添加规则

点击“添加规则”按钮，在弹出的对话框中输入生命周期的具体规则。

- ID： 规则的标志符，可用来描述规则的用途，长度不超过255个字符。
- 对象前缀： 指定处理的资源，缺省表示指定 Bucket 下所有资源。
- 操作： 资源所支持操作。
- 存储级别： “变更存储级别”所支持的类型。
- 天数： 在对象创建或修改指定天数后执行操作，当操作为“取消未完成的分段上传”时，表示在初始化分段上传的指定天数后执行操作。

![bucket_20](bucket_lifecycle_add.png)

### 设置日志功能

对象存储日志功能可以将指定的bucket的访问日志推送到制定的bucket下的指定目录，日志以小时为单位。

#### 设置目标bucket

可以设置当前bucket的日志存放在哪个bucket中。源bucket与目标bucket只能在同一区。
![bucket_21](bucket_logger_add.png)

#### 设置日志前缀

用户可以通过设置日志前缀来实现设置文件存在哪个文件夹，以及日志文件的前缀。可以为空，推荐用户设置，利于文件规划。

例：如果希望存在某以文件夹下同时设置前缀可填写：logs/demo-。此设置表示日志要放在logs/文件夹下。日志命名要添加demo-作为开头。日志一般会延迟1-2个小时输出到设定位置。

查看日志参数详细信息点击描述中的”点击这里“或阅读 OIS API 文档。
![bucket_22](bucket_logger_params.png)


