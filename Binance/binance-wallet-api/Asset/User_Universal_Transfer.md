# User Universal Transfer 

## API Description​

user universal transfer

## HTTP Request​

POST `/sapi/v1/asset/transfer`

You need to enable `Permits Universal Transfer` option for the API Key which requests this endpoint.

## Request Weight(UID)​

**900**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| type | ENUM | YES |  |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| fromSymbol | STRING | NO |  |
| toSymbol | STRING | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

* `fromSymbol` must be sent when type are ISOLATEDMARGIN\_MARGIN and ISOLATEDMARGIN\_ISOLATEDMARGIN
* `toSymbol` must be sent when type are MARGIN\_ISOLATEDMARGIN and ISOLATEDMARGIN\_ISOLATEDMARGIN
* ENUM of transfer types:

  * MAIN\_UMFUTURE Spot account transfer to USDⓈ-M Futures account
  * MAIN\_CMFUTURE Spot account transfer to COIN-M Futures account
  * MAIN\_MARGIN Spot account transfer to Margin（cross）account
  * UMFUTURE\_MAIN USDⓈ-M Futures account transfer to Spot account
  * UMFUTURE\_MARGIN USDⓈ-M Futures account transfer to Margin（cross）account
  * CMFUTURE\_MAIN COIN-M Futures account transfer to Spot account
  * CMFUTURE\_MARGIN COIN-M Futures account transfer to Margin(cross) account
  * MARGIN\_MAIN Margin（cross）account transfer to Spot account
  * MARGIN\_UMFUTURE Margin（cross）account transfer to USDⓈ-M Futures
  * MARGIN\_CMFUTURE Margin（cross）account transfer to COIN-M Futures
  * ISOLATEDMARGIN\_MARGIN Isolated margin account transfer to Margin(cross) account
  * MARGIN\_ISOLATEDMARGIN Margin(cross) account transfer to Isolated margin account
  * ISOLATEDMARGIN\_ISOLATEDMARGIN Isolated margin account transfer to Isolated margin account
  * MAIN\_FUNDING Spot account transfer to Funding account
  * FUNDING\_MAIN Funding account transfer to Spot account
  * FUNDING\_UMFUTURE Funding account transfer to UMFUTURE account
  * UMFUTURE\_FUNDING UMFUTURE account transfer to Funding account
  * MARGIN\_FUNDING MARGIN account transfer to Funding account
  * FUNDING\_MARGIN Funding account transfer to Margin account
  * FUNDING\_CMFUTURE Funding account transfer to CMFUTURE account
  * CMFUTURE\_FUNDING CMFUTURE account transfer to Funding account
  * MAIN\_OPTION Spot account transfer to Options account
  * OPTION\_MAIN Options account transfer to Spot account
  * UMFUTURE\_OPTION USDⓈ-M Futures account transfer to Options account
  * OPTION\_UMFUTURE Options account transfer to USDⓈ-M Futures account
  * MARGIN\_OPTION Margin（cross）account transfer to Options account
  * OPTION\_MARGIN Options account transfer to Margin（cross）account
  * FUNDING\_OPTION Funding account transfer to Options account
  * OPTION\_FUNDING Options account transfer to Funding account
  * MAIN\_PORTFOLIO\_MARGIN Spot account transfer to Portfolio Margin account
  * PORTFOLIO\_MARGIN\_MAIN Portfolio Margin account transfer to Spot account

## Response Example​

```json
{  
    "tranId":13526853623  
}
```

