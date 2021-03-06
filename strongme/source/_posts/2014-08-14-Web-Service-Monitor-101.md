title: 网络服务监控101：识别不良部署
date: 2014-08-14 22:15:20
tags:
- ImportNew
- Translation
categories:
- Translation
--- 
[Strongme](http://www.jobbole.com/members/strongme)翻译自[Andreas Grabner ](http://apmblog.compuware.com/2014/05/28/web-service-monitoring-101-identifying-bad-deployments/) 来自[ImportNew](http://www.jobbole.com/)   


你是否曾经往服务器发布更新的时候想，“一切正常，如期运行！”然后你却必须一直处理用户的抱怨：为什么你们的系统总是报错？   

<img src="http://apmblog.compuware.com/wp-content/uploads/2014/05/IRS_Server_Cartoon1-600x186.png" >   

我们最近正在两个数据中心之间迁移一些系统服务，甚至把一些组件放到了公有云上面。一切准备就绪，系统监控也设置妥当，每个人几乎是 竖着拇指开始行动的。紧接着，系统操控面板持续的输出愉悦的绿色监控信息。不久一个同事就跟我抱怨说：他怎么都无法使用我们迁移过的服务中的一个（免费[dynaTrace AJAX Edition](http://www.compuware.com/en_us/application-performance-management/products/ajax-free-edition/overview.html)），好像是认证网络服务失败了。这时，我们就列出以下几个需要考虑的问题：  
  
  
* 影响：这个问题是只有他的帐号出现还是影响了更多的用户？
* 根源问题：根源问题出现在哪？为什么会出现这样的问题？
* 预警：为什么我们的操作监控面包没有报出任何网络服务失败的信息？ 
<!--more-->


后来验证发现是由于下面几个问题导致的：   

* 由于一个过时的配置文件被部署上去了
* 这个问题指挥影响到那些被不同的后端服务处理的员工帐号
* 没有在操作监控面包提示失败信息是由于使用SOAP框架不论是成功还是失败的信息都会在消息体中返回HTTP 200，而这样就不会在任何网络服务器的日志文件中出现   

这篇博客会给你介绍更多从这次偶然事件中总结出来的的诊断问题和最佳实践的灵感。这样就可以升级我们的技术实现以及产品监控。只有你监控到所有的系统组件以及部署任务结果的关联性，才可以很自信的在不终端业务应用的基础上完成服务部署。   



## 失败的监控：当你的终端用户成了你系统的预警系统   

当我得知一个同时无法使用 dynaTrace AJAX Edition服务器分析一个特定网站的性能的时候，我先复制到 这个网站的地址去验证了问题是否存在。我用自己的安全证书也失败了，表明不是我那个同时本地机器的问题：   
<img src="http://apmblog.compuware.com/wp-content/uploads/2014/05/AJAXEditionError-600x220.png" >   

我去问管理监控这些服务的操作团队，得到下面的回复：   

“我们没有在网络服务器上看到任何错误，同样在我们的验证服务里面也没有报告有任何可用性问题的错误。看下面这张我们监控面板的截图就知道了，全部是绿色的，没有问题。”   

<img src="http://apmblog.compuware.com/wp-content/uploads/2014/05/IISHealthMonitoringDashboard-600x354.png" >   

## 光有网络服务器日志监控是不够的   

正如我最开始一段提到的那样，由于我们的SOAP框架总是在错误消息体中返回HTTP 200。这是一个很常见”最佳（或者是最坏）事件“，大家可以在[Github讨论](https://github.com/savonrb/savon/issues/151)里面去瞧一瞧。   

以这种方式引发的问题在传统的基于网络服务器日志的操作监控不会检测到这些“逻辑/业务”方面的问题。你肯定不想着用户都开始抱怨才去升级你的监控方式吧。那么到底该如何做呢？这些开发以及系统监控工作需要我们坐下来，如何才能监控到这些服务的调用？并且需要我们去跟业务负责任去了解下，我们需要针对业务预警到哪个级别。

如何才能确认你当前的监控方式是否奏效呢？开始仔细的分析用户报告的问题吧，虽然这样的话就是人工监控了。  然后跟工程师了解下是否用到了这里提到的有监控机制的框架。   

## 不良部署：诊断技术问题   

为了确认这个问题的根源，我取到了进行认证失败的调用请求路径，如下面截图所示。如果你的服务没有动态请求调用路径，那也应该有一些详细的应用跟踪日志可以查看吧。我发现用本地IP或者是我传入的用户名来进行的认证请求地址都 被轻易的截取到了。因为dynaTrace总是会把所有的端到端的事务都截取到，不管它是快的，慢的，失败的或者是成功的。我都坚信会被捕获到。你可以发现我确认这个问题的根源是多么容易，可是为什么网络服务器的日志系统就是获取不到这个日志信息呢。   

<img src="http://apmblog.compuware.com/wp-content/uploads/2014/05/PurePathShowingExactRootCause-600x329.png" >    
连接问题：之前可以验证雇员帐号的LDAP服务器现在无法连接是由于我们最近的一次基础设置的变更导致的。   

查看上面的这张截图可以看出我们的网络服务器验证是如何实现的（当我给那些工程师展示的时候吓到他们了），请求进来的时候，会进行3层的内部服务调用：   

* 1st Call。查看这个用户的Session是否依然是验证过的
* 2nd Call。否则就检测JIRA用户目录，看他是否包含在这些免费的用户帐号中   
* 3rd Call。如果在免费账户中都找不到，就会通过LDAP代理来检测合作伙伴的的活动目录（AD）看这个用户是否是一名雇员   

第一个成功的网络服务调用结果就是成功的登录并且向Ajax Editoon返回正数结果。   

## 根源问题：过时的文件被部署到服务器上   

上面的路径截图中我们可以看到这个雇员帐号在第一二次认证请求中都失败了（意思就是我当前session无效并且也不是免费的客户帐号）。第三次，回去AD检测，因为跟LDAP代理的连接无法建立。这就 看起来是个技术问题导致身份验证的失败。我刚开始猜测是由于我们把一些服务从一个数据中心迁移到另一个导致的。当然，我只猜对了一部分。   

我也把请求路径截图给我们的系统架构师看了，他回复说：“等等，我们早就不应该去调用LDAP代理服务了啊，因为我们已经把所有的用户账户全部迁移到了JIRA了”，这下有意思了。只有在我们拥有完整的点对点的事务内部请求记录才可以发现这个问题啊。   

总之,由于我们迁移了服务加上一些过时的配置文件导致我们的认证服务运行起来像没有迁移过我们的用户一样。这就是为什么网络服务还是会去访问LDAP然后失败，因为LDAP代理早就失效了。   

## 我们的开发者、操作人员等等可以学到什么   

正如我们的案例一样，所有的相关人员都能学到点什么：  

* 开发人员：确认你使用的框架不仅仅可以提供你需要的功能性组件，也需要一个生产环境中的监控程序。意思就是要跟着标准实践走，要有错误报告系统。无论是网络服务还是其他框架，确认你给自己的工作选对了工具，同时还得想想其他需要测试监控你程序的人。   

* 系统架构师：持续的监控实时系统，确认它是在按照你设计的方式工作。当需要物理迁移或者是虚拟迁移的时候，要确认一切如期运行。是不是所有的配置文件都部署成功并且是最新的?每个服务是否还能互相调用？跟操作人员协同看下以来系统是否能够连接。   

* 操作人员：确保你理解了所有系统的以来项，一起一些配置元素。跟工程师咨询下如何监控是否成功的部署了，以及如何操作这个网络服务。光检测入口 是否就足够了？你是否需要去监控后端服务？是不是光监控网络服务日志就可以了还是需要扩展监控组件？  

* 业务人员：如果你的业务需要这些网络服务，确保你获取到了相关的监控这些服务的正常运行的面板，用户数据比如失败或者成功请求的数量。可能话，就分析下为什么请求会失败。例如：是不是用户输错了证书（这样你就知道怎么解决问题了）或者还有别的问题（这时候你就需要联系你的操作人员以及开发商）。   

包括我们在内的许多组织尝试持续交付，但是代价太大了，而且总是会有自动部署捕捉不到的问题。开发人员和操作人员也需要持续的进步，这正是我们在努力的方向。我同时也希望分享别人的坑可以帮助你以后不掉进类似的坑。我们也很欢迎你能分享你的故事，跟大家分享下你在你的工作中是如何解决性能和部署问题的。


