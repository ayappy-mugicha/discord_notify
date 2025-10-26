# discordの通知システムfor gas

まずは、``通知チャンネル``ファイルと、``discordsystem``ファイルの中身をコピーして、 /n 
自分のアカウントgasに貼り付けましょう。(別々のファイルで保存しましょう。)  \n
``discordsystem``ファイルの``プロジェクトの設定``から``スクリプト ID``をコピーして、 \n 
``通知チャンネル``ファイルの``ライブラリ``を開いて、``スクリプト ID``を貼り付けましょう。 \n 
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
// 本番環境
if (discordsystem.nowdata(posttime) === 0){
    var url = `${youtubeurl}${data.id.videoId}`;
    // var icon = data.snippet.thumbnails.default.url;
    console.log(url);

// 実行テスト
if (discordsystem.nowdata(posttime) === 1){ 
    var url = `${youtubeurl}${data.id.videoId}`;
    // var icon = data.snippet.thumbnails.default.url;
    console.log(url);
```
最初の実行注意が出てきます。
最初は驚きますが、どんどん進んで実行してください。おそらく``discord``サーバに通知が来るはずです。

# トリガーの設定
トリガーの追加をしましょう。
* ``実行する関数``は``getvideoset``
* ``時間ベースのトリガーのタイプを選択``を``分タイマーベース``
* ``時間の間隔を選択（時間）``を``15分``

## トリガーが15分な理由
``youtube API`` のリクエスト件数が、1日あたり10,000ユニットのクォータ制限があり
``youtube``動画を1回取得につき、100クオーターを消費するので、15分に一回にしています。
[youtube API](https://developers.google.com/youtube/v3/determine_quota_cost?hl=ja)

# その他の機能
初期設定では、動画を取得する設定になっていますが、動画配信のデータを取得するようにもできます。
``getYoutubeData``の引数を``eventType="none"``から``eventType="live"``に変えることで、ライブ配信の通知をすることができます。
```通知チャンネル(29行目)
  // 動画の場合
  var datas = await getYoutubeData(postchannel.channel,eventType="none");

  // 配信の場合
  var datas = await getYoutubeData(postchannel.channel,eventType="live");  
```

# 備考
おすすめはライブ配信用のファイルを作ることをおすすめします。
先程のクオータ制限の問題です。
ファイルを別にすれば、トリガーを15分にして制限がかかることはありません。
