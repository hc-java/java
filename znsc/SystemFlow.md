# 系统流程



将采集到的蔬菜大棚环境数据(例如：温度，湿度...)存入数据采集表中；开启多线程，不断处理采集到的数据。

首先，将数据采集表中的"蔬菜名称" "大棚名称"与蔬菜种植表中的"蔬菜名称" "大棚名称"关联，可以得出当前蔬菜生长的状态(种子期、生长期、成熟期)，

然后数据采集表中的数据会和相对应的蔬菜标准信息种植表对比，若对比结果异常，则计算出"差值"(水电费表会增加"差值")，并记录异常信息(记录日志、报表)，

最后发送相对应的指令给硬件，让硬件做出相应的操作，使大棚内的环境适合该蔬菜生长。

