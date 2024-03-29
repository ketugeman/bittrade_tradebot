# step5:取引ロジックの解説
今回紹介したコードは直近の相場情報から基準となる価格を設定し、その価格から-10,000円の価格に買いの指値を、+10,000円の価格に売りの指値を付けます。  

## 直近の相場情報を取得する
```
    response = requests.get('https://api-cloud.bittrade.co.jp/market/detail/merged?symbol=btcjpy')
    data = response.json()
    current_price = data['tick']['close']
```
エンドポイントからBTC/JPYの相場情報を取得しています。取引を行うペアを変更したい際にはエンドポイント文末の「btcjpy」を任意の取引ペアに変更してください。  
エンドポイントから取得したJSONから最新の価格だけを抽出するためにティックデータからclose(最新の価格)を指定して引き出します。

## 直近の価格を基にエントリーポイントを決定する
```
   buy_price = current_price-10000
   sell_price = current_price+10000
```
直近の相場状況から得た価格にそれぞれ買いと売りのエントリーポイントを設定します。  
今回はシンプルに最新の価格にから上下に10,000円離れた価格にしていますが、±5%のような設定も可能です。取引ペアと相場状況によってうまく使い分けてください。

## 指値注文を行う
```
 response = place_order(api_key, secret_key, symbol, buy_price, 1000, 'buy-limit')
    print(response)

    response = place_order(api_key, secret_key, symbol, sell_price, 1000, 'sell-limit')
    print(response)
```
設定したエントリーポイントに指値の注文を実行します。  
今回は1注文1,000円としていますが、口座の資産状況によって変更してください。  
**※口座内の円と暗号資産が等価または十分な数量あることを前提にしています。稼働前の口座残高が円のみの場合売り注文は実行されません。**


## 取引をループさせる
```
while True:

    time.sleep(10)
```
注文実行後、10秒間インターバルをおいて相場情報の取得から取引をループします。
取引ペアや相場状況によって()の中の数字を変更してください。

