---
date: 2016-12-23T18:00:00+09:00
title: "Firebase Hostingの紹介をした - WEB TOUCH MEETING #96"
---

[WEB TOUCH MEETING #96](https://wtm.connpass.com/event/45081/)で[Firebase](https://firebase.google.com/)の紹介をしました。対象者としては、「レンタルサーバを借りたことがあって、独自ドメインの設定をしたことがある」ぐらいに設定してお話をしました。

<script async class="speakerdeck-embed" data-id="ca045cba785440c6a83027d2127d9707" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Firebaseを試した時に感じたのは、「Googleが用意したWebアプリケーションを作る手軽な場所」でした。
今まではその役割をGoogle App Engineが持っていたようにおもいます。
しかし、次世代のGoogle App Engineとなる Flexible Environmentは無料では始められないようになっています。
この部分にFirebaseが登場したから役目を終えたのではないかと感じました。

次に、気になったのはが独自ドメインのTLS/SSLが無料だったことです。
独自ドメインでなければHTTPSがつかえるサービスは確実に増えていて、困ること減っているとおもいます。
また、VPSなどあれば無料で証明書は発行して、HTTPSに対応できるようになってきましたが、ブログサービスなどの活用をしようとした場合、なかなか無料で独自ドメイン使えるものはありませんでした。

そうしているうちに状況は変化してきて、
2017年1月からGoogle ChormeがHTTPのサイトは警告を出すようになるそうです。

HTTPSでないサイトではiOSの一部の機能が使えないそうです。

先日Jimdoが独自ドメインのHTTPSの対応を発表したりなどありました。

HTTPS everywhereを現実に迎える直前で、良いタイミングだと思ってFirebase Hostingを紹介しました。
アプリケーションに満たないちょっとしたWebサイトを公開する際の一つ

他の話はおまけ程度のつもりで以下の話もしました。

* FTPは使えなくてコマンドを使う必要があるよ
* でも簡単に前の状態に戻せるよ
* Firebase Hostingにドメインを割り当てると、サブドメインを別のFirebaseプロジェクトに割当ができないよ
* Firebaseの他の機能を使うとこんなことができるよ

FTPでファイルを手動でアップロードしてる人たちの考え方が変わると良いなと少し思いました。

### デモサイトの話

[https://eiel.info/](https://eiel.info/)をリニューアルついでにデモとして使いました。

サインインをすると挨拶ができるという機能が実装されています。
リアルタイムに反映されるのをみていただきました。

ソースコードは以下に公開しています。

* [eiel/eiel.info Tag wtm96](https://github.com/eiel/eiel.info/tree/wtm96)

React.js + material-ui を利用もしていますが、その話は一切しませんでした。
最近はウェブサイト作る必要がある際にmaterial-uiを試しています。

Firebaseに関するコードだけ抜粋します。
JavaScriptですがBabelを利用して、ES2017が使える状態にしてあります。

firebaseの利用準備

```
import firebase from 'firebase/app'
import {} from 'firebase/auth'
import {} from 'firebase/database'

const config = {
    apiKey: "xxxxxxxxxxx",
    authDomain: "xxxxxxxxxxxxxx",
    databaseURL: "xxxxxxxxxxxxxx",
    storageBucket: "xxxxxxxxxxxxxxxx",
    messagingSenderId: "xxxxxxxxxxxxxxx"
};
```

認証処理は以下の感じで呼び出せばすぐに認証がはじまるのでボタンクリック次のイベントに設定したりするだけでした。

```
// 認証にGoogleアカウントを使う
let provider = new firebase.auth.GoogleAuthProvider();

// これを呼び出すと認証が開始する
firebase.auth().signInWithPopup(provider).then(function(result) {
  // 認証が成功した時の処理
  let token = result.credential.accessToken;
  let user = result.user;
}).catch(function(error) {
  // 認証が失敗した時の処理
  let errorCode = error.code;
  let errorMessage = error.message;
  let email = error.email;
  let credential = error.credential;
});
```

userの状態が変化したときの処理も登録できるのでそこでサインインしているユーザのアイコンを表示しています。今回のデモはReactのstateをかえるだけで、あとはReactにおまかせしています。

```
firebase.auth().onAuthStateChanged((user) => {
  this.setState({user});
});
```

リアルタイムデータベースのほうは「挨拶をする」を押下時にデータを作成しています。
押下時にメッセージを用意して、以下の感じでデータを挿入しています。

```
addMessage(message) {
  const key = firebase.database().ref('messages').push().key;
  firebase.database().ref(`messages/${key}`).set({created_at: new Date().getTime(), message: message, photoURL: this.state.user.photoURL})
}
```

表示のほうは、`messages`以下のデータの変更検知して再描画しています。

```
firebase.database().ref('messages').on('value', (snapshot) => {
  // データが変更されたときの処理
  let values = snapshot.val();
  if (values) {
    let keys = Object.keys(values);
    let length = keys.length;

    // 描画用にデータを修正
    let messages = [];
    for (let key of keys){
      let message = values[key];
      messages.unshift(message);
    }
    this.setState({messages});

    // 不要なデータを削除
    let updates = {};
    let deleteKeys = keys.splice(0,length-20);
    for (let key of deleteKeys) {
      updates[key] = null;
    }
    firebase.database().ref('messages').update(updates);
  }
});
```

`setState`をすることでReactが再描画してくれて、描画に不要データの削除をしています。
ある程度JavaScriptがかけると比較的すぐに使えるんじゃないかとおもいます。

### スライドに含まれているURL

* [Firebase](https://firebase.google.com/)
* [さくらインターネットの独自SSL](https://www.sakura.ad.jp/function/security/original-ssl.html)
* [Jimdoサイト SSL対応](https://jp.jimdo.com/2016/12/20/ssl-all/)
* [野球場のいらすと](https://azukichi.net/baseball/baseball055.html)
* [Let's Encrypt](https://letsencrypt.jp/)
* [CloudFlare](https://www.cloudflare.com/)
* [Webに接続するiOSアプリは2017年1月からHTTPSの使用が絶対条件になる、デベロッパーはご注意を](http://jp.techcrunch.com/2016/06/15/20160614apple-will-require-https-connections-for-ios-apps-by-the-end-of-2016/)
* [hromeはHTTPの死を早めている…１月からHTTPSでないページに警告を表示](http://jp.techcrunch.com/2016/09/09/20160908chrome-is-helping-kill-http/)
