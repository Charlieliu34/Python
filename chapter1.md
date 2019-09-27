# 壹、交易
## 一、Request-Reply
&emsp;&emsp;**cleint 发送 Request，等待 Server 回复 Reply。client 透过此联机登入、查询帐务。**

### 1.登入

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
  "SubPort":"n"
}
```

注 1: SessionKey，之后所有 Request 指令一律需要带 SessionKey。SessionKey 错误，指令不执行。

注 2: SubPort 提供 client 用来建立第二条 ZeroMQ Pub/Sub 联机，用来接收主推回报。

---
- 登入验证失败
```
{
	"Reply":"LOGIN",
  "Success":"Fail",
	"SessionKey":""
"ErrMsg":""
}
```

<br>

c.登出指令     
```
{
 "Request":"LOGOUT",
 "SessionKey":"XXXXXXXXX"
}
```

<br>

### 2.资金账号
a.资金账号查询
```
{
 "Request":"ACCOUNTS",
 "SessionKey":"XXXXXXXXX"
}
```
<br>

b.资金账号回复
```
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
註1:資金帳號欄位於後面章節[資金帳號主推](chapter2.md)說明。

<br>

### 3.全部合约查询

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
   "CHT" : "期貨", 
   "ENG" : "ALGOSTARS_EXG", 
   "EXGID" : "", 
   "Node" : 
   [
      {"CHS" : "中国金融交易所", 
       "CHT" : "中國金融交易所", 
       "ENG" : "CFFEX", 
       "EXGID" : null, 
       "Node" : 
         [
             {"CHS" : "热门月", 
              "CHT" : "熱門月", 
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

### 4.商品交易设定查询

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
"TC.F.CFFEX.IF":{"Currency":"CNY","Denominator":"1","EXG":"CFFEX","EXG.CTP":"CFFEX","EXG.SIM":"CFFEX","Group.CHS":"指数","Group.CHT":"指數","Group.ENG":"Equities","I3_TickSize":"0.2","Multiplier.ESNH":"1","Multiplier.ESXYA":"1","Multiplier.GQ2":"1","Name.CHS":"沪深300","Name.CHT":"滬深300","Name.ENG":"CSI 300","pinyin":"hs300","OpenCloseTime":"09:30~11:30;13:00~15:00","Symbol":"IF","Symbol.CTP":"IF"...}
  }
}
```

<br>

### 5.回报回补
a.回补回报
```
{
  "Request":"RESTOREREPORT",
  "SessionKey":"XXXXXXXXX",
	"QryIndex":""
}
```

<br>

b.回补回报回复
```
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
註1:回報欄位於後面章節[回報主推](chapter1.md#二、Publish-Subscribe)說明。

<br>

### 6.期货/期权下单
a.下单指令
```
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

b.下单回复
```
{
	"Reply":"NEWORDER",
  "Success":"OK"
}
```
注1: 代码一律采用TC代码。

注2: TimeInForce、OrderType、PositionEffect 列出所有可下单形式，请确认上手可支持下单形式，下单时代上正确的type。值接送上手Synthetic="0"，若要TCore洗价Synthetic="1"。

<br>

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BrokerID<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;券商<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;帳號<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;商品ID (包含復式商品ID) <br>&nbsp;&nbsp;TC.F2.TWF.FITX.201604^FITX.201606<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Price<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;委託價格<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;ex. FILLED-15<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;StopPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;停損價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Side<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;買賣別 (複式單為整體買賣方向) 1:Buy 2:Sell<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TimeInForce<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : ROD  Day order <br>&nbsp;&nbsp;2 : IOC | FAK  Immediate or<br>&nbsp;&nbsp;Cancel | Fill and Kill<br>&nbsp;&nbsp;3 : FOK   Fill or Kill <br>&nbsp;&nbsp;4 : GTC Good-Till-Cancel<br>&nbsp;&nbsp;5 : GTD Good-Till-Date<br>&nbsp;&nbsp;6 : OPG  盤前預約單<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;OrderType<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None Unknown <br>&nbsp;&nbsp;1 : Market order (MARKET)<br>&nbsp;&nbsp;2 : Limit order (LIMIT)<br>&nbsp;&nbsp;3 : Stop order (STOP)<br>&nbsp;&nbsp;4 : Stop limit order (STPLMT)<br>&nbsp;&nbsp;5 : Trailing Stop (TSTOP)<br>&nbsp;&nbsp;6 : Trailing StopLimit (TSTPLMT)<br>&nbsp;&nbsp;7 : Market if Touched Order (MIT)<br>&nbsp;&nbsp;8 : Limit if Touched Order (LIT)<br>&nbsp;&nbsp;9 : Trailing Limit (TLMT)<br>&nbsp;&nbsp;10 : 對方價(HIT)<br>&nbsp;&nbsp;11 : 本方價(JOIN)<br>&nbsp;&nbsp;12 : DOM-Triggered Stop ( DTS )<br>&nbsp;&nbsp;13 : DOM-Triggered Stop Limit ( DTSL )<br>&nbsp;&nbsp;14 : Breakeven (BE )<br>&nbsp;&nbsp;15: 中間價(MID)<br>&nbsp;&nbsp; <br>&nbsp;&nbsp; <br>&nbsp;&nbsp;20 : 最優價 (BST)<br>&nbsp;&nbsp;21 : 最優價轉限價 (BSTL)<br>&nbsp;&nbsp;22 : 五檔市價 (5LvlMKT)<br>&nbsp;&nbsp;23 : 五檔市價轉限價 (5LvlMTL)<br>&nbsp;&nbsp;24 : 市價轉限價 (MTL)<br>&nbsp;&nbsp;25: 一定範圍市價(MWP)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;ContingentSymbol<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;追蹤Symbol <br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TrailingField<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TrailingType<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 依價格<br>&nbsp;&nbsp;2 : 依TrailingPrice百分比<br>&nbsp;&nbsp;3<br>&nbsp;&nbsp;: 依TickSize幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TrailingAmount<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;TrailingType=1時, TrailingAmount為百分比<br>&nbsp;&nbsp;TrailingType=2時, TrailingAmount為點數<br>&nbsp;&nbsp;TrailingType=3時, TrailingAmount為幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TouchPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;觸發價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TouchField<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=3 或 OrderType=4 或 OrderType=7 或 OrderType=8或OrderType=12 或 OrderType=13使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;TouchCondition<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;0 : touch or 穿價<br>&nbsp;&nbsp;1 : Greater<br>&nbsp;&nbsp;2 : GreaterEqual<br>&nbsp;&nbsp;3 : Equl<br>&nbsp;&nbsp;4 : LessEqual<br>&nbsp;&nbsp;5 : Less<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;OrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;下單口數<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;PositionEffect<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : Open  Open position <br>&nbsp;&nbsp;1 : Close  Close position <br>&nbsp;&nbsp;2 : 平今<br>&nbsp;&nbsp;3 : 平昨<br>&nbsp;&nbsp;4 : Auto   Auto select<br>&nbsp;&nbsp;Open/Cloe position  <br>&nbsp;&nbsp;5 : 特殊自動單 有昨倉先平昨<br>&nbsp;&nbsp;其它一律轉開倉<br>&nbsp;&nbsp;10 : 備兌開倉<br>&nbsp;&nbsp;11: 備兌平倉<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;DayTrade<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 當沖<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Synthetic<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Synthetic<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;GroupType<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Normal<br>&nbsp;&nbsp;2 : OCO<br>&nbsp;&nbsp;3 : OTO<br>&nbsp;&nbsp;4 : OTOCO<br>&nbsp;&nbsp;5 : OTOs(單線)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;GroupID<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;GroupID or OCOID or OTOID or OTOCOID or  OTOSID<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;ChasePrice<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;追價 <br>&nbsp;&nbsp;格式為[每次增加價格|追價幾次|每次追價幾秒|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;限價追價 <br>&nbsp;&nbsp;格式為[0|追價限制幾跳|-11 (Chase<br>&nbsp;&nbsp;Trailing) or -12 (Chase if touch)|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;或是[1|追價幾次|-11 (Chase Trailing)<br>&nbsp;&nbsp;or -12 (Chase if touch)|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;a.若 每次增加價格 &lt; 1000 表示幾跳Ticks<br>&nbsp;&nbsp;b.追價幾秒=-11 (Chase<br>&nbsp;&nbsp;Trailing)<br>&nbsp;&nbsp;c.追價幾秒=-12 (Chase if<br>&nbsp;&nbsp;touch)<br>&nbsp;&nbsp;d.Price Type=(LTP, BID, ASK)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedType<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;不支援群組單<br>&nbsp;&nbsp;0 : None                  5 :<br>&nbsp;&nbsp;AllSliced After PriceTrigger<br>&nbsp;&nbsp;1 : Iceberg<br>&nbsp;&nbsp;2 : TimeSliced<br>&nbsp;&nbsp;3 : VolumeSliced<br>&nbsp;&nbsp;4 : AllSliced<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;DiscloseQty<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =1 或 SlicedType =2 或SlicedType =3 或SlicedType =4或SlicedType =5使用<br>&nbsp;&nbsp;逐筆揭露數量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Variance<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =1 或 SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;逐筆變動比例%<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Interval<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;SlicedType =2 : 間隔時間，ms<br>&nbsp;&nbsp;SlicedType =3 : 間隔成交量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;LeftoverAction<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;0 : Leave 繼續掛著(預設)<br>&nbsp;&nbsp;1 : Merge 取消該筆，剩餘數量加入下筆下單<br>&nbsp;&nbsp;2 : Market 轉市價單<br>&nbsp;&nbsp;3 : Payup 追價(未實做)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedPriceField<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 -&gt; Fixed price mode<br>&nbsp;&nbsp;非0 -&gt; Relative<br>&nbsp;&nbsp;price mode<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;0 : None <br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;4 : 對方價<br>&nbsp;&nbsp;5 : 本方價<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedTicks<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedPriceField<br>&nbsp;&nbsp;!= 0<br>&nbsp;&nbsp;TickSize幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Breakeven<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=14使用<br>&nbsp;&nbsp;Breakeven觸發價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BreakevenOffset<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=14使用<br>&nbsp;&nbsp;Breakeven停損價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Threshold<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=12或OrderType=13使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;ExtCommands<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;延伸下單參數 多個參數請用；隔開<br>&nbsp;&nbsp;1.自動單反向延遲 請帶<br>&nbsp;&nbsp;  DelayTransPosition=3000<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;表延遲3秒<br>&nbsp;&nbsp;2.下單前 將平倉掛單都刪單<br>&nbsp;&nbsp; CancelCloseWorking=1 or 2<br>&nbsp;&nbsp;3.下單速度是否調慢與設定相同<br>&nbsp;&nbsp; FitOrderFreq=1<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Consecutive <br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;1 : 連續送單 2:TW連續IOC<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;NumberOfRetries<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Consecutive=1使用<br>&nbsp;&nbsp;持續送單幾次,<br>&nbsp;&nbsp;NumberOfRetries&gt;=9999 表示無線次數<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;RetryInterval<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Consecutive=1使用<br>&nbsp;&nbsp;每次送單間隔ms,<br>&nbsp;&nbsp;RetryInterval=0表是立即送單<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;Strategy<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;策略名稱<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"><br>&nbsp;&nbsp;UserKey1<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;將可在回報的UserKey1欄位得到相同的字串<br>&nbsp;&nbsp;</td>
    
</table>

<br>

### 7.删改单

a.改单指令
```
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

b.改单回复
```
{
	"Reply":"REPLACEORDER",
  "Success":"OK",
  "ReportID":"123456"
}
```

<br>

c.删单指令
```
{
  "Request":"CANCELORDER",
  "Param":
  {
    "ReportID":"123456"
  }	
}
```

<br>

d.删单回复
```
{
	"Reply":"CANCELORDER",
  "Success":"OK",
  "ReportID":"123456"
}
```

<br>

### 8.期货/期权 资金查询

a.资金查询指令
```
{
  "Request":"MARGINS",
  "SessionKey":"XXXXXXXXX"
"AccountMask":"XXXXXXXXX"
}
```

<br>

b.资金查询回复
```
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

### 9.期货/期权 部位查询

a.部位查询指令
```
{
  "Request":"POSITIONS",
  "SessionKey":"XXXXXXXXX",
	"AccountMask":"XXXXXXXXX",
	"QryIndex":"n"
}
```
<br>

b.资金查询回复
```
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

### 10.PONG

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
&emsp;&emsp;**cleint 透过 pub/sub 联机，接收资金账号变动及主推回报**

### 1.资金账号主推
```
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

### 2.回报主推
```
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

<br>

### 4.PING
```
{
	"DataType":"PING"
}
```