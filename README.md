# maker
一、注意事项：

1. 使用本程序需自己把控风险，不能保证稳赚不赔。虽然本人尽可能的优化和改进程序，但是不能完全排除程序有可能会出现宕机、下单不及时、撤单不及时、撤单不成功、下单出错等异常情况，也不能保证策略就是赚钱的，非常有可能亏损大于解锁的情况，导致的亏损本人概不负责。需要自己多盯着，多留心，多关注。必须接受这点才能使用本程序。

2.本程序免费使用，但是使用前需将用户名（fmex用户名，中间可以用XXXX代替，但是开头和结尾需保留，注意发我什么就填什么，发我的中间是XXXX，配置里也得填XXXX，不然程序不会运行，如FC_XXXX1243）和API key以及的secret的最后8位发给我授权使用，还得发下使用本程序的简单密码，如"123"，可以私信我或者发我邮箱 admin@zuanqianla.cn

3. 该程序需要在Linux环境下运行，需要有一定的操作基础。建议注册AWS或者阿里云的spot竞价请求，选东京的，大概几毛钱一小时，且可随时关闭停止。

4. API申请的时候请务必绑定IP地址，如若因API导致泄密或者被盗，本人不负责。同时申请API的时候需勾选允许只读和允许交易。

5. 由于有很多账户相关的查询，建议单独用一个账号使用该程序，不要跟其他策略混合用。

6. 如果需要停止程序，请先终止daemon程序，再kill掉对应的maker进程。同时需要手动在网页上操作撤单平仓等动作。

二、简单说明：

程序有2个，1个是maker主程序，一个是daemon守护程序。将该压缩包解压放到/home/目录下，运行daemon程序既可自动运行。默认配置了一个程序（买卖各一个档位），如果需要排多个档位，需要多开，可以编辑daemon中的数字，改成需要的个数。同时配置对应编号的config文件。注意，每个maker进程必须单独放在一个目录里，并且不能重名。

三、关于功能和配置：

stop用于开启或者停止，=1停止并待机，=0程序开始工作

auto_upgrade=1，自动升级是否打开，=1有新版本会自动升级，=0不会自动升级 （注意：开启自动升级功能，程序只能放在/home/maker1/目录下，maker2, maker3将不能使用）

professional=0
，=0参数自动生成，只需要设置方向，风险等级，和仓位3个参数，其他都由系统自动生成，可以设置止盈止损。=1，参数完全由自己设置，有经验者可自己调整参数

direction=3，方向设置，1,强烈看多，=2看多，=3平衡，=4看空，=5，强烈看空  =6，方向由MACD智能判断，=7，价格高于high做空，低于low做多，中间平衡，=8， 价格高于high做多，低于low做空，中间平衡 （该参数只有当professional=0时才生效）

risk=3
，风险等级设置，1,非常谨慎，2谨慎，3正常，4激进，5非常激进 （该参数只有当professional=0时才生效）

max_position=2000.0，最大持仓设置，很有可能由于获取仓位慢了超过一点 （该参数只有当professional=0时才生效）

buy_enable，买使能，=1使能，=0，禁用。  对自动平仓不起作用

sell_enable，卖使能，=1使能，=0，禁用。  对自动平仓不起作用

usr_id，使用该软件的用户名，用于授权使用该软件的, 请使用fmex用户名，建议不要写全，中间可以用XXXX代替，但是开头和结尾需保留，以便我区分。如14XXXXXX62这样的，注意发我什么就填什么，发我的中间是XXXXXX，也得填XXXXXX，不然程序不会运行。

password，使用该软件的密码，用于授权使用该软件的，不是fmex的密码，同时建议不要用你自己的常用密码，用个123等简单的就行，主要是防止别人用这个用户名。

api_key, 仅填前面24位，保证安全。后面8位发给我

api_secret, API秘钥，仅填前面24位，保证安全。后面8位发给我

huntdown_enable， =1开启追涨杀跌功能，=0，禁用追涨杀跌功能，适合单边行情和暴涨暴跌行情。注意该功能会与止盈止损功能冲突，开启它请禁用止盈止功能，MACD智能判断建议禁用这个功能

stop_lost_enable， =1开启止损功能，=0，禁用止损功能 （注意，该参数的改变需要重启程序，该功能与追涨杀跌功能冲突，若要启用它，请先禁用追涨杀跌功能，MACD智能判断建议禁用这个功能)

stop_lost_value=1.0, 持仓价与当前价格的最大价差百分比，比如设置1.0，持有空单均价10000，当前价9900，相差了1%，就会触发止损。大家需要根据各自的杠杆率来设置这个值

stop_lost_sleep=900，止损后的停机时间，单位秒

stop_lost_reverse=100.0，止损后的反向操作数量,单位(张)，比如9900触发空单止损，将继续下N张多单，可以为0，就表示不反向操作

stop_profit_enable, 	=1开启止盈功能，=0，禁用止盈功能 （注意，该参数的改变需要重启程序，该功能与追涨杀跌功能冲突，若要启用它，请先禁用追涨杀跌功能，MACD智能判断建议禁用这个功能)

stop_profit_value=0.5,  持仓价与当前价格的最大价差百分比，比如设置0.5，持有空单均价10000，当前价9950,相差了0.5%，就会触发止盈。大家需要根据各自的杠杆率来设置这个值

stop_profit_sleep=900,  止盈后的停机时间，单位秒

stop_profit_reverse=100.0，止盈后的反向操作数量,单位(张)，比如9360触发空单止盈，将继续下N张多单，可以为0，就表示不反向操作

-----------------------------------------------------------------------------
下面这些参数若professional=0
，就不要去改了(macd相关参数除外)
-----------------------------------------------------------------------------

maker_buy_quantity，单次挂买单数量

maker_sell_quantity，单次挂卖单数量

maker_offset, 偏移量，每个程序配置的不一样，这样就不会挤在一起去。允许负值，如果调不到盘口，可以试着为负值，但风险需要把握

maker_range=1.5100，允许的上下波动范围，防止频繁撤单，建议挂的近调小点，挂的远调大点。后面多加0.01，防止精度问题出错 

maker_buy_C=0.0200，买安全系数，建议以0.02为基础，0.01为步进这样微调

maker_sell_C=0.0200，卖安全系数，建议以0.02为基础，0.01为步进这样微调

maker_S=0.5000，5分钟振幅系数，假设5分钟波动20美元，0.5的话就是10美元

maker_max_stop=100000.0，超过这个持仓，不再下这个方向的挂单。只能大概，有可能由于获取资金慢了，导致超过这个阈值。也可能刹车不及时有点点超过

taker_enable=1，吃单使能，=1使能，=0，禁用。

taker_buy_quantity=444.0000，每次买的吃单数量

taker_sell_quantity=222.0000，每次卖的吃单数量

taker_buy_T1=0.0500, 买吃单灵敏度系数1，触发吃单条件1系数，自由调整，调大点就不容易吃，调小点就经常吃单

taker_sell_T1=0.0500, 卖吃单灵敏度系数1，触发吃单条件1系数，自由调整，调大点就不容易吃，调小点就经常吃单

taker_buy_T2=0.0250, 买吃单灵敏度系数2，触发吃单条件2系数，自由调整，调大点就不容易吃，调小点就经常吃单，推荐T2=0.5T1，当然，也不是绝对，可以根据实际情况调整

taker_sell_T2=0.0250, 卖吃单灵敏度系数2，触发吃单条件2系数，自由调整，调大点就不容易吃，调小点就经常吃单，推荐T2=0.5T1，当然，也不是绝对，可以根据实际情况调整

taker_price=4，吃单档位，买吃单价=卖1价+0.5×taker_price， 卖吃单价=买1价-0.5×taker_price

taker_sleep=1000，吃单完成后的延时，单位毫秒，防止吃单过快过多。

autoclose_enable=0，自动平仓功能是否开启，=1将开启 =0不生效，（注意，该参数的改变需要重启程序)

autoclose_time=1800，自动平仓功能的时间上限。单位秒，有几秒钟的误差。

autoclose_max=185000.0，自动平仓功能的持仓上限。自动平仓功能开启才生效，如果仓位连续autoclose_time大于这个值，将自动触发自动平仓。如果中途autoclose_max不符合，时间将复位重新计时。注意，这个值必须大于等于上面的maker_max_stop参数,否则可能会有问题

autoclose_price=1，自动平仓的价格档位，买吃单价=卖1价+0.5×taker_price， 卖吃单价=买1价-0.5×taker_price

autoclose_quantity=99999.0，自动平仓单次最大下单量

miner_enable=0，交易挖矿是否开启，注意开启注定有矿损，请慎重，竞争不大时候可以刷1试试。假设刷一次手续费万分之5，一次1美元，不考虑高买低卖情况，只消耗万分之5美元的手续费，而1分钟的解锁量1天1btc的话大概是1美元，竞争不大还是有搞头

miner_mode=0，交易挖矿的挖矿模式，0-智能判断，1-只做多，2，只做空，3，多空循环交替

miner_time=59，刷单时间，单位秒，默认1分钟刷一次

miner_quantity=1.00，一次刷单量，建议少量，因为基本上是要亏手续费的

cancel_time=3601，挂单里的最长挂单时间，超过这个值将撤单，单位秒。5分钟轮询一次，可能会有5分钟误差。主要是撤单失败的补充。

filled_sleep=5000, 如果单子被成交，到下一个挂单的延迟时间，单位毫秒，防止行情数据不及时或者方向判断出错，导致错误的快速大量下单

gap=20，允许偏离其他交易所的最大差值。防止深度太差，挂出天价买单和天价卖单导致大量亏损，只对挂单有效，吃单不受这个限制


[ADVANCED]

high=20000.00，价格区间顶部，只有当professional=0且direction=7或者8才生效

low=3000.00，价格区间底部，只有当professional=0且direction=7或者8才生效

注意：MACD相关参数，只有当professional=0

且direction=6才生效

macd_period=1day K线周期选择,有这些选项,1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year

macd_fastlen=12	快线

macd_slowlen=26	慢线

macd_signallen=9	信号长度

min_quantity=1.0000 最小下单数量

dt=9000，行情数据与当前系统时间的最大差值，单位毫秒

time_out=25，下单超时时间，单位秒

wdg_time=60，各线程死机看门狗时间，单位秒

ws_wdg_time=60 wss行情数据不更新看门狗时间，单位秒

night_rate=1.0 夜间模式下单比例

night_start=1，夜间模式起始时间

night_end=9
，夜间模式截止时间


给出计算公式，

买挂单价=多空参考价 - 买1价*maker_buy_C/100 - 5分钟振幅*S - 偏移量  

卖挂单价=多空参考价 + 卖1价*maker_sell_C/100 + 5分钟振幅*S + 偏移量

挂单后的撤单触发条件，当前价 >挂单价+range 或 当前价 < 挂单价-range

----------------------------------------------------------------------------------

一个参考配置

[BASE]

auto_upgrade=1

professional=0

direction=7

risk=3

max_position=6000.0

stop=0

buy_enable=1

sell_enable=1

symbol=btcusd_p

usr_id=xx

password=xx

api_key=xx

api_secret=xx

###################################

huntdown_enable=1

stop_lost_enable=0

stop_lost_value=0.5

stop_lost_sleep=900

stop_lost_reverse=0.0

stop_profit_enable=0

stop_profit_value=0.5

stop_profit_sleep=900

stop_profit_reverse=0.0

###################################

maker_enable=1

maker_buy_quantity=3333.0000

maker_sell_quantity=3333.0000

maker_offset=0.00

maker_range=0.1100

maker_buy_C=-0.0249

maker_sell_C=-0.0249

maker_S=0.0500

maker_max_stop=3333.0000

###################################

taker_enable=1

taker_buy_quantity=2222.0000

taker_sell_quantity=2222.0000

taker_buy_T1=0.045

taker_sell_T1=0.045

taker_buy_T2=0.0154

taker_sell_T2=0.0154

taker_price=10

taker_sleep=1000

###################################

autoclose_enable=0

autoclose_max=1950.0000

autoclose_time=1800

autoclose_price=4

autoclose_quantity=500.0000

###################################
miner_enable=0

miner_mode=0

miner_time=59

miner_quantity=1.00

###################################

cancel_time=3601

gap=10.0000

filled_sleep=5000

###################################

[ADVANCED]

high=20000.00

low=3000.00

macd_period=5min

#1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1mon, 1week, 1year

macd_fastlen=12

macd_slowlen=26

macd_signallen=9

min_quantity=1.0000

dt=9000

time_out=25

wdg_time=60

ws_wdg_time=60

night_rate=1.0

night_start=1

night_end=9





