# 壹、交易
## 一、Request-Reply
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

#### c.登出指令     

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
注1:资金账号栏位位于后面章节[资金账号主推](chapter2.md)說明。

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
"TC.F.CFFEX.IF":{"Currency":"CNY","Denominator":"1","EXG":"CFFEX","EXG.CTP":"CFFEX","EXG.SIM":"CFFEX","Group.CHS":"指数","Group.CHT":"指數","Group.ENG":"Equities","I3_TickSize":"0.2","Multiplier.ESNH":"1","Multiplier.ESXYA":"1","Multiplier.GQ2":"1","Name.CHS":"沪深300","Name.CHT":"滬深300","Name.ENG":"CSI 300","pinyin":"hs300","OpenCloseTime":"09:30~11:30;13:00~15:00","Symbol":"IF","Symbol.CTP":"IF"...}
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
注1: 回报栏位位于后面章节[回报主推](chapter2.md)說明。

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
    <td class="tg-0pky"><br>&nbsp;&nbsp;券商<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Account<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;帳號<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Symbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;商品ID (包含復式商品ID) <br>&nbsp;&nbsp;TC.F2.TWF.FITX.201604^FITX.201606<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Price<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;委託價格<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;ex. FILLED-15<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;StopPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;停損價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or MID or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Side<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;買賣別 (複式單為整體買賣方向) 1:Buy 2:Sell<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TimeInForce<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : ROD  Day order <br>&nbsp;&nbsp;2 : IOC | FAK  Immediate or<br>&nbsp;&nbsp;Cancel | Fill and Kill<br>&nbsp;&nbsp;3 : FOK   Fill or Kill <br>&nbsp;&nbsp;4 : GTC Good-Till-Cancel<br>&nbsp;&nbsp;5 : GTD Good-Till-Date<br>&nbsp;&nbsp;6 : OPG  盤前預約單<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None Unknown <br>&nbsp;&nbsp;1 : Market order (MARKET)<br>&nbsp;&nbsp;2 : Limit order (LIMIT)<br>&nbsp;&nbsp;3 : Stop order (STOP)<br>&nbsp;&nbsp;4 : Stop limit order (STPLMT)<br>&nbsp;&nbsp;5 : Trailing Stop (TSTOP)<br>&nbsp;&nbsp;6 : Trailing StopLimit (TSTPLMT)<br>&nbsp;&nbsp;7 : Market if Touched Order (MIT)<br>&nbsp;&nbsp;8 : Limit if Touched Order (LIT)<br>&nbsp;&nbsp;9 : Trailing Limit (TLMT)<br>&nbsp;&nbsp;10 : 對方價(HIT)<br>&nbsp;&nbsp;11 : 本方價(JOIN)<br>&nbsp;&nbsp;12 : DOM-Triggered Stop ( DTS )<br>&nbsp;&nbsp;13 : DOM-Triggered Stop Limit ( DTSL )<br>&nbsp;&nbsp;14 : Breakeven (BE )<br>&nbsp;&nbsp;15: 中間價(MID)<br>&nbsp;&nbsp; <br>&nbsp;&nbsp; <br>&nbsp;&nbsp;20 : 最優價 (BST)<br>&nbsp;&nbsp;21 : 最優價轉限價 (BSTL)<br>&nbsp;&nbsp;22 : 五檔市價 (5LvlMKT)<br>&nbsp;&nbsp;23 : 五檔市價轉限價 (5LvlMTL)<br>&nbsp;&nbsp;24 : 市價轉限價 (MTL)<br>&nbsp;&nbsp;25: 一定範圍市價(MWP)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ContingentSymbol<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;追蹤Symbol <br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 依價格<br>&nbsp;&nbsp;2 : 依TrailingPrice百分比<br>&nbsp;&nbsp;3<br>&nbsp;&nbsp;: 依TickSize幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TrailingAmount<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=5 或 OrderType=6使用<br>&nbsp;&nbsp;TrailingType=1時, TrailingAmount為百分比<br>&nbsp;&nbsp;TrailingType=2時, TrailingAmount為點數<br>&nbsp;&nbsp;TrailingType=3時, TrailingAmount為幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchPrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;觸發價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=3 或 OrderType=4 或 OrderType=7 或 OrderType=8或OrderType=12 或 OrderType=13使用<br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;TouchCondition<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Synthetic<br>&nbsp;&nbsp;= 1<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8使用<br>&nbsp;&nbsp;0 : touch or 穿價<br>&nbsp;&nbsp;1 : Greater<br>&nbsp;&nbsp;2 : GreaterEqual<br>&nbsp;&nbsp;3 : Equl<br>&nbsp;&nbsp;4 : LessEqual<br>&nbsp;&nbsp;5 : Less<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;OrderQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;下單口數<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;PositionEffect<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : Open  Open position <br>&nbsp;&nbsp;1 : Close  Close position <br>&nbsp;&nbsp;2 : 平今<br>&nbsp;&nbsp;3 : 平昨<br>&nbsp;&nbsp;4 : Auto   Auto select<br>&nbsp;&nbsp;Open/Cloe position  <br>&nbsp;&nbsp;5 : 特殊自動單 有昨倉先平昨<br>&nbsp;&nbsp;其它一律轉開倉<br>&nbsp;&nbsp;10 : 備兌開倉<br>&nbsp;&nbsp;11: 備兌平倉<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;DayTrade<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : 當沖<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Synthetic<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Synthetic<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;GroupType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 : None<br>&nbsp;&nbsp;1 : Normal<br>&nbsp;&nbsp;2 : OCO<br>&nbsp;&nbsp;3 : OTO<br>&nbsp;&nbsp;4 : OTOCO<br>&nbsp;&nbsp;5 : OTOs(單線)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;GroupID<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;GroupID or OCOID or OTOID or OTOCOID or  OTOSID<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ChasePrice<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;追價 <br>&nbsp;&nbsp;格式為[每次增加價格|追價幾次|每次追價幾秒|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;限價追價 <br>&nbsp;&nbsp;格式為[0|追價限制幾跳|-11 (Chase<br>&nbsp;&nbsp;Trailing) or -12 (Chase if touch)|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;或是[1|追價幾次|-11 (Chase Trailing)<br>&nbsp;&nbsp;or -12 (Chase if touch)|最後下市價或刪單或掛單(M or C or<br>&nbsp;&nbsp;L)|PriceType]<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;a.若 每次增加價格 &lt; 1000 表示幾跳Ticks<br>&nbsp;&nbsp;b.追價幾秒=-11 (Chase<br>&nbsp;&nbsp;Trailing)<br>&nbsp;&nbsp;c.追價幾秒=-12 (Chase if<br>&nbsp;&nbsp;touch)<br>&nbsp;&nbsp;d.Price Type=(LTP, BID, ASK)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedType<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;不支援群組單<br>&nbsp;&nbsp;0 : None                  5 :<br>&nbsp;&nbsp;AllSliced After PriceTrigger<br>&nbsp;&nbsp;1 : Iceberg<br>&nbsp;&nbsp;2 : TimeSliced<br>&nbsp;&nbsp;3 : VolumeSliced<br>&nbsp;&nbsp;4 : AllSliced<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;DiscloseQty<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =1 或 SlicedType =2 或SlicedType =3 或SlicedType =4或SlicedType =5使用<br>&nbsp;&nbsp;逐筆揭露數量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Variance<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =1 或 SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;逐筆變動比例%<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Interval<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;SlicedType =2 : 間隔時間，ms<br>&nbsp;&nbsp;SlicedType =3 : 間隔成交量<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;LeftoverAction<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅SlicedType =2或 SlicedType =3使用<br>&nbsp;&nbsp;0 : Leave 繼續掛著(預設)<br>&nbsp;&nbsp;1 : Merge 取消該筆，剩餘數量加入下筆下單<br>&nbsp;&nbsp;2 : Market 轉市價單<br>&nbsp;&nbsp;3 : Payup 追價(未實做)<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedPriceField<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;0 -&gt; Fixed price mode<br>&nbsp;&nbsp;非0 -&gt; Relative<br>&nbsp;&nbsp;price mode<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;0 : None <br>&nbsp;&nbsp;1 : Last<br>&nbsp;&nbsp;2 : Bid<br>&nbsp;&nbsp;3 : Ask<br>&nbsp;&nbsp;4 : 對方價<br>&nbsp;&nbsp;5 : 本方價<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;SlicedTicks<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;SlicedPriceField<br>&nbsp;&nbsp;!= 0<br>&nbsp;&nbsp;TickSize幾跳<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Breakeven<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=14使用<br>&nbsp;&nbsp;Breakeven觸發價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;BreakevenOffset<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=14使用<br>&nbsp;&nbsp;Breakeven停損價<br>&nbsp;&nbsp;or<br>&nbsp;&nbsp;欄位 + or - 價格<br>&nbsp;&nbsp;欄位為固定字, 可為LAST or BID or<br>&nbsp;&nbsp;ASK or FILLED<br>&nbsp;&nbsp;FILLED為OTO, OTOCO使用, 其他單不可使用FILLED<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Threshold<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅OrderType=12或OrderType=13使用<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;ExtCommands<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;延伸下單參數 多個參數請用；隔開<br>&nbsp;&nbsp;1.自動單反向延遲 請帶<br>&nbsp;&nbsp;  DelayTransPosition=3000<br>&nbsp;&nbsp; <br>&nbsp;&nbsp;表延遲3秒<br>&nbsp;&nbsp;2.下單前 將平倉掛單都刪單<br>&nbsp;&nbsp; CancelCloseWorking=1 or 2<br>&nbsp;&nbsp;3.下單速度是否調慢與設定相同<br>&nbsp;&nbsp; FitOrderFreq=1<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Consecutive <br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;1 : 連續送單 2:TW連續IOC<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;NumberOfRetries<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Consecutive=1使用<br>&nbsp;&nbsp;持續送單幾次,<br>&nbsp;&nbsp;NumberOfRetries&gt;=9999 表示無線次數<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;RetryInterval<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;僅Consecutive=1使用<br>&nbsp;&nbsp;每次送單間隔ms,<br>&nbsp;&nbsp;RetryInterval=0表是立即送單<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;Strategy<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;策略名稱<br>&nbsp;&nbsp;</td>
    
    
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;UserKey1<br>&nbsp;&nbsp;</td>
    <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky"><br>&nbsp;&nbsp;將可在回報的UserKey1欄位得到相同的字串<br>&nbsp;&nbsp;</td>
    
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
    <td class="tg-wa1i" colspan="2"><br>&nbsp;&nbsp;下單錯誤碼<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-10<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;Unknow Error<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-11<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;買賣別不對<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-12<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;複式單商品代碼解晰錯誤<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-13<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;下單帳號, 不可下此交易所商品<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-14<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;下單錯誤, 不支援的價格 或 OrderType 或 TimeInForce<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-15<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;不支援證券下單<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-20<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;連線未建立<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-22<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;價格的TickSize錯誤<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-23<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;下單數量超過該商品的上下限<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-24<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;下單數量錯誤<br>&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td class="tg-baqh"><br>&nbsp;&nbsp;-25<br>&nbsp;&nbsp;</td>
    <td class="tg-0lax"><br>&nbsp;&nbsp;價格不能小於和等於0 (市價類型不會去檢查)<br>&nbsp;&nbsp;</td>
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
    <td class="tg-0pky">  回報編號  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReplaceExecType  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  0:改價  1:改量  2:改價改量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OrderQty  <br>&nbsp;&nbsp; </td>
     <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  使用者期望減少的委託量  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Price  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  使用者期望的委託價格  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  StopPrice  <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  使用者期望的停損價格</td>
  </tr>
</table>
</center>

<br>

### 8.期货/期权资金查询
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
    <td class="tg-0pky">  券商 (對應下單與回報欄位)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerName   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  券商名稱  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Account   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  帳號 (對應下單與回報欄位)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountName   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  帳號名稱  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;<br>&nbsp;&nbsp;  TransactDate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新日期 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TransactTime   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  更新時間 UTC+0  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BeginningBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期初結存  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Commissions   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易手續費  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenCommission   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  凍結手續費  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeClearinigFee   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所費用與結算費用  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  BrokerageFee   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  經紀商費用  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  GrossPL   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  已實現損益(未扣除費用)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OptionPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  選擇權權利金收入  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CashIn   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  证券买卖当日收支  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  NetPL   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  已實現損益(扣除費用並加上選擇權權利金收入)  </td>
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
    <td class="tg-0pky">  可用資金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  WithdrawQuota   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  可取資金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  EndingBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期末結存  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OpenTradeEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未平倉損益(包含選擇權和期貨)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TotalEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  總權益數  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  OptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  選擇權市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AccountValueAtMarket   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  帳戶市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  InitialMarginRequirement   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  初始保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MaintenanceMarginRequirement   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  維持保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  當前保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarginDeficit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  追繳保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  凍結保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenCash   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  凍結資金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReserveBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  保底準備金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Credit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  信用額度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Mortgage   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  質押金額  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreMortgage   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次質押金額  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreCredit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次信用額度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreDeposit   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次存款額  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  PreMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  上次佔用的保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  DeliveryMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  投資者交割保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExchangeDeliveryMargin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  交易所交割保证金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyToSystem   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  系統幣別  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyConversionRate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  幣別轉換比率(轉換到帳戶幣別使用)  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  CurrencyToClient   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  帳戶幣別  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ConvertedAccountValueAtMkt   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  幣別轉換後帳戶市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ExerciseIncome   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  行權盈虧 到期履約損益　  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  IncomeBalance   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  盈虧金額  </td>
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
    <td class="tg-0pky">  保证金水平 風險指標  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  UPLForOptions   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未沖銷選擇權浮動損益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  LongOptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未沖銷買方選擇權市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ShortOptionNetMarketValue   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未沖銷賣方選擇權市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FrozenpPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  委託權利金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarginExcess   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  加收原始保證金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  AdjustedEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  契約調整權益數 加/減項  </td>
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
    <td class="tg-0pky">  逐日浮動盈虧  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FloatProfitByTrade   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  逐筆浮動盈虧  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  FutureProfitByDay   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  期貨當日盈虧   </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  ReferenceRiskRate   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  參考風險度  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  TryExcessEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  試算可用資金  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  DynamicEquity   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  動態權益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  MarketPremium   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  市值權益  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  Op tionPremiumCoin   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  未沖銷權利金市值  </td>
  </tr>
  <tr>
    <td class="tg-c3ow"><br>&nbsp;&nbsp;  StockReferenceMarket   <br>&nbsp;&nbsp;</td>
   <td class="tg-c3ow"><br>BSTR<br>&nbsp;&nbsp;</td>
    <td class="tg-0pky">  股票參考市值</td>
  </tr>
</table>
  


### 9.期货/期权部位查询
### 10.PONG  