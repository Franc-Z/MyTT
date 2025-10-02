# MyTT (My Mai language, Tongdaxin, Tonghuashun)
MyTT is the Swiss Army knife in your quantitative toolbox — concise and efficient. It ports indicator formulas from Tongdaxin, Tonghuashun, and Wenhua Mai language to Python with minimal code. The core library is a single file with only a few hundred lines, implementing and converting common indicators such as MACD, RSI, BOLL, ATR, KDJ, CCI, PSY, etc. All functions are built on numpy and pandas, resulting in simple and high-performance implementations. It can be easily applied to stock market technical analysis, automated trading, and crypto (e.g., BTC) quantitative workflows. Mini Python library with most stock market indicators.

[![license](https://img.shields.io/:license-gpl-blue.svg)](https://badges.gpl-license.org/)

# Features
* Lightweight core: The project is a single file [MyTT.py](https://github.com/mpquant/MyTT/blob/main/MyTT.py). No installation or setup required. Freely trim and just use `from MyTT import *`.

* Human-readable code: no fancy tricks. Beginners can understand it, add indicators themselves, and use it immediately in projects.

* No ta-lib required: the core logic is implemented in pure Python. Avoids the pain many have had installing ta-lib.

* Fully compatible with Tongdaxin and Tonghuashun indicator syntax. New formulas typically work out of the box with little to no changes.

* High performance: almost no loops; indicators are implemented with numpy and pandas built-ins.

* Like TA-Lib: multi-period input and multi-period output (sequence in, sequence out), convenient for plotting and observing trends.

* Indicator values from MyTT match Tongdaxin, Tonghuashun, and Xueqiu to two decimal places.

* Advanced edition with complex utilities and experimental functions: [MyTT_plus](https://github.com/mpquant/MyTT/blob/main/MyTT_plus.py)

* Python 2 compatibility with older versions of pandas: use [MyTT_python2](https://github.com/mpquant/MyTT/blob/main/MyTT_python2.py)


### Quick start example  

```python

# Demonstration of cryptocurrency market data retrieval and indicator calculation
from  hb_hq_api import *         # Cryptocurrency market data library
from  MyTT import *              # MyTT indicator toolbox

# Fetch 120 days of data for btc.usdt pair
df=get_price('btc.usdt',count=120,frequency='1d');     # '1d' means 1 day; '4h' means 4 hours

#----------- df result table (similar for stock markets) -------------------------------------------
```

|  |open|	close|	high	|low|	vol|
|--|--|--|--|--|--|
|2021-05-16	|48983.62|	47738.24|	49800.00|	46500.0	|1.333333e+09 |
|2021-05-17	|47738.24|	43342.50|	48098.66|	42118.0	|3.353662e+09 |
|2021-05-18	|43342.50|	44093.24|	45781.52|	42106.0	|1.793267e+09 |


```python

#------- Data ready; now start the main part -------------
CLOSE=df.close.values;  OPEN=df.open.values           # Basic data definitions; any sequence-like input works   
HIGH=df.high.values;    LOW=df.low.values             # For example, CLOSE=list(df.close) is equivalent

MA5=MA(CLOSE,5)                                       # Get 5-day moving average series
MA10=MA(CLOSE,10)                                     # Get 10-day moving average series

print('BTC 5-day MA', MA5[-1] )                       # Take only the last value   
print('BTC 10-day MA',RET(MA10))                      # RET(MA10) == MA10[-1]
print('Did the 5-day MA cross above the 10-day MA today?',RET(CROSS(MA5,MA10)))
print('Are the last 5 closes all above the 10-day MA?',EVERY(CLOSE>MA10,5) )

```
### Installation
* Copy `MyTT.py` into your project. Use `from MyTT import *` to call all functions in the file.

* Install via pip: `pip install MyTT`

```python
from  MyTT import *                 # Import MyTT; note case sensitivity
S=np.random.randint(1,99,[10])      # Generate 10 integers in the range [1, 99]
EMA(S,6)                            # Compute 6-period EMA for sequence S
```

### Tutorials and examples
* [通达信公式转Python神器——MyTT库](https://www.joinquant.com/view/community/detail/a6cc7d1fb73a57dbac4b77044a33b15d)  

* [利用MyTT库整合通达信指标公式](https://www.joinquant.com/view/community/detail/4237ebaa5db39a5a9a2195338e8be588)  

* [MyTT库应用示例及计算精度验证](https://www.joinquant.com/view/community/detail/bd26874654a6f9f1958f23043ca06149)  

* [如何在聚宽研究环境中建立myTT.py库文件](https://www.joinquant.com/view/community/detail/2abf0cc457352b59ef2e873ad7c4e430)  

* [基于MyTT来编写Python版通达信指标](https://www.joinquant.com/view/community/detail/7a0297fb7bd717cfb2be40b4c8062eeb)  

* [MyTT基础函数EMA指数平均的公式推导](https://www.joinquant.com/view/community/detail/ab76489c8fdfd1f201b6df47f11a5360)


### Selected utility functions in MyTT
* n天前的数据：`REF`
```python
REF(CLOSE, 1)              # 截止到昨天收盘价 序列
REF(CLOSE, 1)              # Series up to yesterday's close
```

* 移动平均线计算：MA
```python
MA(CLOSE, 5)             # 5-day moving average of close
```

* 加权移动平均计算：EMA
```python
EMA(CLOSE, 5)            # For accuracy, EMA needs at least 120 periods   
```

* 中国式的SMA计算：SMA
```python
SMA(CLOSE, 5)            # For accuracy, SMA needs at least 120 periods   
```

*  返回序列标准差：STD
```python
STD(CLOSE, 5)             # Standard deviation of close over the last 5 days
```

*  平均绝对偏差：`AVEDEV`
```python
AVEDEV(CLOSE, 5)    # Mean absolute deviation from the average
```

* 金叉判断：CROSS
```python
CROSS(MA(CLOSE, 5), MA(CLOSE, 10))       # 5-day MA crosses above 10-day MA
```

* 两个序列取最大值,最小值：`MAX`  `MIN`
```python
MAX(OPEN, CLOSE )                       # Highest of OPEN and CLOSE (candle body high)
```

* n天内满足条件的天数：COUNT
```python
COUNT(CLOSE > OPEN, 10)                 # Number of up closes in the last 10 days
```

* n天内全部满足条件的天数：EVERY
```python
EVERY(CLOSE >OPEN, 5)                   # All of the last 5 days are up closes
```

* 从前A日到前B日一直满足条件 ：LAST
```python
LAST(CLOSE>OPEN,5,3)                    # Whether 5 to 3 days ago were all up closes
```

* n天内是否至少满足条件一次：EXIST
```python
EXIST(CLOSE>OPEN, 5)                   # Whether there is at least one up close in the last 5 days
```

* 上一次条件成立到当前的周期：BARSLAST
```python
BARSLAST(CLOSE/REF(CLOSE)>=1.1)         # Days since the last limit-up
```

* 返回序列的线性回归斜率：`SLOPE`
```python
SLOPE(MA(CLOSE,10),5)                   # Slope of 10-day MA over the last 5 days (MA direction)
```

* 取回线性回归后的预测值：`FORCAST`
```python
FORCAST(CLOSE,20)                       # Predict tomorrow's close from the last 20 days' trend
```

*  n天内最大值：`HHV`
```python
HHV(MAX(OPEN, CLOSE), 20)               # Highest of OPEN and CLOSE over the last 20 days
```

* n天内最小值：`LLV`
```python
LLV(MIN(OPEN, CLOSE), 60)              # Lowest of OPEN and CLOSE over the last 60 days
```

* 条件 `IF`
```python
IF(OPEN > CLOSE, OPEN, CLOSE)          # If open > close, return OPEN; otherwise return CLOSE
```

### 具体指标的实现，全部基于MyTT库中的工具函数 （更多指标可以自行添加）

```python
def MACD(CLOSE,SHORT=12,LONG=26,M=9):    # With EMA, use 120 days of CLOSE; matches Xueqiu to 2 decimals
    DIF = EMA(CLOSE,SHORT)-EMA(CLOSE,LONG);  
    DEA = EMA(DIF,M);      MACD=(DIF-DEA)*2
    return RD(DIF),RD(DEA),RD(MACD)
```

```python
def KDJ(CLOSE,HIGH,LOW, N=9,M1=3,M2=3):   
    RSV = (CLOSE - LLV(LOW, N)) / (HHV(HIGH, N) - LLV(LOW, N)) * 100
    K = EMA(RSV, (M1*2-1));    D = EMA(K,(M2*2-1));        J=K*3-D*2
    return K, D, J
```

```python
def RSI(CLOSE, N=24):                     # RSI indicator
    DIF = CLOSE-REF(CLOSE,1) 
    return RD(SMA(MAX(DIF,0), N) / SMA(ABS(DIF), N) * 100)  
```

```python
def WR(CLOSE, HIGH, LOW, N=10, N1=6):    # Williams %R indicator
    WR = (HHV(HIGH, N) - CLOSE) / (HHV(HIGH, N) - LLV(LOW, N)) * 100
    WR1 = (HHV(HIGH, N1) - CLOSE) / (HHV(HIGH, N1) - LLV(LOW, N1)) * 100
    return RD(WR), RD(WR1)
```

```python
def BIAS(CLOSE,L1=6, L2=12, L3=24):      # BIAS (bias rate)
    BIAS1 = (CLOSE - MA(CLOSE, L1)) / MA(CLOSE, L1) * 100
    BIAS2 = (CLOSE - MA(CLOSE, L2)) / MA(CLOSE, L2) * 100
    BIAS3 = (CLOSE - MA(CLOSE, L3)) / MA(CLOSE, L3) * 100
    return RD(BIAS1), RD(BIAS2), RD(BIAS3)
```

```python
def BOLL(CLOSE,N=20, P=2):                # Bollinger Bands    
    MID = MA(CLOSE, N); 
    UPPER = MID + STD(CLOSE, N) * P
    LOWER = MID - STD(CLOSE, N) * P
    return RD(UPPER), RD(MID), RD(LOWER)    
```

```python
def PSY(CLOSE,N=12, M=6):                 # PSY (Psychological Line) indicator
    PSY=COUNT(CLOSE>REF(CLOSE,1),N)/N*100
    PSYMA=MA(PSY,M)
    return RD(PSY),RD(PSYMA)
```

```python
def CCI(CLOSE,HIGH,LOW,N=14):            # CCI (Commodity Channel Index)
    TP=(HIGH+LOW+CLOSE)/3
    return (TP-MA(TP,N))/(0.015*AVEDEV(TP,N))
```

```python
def ATR(CLOSE,HIGH,LOW, N=20):           # Average True Range over N days
    TR = MAX(MAX((HIGH - LOW), ABS(REF(CLOSE, 1) - HIGH)), ABS(REF(CLOSE, 1) - LOW))
    return MA(TR, N)
```

```python
def BBI(CLOSE,M1=3,M2=6,M3=12,M4=20):    # BBI (Bull and Bear Index)   
    return (MA(CLOSE,M1)+MA(CLOSE,M2)+MA(CLOSE,M3)+MA(CLOSE,M4))/4  
```


```python
def TAQ(HIGH,LOW,N):                         # Donchian Channel (Turtle) indicator; simple and robust across bull/bear
    UP=HHV(HIGH,N);    DOWN=LLV(LOW,N);    MID=(UP+DOWN)/2
    return UP,MID,DOWN
```

```python
def KTN(CLOSE,HIGH,LOW,N=20,M=10):           # Keltner Channel, N=20, ATR period=10
    MID=EMA((HIGH+LOW+CLOSE)/3,N)
    ATRN=ATR(CLOSE,HIGH,LOW,M)
    UPPER=MID+2*ATRN;   LOWER=MID-2*ATRN
    return UPPER,MID,LOWER   
```

```python
def TRIX(CLOSE,M1=12, M2=20):                # Triple Exponential Moving Average
    TR = EMA(EMA(EMA(CLOSE, M1), M1), M1)
    TRIX = (TR - REF(TR, 1)) / REF(TR, 1) * 100
    TRMA = MA(TRIX, M2)
    return TRIX, TRMA
```

```python
def BRAR(OPEN,CLOSE,HIGH,LOW,M1=26):         # BRAR (AR/BR) sentiment indicators  
    AR = SUM(HIGH - OPEN, M1) / SUM(OPEN - LOW, M1) * 100
    BR = SUM(MAX(0, HIGH - REF(CLOSE, 1)), M1) / SUM(MAX(0, REF(CLOSE, 1) - LOW), M1) * 100
    return AR, BR
```

```python
def MTM(CLOSE,N=12,M=6):                    # Momentum indicator
    MTM=CLOSE-REF(CLOSE,N);         MTMMA=MA(MTM,M)
    return MTM,MTMMA
```
```python
def ROC(CLOSE,N=12,M=6):                     # Rate of Change indicator
    ROC=100*(CLOSE-REF(CLOSE,N))/REF(CLOSE,N);    MAROC=MA(ROC,M)
    return ROC,MAROC
```
```python
def EXPMA(CLOSE,N1=12,N2=50):                # Exponential Moving Average indicator
    return EMA(CLOSE,N1),EMA(CLOSE,N2);
``` 
```python
def OBV(CLOSE,VOL):                          # On-Balance Volume indicator
    return SUM(IF(CLOSE>REF(CLOSE,1),VOL,IF(CLOSE<REF(CLOSE,1),-VOL,0)),0)/10000
``` 

```python
def MFI(CLOSE,HIGH,LOW,VOL,N=14):            # MFI is an RSI-style indicator using volume
    TYP = (HIGH + LOW + CLOSE)/3
    V1=SUM(IF(TYP>REF(TYP,1),TYP*VOL,0),N)/SUM(IF(TYP<REF(TYP,1),TYP*VOL,0),N)  
    return 100-(100/(1+V1))    
``` 



* More indicators are available in the library file [MyTT.py](https://github.com/mpquant/MyTT/blob/main/MyTT.py)

### Syntax notes: `=:` is invalid; Python uses `=`. Logical AND is `&`, OR is `|`.
```python

# Tongdaxin function VAR1:=(C>REF(C,1) AND C>REF(C,2));
 Python version: VAR1=( (CLOSE>REF(CLOSE,1)) & (CLOSE>REF(CLOSE,2)) );

# Close above the 10-day MA and the 10-day MA above the 20-day MA
Python version: (C > MA(C, 10)) & (MA(C, 10) > MA(C, 20))

# Up close or close greater than previous close
Python version: (CLOSE > O) | (CLOSE > REF(CLOSE, 1))

```


### Bollinger Bands (BOLL) data retrieval and plotting demo (Shanghai Composite)

```python
up,mid,lower=BOLL(CLOSE)                                        # Get Bollinger Bands data 

plt.figure(figsize=(15,8))  
plt.plot(CLOSE,label='Shanghai Composite');    plt.plot(up,label='up');        # Plot 
plt.plot(mid,label='mid');      plt.plot(lower,label='lower');

```
<div  align="center"> <img src="/img/boll.png" width = "960" height = "400" alt="Boll line" /> </div>


### Donchian Channel (Turtle) indicator calculation and plotting demo (CSI 300 Index)

```python
up,mid,down=TAQ(HIGH,LOW,20)                                    # Get Donchian Channel data; simple and robust across markets
plt.figure(figsize=(15,8))  
plt.plot(CLOSE,label='CSI 300 Index')                                  
plt.plot(up,label='Donchian-Upper');     plt.plot(mid,label='Donchian-Middle');      plt.plot(down,label='Donchian-Lower')
```
<div  align="center"> <img src="/img/taq.jpg" width = "960" height = "400" alt="taq" /> </div>


### Required third-party libraries (no ta-lib; only pandas is needed)
* pandas


### Contributors 
    火焰，jqz1226, stanene, bcq


----------------------------------------------------
### Other open-source projects from our team — If this project helps you, please star it!
* [MyTT 通达信,同花顺公式指标，文华麦语言的python实现](https://github.com/mpquant/MyTT)

* [Ashare最简股票行情数据接口API,A股行情完全开源免费](https://github.com/mpquant/Ashare)



----------------------------------------------------




----------------------------------------------------
## Star History
[![Star History Chart](https://api.star-history.com/svg?repos=mpquant/MyTT&type=Date)](https://star-history.com/#mpquant/MyTT&Date)
