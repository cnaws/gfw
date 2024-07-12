301穿墙的运作机制是什么？

301穿墙，主要是在穿透GFW的同时使用301状态码；主要应用场景是：让国内的搜索引擎爬虫蜘蛛能透过GFW/移动墙，能通过301状态码访问到被墙域名，从而实现搜索引擎的权重从老被墙域名转移去新域名的目的。

使用了301穿墙，搜索引擎结果里展示的是哪个域名?
搜索引擎如百度 谷歌 神马 搜狗 等搜索结果索引更新需要一些时间，不同词的热度不同搜索结果更新可能需要几小时到一周不等。
理论上，国内搜索引擎会识别301状态码，并且转移权重给新域名，各大搜索引擎结果会在后续几个小时到几天内陆续把搜索结果里原来被墙的域名，更新为新的没被墙域名。

GFW真墙屏蔽 与 假墙攻击的区别

网站解析到任意境外IP后，马上使用 boce 或 17ce get 全国节点，进行多次测速，始终是无法访问，返回 000 状态，基本就是GFW真墙屏蔽；
网站解析到任意境外IP后，马上使用 boce 或 17ce get 全国节点，进行多次测速，新的境外IP返回 200 状态，之后持续检测 会在 200状态、000状态码之间变化，或者 持续为200状态码，则可能是被假墙攻击触发了GFW临时封禁了该境外IP。

怎么判断是否被真墙或假墙攻击？

我们尝试了很多方案，并对系统进行了各种修改，经过几个月的大量实践发现，有一些方法方案可以抵御假墙攻击，但并没有一个真正可以完全抵御的方案；
因为被攻击的网站除了被假墙攻击，还可能夹带DDOS、CC攻击；即便是使用国内服务器 如：阿里云、腾讯云，一样可能会被DDOS、CC打到断网24小时，需要购买其上万元甚至更贵的防御。
如果你的网站频繁出现 甚至 长时间的无法访问，自查发现 域名解析正常、WEB服务正常、国外访问正常(国外HTTP测速)、域名没有被墙、也没有被污染，但是服务器的80端口无法访问，那么基本上是受到了GFW真墙屏蔽 或 假墙攻击。
检测网址：检查国内访问情况以及域名是否被墙、被劫持，DNS是否被污染。

什么是中国移动墙？ 中国联通墙？中国电信墙？

大多数灰产、黑产网站域名经常会出现还没背GFW墙（电信、网通可以访问），但是移动线路已经被墙的情况。 
国外服务器有些国家是走的固定线路，比如香港走南方电信，俄罗斯走北京联通；中国移动的宽带用户如果访问这些境外网站，会增加中国移动给电信、联通的结算费用，所以移动会选择直接阻断访问，也就是墙掉域名无法访问；
2022年5月左右，出现部分地区运营商节点不通的情况；

移动墙的原因和动机： 
你使用中国移动的宽带网络访问搭建在电信或者联通网络上的网站，会产生的跨网流量，中国移动就会给中国电信和中国联通付费。 由于历史原因，建立在电信或者联通(网通)网络上的服务器非常多，移动给电信和联通交的费用远大于收到的费用。

因此移动会自行劫持DNS（地区不同可能劫持强度不同），尽可能的返回位于移动网络内的服务器，针对热门HTTP内容还会劫持到移动自建的缓存服务器，对于国外网站，多半会跨网，就QoS限速，对于非法网站，直接阻断访问。

简言之，中国移动为了减少网间结算，把热门内容劫持到自己的镜像服务器，并建立黑名单机制，在黑名单里的就qcs或者间歇性阻断，或完全阻断。

什么是假墙攻击？
假墙攻击是2021年3月左右出现的一种攻击技术，主要是利用、触发GFW的临时封禁策略，致使境外IP被GFW临时封禁半个小时到数个小时不等，之后自行解封（或人工干预解封？）。如果持续解析到境外IP，就会被持续墙IP长达数小时乃至数日，SEO排名较好的网站，排名急剧下降付诸东流。

该攻击方法主要被一伙广告联盟的人(烈马团队)，利用来敲诈勒索、或强迫有流量的网站、国内搜索引擎中有SEO排名流量的站点接入他们的广告JS；（注：JS本身除了可以任意添加更多广告、获取用户隐私，还能直接将访问网站的用户劫持跳转去其他网站，这是所有中小广告联盟的惯用伎俩）。

敲诈勒索手段主要是通过被攻击网站域名的统计中的来源refer、或主动联系站长等方式，告知被攻击者即将被关闭网站，如需解封请联系勒索人的Telegram账号；留其他联系方式的基本可以判定不是源头攻击方。

假墙与真墙的区别：
国外IP服务器无法通过GFW返回数据给国内用户，只是封禁时间较短，不是长期封禁；

全国各地检测结果 参考：https://xvideos.com

假墙与移动墙的区别：

移动墙只是中国移动线路墙了域名，而域名被GFW墙/假墙，是国内三网运营商均被GFW拦截，致使境外IP服务器无法通过GFW返回数据给终端用户；

假墙的具体技术表现

从2021年4月开始客户持续遭遇假墙攻击，我们使用假墙攻击、自行搭建各种防御措施等，尝试了多种方法实践，发现假墙攻击具有以下特点：

可以不直接访问被攻击服务器，即服务器上 并未出现 日志、流量、并发 等统计异常；团队实测 阿里云、腾讯云 香港服务器的官方统计数据无异常波动。

可能夹杂DDOS、CC攻击，导致只具备基础防御的服务器被攻击至断网；阿里云、腾讯云等服务器的保护机制均为断网24小时。

防御假墙攻击的方法
1、 域名使用DNS轮询解析+多IP
    经过大量实践，目前客户采取较多的方案是：购买我们的服务器+购买额外的IP+DNS解析轮询。

    如果IP在60个上下，发现约30分钟左右即可在全国所有运营商中全部墙完60个IP；最快4分钟在全国范围墙掉IP，所以建议采购100个以上的IP+策略来实现轮询解析，可以实现15分钟轮换解析到1个新IP；

2、使用国内服务器IP

    GFW设计的初衷是为了 拦截境外IP 的网站及内容，国内到国内的线路访问不走GFW；

    使用国内服务器需要域名备案，且防御成本较贵。

3、被假墙攻击的域名套上CDN
    业内不存在抗假墙CDN，只有上更多的IP硬抗的CDN，费率及机房成本很高；不如直接购买服务器+购买额外的IP+DNS解析轮询，成本相对可控且更便宜；
    
推荐工具：
https://www.boce.com  网站测速(HTTP测速) 
https://zijian.aliyun.com/detect/http 网站测速(HTTP测速)
https://www.17ce.com/  有 下载异常 和 * 即无法正常访问网站；
https://aicesu.com/ boce
https://www.dnspod.cn/tech/  80 443 端口检查
http://www.jucha.com/safe/ 可以检测微信、QQ屏蔽，域名被墙被污染没有 boce 检测的准确、精细；
测试网站在全国各地区节点反回的数据，并不是100%可信：可能因为网络波动、系统异常等，有数个节点无法正常返回数据，不代表该地区网站无法访问。可以尝试多次测试、拿其他热门网站测速的结果进行对比、确认，示例：云南联通无法打开百度，实际为该节点无法提供服务，因为测试 www.qq.com 该地区也无法返回结果；
 状态 包含 非000状态 均为正常：200、404、301、302、502、500 等状态 均为服务器返回的状态，说明服务器运行正常，具体原因请检查服务器程序配置；
如果使用CloudFlare、CDN等多IP业务，出现个别IP全国都不通(不是所有IP被墙)，则可能是共用该IP的其他域名受到了假墙攻击导致共用IP被墙，并非本站受到攻击。
状态  包含 000状态，即发现异常： 示例：https://xvideos.com
如果 000 状态 数量较少(少于5个左右)，可能只是这些检测节点有异常，没有返回检测结果。
如果 000 状态 80%以上都是自己域名解析到的IP，则说明解析正常，域名已经被GFW墙，所以无法返回数据给终端用户；
则说明可能解析有问题、或者被墙、被污染、或者服务器未正常提供服务；
如果 移动 这个线路，大多数都不通，则说明被移动墙；详见 中国移动墙中墙解决方案
状态 包含大多数或全部为 * 说明 域名未解析 或 服务器IP无响应，或已被国内DNS污染，请检查域名解析和服务器状态；
127.0.0.1  或 其他非自己服务器IP的境内IP 表示该地区该运营商劫持了该域名，需要找运营商申诉，或放弃该域名不再续费使用；示例：www.google.com 
127.0.0.1、本机地址、贵州、江苏 大概率是被反诈中心拦截，或被地方运营商拦截，强制解析到这几个IP地址，无解；查看示例：www.hostcli.com 
如果 大多地区无法解析 显示为 * 或 解析到国外IP，可能是说明 域名未解析 或 服务器IP无响应，也可能是被国内DNS污染，基本无解，需要走监管部门会议流程申请、审批；示例：www.facebook.com
