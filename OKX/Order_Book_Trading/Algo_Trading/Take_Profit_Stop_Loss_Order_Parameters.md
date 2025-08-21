### Take Profit / Stop Loss Order Parameters

| Parameter      | Type   | Required | Description                                      |
|----------------|--------|----------|--------------------------------------------------|
| tpTriggerPx    | String | No       | Take-profit trigger price (must accompany tpOrdPx) |
| tpTriggerPxType| String | No       | Take-profit trigger price type (default "last")  |
| tpOrdPx        | String | No       | Take-profit order price (must accompany tpTriggerPx) |
| slTriggerPx    | String | No       | Stop-loss trigger price (must accompany slOrdPx)|
| slTriggerPxType| String | No       | Stop-loss trigger price type (default "last")    |
| slOrdPx        | String | No       | Stop-loss order price (must accompany slTriggerPx)|

> When placing net TP/SL order (ordType=conditional) with both TP and SL parameters, only stop-loss logic will be performed; take-profit logic will be ignored.

***
