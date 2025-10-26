# discordの通知システムfor gas

まずは、``通知チャンネル``ファイルと、``discordsystem``ファイルの中身をコピーして、自分のアカウントgasに貼り付けましょう。(別々のファイルで保存しましょう。)
``discordsystem``ファイルの``プロジェクトの設定``から``スクリプト ID``をコピーして、
``通知チャンネル``ファイルの``ライブラリ``を開いて、``スクリプト ID``を貼り付けましょう。
名前は、``discordsystem``で保存しましょう。

# youtube API の登録
``通知チャンネル``ファイルの、サービスの検索欄に``YouTube Data API``を入力して設定しましょう(バージョンは``v3``)

# youtubeURLとdiscord webhookのURLの登録
## discord webhook URL
[webhookの作成方法](https://zenn.dev/lambta/articles/5edbda4ccb1ec6)
* ``サーバーの設定``
* ``連携サービス``
* ``ウェブフック``
* ``新しいウェブフック``
* 名前と投稿先のチェンネルを選んだら「ウェブフックURLをコピー」をクリック
でURLを手に入ります。

``通知チャンネル``ファイルに、以下の構文があると思います
見たとおりですが``webhooks``には、discordサーバーで作成した``webhook``のURLを貼ってください

```通知チャンネル(15行目)
// 基本こことトリガーを設定すれば動くと思う。
var postchannel = {
  webhooks: '', // webhookのURL
  // webhooks: "", // テスト環境
  icon : '', // iconの設定(必要なければコメントアウトしてください)
  channel: '' // youtubeチャンネルのID
}
```
## youtube URL
### チャンネルIDの調べ方
[youtubeチャンネルの取得方法](https://reposub.jp/blogs/tips/youtube_channel_id?srsltid=AfmBOops7bhgTrXAWZ07lyqQM3A0_F9b_0vO2eFn78xwN6PZ5hm5z1Ug)

* youtubeチャンネルにアクセスし、``このチャンネルの詳細``をクリック
* ``チャンネルを共有``をクリック
* ``チャンネルIDをコピー``をクリック

### チャンネルアイコンの取得(必要であれば)
* チャンネルのアイコンを右クリック
* 画像アドレスをコピー

# テスト実行
以下の``if``構文の数値を``1``にして実行しましょう。

```通知チャンネル(42行目)
// 時間内であれば実行する
if (discordsystem.nowdata(posttime) === 0){ // 実行テストの場合は「0」ではなく「1」にして実行してください。
    var url = `${youtubeurl}${data.id.videoId}`;
    // var icon = data.snippet.thumbnails.default.url;
    console.log(url);
```
最初の実行注意が出てきます。
最初は驚きますが、どんどん進んで実行してください。おそらく``discord``サーバに通知が来るはずです。

以上
