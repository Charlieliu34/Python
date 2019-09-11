# 壹、交易
## 一、Request-Reply
&emsp;&emsp;**cleint 发送 Request，等待 Server 回复 Reply。client 透过此联机登入、查询帐务。**

### 1.登入

**a.登入指令**    
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

**b.登入回复**                     
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

**c.登出指令**        
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
註1:回報欄位於後面章節[回報主推](chapter2.md)說明。

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

### 10.PING/PONG

```
{
  "Request":"PING",
  "SessionKey":"XXXXXXXXX"
}
回复
{
   "Reply":"PONG"
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
