# complete:コピペで稼働可能なコード
step1～step3までに解説したコードを基に1つにまとめます。  
アカウントIDとAPIキー、シークレットキーを代入すると実際に動き出すので注意してください。
```
import time
import requests
import json
import hmac
import hashlib
import base64
from datetime import datetime
from urllib.parse import urlencode

def place_order(api_key, secret_key, symbol, price, amount, order_type):
    endpoint = '/v1/order/orders/place'
    timestamp = str(datetime.utcnow().isoformat())[0:19]
    params = urlencode({
        'AccessKeyId': api_key,
        'SignatureMethod': 'HmacSHA256',
        'SignatureVersion': '2',
        'Timestamp': timestamp
    })
    method = 'POST'
    base_uri = 'api-cloud.bittrade.co.jp'
    pre_signed_text = f'{method}\n{base_uri}\n{endpoint}\n{params}'
    hash_code = hmac.new(secret_key.encode(), pre_signed_text.encode(), hashlib.sha256).digest()
    signature = base64.b64encode(hash_code).decode()
    url = f'https://{base_uri}{endpoint}?{params}&Signature={signature}'
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
    response = requests.get('https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=btcjpy')
    data = response.json()
    current_price = data['tick']['close']

    buy_price = current_price-10000
    sell_price = current_price+10000

    response = place_order(api_key, secret_key, symbol, buy_price, 1000, 'buy-limit')
    print(response)

    response = place_order(api_key, secret_key, symbol, sell_price, 1000, 'sell-limit')
    print(response)

    time.sleep(10)
```
step3で解説したようにアカウントID、APIキー、シークレットキーを3か所代入するだけで他はコピペで稼働可能です。
