from datetime import datetime
import requests
import json
import hmac
import hashlib
import base64
from urllib.parse import urlencode

# アカウントIDを取得する関数
def get_account_id(api_key, secret_key):
    endpoint = '/v1/account/accounts'
    params = {'AccessKeyId': api_key}

    # プライベートAPIエンドポイントにアクセスするための認証情報を追加
    timestamp = str(datetime.utcnow().isoformat())[0:19]
    params['SignatureMethod'] = 'HmacSHA256'
    params['SignatureVersion'] = '2'
    params['Timestamp'] = timestamp

    # リクエストの署名を作成
    sorted_params = sorted(params.items())
    encoded_params = urlencode(sorted_params)
    pre_signed_text = f'GET\napi-cloud.bittrade.co.jp\n{endpoint}\n{encoded_params}'
    hash_code = hmac.new(secret_key.encode(), pre_signed_text.encode(), hashlib.sha256).digest()
    signature = base64.b64encode(hash_code).decode()
    params['Signature'] = signature

    response = requests.get(f'https://api-cloud.bittrade.co.jp{endpoint}', params=params)
    data = response.json()
    if 'data' in data and len(data['data']) > 0:
        account_id = data['data'][0]['id']
        return account_id
    else:
        return None

# あなたのAPIキーと秘密鍵を使用してアカウントIDを取得
api_key = '2e2348b0-xa2b53ggfc-d45cde66-a932c'
secret_key = '8230f04f-23b093aa-f650e9fc-bc767'
account_id = get_account_id(api_key, secret_key)

if account_id is not None:
    print(f'アカウントID: {account_id}')
else:
    print('アカウントIDの取得に失敗しました。')
