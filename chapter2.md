# 贰、行情
## 一、Request-Reply
&emsp;&emsp;**cleint 发送 Request，等待 Server 回复 Reply。client 透过此联机登入即发送订阅报价指令。**

### 1.账号登入

a.登入指令
```
{
  "Request":"LOGIN",
  "Param":
  {
    "SystemName":"xxxxx",
    "ServiceKey":"xxxxx"
  }
}
```

<br>

b.登入回复
- 登入验证成功 
```
{
	"Reply":"LOGIN",
  "Success":"OK",
	"SessionKey":"XXXXXXXXX",
  "SubPort":n
}
```

注1:SessionKey，之后所有Request指令一律需要带SessionKey。SessionKey错误，指令部执行。

注2:SubPort提供client用来建立第二条ZeroMQ Pub/Sub联机，用来接收主推行情。

<br>

- 登入验证失败
```
{
	"Reply":"LOGIN",
  "Success":"Fail",
	"SessionKey":""
}
```

<br>

c.注销指令
```
{
  "Request":"LOGOUT",
}
```

<br>

### 2.全部合约查询
a.全部合约查询
```
{
  "Request":"QUERYALLINSTRUMENT",
  "SessionKey":"XXXXXXXXX"
  "Type":"Future"
}
```
 **Type:Future or Options or Stock**

 <br>
b.全部合约回复
```
{
	"Reply":"QUERYALLINSTRUMENT",
  "Success":"OK",
  "Instruments":
  {"CHS" : "期货", 
   "CHT" : "期货", 
   "ENG" : "ALGOSTARS_EXG", 
   "EXGID" : "", 
   "Node" : 
   [
      {"CHS" : "中国金融交易所", 
       "CHT" : "中国金融交易所", 
       "ENG" : "CFFEX", 
       "EXGID" : null, 
       "Node" : 
         [
             {"CHS" : "热门月", 
              "CHT" : "热门月", 
              "ENG" : "HOT",
              "Contracts" : 
              ["TC.F.CFFEX.IF.201906", 
               "TC.F.CFFEX.IH.201906", 
               "TC.F.CFFEX.IC.201906", 
               "TC.F.CFFEX.TF.201906", 
               "TC.F.CFFEX.T.201909"], 
              "ExpirationDate" : 
              ["20190621", 
               "20190621", 
               "20190621", 
               "20190614", 
               "20190916"], 
              "InstrumentID" : 
              ["IF1906", 
               "IH1906", 
               "IC1906",…]
             …}
         …]
      …}
   …]
  …}
}
```

<br>

### 3.商品交易设定查询

a.商品交易设定查询
```
{
  "Request":"QUERYINSTRUMENTINFO",
  "SessionKey":"XXXXXXXXX",
  "Symbol":"TC.F.CFFEX.IF.201903"
}
```

<br>

b.商品交易设定回复
```
{
	"Reply":"QUERYINSTRUMENTINFO",
  "Success":"OK",
  "Info":
{
"TC.F.CFFEX.IF":{"Currency":"CNY","Denominator":"1","EXG":"CFFEX","EXG.CTP":"CFFEX","EXG.SIM":"CFFEX","Group.CHS":"指数","Group.CHT":"指数","Group.ENG":"Equities","I3_TickSize":"0.2","Multiplier.ESNH":"1","Multiplier.ESXYA":"1","Multiplier.GQ2":"1","Name.CHS":"沪深300","Name.CHT":"沪深300","Name.ENG":"CSI 300","pinyin":"hs300","OpenCloseTime":"09:30~11:30;13:00~15:00","Symbol":"IF","Symbol.CTP":"IF"...}
  }
}
```

<br>

### 4.订阅报价

a.订阅指令
```
{
  "Request":"SUBQUOTE",
  "SessionKey":"XXXXXXXXX"
"Param":
  {
    "Symbol":"TC.F.CFFEX.IF.201903",
    "SubDataType":"REALTIME",
     ...
  }
}
```

<br>

b.订阅回复
```
{
	"Reply":"SUBQUOTE",
  "Success":"OK"
}
```

<br>

c.解除订阅指令
```
{
  "Request":"UNSUBQUOTE",
  "SessionKey":"XXXXXXXXX"
"Param":
  {
    "Symbol":"TC.F.CFFEX.IF.201903",
    "SubDataType":"REALTIME",
     ...
  }
}
```
注1:报价订阅成功，会在Pub/Sub联机收到行情。

<br>

### 5.PONG
```
{
  "Request":"PONG",
  "SessionKey":"XXXXXXXXX"
}
回復
{
   "Reply":"PONG",
  	"Success":"OK"
}
```

<br>


## 二、Publish-Subscribe
&emsp;&emsp;**cleint 透过 pub/sub 联机，接收主推行情**

### 1.实时行情
```
TC.F.CFFRX.IF.201903:{
	"DataType":"REALTIME",
  "Quote":
  {
     "Symbol":"TC.F.CFFRX.IF.201903",
     "TradingPrice":"3886.5",
     "TradeQuantity":"35435",
     ...
  }
}
```

<br>

### 2.Greeks
```
TC.F.CFFRX.IF.201903:{
	"DataType":"GREEKS",
  "Quote":
  {
     "Symbol":"TC.F.CFFRX.IF.201903",
     "TradingPrice":"3886.5",
     "TradeQuantity":"35435",
     ...
  }
}
```

<br>

### 3.DaVinci 讯号
```
{
	"DataType":"DavinciSignal",
  "Data":
  {
    "Symbol":"XXX",
    "Account":"XXX",
     ...
  }
}
```

<br>

### 4.PING
```
{
	"DataType":"PING"
}
```

## 三、历史资料取得
&emsp;&emsp;**使用 REQ/REP 联机，回补历史数据**

### 1.历史资料

a.回补指令
```
{
  "Request":"SUBQUOTE",
  "SessionKey":"XXXXXXXXX"
"Param":
  {
    "Symbol":"TC.F.CFFEX.IF.201903",
    "SubDataType":"1K",
     ...
  }
}
```
注1:SubDataType使用TICKS or 1Kor DK，且填写StartTime、EndTime，回补历史数据。

<br>

b.回补指令回复
```
{
	"Reply":"SUBQUOTE",
  "Success":"OK"
}
```
于PUB/SUB联机，收到数据准备完成通知。

<br>

C.订阅回复
```
TC.F.CFFRX.IF.201903:{
	"DataType":"1K",
  "StartTime":"YYMMDDHH",
  "EndTime":"YYMMDDHH",
  "Symbol":"TC.F.CFFRX.IF.201903",
  "Status":"Ready"
}
```
收报数据准备完成通之后，透过REQ/REP联机取得数据

<br>

d.取得历史资指令
```
{
  "Request":"GETHISDATA",
  "SessionKey":"XXXXXXXXX",
"Param":
  {
    "Symbol":"TC.F.CFFEX.IF.201903",
    "SubDataType":"n",
     "QryIndex":"",
     ...
  }
}
```
注1:SubDataType、StartTime、EndTime，需与回补数据指令相同。

<br>

d.取得历史资指令
```
TC.F.CFFRX.IF.201903:{
	"DataType":"1K",
  "HisData":
  [
     {
       "Date":"YYYYMMDD",
       "Time":"HHMMSS",
       "Open":"3882",
       "High":"3886.5",
       "QryIndex":""
       ...
     },
     {
       "Date":"YYYYMMDD",
       "Time":"HHMMSS",
       "Open":"3882",
       "High":"3886.5",
       "QryIndex":""
       ...
     }
  ]
}
```

<br>

e.取得歷史資料返回結果
```
TC.F.CFFRX.IF.201903:{
	"DataType":"1K",
  "HisData":
  [
     {
       "Date":"YYYYMMDD",
       "Time":"HHMMSS",
       "Open":"3882",
       "High":"3886.5",
       "QryIndex":""
       ...
     },
     {
       "Date":"YYYYMMDD",
       "Time":"HHMMSS",
       "Open":"3882",
       "High":"3886.5",
       "QryIndex":""
       ...
     }
  ]
}
```
