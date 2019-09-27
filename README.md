

# 注意事項

1. 分頁查詢需要帶入QryIndex(帶空則回第一頁)，每頁50筆資料，拿最後一筆QryIndex資訊可以往下查，最後一頁則回空資料。

2. service會通過PUB連線發送PING訊息，客戶端需要發送PONG請求來回應，超過一分鐘service沒收到回應則會清掉該連線session

<br>

 ![](./A/python.png)

    Client App行情需建立兩條ZeroMQ連線，一條REP/REQ連線，一條PUB/SUB連線。若要連接交易，需再建立另兩條REP/REQ、PUB/SUB連線。




|  |    |
|----------|----------|
|BrokerID             |BSTR|        
|Account              |BSTR|
|Symbol               |BSTR|
|Price                |BSTR|
|StopPrice            |BSTR|
|Side                 |BSTR|
|TimeInForce          |BSTR|
|OrderType            |BSTR|
|ContingentSymbol     |BSTR|
|TrailingField        |BSTR|
|TrailingType         |BSTR|
|TrailingAmount       |BSTR|
|TouchPrice           |BSTR|
|TouchField           |BSTR|
|TouchCondition       |BSTR|
|OrderQty             |BSTR|
|PositionEffect       |BSTR|
|DayTrade             |BSTR|
|Synthetic            |BSTR|
|GroupType            |BSTR|
|GroupID              |BSTR|
|ChasePrice           |BSTR|
|SlicedType           |BSTR|
|DiscloseQty          |BSTR|
|Variance             |BSTR|
|Interval             |BSTR|
|LeftoverAction       |BSTR|
|SlicedPriceField     |BSTR|
|SlicedTicks          |BSTR|
|Breakeven            |BSTR|
|BreakevenOffset      |BSTR|
|Threshold            |BSTR|
|ExtCommands          |BSTR|
|Consecutive          |BSTR|
|NumberOfRetries      |BSTR|
|RetryInterval        |BSTR|
|Strategy             |BSTR|
|UserKey1             |BSTR|




<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0pky"> </th>
    <th class="tg-0pky"> </th>
    <th class="tg-0pky"> </th>
    <th class="tg-0pky"></th>
    <th class="tg-0pky"></th>
  </tr>
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
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">15: 中間價(MID)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">20 : 最優價 (BST)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">21 : 最優價轉限價 (BSTL)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">22 : 五檔市價 (5LvlMKT)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">23 : 五檔市價轉限價 (5LvlMTL)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">24 : 市價轉限價 (MTL)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">25: 一定範圍市價(MWP)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">ContingentSymbol</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">追蹤Symbol </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=7 或 OrderType=8使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TrailingField</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=5 或 OrderType=6使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Last</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : Bid</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : Ask</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TrailingType</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=5 或 OrderType=6使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : 依價格</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : 依TrailingPrice百分比</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : 依TickSize幾跳</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TrailingAmount</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=5 或 OrderType=6使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">TrailingType=1時, TrailingAmount為百分比</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">TrailingType=2時, TrailingAmount為點數</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">TrailingType=3時, TrailingAmount為幾跳</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TouchPrice</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=7 或 OrderType=8使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">觸發價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">or</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位 + or - 價格</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位為固定字, 可為LAST or BID or ASK or<br>&nbsp;&nbsp;FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">FILLED為OTO, OTOCO使用, 其他單不可使用FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TouchField</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=3 或 OrderType=4 或<br>&nbsp;&nbsp;OrderType=7 或 OrderType=8或OrderType=12 或 OrderType=13使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Last</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : Bid</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : Ask</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">TouchCondition</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Synthetic = 1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">OrderType=7 或 OrderType=8使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : touch or 穿價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Greater</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : GreaterEqual</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : Equl</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">4 : LessEqual</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">5 : Less</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">OrderQty</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">下單口數</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">PositionEffect</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">0 : Open  Open position </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Close  Close position </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : 平今</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : 平昨</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">4 : Auto   Auto select Open/Cloe position  </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">5 : 特殊自動單 有昨倉先平昨<br>&nbsp;&nbsp;其它一律轉開倉</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">10 : 備兌開倉</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">11: 備兌平倉</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">DayTrade</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : 當沖</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Synthetic</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Synthetic</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">GroupType</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">0 : None</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Normal</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : OCO</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : OTO</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">4 : OTOCO</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">5 : OTOs(單線)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">GroupID</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">GroupID or OCOID or OTOID or OTOCOID or  OTOSID</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">ChasePrice</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">追價 </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">格式為[每次增加價格|追價幾次|每次追價幾秒|最後下市價或刪單或掛單(M<br>&nbsp;&nbsp;or C or L)|PriceType]</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">限價追價 </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">格式為[0|追價限制幾跳|-11 (Chase Trailing) or<br>&nbsp;&nbsp;-12 (Chase if touch)|最後下市價或刪單或掛單(M or C or L)|PriceType]</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">或是[1|追價幾次|-11 (Chase Trailing) or -12<br>&nbsp;&nbsp;(Chase if touch)|最後下市價或刪單或掛單(M or C or L)|PriceType]</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">a.若 每次增加價格 &lt; 1000 表示幾跳Ticks</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">b.追價幾秒=-11 (Chase Trailing)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">c.追價幾秒=-12 (Chase if touch)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">d.Price Type=(LTP, BID, ASK)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">SlicedType</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">不支援群組單</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : None                  5 : AllSliced After<br>&nbsp;&nbsp;PriceTrigger</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Iceberg</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : TimeSliced</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : VolumeSliced</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">4 : AllSliced</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">DiscloseQty</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅SlicedType =1 或 SlicedType =2 或SlicedType =3 或SlicedType =4或SlicedType =5使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">逐筆揭露數量</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Variance</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅SlicedType =1 或 SlicedType =2或 SlicedType =3使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">逐筆變動比例%</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Interval</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅SlicedType =2或 SlicedType =3使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">SlicedType =2 :<br>&nbsp;&nbsp;間隔時間，ms</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">SlicedType =3 :<br>&nbsp;&nbsp;間隔成交量</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">LeftoverAction</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅SlicedType =2或 SlicedType =3使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : Leave 繼續掛著(預設)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Merge 取消該筆，剩餘數量加入下筆下單</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : Market 轉市價單</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : Payup 追價(未實做)</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">SlicedPriceField</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">0 -&gt; Fixed price mode</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">非0 -&gt; Relative price mode</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">0 : None </td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1 : Last</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2 : Bid</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3 : Ask</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">4 : 對方價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">5 : 本方價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">SlicedTicks</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">SlicedPriceField != 0</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">TickSize幾跳</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Breakeven</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅OrderType=14使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">Breakeven觸發價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">or</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位 + or - 價格</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位為固定字, 可為LAST or BID or ASK or<br>&nbsp;&nbsp;FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">FILLED為OTO, OTOCO使用, 其他單不可使用FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">BreakevenOffset</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅OrderType=14使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">Breakeven停損價</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">or</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位 + or - 價格</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">欄位為固定字, 可為LAST or BID or ASK or<br>&nbsp;&nbsp;FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">FILLED為OTO, OTOCO使用, 其他單不可使用FILLED</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Threshold</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅OrderType=12或OrderType=13使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">ExtCommands</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">延伸下單參數 多個參數請用；隔開</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">1.自動單反向延遲 請帶</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> <br>&nbsp;&nbsp;DelayTransPosition=3000</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> <br>&nbsp;&nbsp;表延遲3秒</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">2.下單前 將平倉掛單都刪單</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> CancelCloseWorking=1 or 2</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">3.下單速度是否調慢與設定相同</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> FitOrderFreq=1</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Consecutive </td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">1 : 連續送單 2:TW連續IOC</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">NumberOfRetries</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Consecutive=1使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">持續送單幾次, NumberOfRetries&gt;=9999<br>&nbsp;&nbsp;表示無線次數</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">RetryInterval</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">僅Consecutive=1使用</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky"> </td>
    <td class="tg-0pky">每次送單間隔ms, RetryInterval=0表是立即送單</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">Strategy</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">策略名稱</td>
    
    
  </tr>
  <tr>
    <td class="tg-0pky">UserKey1</td>
    <td class="tg-0pky">BSTR</td>
    <td class="tg-0pky">將可在回報的UserKey1欄位得到相同的字串</td>
    
    
  </tr>
</table>


