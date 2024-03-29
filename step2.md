# step2:署名処理を行って取引口座にアクセスします。

## 署名処理手順
1.リクエスト方法(GET或いはPOST)、続けて改行を追加する。  

2.小文字のアクセスアドレスに続けて改行を追加する。  

3.アクセスメゾットへのパス、続けて改行を追加する。  

4.パラメーター名は、ASCIIコードの順にソートし、URLエンコーディングしてください。  

5.上記の順次で各パラメーターは＆を仕様して連結します。  

6.シークレットキーと変換後のリクエスト文字列を使って、署名処理を行い、その結果はBase64エンコードします。  

7.最終的にサーバーに送信するAPIリクエストは次のようになります。

```
def place_order(api_key, secret_key, symbol, amount, order_type):
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
```
基本的に署名処理はこのままコピペで問題ありません。
自動売買以外にもプライベートAPIを利用する際には署名処理が必要になるにで使い回してください。
