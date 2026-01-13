※これは講義用教材です。
TAは、この内容に基づいて説明してください。

# 次世代のユーザーインタフェイスVUI「Vapi」に挑戦するぞ

VAPI: https://vapi.ai/

![vapi](https://github.com/tnakajo42/vapi/blob/main/day4/img/vapi-top.png)

「TALK TO VAPI」で、最初は英語で質問してくるが、「日本語で」とお願いしていると、日本語になります。

<!-- Webアプリに組み込むのは、多分できるはずなので、最悪こっちメインで行こうと思います（いきなり日本語でもいけそう） -->

# はじめに

今回の講座は、福井初公開どころか、日本初公開、いや世界初のVAPIを使った講義になります。

スマホの次のユーザーインターフェイスと期待されている、声のインタフェイスVUI『ボイス・ユーザーインターフェイス』

日本でも、最近はタクシーを呼ぶときに、裏側の声はAIが対応して、配車をしていると聞いたことがありますし、お店の予約などもAIが電話対応をして完了する時代はもうすぐそこに来ています。

また、Webサイトを見ていて、AIに聞いてみるというボタンはよく見ますが、ちょっとチャットするのは大変だったりします。それが、音声で対応してくれる、と言ったらどうでしょう？

今回は、そんなことを実際にプロトタイプとして実装して、次世代のユーザーインターフェイス「VUI」を体験していただければと思います。

今回のゴール: https://youtube.com/shorts/eel44_lUL4E

デモ（のびすけAI）: +1 (640) 227-4161

# VAPI × Twilio 「電話対応はAIにお任せせよ！」

Twilio: https://www.twilio.com/ja-jp

## Twilio 側での設定

![twilio-dashboard](https://github.com/tnakajo42/vapi/blob/main/day4/img/twilio-dashboard.png)

まず、電話番号の取得をしてください。

次に、電話番号と Account SID と、Auth Token を取得します。

※アメリカの番号(+1 から始まる番号)でも大丈夫です。

![Twilio-auth-info](https://github.com/tnakajo42/vapi/blob/main/day4/img/Twilio-auth-info.png)

Account SID と Auth Token は、英数字の羅列なのですが、Twilioのアカウント設定時に見つけられなかったとしても、[Twilio Console](https://console.twilio.com/) の Quick Tutrial の中の Start building をクリックして、下の方の Account Info に書いてあります。

![Twilio-account-info](https://github.com/tnakajo42/vapi/blob/main/day4/img/Twilio-account-info.png)


## vapi 側での設定 (VapiにTwilio番号をインポート)

![vapi-dashboard](https://github.com/tnakajo42/vapi/blob/main/day4/img/vapi-dashboard.png)

1. Vapiダッシュボードにログイン
2. 左メニューの 「Phone Numbers」→「Import」 をクリック
3. フォームに下記を入力したら「Import」 
- Phone number（Twilioの電話番号）
- Twilio Account SID
- Twilio Auth Token

ラベルは、「Fukui」とでもしておきましょうか。

![vapi-auth-setting](https://github.com/tnakajo42/vapi/blob/main/day4/img/vapi-auth-setting.png)

4. インポートが成功すると、Vapi上にその番号が一覧表示されます。

### どのAIを出るかの設定

1. インポートした番号をクリック (Twilio - fukui)
2. 「Inbound Settings」の項目（どのアシスタントが着信に出るか）を、自分が作った Assistant に設定（今回はRiley）
3. 保存

これで「その番号に着信が来たら、このAssistantを起動する」というルールが Vapi 側で用意されます。

ちなみに、Squad（任意）は、チームのような概念。
複数Assistantをまとめて動かしたい時に使う機能。

例：
- 営業用AI
- サポート用AI

を順番に回したい／分岐したい場合など。単一AssistantでOKなら不要です。また、警告っぽく見えるけど “機能説明” なだけです。

Workflow（任意）は、着信フローをカスタムしたい場合だけ使います。

例：
- 営業時間外 → 留守電
- 最初だけIVR（1番→AI、2番→人間）
- SMS送信

などをやりたい時。ただのAI電話なら不要です。

Fallback Destination（任意）は、AIが出られなかった場合の転送先です。

記入例：
- 自分の携帯
- オフィス電話
- 別のTwilio番号

入れなくても問題ないですが、実運用では入れておくと安心です。

### アシスタントをカスタマイズ

デフォルトの Riley を使いたくない場合は、assistants から、Create Assistant で追加可能ですし、Riley の設定などを変えても面白いと思います。

テストで作った[AIのびすけ](https://youtube.com/shorts/eel44_lUL4E)は、First Message(電話に出て最初に発する言葉)と、System Prompt（設定側のプロンプト）は、下記にしていますので、ご参考までに。

First Message: 
```FirstMessage
あ、こんにちは。のびすけ です。 お電話ありがとうございます。え〜 本日は、どのようなご用件でしょうか？
```

System Prompt: 
```
[Identity]  
あなたは、講師である「なかじょう たかゆき」のサポートを行うティーチング・アシスタント（TA）。名前は、のびすけです。現在、Twilioを使ったVUI（ボイス・インターフェース）に関する講義をサポートしています。

[Style]  
- 会話は常に日本語で行います。  
- 丁寧で安心感のある敬語を使用し、聞き取りやすい自然な話し方を心掛けます。  
- 落ち着いて、ゆっくり、聞き取りやすく話す。  
- 専門用語は、できるだけ平易な言葉で説明する。  

[Response Guidelines]  
- 必要であれば例や比喩を使い、長くなりすぎないよう簡潔にまとめる。  
- 相手が理解できているか、適宜確認する。

[Task & Goals]  
1. 受講生が内容を理解できるように分かりやすく補助説明を行う。  
2. ただし、講義の内容や方針の最終決定権は講師に任せる。  
3. 受講生が安心して質問できる雰囲気を作り、理解と学びを優しく後押しする。

[Error Handling / Fallback]  
- 講師の意図と矛盾する断定はしない。  
- 受講生への批判的・高圧的な態度は禁止。  
- 不確実な情報は「確認します」と伝える。

[Verbal Tic]  
- 僕  
- あ〜  
- いや、そうではなくて  
- 全然OKっすね  
- なるほど  
- いいっすね
- 〜ですね
- ですです
```

「No Structured Outputs Configured」という赤文字の警告は、出ていても無視してOKです。本番運用で通話内容を分析するための出力設定ができていないというだけで、通話自体の機能には一切影響しませんのでご安心ください。

![no-structured](https://github.com/tnakajo42/vapi/blob/main/day4/img/no-structured.png)

ちなみに、Filesという箇所に、補足資料を追加できます。[AIのびすけ](https://youtube.com/shorts/eel44_lUL4E)は、このREADME.mdの内容を補足資料として読み込んでTAをしてくれています。

あと、OpenAIのLLMのモデルは、`GPT 4o Mini Cluster` が一番早いのでオススメです（2026年1月現在）。 

### アシスタントの日本語対応

デフォルトが英語（en）なので、ここは日本語にしないと、日本語がうまく認識してくれません。

上のメニューバーの Transcriber の Language を 日本語（Japanese）に変更しましょう。

![vapi-toJapanese](https://github.com/tnakajo42/vapi/blob/main/day4/img/vapi-toJapanese.png)

## Twilio 側で着信先をVapiに指定

Twilioの番号設定で「電話がかかってきたとき、どこに投げるか」を設定しないと、何も起こりません。

下記の手順で、頑張って設定しましょう！

1. [Twilio Console](https://console.twilio.com/) の左メニューの、Develop → Phone Numbers → Manage → Active Numbers をクリック
2. あなたの番号をクリック
3. Voice Configuration の、A call comes in を「Webhook」に変更
4. Voice Configuration の、URL を「https://api.vapi.ai/twilio/inbound_call 」に変更

これで、あなたの番号に電話をかけてみてください。AIが対応すれば、大成功。

※Twilioのデフォルトは、Trial版なため、実際に運用する場合はアップグレードが必要です。

## AIと会話してみよう

VAPIの右上の緑色のボタン「Talk to assistant」から、設定しているAIと電話ができます。

ここで、実際にAIと会話ができるか試してみてください。

![talk-to-assistant](https://github.com/tnakajo42/vapi/blob/main/day4/img/talk-to-assistant.png)

しっかり会話ができればOKです。

# 【できる人はやってみよう①】本番環境の実装（AIが電話応答）

電話をかけて、Trial というエラーが電話で出てくる場合、同じ Active Numbers のページの Calls Log を見てみましょう。

![twilio-failed](https://github.com/tnakajo42/vapi/blob/main/day4/img/twilio-failed.png)

ここに、原因は書いてあるので、頑張って解読して、再度電話をかけてみましょう。

### Twilio の Trial ではできませんので、アップグレードが必要

現在、画面上部に「Trial: $XX.XXXX Upgrade」というボタンがあると思います。これは、まだクレジットカード等の支払い情報を登録していない無料トライアル期間であることを示しています。Twilioは不正利用（国際電話詐欺など）を防ぐため、無料アカウントでは一部の日本の番号（特定の特殊番号やハイリスク判定された番号）への発信をブロックしています。なので、画面上の 「Upgrade」 ボタンを押し、住所情報と支払い情報を登録して、アカウントを製品版にアップグレードしてください。

最初に20ドル必要ですが、アップグレードできたら、カード情報（支払いアイテム）は削除してしまっても構いません。この講座が終わったら、下記の手順で削除しましょう。

1. [Twilio Console](https://console.twilio.com/) の右上の「Admin」の「Account billing」をクリック。
2. 左メニューの「Payment Settings」の「Payment Methods」からクレジットカード番号を選択。
3. 「Delete payment method」で削除できます。

※デフォルトでは、自動で追加支払いがされてしまうため、削除できない場合はまず「[auto recharge settings](https://console.twilio.com/us1/billing/manage-billing/payment-settings)」から解約しましょう。

クレジットカードも解約して、下記の画面になっていればOKです。

![no-payment-settings](https://github.com/tnakajo42/vapi/blob/main/day4/img/no-payment-settings.png)

# 【できる人はやってみよう②】WebサイトにAIコールボタンを実装（チャットではなくボイスでAIが応答）

YouTubeで解説しています: https://www.youtube.com/watch?v=8tRyf5AC_nc

[![vapi-youtube](https://github.com/tnakajo42/vapi/blob/main/day4/img/vapi-youtube.png)](https://www.youtube.com/watch?v=8tRyf5AC_nc)

## 参考文献

Vapi公式「Twilio SIPトランク連携ガイド」: https://docs.vapi.ai/advanced/sip/twilio

次世代音声基盤Vapiを試す：https://developers.cyberagent.co.jp/blog/archives/60442/
