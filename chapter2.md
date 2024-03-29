# 贰、行情
## 一、Request-Reply
&emsp;&emsp;**client 发送 Request，等待 Server 回复 Reply。client 透过此联机登入即发送订阅报价指令。**

### 1.账号登入

#### a.登入指令
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

#### b.登入回复
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

#### c.注销指令
```
{
  "Request":"LOGOUT",
}
```

<br>

### 2.全部合约查询
#### a.全部合约查询
```
{
  "Request":"QUERYALLINSTRUMENT",
  "SessionKey":"XXXXXXXXX"
  "Type":"Future"
}
```
 **Type:Future or Options or Stock**

 <br>

#### b.全部合约回复

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

#### a.商品交易设定查询
```
{
  "Request":"QUERYINSTRUMENTINFO",
  "SessionKey":"XXXXXXXXX",
  "Symbol":"TC.F.CFFEX.IF.201903"
}
```

<br>

#### b.商品交易设定回复
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

#### a.订阅指令
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

#### b.订阅回复
```
{
  "Reply":"SUBQUOTE",
  "Success":"OK"
}
```

<br>

#### c.解除订阅指令
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
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-nrix{text-align:center;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
   <tr>
    <td class="tg-c3ow">Symbol</td>
   <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;合约代码 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SubDataType</td>
  <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">实时行情  (REALTIME)<br> 历史ticks  (TICKS)<br> 历史1K     (1K)<br> 历史日K    (DK)<br> Greeks    (GREEKS) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">StartTime</td>
   <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;YYMMDDHH </td>
  </tr>
    <tr>
    <td class="tg-c3ow">EndTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;YYMMDDHH </td>
  </tr>
</table>
</center>

### 5.PONG
```
{
  "Request":"PONG",
  "SessionKey":"XXXXXXXXX"
}
回复
{
   "Reply":"PONG",
    "Success":"OK"
}
```

<br>

## 二、Publish-Subscribe
&emsp;&emsp;**client 透过 pub/sub 联机，接收主推行情**

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
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
    <tr>
    <td class="tg-c3ow">Symbol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">合约代码 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradeDate</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">交易日期 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">FilledTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">成交时间 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradingPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">成交价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradeQuantity</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">成交单量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Change</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">涨跌 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradeVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">累计成交量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">OpeningPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">开盘价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HighPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">当日最高价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">LowPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">当日最低价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">ClosingPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">收盘价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">ReferencePrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">参考价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">UpperLimitPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">涨停价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">LowerLimitPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">跌停价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">YClosedPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">昨收 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">YTradeVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">昨量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">OpenInterest</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">今日未平仓量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">YOpenInterest</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">昨日未平仓量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">FlagOfBuySell</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">内外盘 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">OpenTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">开盘时间 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">CloseTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">收盘时间 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#1 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid1</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#2 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid2</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#3 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid3</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#4 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid4</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#5 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid5</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#6 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid6</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#7 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid7</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#8 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid8</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#9 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid9</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价#10 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#1 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume1</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#2 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume2</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#3 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume3</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#4 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume4</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#5 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume5</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#6 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume6</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#7 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume7</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#8 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume8</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#9 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BidVolume9</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买量#10 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#1 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask1</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#2 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask2</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#3 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask3</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#4 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask4</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#5 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask5</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#6 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask6</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#7 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask7</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#8 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask8</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#9 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask9</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价#10 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#1 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume1</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#2 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume2</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#3 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume3</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#4 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume4</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#5 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume5</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#6 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume6</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#7 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume7</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#8 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume8</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#9 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">AskVolume9</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖量#10 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalBidCount</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">委托买进总笔数 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalBidVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">委托买进总数量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalAskCount</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">委托卖出总笔数 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalAskVolume</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">委托卖出总数量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BuyCount</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">累进买进成交笔数 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SellCount</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">累进卖出成交笔数 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SettlementPrice</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">结算价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">EndDate</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">到期日 </td>
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
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
   <tr>
    <td class="tg-c3ow">TradingHours</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">交易时间 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradingDay</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">交易日 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Last</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">最新价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Vol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">合约交易量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">买价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">卖价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">ImpVol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">IV </td>
  </tr>
    <tr>
    <td class="tg-c3ow">BIV</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">BIV </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SIV</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">SIV </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Delta</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Delta </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Gamma</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Gamma </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Vega</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Vega </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Theta</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Theta </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Rho</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Rho </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TheoVal</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">理论价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">ExtVal</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">时间价值 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Margin</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">保证金 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">OI</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">未平仓量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">PreOI</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">昨日未平仓量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">IntVal</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">内含价值 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">PreImpVol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">昨日IV </td>
  </tr>
    <tr>
    <td class="tg-c3ow">VIX</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">VIX值 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">UndVol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">目标交易量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">UndOI</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">目标未平仓量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HV_W4</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">HV-w4 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HV_W8</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">HV-w8 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HV_W13</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">HV-w13 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HV_W26</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">HV-w26 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">HV_W52</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">HV-w52 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">UnderlyingAsset</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">目标(using GetGreeksString) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">VIX1</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">VIX1(using GetGreeksValue) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">VIX2</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">VIX2(using GetGreeksValue) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">CallOI</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Call的未平仓量 (using GetGreeksValue) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">CallVol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Call的交易量 (using GetGreeksValue) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">PutOI</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Put的未平仓量 (using GetGreeksValue) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">PutVol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">Put的交易量 (using GetGreeksValue) </td>
  </tr>
</table>
</center>

<br>

### 3.DaVinci讯号
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
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
  <tr>
    <td class="tg-c3ow">Symbol</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">合约 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Account</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">账号 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">ReferenceVolume</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">参考口数 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalScore</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">总评分 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalScoreChange</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">评分变化量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">FloatProfit</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">持仓实时损益 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Inc</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">损益权重分配 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">RiskTake</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">动用风险 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">NewOrderQty</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">下单数量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TotalN</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">开放交易的策略总数上限 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SumChannel</td>
    <td class="tg-c3ow">string</td>
    <td class="tg-0pky">商品类别 </td>
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

#### a.回补指令
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

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
  <tr>
    <td class="tg-c3ow">Symbol</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;合约代码 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">SubDataType</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;1实时行情   (REALTIME)2历史ticks  (TICKS)4历史1K     (1K)5历史日K    (DK)6 Greeks    (GREEKS) </td>
  </tr>
    <tr>
    <td class="tg-c3ow">StartTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;YYMMDDHH</td>
   </tr>
   <tr>
    <td class="tg-c3ow">EndTime</td>
    <td class="tg-c3ow">BSTR</td>
    <td class="tg-0pky">&nbsp;&nbsp;YYMMDDHH </td>
  </tr>
   <tr>
    <td class="tg-c3ow">GreeksType<br></td>
   <td class="tg-c3ow"><br>BSTR</td>
    <td class="tg-0pky"> </td>
  </tr>
</table>
</center>

<br>

#### b.回补指令回复
```
{
  "Reply":"SUBQUOTE",
  "Success":"OK"
}
```
  于PUB/SUB联机，收到数据准备完成通知。

<br>

#### c.订阅回复
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

#### d.取得历史资指令
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

#### d.取得历史资料返回结果
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

#### e.取得历史资料返回结果
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

**IK/DK 资料栏位**

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
  <tr>
    <td class="tg-c3ow">  Date  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">日期</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  Time  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">时间</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  Open  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">开盘价</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  High  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">最高价</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  Low  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">最低价</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  Close  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">收盘价</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  Volume  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">成交量</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  UpTick  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">分K上涨Tick数</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  UpVolume  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">分K上涨交易量</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  DownTick  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">分K下跌Tick数</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  DownVolume  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">分K下跌交易量</td>
  </tr>
  <tr>
    <td class="tg-c3ow">  UnchVolume  </td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">未变动交易量</td>
  </tr>
</table>
</center>

<br>

**Tick 资料栏位**

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
   <tr>
    <td class="tg-c3ow">Date</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">日期 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">FilledTime</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">成交时间 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Bid</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">买价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">Ask</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">卖价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradingPrice</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">成交价 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradeQuantity</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">成交单量 </td>
  </tr>
    <tr>
    <td class="tg-c3ow">TradeVolume</td>
    <td class="tg-c3ow">BSTR  </td>
    <td class="tg-0pky">总量 </td>
  </tr>
</table>
</center>

