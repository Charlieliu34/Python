# 壹、交易
## 一、Request-Reply
**cleint发送Request，等待Server回复Reply。client透过此联机登入、查询帐务。**

<br>

### 1.登入              

#### a.登入指令

```python
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

```python
{
	"Reply":"LOGIN",
  "Success":"OK",
	"SessionKey":"XXXXXXXXX",
  "SubPort":"n"
}
```

  注 1: SessionKey，之后所有 Request 指令一律需要带 SessionKey。SessionKey 错误，指令不执行。

  注 2: SubPort 提供 client 用来建立第二条 ZeroMQ Pub/Sub 联机，用来接收主推回报。

<br>

- 登入验证失败

```python
{
	"Reply":"LOGIN",
  "Success":"Fail",
	"SessionKey":""
"ErrMsg":""
}
```

<br>

#### c.注销指令     

```python
{
 "Request":"LOGOUT",
 "SessionKey":"XXXXXXXXX"
}
```

<br>

### 2.资金账号    

#### a.资金账号查询

```python
{
 "Request":"ACCOUNTS",
 "SessionKey":"XXXXXXXXX"
}
```
<br>

#### b.资金账号回复

```python
{
	"DataType":"ACCOUNTS",
  "Success":"OK",

  "Accounts":
  [
    {

      "BrokerID":"123456",
      "Account":"XXX",
      "UserName":"XXX",
             "AccountName":"XXX",
       ...
    },
    {

      "BrokerID":"123456",
      "Account":"XXX",
      "UserName":"XXX",
             "AccountName":"XXX",
       ...
    }
  ]
}
 ```

  注1: 资金账号栏位位于后面章节 [资金账号主推](#1资金账号主推) 说明。

<br>

### 3.全部合约查询      

#### a.全部合约查询

```python
{
  "Request":"QUERYALLINSTRUMENT",
  "SessionKey":"XXXXXXXXX"
  "Type":"Future"
}
```
- **Type:Future or Options or Stock**

<br>

#### b.全部合约回复

```python
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
               "IH1906", s
               "IC1906",…]
             …}
         …]
      …}
   …]
  …}
}
```

<br>

### 4.商品交易设定查询  

#### a.商品交易设定查询

```python
{
  "Request":"QUERYINSTRUMENTINFO",
  "SessionKey":"XXXXXXXXX",
  "Symbol":"TC.F.CFFEX.IF.201903"
}
```
<br>

####   b.商品交易设定回复

```python
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

### 5.回报回补      

#### a.回补回报

```python
{
  "Request":"RESTOREREPORT",
  "SessionKey":"XXXXXXXXX",
	"QryIndex":""
}
```

<br>

#### b.回补回报回复

```python
{
	"Reply":"RESTOREREPORT",
  "Success":"OK",
  "Orders":
    [
      {"ReportID":"123456",
       "Account":"XXX",
       "BrokerID":"XXX",
               "Symbol":"XXX",
               "Side":"n",
		"QryIndex":"n",
       …},
      {"ReportID":"123456",
       "Account":"XXX",
       "BrokerID":"XXX",
               "Symbol":"XXX",
               "Side":"n",
		"QryIndex":"n",
       …}
   ]
}
```
  注1: 回报栏位位于后面章节[回报主推](#2回报主推)说明。

<br>   

### 6.期货/期权下单   
  #### a.下单指令

```python
{
  "Request":"NEWORDER",
  "SessionKey":"XXXXXXXXX",
  "Param":
  {    
    "BrokerID":"xxxxx",
    "Account":"xxxxx",
    "Symbol":"TC.F.xxx.xxx.201710",
    "Price":"xxxxx",
    "StopPrie":"xxxxx",
    "Side":"n",
    ...
  }
}
```

<br>

#### b.下单回复

```python
{
	"Reply":"NEWORDER",
  "Success":"OK"
}
```
  注1: 代码一律采用TC代码。

  注2: TimeInForce、OrderType、PositionEffect 列出所有可下单形式，请确认上手可支持下单形式，下单时代上正确的type。值接送上手Synthetic="0"，若要TCore洗价Synthetic="1"。

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
    <td class="tg-c3ow"><br>&nbsp;&nbsp;BrokerID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 券商<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 账号<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 商品ID (包含复式商品ID) <br>&nbsp;&nbsp;TC.F2.TWF.FITX.201604^FITX.201606<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Price<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 委托价格<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;字段 + or - 价格<br>&nbsp;&nbsp;字段为固定字, 可为LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED为OTO, OTOCO使用, 其他单不可使用FILLED<br>&nbsp;&nbsp;ex. FILLED-15<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;StopPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 停损价<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;字段 + or - 价格<br>&nbsp;&nbsp;字段为固定字, 可为LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED为OTO, OTOCO使用, 其他单不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Side<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 买卖别 (复式单为整体买卖方向) 1:Buy 2:Sell<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TimeInForce<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : ROD  Day order <br>&nbsp;&nbsp;2 : IOC | FAK  Immediate or<br>&nbsp;&nbsp;Cancel | Fill and Kill<br>&nbsp;&nbsp;3 : FOK   Fill or Kill <br>&nbsp;&nbsp;4 : GTC Good-Till-Cancel<br>&nbsp;&nbsp;5 : GTD Good-Till-Date<br>&nbsp;&nbsp;6 : OPG  盘前预约单<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;0 : None Unknown <br>&nbsp;&nbsp;1 : Market order (MARKET)<br>&nbsp;&nbsp;2 : Limit order (LIMIT)<br>&nbsp;&nbsp;3 : Stop order (STOP)<br>&nbsp;&nbsp;4 : Stop limit order (STPLMT)<br>&nbsp;&nbsp;5 : Trailing Stop (TSTOP)<br>&nbsp;&nbsp;6 : Trailing StopLimit (TSTPLMT)<br>&nbsp;&nbsp;7 : Market if Touched Order (MIT)<br>&nbsp;&nbsp;8 : Limit if Touched Order (LIT)<br>&nbsp;&nbsp;9 : Trailing Limit (TLMT)<br>&nbsp;&nbsp;10 : 对方价(HIT)<br>&nbsp;&nbsp;11 : 本方价(JOIN)<br>&nbsp;&nbsp;12 : DOM-Triggered Stop ( DTS )<br>&nbsp;&nbsp;13 : DOM-Triggered Stop Limit ( DTSL )<br>&nbsp;&nbsp;14 : Breakeven (BE )<br>&nbsp;&nbsp;15: 中间价(MID)<br>&nbsp;&nbsp; <br>&nbsp;&nbsp; <br>&nbsp;&nbsp;20 : 最优价 (BST)<br>&nbsp;&nbsp;21 : 最优价转限价 (BSTL)<br>&nbsp;&nbsp;22 : 五档市价 (5LvlMKT)<br>&nbsp;&nbsp;23 : 五档市价转限价 (5LvlMTL)<br>&nbsp;&nbsp;24 : 市价转限价 (MTL)<br>&nbsp;&nbsp;25: 一定范围市价(MWP)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ContingentSymbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;追踪Symbol = 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 依价格<br>&nbsp;&nbsp;2 : 依TrailingPrice百分比<br>&nbsp;&nbsp;3: 依TickSize几跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingAmount<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;TrailingType=1时, TrailingAmount为百分比<br>&nbsp;&nbsp;TrailingType=2时, TrailingAmount为点数<br>&nbsp;&nbsp;TrailingType=3时, TrailingAmount为几跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;触发价<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;字段 + or - 价格<br>&nbsp;&nbsp;字段为固定字, 可为LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED为OTO, OTOCO使用, 其他单不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=3 或 OrderType=4 或 <br>&nbsp;&nbsp;OrderType=7 或 OrderType=8 或<br>&nbsp;&nbsp;OrderType=12 或 OrderType=13使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchCondition<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;仅Synthetic = 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;0 : touch or 穿价<br>&nbsp;&nbsp;1 : Greater<br>&nbsp;&nbsp;2 : GreaterEqual<br>&nbsp;&nbsp;3 : Equl<br>&nbsp;&nbsp;4 : LessEqual<br>&nbsp;&nbsp;5 : Less<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 下单口数<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;PositionEffect<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : <strong> Open</strong>   Open position <br>&nbsp;&nbsp;1 : <strong>Close</strong>  Close position <br>&nbsp;&nbsp;2 : 平今<br>&nbsp;&nbsp;3 : 平昨<br>&nbsp;&nbsp;4 : <strong> Auto</strong>   Auto select Open/Cloe position  <br>&nbsp;&nbsp;5 : 特殊自动单 有昨仓先平昨 其它一律转开仓<br>&nbsp;&nbsp;10 : 备兑开仓<br>&nbsp;&nbsp;11: 备兑平仓<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;DayTrade<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 当冲<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Synthetic<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Synthetic<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;GroupType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Normal<br>&nbsp;&nbsp;2 : OCO<br>&nbsp;&nbsp;3 : OTO<br>&nbsp;&nbsp;4 : OTOCO<br>&nbsp;&nbsp;5 : OTOs(单线)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;GroupID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>  GroupID or OCOID or OTOID or OTOCOID or  OTOSID<br></td>

  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ChasePrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><strong>追价</strong>  <br>&nbsp;&nbsp;格式为[每次增加价格|追价几次|每次追价几秒|最后下市价或删单或挂单(M or C or L)|PriceType]<br>&nbsp;&nbsp; <br><strong>限价追价</strong> <br>&nbsp;&nbsp;格式为[0|追价限制几跳|-11 (Chase<br>&nbsp;&nbsp;Trailing) or -12 (Chase if touch)|最后下市价或删单或挂单(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>
  或是[1|追价几次|-11 (Chase Trailing)<br>&nbsp;&nbsp;or -12 (Chase if touch)|最后下市价或删单或挂单(M or C or L)|PriceType]<br>&nbsp;&nbsp;
  
   &nbsp;&nbsp;<strong>a.若 每次增加价格 &lt; 1000 表示几跳Ticks<br>&nbsp;&nbsp; b.追价几秒=-11 (Chase Trailing)<br>&nbsp;&nbsp; c.追价几秒=-12 (Chase if touch) <br>&nbsp;&nbsp; d.Price Type=(LTP, BID, ASK) <br>&nbsp;&nbsp;</strong></td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 不支援群组单<br>&nbsp;&nbsp;0 : None <br>&nbsp;&nbsp;1 : Iceberg<br>&nbsp;&nbsp;2 : TimeSliced<br>&nbsp;&nbsp;3 : VolumeSliced<br>&nbsp;&nbsp;4 : AllSliced<br>&nbsp;&nbsp;5 :&nbsp;&nbsp;AllSliced After PriceTrigger</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;DiscloseQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 仅SlicedType =1 或 SlicedType =2 或SlicedType =3 或 SlicedType =4 或 SlicedType =5 使用逐笔揭露数量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Variance<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 仅SlicedType =1 或 SlicedType =2或 SlicedType =3使用逐笔变动比例%<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Interval<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedType =2 : 间隔时间<br>&nbsp;&nbsp;SlicedType =3 : 间隔成交量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;LeftoverAction<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 仅SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;0 : Leave 继续挂着(预设)<br>&nbsp;&nbsp;1 : Merge 取消该笔，剩余数量加入下笔下单<br>&nbsp;&nbsp;2 : Market 转市价单<br>&nbsp;&nbsp;3 : Payup 追价(未实做)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedPriceField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 -&gt; Fixed price mode<br>&nbsp;&nbsp;非 0 -&gt; Relative price mode<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;0 : None <br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;4 : 对方价<br>&nbsp;&nbsp;5 : 本方价<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedTicks<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedPriceField= 0<br>&nbsp;&nbsp;TickSize几跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Breakeven<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅OrderType=14使用<br>&nbsp;&nbsp;Breakeven触发价<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;字段 + or - 价格<br>&nbsp;&nbsp;字段为固定字, 可为LAST or BID or ASK or FILLED <br>&nbsp;&nbsp;FILLED为OTO, OTOCO使用, 其他单不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;BreakevenOffset<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅OrderType=14使用<br>&nbsp;&nbsp;Breakeven停损价<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;字段 + or - 价格<br>&nbsp;&nbsp;字段为固定字, 可为LAST or BID orASK or FILLED<br>&nbsp;&nbsp;FILLED为OTO, OTOCO使用, 其他单不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Threshold<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅OrderType=12或OrderType=13使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ExtCommands<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 延伸下单参数 多个参数请用<strong>；</strong>隔开<br>&nbsp;&nbsp;1.自动单反向延迟 请带<br>&nbsp;&nbsp;  DelayTransPosition=3000, 表延迟3秒<br>&nbsp;&nbsp;2.下单前 将平仓挂单都删单<br>&nbsp;&nbsp; CancelCloseWorking=1 or 2<br>&nbsp;&nbsp;3.下单速度是否调慢与设定相同<br>&nbsp;&nbsp; FitOrderFreq=1<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Consecutive <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 1 : 连续送单 &nbsp;&nbsp; 2:TW连续IOC<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;NumberOfRetries<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;仅Consecutive=1使用, 持续送单几次,<br>&nbsp;&nbsp;NumberOfRetries&gt;=9999 表示无线次数<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;RetryInterval<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Strategy 仅Consecutive=1使用, 每次送单间隔<br>&nbsp;&nbsp;RetryInterval=0表示立即送单<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Strategy<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;策略名称<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;UserKey1<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> &nbsp;&nbsp;将可在回报的UserKey1字段得到相同的字符串<br>&nbsp;&nbsp;</td>
    
</table>
</center>

<br>

<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-wa1i{font-weight:bold;text-align:center;vertical-align:middle}
.tg .tg-nrix{text-align:center;vertical-align:middle}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 426px">
<colgroup>
<col style="width: 68px">
<col style="width: 358px">
</colgroup>
  <tr>
    <td class="tg-wa1i" colspan="2"><br>&nbsp;&nbsp;下单错误码<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-10<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>Unknow Error<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-11<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>买卖别不对<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-12<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>复式单商品代码解晰错误<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-13<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>下单账号, 不可下此交易所商品<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-14<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>下单错误, 不支持的价格 或 OrderType 或 TimeInForce<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-15<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>不支援证券下单<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-20<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>联机未建立<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-22<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>价格的TickSize错误<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-23<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>下单数量超过该商品的上下限<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-24<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>下单数量错误<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-25<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>价格不能小于和等于0 (市价类型不会去检查)<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

<br>




### 7.删改单     

#### a.改单指令

```python
{
  "Request":"REPLACEORDER",
  "SessionKey":"XXXXXXXXX",
  "Param":
  {
    "ReportID":"123456",
    "ReplaceExecType":"n",
    "OrderQty":"n",
    "Price":"xxxxx",
	  "StopPrice":"xxxxx",
  }	
}
```

<br>

#### b.改单回复

```python
{
	"Reply":"REPLACEORDER",
  "Success":"OK",
  "ReportID":"123456"
}
```

<br>

#### c.删单指令

```python
{
  "Request":"CANCELORDER",
  "Param":
  {
    "ReportID":"123456"
  }	
}
```

<br>

#### d.删单回复

```python
{
	"Reply":"CANCELORDER",
  "Success":"OK",
  "ReportID":"123456"
}
```       
<center>
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}s
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg td{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:center}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:center}
</style>
<table class="tg">
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp; ReportID  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  回报编号  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReplaceExecType  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  0:改价 &nbsp;&nbsp; 1:改量 &nbsp;&nbsp; 2:改价改量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OrderQty  <br>&nbsp;&nbsp; </td>
     <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  使用者期望减少的委托量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Price  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  用户期望的委托价格  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  StopPrice  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  用户期望的停损价格</td>
  </tr>
</table>
</center>

<br>

### 8.期货/期权资金查询

#### a.资金查询指令

```python
{
  "Request":"MARGINS",
  "SessionKey":"XXXXXXXXX"
"AccountMask":"XXXXXXXXX"
}
```

<br>

#### b.资金查询回复

```python
{
  "Reply":"MARGINS",
  "Success":"OK",
  "Margins":
    [
      {"BrokerID":"XXX",
    	"Account":"XXX",
       "TransactDate":"n",
       "TransactTime":"n",
       "BeginningBalance":"XXX",
       "Commissions":"XXX",
       ...},
      {"BrokerID":"XXX",
       "Account":"XXX",
       "TransactDate":"n",
       "TransactTime":"n",
       "BeginningBalance":"XXX",
       "Commissions":"XXX",
       ...}
   ]   
}
```
  注1:并非所有字段都有数据，要视上手提供多少数据。

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
    <td class="tg-c3ow"><br>&nbsp;&nbsp;BrokerID   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  券商 (对应下单与回报字段)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerName   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  券商名称  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Account   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账号 (对应下单与回报字段)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountName   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账号名称  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;<br>&nbsp;&nbsp;  TransactDate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新日期 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TransactTime   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新时间 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BeginningBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期初结存  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Commissions   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易手续费  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenCommission   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  冻结手续费  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeClearinigFee   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所费用与结算费用  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerageFee   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  经纪商费用  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  GrossPL   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  已实现损益(未扣除费用)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OptionPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  选择权权利金收入  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CashIn   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  证券买卖当日收支  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  NetPL   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  已实现损益(扣除费用并加上选择权权利金收入)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Deposit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今日入金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Withdraw   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今日出金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CashActivity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今日出入金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExcessEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  可用资金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  WithdrawQuota   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  可取资金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  EndingBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期末结存  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OpenTradeEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未平仓损益(包含选择权和期货)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TotalEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  总权益数  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  选择权市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountValueAtMarket   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账户市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  InitialMarginRequirement   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  初始保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MaintenanceMarginRequirement   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  维持保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  当前保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarginDeficit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  追缴保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  冻结保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenCash   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  冻结资金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReserveBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  保底准备金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Credit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  信用额度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Mortgage   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  质押金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreMortgage   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次质押金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreCredit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次信用额度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreDeposit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次存款额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次占用的保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  DeliveryMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  投资者交割保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeDeliveryMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所交割保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyToSystem   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  系统币别  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyConversionRate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  币别转换比率(转换到账户币别使用)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyToClient   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账户币别  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ConvertedAccountValueAtMkt   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  币别转换后账户市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExerciseIncome   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  行权盈亏 到期履约损益　  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  IncomeBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  盈亏金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  InterestBase   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  利息基数  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Interest   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  利息收入  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarginLevel   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  保证金水平 风险指标  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  UPLForOptions   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未冲销选择权浮动损益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongOptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未冲销买方选择权市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortOptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未冲销卖方选择权市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenpPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  委托权利金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarginExcess   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  加收原始保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AdjustedEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  契约调整权益数 加/减项  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreFundMortgageIn   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次货币质入金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreFundMortgageOut   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次货币质出金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FundMortgageIn   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  货币质入金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FundMortgageOut   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  货币质出金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FundMortgageAvailable   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  货币质押余额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MortgageableFund   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  可质押货币金额  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品占用保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductFrozenMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品冻结保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductCommission   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品手续费  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductFrozenCommission   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品冻结手续费  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductPositionProfit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品持仓盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductCloseProfit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品平仓盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductPositionProfitByAlg   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  根据持仓盈亏算法计算的特殊产品持仓盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SpecProductExchangeMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  特殊产品交易所保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FloatProfitByDate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐日浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FloatProfitByTrade   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐笔浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FutureProfitByDay   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期货当日盈亏   </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReferenceRiskRate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  参考风险度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TryExcessEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  试算可用资金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  DynamicEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  动态权益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarketPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  市值权益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Op tionPremiumCoin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未冲销权利金市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  StockReferenceMarket   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  股票参考市值</td>
  </tr>
</table>
  </center>

<br>

### 9.期货/期权部位查询

#### a.部位查询指令

```python
{
  "Request":"POSITIONS",
  "SessionKey":"XXXXXXXXX",
	"AccountMask":"XXXXXXXXX",
	"QryIndex":"n"
}
```
<br>

#### b.资金查询回复

```python
{
  "Reply":"POSITIONS",
  "Success":"OK",
  "Positions":
    [
      {"BrokerID":"XXX",
    	"Account":"XXX",
        "TransactDate":"n",
                "TransactTime":"n",
                "Quantity":"n",
                "SumLongQty":"n",
                "SumShortQty":"n",
		"QryIndex":"n",
       ...},
      {"BrokerID":"XXX",
       "Account":"XXX",
       "TransactDate":"n",
       "TransactTime":"n",
                "Quantity":"n",
                "SumLongQty":"n",
                "SumShortQty":"n",
		"QryIndex":"n",
       ...}
   ]   
}
```

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
    <td class="tg-c3ow"><br>&nbsp;&nbsp; BrokerID <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  券商  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerName <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  券商名称  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Account <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账号  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountName <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  账号名称  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TransactDate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新日期 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TransactTime <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新时间 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Symbol <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  商品TCore代码  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Side <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  买卖方向 1.Buy &nbsp;&nbsp; 2.Sell  (复式单为整体买卖方向)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Quantity <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  净仓数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  成本均价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OpenPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  开仓价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyToSystem <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  系统币别  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Covered <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 备兑仓数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SumLongQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 持仓数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  SumShortQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 持仓数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TodayLongQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 今日持仓  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TodayShortQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 今日持仓  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  YdLongQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 昨日持仓  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  YdShortQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 昨日持仓  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongAvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 成本均价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortAvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 成本均价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongOpenPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 开仓均价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortOpenPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 开仓均价  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  WorkingLong <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 委托中数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  WorkingShort <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 委托中数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdBuyQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今买入  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdSellQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今卖出  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdTotalQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今成交  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongFrozen <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 冻结  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortFrozen <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 冻结  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Lock_ExecFrozen <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  锁券/执行冻结  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongAvailable <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多头 可平仓量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortAvailable <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空头 可平仓量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdBuyAvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今买成均  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdSellAvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今卖成均  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TdNetAvgPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  今净成均  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FloatProfitByDate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐日浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FloatProfitByTrade <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐笔浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongFloatProfitByDate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多逐日浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortFloatProfitByDate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空逐日浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongFloatProfitByTrade <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  多逐笔浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortFloatProfitByTrade <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  空逐笔浮动盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CloseProfitByTrade <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐笔对冲平仓盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CloseProfitByDate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐日盯市平仓盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TodayProfit <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  当日盈亏  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarketPrice <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  市值权益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OpenCost <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  开仓成本  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PositionCost <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  持仓成本  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeRate <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所汇率  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  YdOrgNetQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  当日起始 昨净仓数量(正负)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  YdOrgLongQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  当日起始 昨多仓数量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  YdOrgShortQty <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  当日起始 昨空仓数量</td>
  </tr>
</table>
</center>

<br>

### 10.PONG  

```python
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

&emsp;&emsp;**cleint 透过 pub/sub 联机，接收资金账号变动及主推回报**
### 1.资金账号主推
```python
{
	"DataType":"ACCOUNTS",
    "Accounts":
  [
    {

      "BrokerID":"123456",
      "Account":"XXX",
      "UserName":"XXX",
             "AccountName":"XXX",
       ...
    },
    {

      "BrokerID":"123456",
      "Account":"XXX",
      "UserName":"XXX",
             "AccountName":"XXX",
       ...
    }
  ]
}
```

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
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 券商 (对应下单与回报字段)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Account<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 账号 (对应下单与回报字段)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  UserName<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 登入账号名称<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountName<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 账号名称<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountMask<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> BrokerID-Account<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerName<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 券商名称<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Status<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 账号状态<br>(0:尚未登入 &nbsp;&nbsp;1:登入中&nbsp;&nbsp;   2:登入完成)<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> S/F/O<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OrderExchange<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 可下单交易所<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Level<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> ETF期权, 使用者级别<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountReleated<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 证券/期货账号 连结ID<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Region<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 1:CN &nbsp;&nbsp;2:TW &nbsp;&nbsp;4:OB<br>&nbsp;&nbsp;</td>
  </tr>
</table>
</center>

<br>

### 2.回报主推

```python
{
	"DataType":"EXECUTIONREPORT",
  "Report":
  {
    "ReportID":"123456",
    "Account":"XXX",
    "BrokerID":"XXX",
         "Symbol":"XXX",
         "Side":"n",
     ...
  }
}
```

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
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ReportID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">TCore回报编号 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">账号 (对应下单与回报字段) </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;BrokerID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">券商 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">商品TCore代码 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Side<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">买卖别 1:Buy 2:Sell <br>&nbsp;&nbsp;(复式单为整体买卖方向) </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">委托书号 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OriginalQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">new order时候的委托数量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">实际委托数量=成交数量+剩余有效委托口数 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Price<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">委托价格 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;StopPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">停损价 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;0 : None Unknown <br>&nbsp;&nbsp;1 : Market order <br>&nbsp;&nbsp;2 : Limit order <br>&nbsp;&nbsp;3 : Stop order <br>&nbsp;&nbsp;4 : Stop limit order <br>&nbsp;&nbsp;5 : Trailing Stop<br>&nbsp;&nbsp;6 : Trailing StopLimit <br>&nbsp;&nbsp;7 : Market if Touched Order<br>&nbsp;&nbsp;8 : Limit if Touched Order<br>&nbsp;&nbsp;9 : Trailing Limit (TLMT)<br>&nbsp;&nbsp;10 : 对方价(HIT)<br>&nbsp;&nbsp;11 : 本方价(JOIN)<br>&nbsp;&nbsp;12 : DOM-Triggered Stop ( DTS )<br>&nbsp;&nbsp;13 : DOM-Triggered Stop Limit ( DTSL )<br>&nbsp;&nbsp; <br>&nbsp;&nbsp; <br>&nbsp;&nbsp;20 : 最优价 (BST)<br>&nbsp;&nbsp;21 : 最优价转限价 (BSTL)<br>&nbsp;&nbsp;22 : 五档市价 (5LvlMKT)<br>&nbsp;&nbsp;23 : 五档市价转限价 (5LvlMTL)<br>&nbsp;&nbsp;24 : 市价转限价 (MTL)<br>&nbsp;&nbsp;25: 一定范围市价(MWP)<br>&nbsp;&nbsp;201:锁券<br>&nbsp;&nbsp;202:解锁 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TradeType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">证券<br>&nbsp;&nbsp;0:Normal<br>&nbsp;&nbsp;1:融资<br>&nbsp;&nbsp;2.融券 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ExCode<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">证券<br>&nbsp;&nbsp;0:现股<br>&nbsp;&nbsp;1:零股<br>&nbsp;&nbsp;2:盘后 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;PositionEffect<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;0 : <strong>Open</strong>  Open position <br>&nbsp;&nbsp;1 : <strong> Close</strong>  Close position <br>&nbsp;&nbsp;2 : 平今<br>&nbsp;&nbsp;3 : 平昨<br>&nbsp;&nbsp;4 :  <strong>Auto </strong>   Auto select<br>&nbsp;&nbsp;Open/Cloe position  <br>&nbsp;&nbsp; <br>&nbsp;&nbsp;10: 备兑开仓<br>&nbsp;&nbsp;11: 备兑平仓<br>&nbsp;&nbsp;201:锁券<br>&nbsp;&nbsp;202:解锁 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;DayTrade<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 当冲 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TimeInForce<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : ROD  Day order <br>&nbsp;&nbsp;2 : IOC   Immediate or Cancel <br>&nbsp;&nbsp;3 : FOK   Fill or Kill <br>&nbsp;&nbsp;4 : GTC Good-Till-Cancel<br>&nbsp;&nbsp;5 : GTD Good-Till-Date<br>&nbsp;&nbsp;6 : OPG  盘前预约单 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingAmount<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">TrailingAmount(Last, Bid, Ask) </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchCondition<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">触发条件<br>&nbsp;&nbsp;ex. Last &gt;= TouchPrice </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Group<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">格式为Type :<br>&nbsp;&nbsp;GroupID<br>&nbsp;&nbsp;Type = Normal, OCO, OTO, OTOCO </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;UserKey1<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">对应委托参数的UserKey1字段数据 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;CumQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">已成交数量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;LeavesQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">委托剩余有效量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ExecType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br> 委托执行状态<br>&nbsp;&nbsp;-10: 请UI隐藏该笔回报 不要显示<br>&nbsp;&nbsp; (原预埋单回报 与 送至交易所主推回报<br>&nbsp;&nbsp;无法合单 所以隐藏该笔原预埋单回报)<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;0:委托成功(包含改价改量)<br>&nbsp;&nbsp;1:部份委托成功其余处理中<br>&nbsp;&nbsp;2:部份委托成功其余错误<br>&nbsp;&nbsp;3:全部成交<br>&nbsp;&nbsp;4:部份成交其余委托处理中<br>&nbsp;&nbsp;5:部份成交其余删单<br>&nbsp;&nbsp;6:部分成交尚有有效单<br>&nbsp;&nbsp;7.部份成交其余错误<br>&nbsp;&nbsp;8:完全删单成功<br>&nbsp;&nbsp;9.部份删单成功<br>&nbsp;&nbsp;10:委托失败<br>&nbsp;&nbsp;11:委托处理中<br>&nbsp;&nbsp;12:删改单错误<br>&nbsp;&nbsp;13:洗价中<br>&nbsp;&nbsp;14:ITS主机已收单(增加中)<br>&nbsp;&nbsp;15:等待中(不可撤单)<br>&nbsp;&nbsp;16:洗价单, 触价送单<br>&nbsp;&nbsp;17:锁券成功<br>&nbsp;&nbsp;18:锁券失败<br>&nbsp;&nbsp;19:锁券已提交<br>&nbsp;&nbsp;20:等待中(可撤单)<br>&nbsp;&nbsp;21:下单锁定 Block<br>&nbsp;&nbsp;22:撤单传送中<br>&nbsp;&nbsp;23:预约单<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;31:洗价暂停中 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ErrorCode<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;ExecType=10 or ExecType=12 or<br>&nbsp;&nbsp;ExecType=18错误单时<br>&nbsp;&nbsp;请抓取ErrorCode<br>&nbsp;&nbsp;ErrorCode&lt;0 表示有错误<br>&nbsp;&nbsp;若ErrorCode=-777为Undefine<br>&nbsp;&nbsp;Error   错误内容 请看ExecTypeText<br>&nbsp;&nbsp;其他ErrorCode定义如后[12.回报ErrorCode定义] </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TriggeredPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">洗价单 触发价格 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TransactDate<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">更新日期 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TransactTime<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">更新时间 (UpdateTime) </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;AvgPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">成交均价 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ExecTypeText<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">委托执行状态描述[server错误讯息] </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;FilledOrdersCount<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">成交回报数量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;IsRestoreData<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">&nbsp;&nbsp;0 : 实时回报<br>&nbsp;&nbsp;1 : 回补回报 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;HedgeFlag<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">投机套保标志 <br>&nbsp;&nbsp;(1:投机 2:套利 3:套保 4:备兑) </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrgSource<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">下单来源别 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TradeDate<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">交易日 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SetPRIADJ<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">履约价调整(中国ETF期权第几次调整合约履约价) </td>
  </tr>
</table>
</center>

<br>

### 3.DaVinci讯号

```python
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
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">合约 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">账号 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ReferenceVolume<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">参考口数 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TotalScore<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">总评分 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TotalScoreChange<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">评分变化量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;FloatProfit<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">持仓实时损益 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Inc<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">损益权重分配 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;RiskTake<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">动用风险 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;NewOrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">下单数量 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TotalN<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">开放交易的策略总数上限 </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SumChannel<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>string<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">商品类别 </td>
  </tr>
</table>
</center>

<br>

### 4.PING

```python
{
	"DataType":"PING"
}
```
