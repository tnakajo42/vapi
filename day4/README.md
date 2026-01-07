# 次世代のユーザーインタフェイスVUI「Vapi」に挑戦するぞ

https://vapi.ai/

![vapi](https://github.com/protoout/fukui-day4-5/blob/main/day4/img/vapi-top.png)

「TALK TO VAPI」で、最初は英語で質問してくるが、「日本語で」とお願いしていると、日本語になります。

<!-- Webアプリに組み込むのは、多分できるはずなので、最悪こっちメインで行こうと思います（いきなり日本語でもいけそう） -->

# はじめに

今回の講座は、福井初公開どころか、日本初公開、いや世界初のVAPIを使った講義になります。

スマホの次のユーザーインターフェイスと期待されている、声のインタフェイスVUI『ボイス・ユーザーインターフェイス』

日本でも、最近はタクシーを呼ぶときに、裏側の声はAIが対応して、配車をしていると聞いたことがありますし、お店の予約などもAIが電話対応をして完了する時代はもうすぐそこに来ています。

また、Webサイトを見ていて、AIに聞いてみるというボタンはよく見ますが、ちょっとチャットするのは大変だったりします。それが、音声で対応してくれる、と言ったらどうでしょう？

今回は、そんなことを実際にプロトタイプとして実装して、次世代のユーザーインターフェイス「VUI」を体験していただければと思います。

# VAPI × Twilio 「電話対応はAIにお任せせよ！」

Twilio: https://www.twilio.com/ja-jp

## Twilio 側での設定

電話番号の取得をしてください。

次に、電話番号と Account SID と、Auth Token を取得します。

※アメリカの番号(+1 から始まる番号)でも大丈夫です。

## vapi 側での設定 (VapiにTwilio番号をインポート)

1. Vapiダッシュボードにログイン
2. 左メニューの 「Phone Numbers」→「Import」 をクリック
3. フォームに下記を入力したら「Import」 
- Phone number（Twilioの電話番号）
- Twilio Account SID
- Twilio Auth Token

ラベルは、「Fukui」とでもしておきましょうか。

4. インポートが成功すると、Vapi上にその番号が一覧表示されます。

## Twillio で API キーの発行

https://console.twilio.com/us1/account/keys-credentials/api-keys/create


## 参考文献

https://developers.cyberagent.co.jp/blog/archives/60442/
