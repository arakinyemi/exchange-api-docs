### Get / Order history (last 3 months)

Get completed orders placed in the last 3 months, including those placed 3 months ago but completed in this period.  
Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request  
`GET /api/v5/trade/orders-history-archive`

Request Example  
`GET /api/v5/trade/orders-history-archive?ordType=post_only,fok,ioc&instType=SPOT`

Request Parameters  
*(Same as "GET / Order history (last 7 days)" parameters)*

Response Example  
*(Same structure and example as "GET / Order history (last 7 days)" response example)*
