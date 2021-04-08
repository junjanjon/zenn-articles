---
title: "iOS アプリ Rubyist を使ってチャットワークにおはようと投稿する"
emoji: "💎"
type: "tech"
topics: ["Rubyist","Siri","Chatwork"]
published: true
---

# iOS アプリ Rubyist とは

https://apps.apple.com/jp/app/rubyist/id1539089868

iOS 上で mruby のコーディングと実行ができるアプリで、2021年4月6日に App Store でリリースされました[^1]。

[^1]: https://twitter.com/rubyistapp/status/1379197531175849988?s=21

インストールしてみたら、わかりやすいサンプルコードとかなり丁寧なドキュメントが内包されていて、さらに Siri や iOS のショートカットとの連携もやりやすく夢が広がる感じでした。

今回はチャットワークにメッセージを投稿するサンプルを書きます。


# チャットワークにメッセージを投稿する

チャットワークAPIを使って、メッセージを投稿するサンプルです。`token`, `room_id` を設定すれば動きます。

チャットワークAPI: https://developer.chatwork.com/ja/endpoint_rooms.html#POST-rooms-room_id-messages

```ruby
token = "..."
room_id = "..."
API = "https://api.chatwork.com/v2/rooms/#{room_id}/messages"
message = "body=おはよう"
response = HttpRequest.post(API, message, hedaders: { "X-ChatWorkToken":  token })
puts response.body
```

これで実行するとチャットワークに "おはよう" と投稿できました。
HttpRequest がやや見慣れないですが、Rubyist 独自の実装は内包されているドキュメントですぐに説明を読むことができます。

# Siri におはようと言ったら、チャットワークにメッセージを投稿する（iOS のショートカットを利用）

エディタモードで右上のボタンを押すと Siri、ショートカットの起動トリガーの設定ができます。
Siri の場合、「Add to Siri」を押した後起動フレーズを入力したら完了です。

![](https://storage.googleapis.com/zenn-user-upload/ikvut22d7q1xe1l0uvucimzzis1l)

# ほか

- PC でだいたいのコードを書いて、チャットワーク経由でコピペして iPhone でちょっと編集するという形で書いている。
- アプリエディタに補完機能があるのでアプリだけで書くのもありかもしれない。
- 作ったコードの共有方法やバックアップ方法はまだわからない。
  - 合わせて、トークン直書きとなってしまっているので環境変数ができるといいな。
- Alert, Confirm, Prompt でユーザ入力処理がかんたんに書けそう。
- `Siri.reply(phrase)` でかんたんに Siri にしゃべらせることができそう。
- ショートカットのアクションとして実行すると ARGV で引数を受けられそう。
- ショートカットのアクションとして実行すると標準出力を後続に渡せた。Rubyist の結果を LINE に送るとかできそう。
- 新規作成ボタンが効かないときがあったりする？
- 古い iPhone 7 だからかかなり落ちる。

スマホで API を実行できるというだけでもかなりいいのに、慣れ親しんだ言語であるということと iOS の自動化にも組み込めることが合わさり夢が広がっています。
