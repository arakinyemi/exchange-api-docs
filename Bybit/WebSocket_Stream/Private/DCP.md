# Dcp

Subscribe to the dcp stream to trigger DCP function.

For example:  
- Connection A subscribes "dcp.xxx"  
- Connection B does not  
- Connection C subscribes "dcp.xxx"  

If A is alive, B is dead, C is alive, then this case will not trigger DCP.  
If A is alive, B is dead, C is dead, then this case will not trigger DCP.  
If A is dead, B is alive, C is dead, then DCP is triggered when reach the timeWindow threshold.  

In summary, for private connections, if all subscribing "dcp" topic are dead, then DCP will be triggered.

**Topic:** dcp.future, dcp.spot, dcp.option

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "dcp.future"
    ]
}
```