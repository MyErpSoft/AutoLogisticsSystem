# 概述
自动化的货运系统期望实现端对端的自动化、低能耗和快速的货物运输。本文主要以电子商务形成的快递业务为描述对象，但还包括但不仅限于工厂到工厂、工厂到商场的货物运输。  
# 目标
## 短期目标 站点到站点运输
在短期目标中，利用现有的铁路和公路交通系统，实现：  
* 货物在站点的自动化分拣、装货
* 货物的路线制定
* 货物的追踪反馈

例如，一位网购者购买了一件衣服，电商向他熟悉的物流公司提交了一个快递单，物流公司仍然使用人工的方法，完成取货，分拣及运输到城市的总站。  
现在城市的站点扫描货物并接受了快递，快递被放在一个特制的、大小合适的盒子，这样盒子与快递关联起来，然后盒子被自动分拣系统存入仓库等待发货。  
在后台，站点与运输商的计算机互相沟通，告知已有快递的大小、路线、时间限制等一系列信息，运输商对自己的运输资源进行配置从而制定了运输路线，运输商的司机收到指令清单，将火车或汽车行驶到清单指定的站点，自动化分拣系统将快递转入集装箱，然后由司机将集装箱从一个站点运输到另外一个站点，到达后，自动化分拣系统负责卸下整个集装箱或者仅是抽取几个盒子，然后再装上新的集装箱或盒子，继续前行。  
货物到达最后的站点后，人工拆除盒子取出货物，并继续采用人工的方式发往目的地，最终网购者拿到货物。  
## 中期目标 自动化的运输
中期目标中，系统充分利用现有的轨道系统以及最新的自动驾驶系统完成站点到站点的自动化运输，实现低能耗和快速的运输。   
典型的场景是使用现有的铁路系统，自动化控制的小型车头仅仅拖动1-3节短小的车厢，根据运输商的指令开往站点，然后自动化分拣系统卸载和装入货物，完成后小型列车头利用自动驾驶系统运输到指定的地点。  
由于没有人工的参与，自动化控制的运输工具能够有以下好处：  
* 实现24小时不间断的运输，从而提高路网利用率；
* 可以使用60~90公里/小时的经济时速，从而节约能源；
* 小型化的车厢大大减少对路基、桥梁等基础设施的损害，同时也降低噪音；
* 自动化驾驶系统减少因为人工驾驶造成的交通事故。

以上优点虽然更适合轨道系统，但同样适用于公路运输，公路运输可以避开高峰期，而且站点到站点通常是高速公路连接，这样也很好的避开自动化驾驶的弊端。  
## 远期目标 全程自动化
最终的目标是实现整个物流的自动化，例如网购者购买了某个商品后，电商服务器向物流公司下单，物流公司的服务器制定路线确定最优路线和时间，由自动驾驶的小车驶入电商仓库。  
较大的电商拥有自己的分拣系统，自动完成装货。较小的电商通过小车自带的交互系统获取包装盒并放入快件，如果出现问题，也可以通过远程的视频联线人工辅导完成。  
小车将快件运输到站点，由分拣系统暂存到仓库。在之后由中期的系统自动运输到指定站点。再之后，就由自动驾驶的小车派发到指定地点，当然，这里涉及到收货人是否在家/公司的问题，还有收货人的身份确认，物品是否完好的问题。这个问题可以通过遍布的便利店完成，小车实际将快件存入便利店，收货人到便利店完成快件的签收确认。  
# 组成
以下所有论述中，除非特殊声明，所有的系统都是分布式的系统，并没有唯一的入口点。
## 接单系统
整个系统比较庞大，但对于用户来说，仅需要与物流公司打交道。  
{图1}
发件人通过网站提交自己的快递单，以及通过此网站查询物流的进度。更常见的方式是发件人（例如电商）通常将自己的系统与接单系统进行集成，在客户下单后自动产生快递单。  
接单系统是一个与客户打交道的系统，负责接单，查询进度以及与客户进行结算的系统。对内他还负责将确定的订单发送给物流系统，以便开始整个运输过程。另外还接收运输系统传回的状态信息并更新订单的状态。  
接单系统并不负责快件如何运输，例如是自动化运输还是人工派送，路线是什么，都不会他的职责，他将这个任务交付给物流系统。  
## 物流系统
物流系统负责货物的运输工作，他属于物流公司的一部分，物流公司通常包含一个或多个站点，每个站点包含分拣系统和仓库，以及自己的运输工具。  
{图2}
仓库负责暂存快件，等待合适的时机运输出去，他由仓储系统维护。分拣系统负责将仓库中的快件取出装入集装箱，或者从集装箱中取出货物并存入仓库。  
车辆调度系统确定当前物流公司或外部的车辆资源等信息，然后制定一个最优的行车计划并下达给运输系统，运输系统可能采用人工，也可能使用自动化的交通工具完成运输。  
在运输的中途可能需要改为铁路等外部运输公司完成任务，但对于物流系统来说，他是黑箱的，只需要发出指令给运输系统并接收运输状态即可。  
## 其他系统
为配合整个物流的完成，必须有统一的黄页系统，例如物流系统通过它知道从一个地点到另外一个地点有哪些运输公司可以提供服务，有哪些站点可以提供仓储暂存，如何连接他们的服务器，以便自动的询价，确定是否有资源等信息。  
运输系统提供实际的运输管理，他因为功能单一，暂且放入其他系统。  
# 运营
## 标准委员会
由主要的物流公司，货主，运输公司资助成立标准委员会，标准委员会负责：
* 各个系统的界限，定义；
* 制定各种业务交互的计算机交互协议；
* 定义各种盒子的尺寸，叠加方式等规范；
* 推动核心算法的进步；

标准委员会在确定有哪些系统组成，以及他们之间交互的协议后，并不直接开发所有的软件，但会积极推动软硬件厂商开发系统，设立奖金进行算法比赛，资助开源免费的算法和软件。
## 分成
物流公司作为业务的开始，接受快递费。由物流公司与其他系统交互，采用询价，协议价格等市场化的手段完成分成，不存在最高的利益或管理机构。
# 健壮性
整个系统的建设应考虑：
* 各个系统独立，分工明确，即可以自动化运作，也可以人工参与；
* 各个系统必须是分布式的，防止存在中央系统的集中化，导致瓶颈；
* 各系统要具有较强的容错性，例如中途被盗了某个快递，即能够及时发现，又能够不至于整个集装箱停运。或者交通出现拥堵，甚至某个站点被洪水冲毁，都不至于其他系统的瘫痪。  

# 关于
* 作者： 谈少民  
* Email：tansm@msn.com  

本文发布在：https://github.com/tansm/AutoLogisticsSystem  
> V0.1   
2013/2/8 至 2013/2/17 完成草稿。
