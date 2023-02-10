## data description:
### for smoothed time series comparison
- SN_m_tot_V2.0.csv   太阳黑子月数据，视为raw data
- SN_ms_tot_V2.0.csv    太阳黑子13-month average 数据 (官方计算，用于对比，不使用在正式代码中）
- SN_rolling13_tot.csv    太阳黑子13个月的移动平均值
- SN_rolling6_tot.csv    太阳黑子6个月的移动平均值
- SN_weighted_moving_average130_tot.csv   太阳黑子130个月的加权移动平均值
- SN_weighted_moving_average13_tot.csv   太阳黑子13个月的加权移动平均值
- SN_weighted_moving_average6_tot.csv   太阳黑子6个月的加权移动平均值

### for sliced time slices
- data_view.json    针对edit view需要展示的table数据
- slice_info    对应data_view.json对应的slice方法得到的slice的信息，进行每个slice的信息对比

### tips：
moving average 移动平均值 针对series 以x为窗口，针对前x个得到v_x的值，接着往后滚动一格；这样的话前x-1个元素无法滚动平均，每个元素相同的权重 前x-1我使用对应的平均值进行计算
.rolling(window=x).mean()

weighted moving average 加权移动平均 本质是一种近似求平均的方法，指数式递减加权的移动平均。各个数值的加权随着时间而指数式递减，近期的数据加权重   相当于一种递推


