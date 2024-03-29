# はじめにお読みください
このコードはBitTradeの取引所で自動売買を行うシンプルなサンプルコードです。サンプルコードは改変して利用することを前提にしています。
そのまま使っても取引は行いますが、取引ロジックはほぼ無いようなものなので各自ロジックを考えて書き換えて下さい。
仮にコードを利用して損失が発生した場合、制作者は一切の責任を負いません。

エンドポイントやパラメーターは[APIドキュメント](https://github.com/BitTrade-Inc/BitTrade-api-docs)を参照して書き換えてください。

# 使い方
APIキーの発行が完了している前提で解説していきます。
はじめてAPIを利用する方やアカウントIDを発行したことのない方は[step1.md](./step1.md)から順番に進めれば問題ありません。
最終的に稼働に必要な内容は[complete.md](./complete.md) になりますが、内容を確認しながら進めたい方は[step2.md](./step2.md) と[step3.md](./step3.md) を見ながら進めてください。

ドキュメント | 内容
------------ | ------------
[step1.md](./step1.md) | プライベートAPIを利用する際に必要なアカウントIDを取得します。アカウントIDは共通で利用できるので取得済みであれば必要ありません。
[step2.md](./step2.md) | 署名を行ってAPIから取引口座にログインします。
[step3.md](./step3.md) | 取引の実行を行うコードです。
[step4.md](./step4.md) | APIキー、シークレットキー、アカウントIDを入力すると実際に取引が開始できるコードです。
[step5.md](./step5.md) | 取引ロジックの説明。
