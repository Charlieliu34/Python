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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-nrix{text-align:center;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</th>
    <th class="tg-nrix"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;合約代碼<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;SubDataType<br>&nbsp;&nbsp;</td>
    <td class="tg-nrix"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;即時行情   (REALTIME)<br>&nbsp;&nbsp;歷史ticks  (TICKS)<br>&nbsp;&nbsp;歷史1K     (1K)<br>&nbsp;&nbsp;歷史日K    (DK)<br>&nbsp;&nbsp;Greeks    (GREEKS)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;StartTime<br>&nbsp;&nbsp;</td>
    <td class="tg-nrix"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;YYMMDDHH<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;EndTime<br>&nbsp;&nbsp;</td>
    <td class="tg-baqh"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;YYMMDDHH<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;合約代碼<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;TradeDate<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;交易日期<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;FilledTime<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;成交時間<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradingPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;成交價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradeQuantity<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;成交單量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Change<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;漲跌<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradeVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;累計成交量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;OpeningPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;開盤價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HighPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;當日最高價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;LowPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;當日最低價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;ClosingPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;收盤價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;ReferencePrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;參考價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UpperLimitPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;漲停價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;LowerLimitPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;跌停價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;YClosedPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;昨收<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;YTradeVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;昨量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;OpenInterest<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;今日未平倉量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;YOpenInterest<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;昨日未平倉量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;FlagOfBuySell<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;內外盤<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;OpenTime<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;開盤時間<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;CloseTime<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;收盤時間<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#1<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid1<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#2<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid2<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#3<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid3<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#4<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid4<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#5<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid5<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#6<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid6<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#7<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid7<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#8<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid8<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#9<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid9<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價#10<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#1<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume1<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#2<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume2<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#3<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume3<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#4<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume4<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#5<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume5<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#6<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume6<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#7<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume7<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#8<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume8<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#9<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BidVolume9<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買量#10<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#1<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask1<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#2<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask2<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#3<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask3<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#4<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask4<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#5<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask5<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#6<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask6<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#7<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask7<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#8<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask8<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#9<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask9<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價#10<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#1<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume1<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#2<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume2<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#3<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume3<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#4<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume4<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#5<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume5<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#6<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume6<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#7<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume7<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#8<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume8<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#9<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;AskVolume9<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣量#10<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalBidCount<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;委託買進總筆數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalBidVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;委託買進總數量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalAskCount<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;委託賣出總筆數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalAskVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;委託賣出總數量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BuyCount<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;累進買進成交筆數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;SellCount<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;累進賣出成交筆數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;SettlementPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;結算價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;EndDate<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;到期日<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;TradingHours<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;交易時間<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;TradingDay<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;交易日<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;Last<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;最新價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Vol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;合約交易量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Bid<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;ImpVol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;IV<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BIV<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BIV<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;SIV<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;SIV<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Delta<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Delta<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Gamma<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Gamma<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Vega<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Vega<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Theta<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Theta<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Rho<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Rho<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TheoVal<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;理論價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;ExtVal<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;時間價值<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Margin<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;保證金<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;OI<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;未平倉量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;PreOI<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;昨日未平倉量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;IntVal<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;內含價值<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;PreImpVol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;昨日IV<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX值<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UndVol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;標的交易量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UndOI<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;標的未平倉量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV_W4<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV-w4<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV_W8<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV-w8<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV_W13<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV-w13<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV_W26<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV-w26<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV_W52<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;HV-w52<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UnderlyingAsset<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;標的<br>&nbsp;&nbsp;(using GetGreeksString)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX1<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX1<br>&nbsp;&nbsp;(using<br>&nbsp;&nbsp;GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX2<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;VIX2<br>&nbsp;&nbsp;(using<br>&nbsp;&nbsp;GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;CallOI<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Call的未平倉量 (using GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;CallVol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Call的交易量 (using GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;PutOI<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Put的未平倉量 (using GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;PutVol<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Put的交易量 (using GetGreeksValue)<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;合約<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;帳號<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;ReferenceVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;參考口數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalScore<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;總評分<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalScoreChange<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;評分變化量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;FloatProfit<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;持倉即時損益<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Inc<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;損益權重分配<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;RiskTake<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;動用風險<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;NewOrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;下單數量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TotalN<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;開放交易的策略總數上限<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;SumChannel<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;string<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;商品類別<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;合約代碼<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;SubDataType<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;1<br>&nbsp;&nbsp;即時行情   (REALTIME)<br>&nbsp;&nbsp;2<br>&nbsp;&nbsp;歷史ticks  (TICKS)<br>&nbsp;&nbsp;4<br>&nbsp;&nbsp;歷史1K     (1K)<br>&nbsp;&nbsp;5<br>&nbsp;&nbsp;歷史日K    (DK)<br>&nbsp;&nbsp;6 Greeks    (GREEKS)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;StartTime<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;YYMMDDHH<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

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

d.取得历史资料返回结果
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

**IK/DK 资料栏位**

<CENTER>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Date<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;日期<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;Time<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;時間<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;Open<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;開盤價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;High<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;最高價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Low<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;最低價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Close<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;收盤價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Volume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;成交量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UpTick<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;分K上漲Tick數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UpVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;分K上漲交易量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;DownTick<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;分K下跌Tick數<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;DownVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;分K下跌交易量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;UnchVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;未變動交易量<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</CENTER>

<br>

**Tick 资料栏位**

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-cly1"><br>&nbsp;&nbsp;Date<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</th>
    <th class="tg-cly1"><br>&nbsp;&nbsp;日期<br>&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;FilledTime<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;成交時間<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-cly1"><br>&nbsp;&nbsp;Bid<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-cly1"><br>&nbsp;&nbsp;買價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Ask<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;賣價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradingPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;成交價<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradeQuantity<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;成交單量<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-0lax"><br>&nbsp;&nbsp;TradeVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;總量<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>