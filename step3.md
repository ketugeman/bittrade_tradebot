# 取引を実行するコード
実際に取引を行うコードです。  
取引のロジックはこの箇所を書き換えることで変更可能です。
```py
    headers = {
        'Content-Type': 'application/json',
        'X-BT-APIKEY': api_key
    }
    payload = {
        'account-id': '※※※※',
        'amount': str(amount),
        'price': str(price),
        'source': 'api',
        'symbol': symbol,
        'type': order_type
    }
    response = requests.post(url, headers=headers, data=json.dumps(payload))
    return response.json()

api_key = '※※※※'
secret_key = '※※※※'

symbol = 'btcjpy'

while True:
    response = requests.get('https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=xrpjpy')
    data = response.json()
    current_price = data['tick']['close']

    buy_price = current_price-10000
    sell_price = current_price+10000

    response = place_order(api_key, secret_key, symbol, buy_price, 10000, 'buy-limit')
    print(response)

    response = place_order(api_key, secret_key, symbol, sell_price, 10000, 'sell-limit')
    print(response)

    time.sleep(10)

```

```
'account-id': '※※※※',
```
このの欄にはstep1で取得したアカウントIDを入力してください。


```
api_key = '※※※※'
secret_key = '※※※※'
```
この欄にはBitTradeのHPから取得したAPIキーとシークレットキーを入力します。

