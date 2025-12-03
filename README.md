# ①課題名
StaffChat（店舗スタッフ向け カスタマー情報チャット）

## ②課題内容（どんな作品か）
- 小規模飲食店などの「店舗スタッフ専用」チャットアプリです。
- お客様ごとに「カルテ（顧客カード）」を作成し、その中でスタッフ同士がメモを残せます。
- 好み・NG食材・来店頻度・一言メモなどを共有して、引き継ぎをスムーズにすることを目的としています。
- 個人情報保護の観点から、フルネームや電話番号などは扱わず、「表示名＋ざっくりしたメモ」のみに絞っています。

## ③アプリのデプロイURL
https://sin2yk.github.io/staffchat/

## ④アプリのログイン用IDまたはPassword（ある場合）

※すべて Firebase Authentication（Email/Password）で作成したアカウントです。  
※実際の本番運用では適宜変更・削除してください。

- 担任用（teacher）
  - ID: `teacher@staffchat.dev`
  - PW: `gsacademy2025`

- デモ用スタッフ1
  - ID: `staff01@staffchat.dev`
  - PW: `staff2025`

- デモ用スタッフ2
  - ID: `staff02@staffchat.dev`
  - PW: `staff2025`

- デモ用スタッフ3
  - ID: `staff03@staffchat.dev`
  - PW: `staff2025`

（※上記アカウントは Firebase コンソールの Authentication 画面から手動で作成）

## ⑤工夫した点・こだわった点
- 最初はローカル配列だけで動く StaffChat を作り、その後 Firebase Auth / Firestore に「段階的に移行」する形で実装しました。
- Firestore では `customers` コレクションと、その配下の `customers/{customerId}/notes` サブコレクションという構造を採用し、
  顧客とメモの関係がシンプルに追えるようにしました。
- 顧客ドキュメントには `lastUpdatedAt` を持たせ、最後にメモが書かれた顧客から順にソートして表示することで、
  「最近動きがあったお客様」がひと目で分かるようにしました。
- Auth のユーザー情報とは別に `users/{uid}` コレクションを用意し、スタッフの表示名をそちらで一元管理する構成にしました。
- Firebase SDK は CDN + ES Modules で読み込み、`firebase-config.js` と `app.js` を分離することで、
  設定とアプリロジックを整理しました。

## ⑥難しかった点・次回トライしたいこと（又は機能）
- 最初はローカル配列のまま動いていたアプリを、どの順番で Firestore に移行していくかを整理するのが難しかったです。
  - まず Auth → 次に customers 読み込み → notes の書き込み → notes のリアルタイム購読 → lastUpdatedAt 更新
  の順で一歩ずつ差し替えることで、毎回動く状態を保ちながら移行できました。
- Firestore のルール（`request.auth != null`）を設定して、認証ユーザーだけ読み書きできるようにしたところで、
  セキュリティの考え方を意識できました。
- 次回は、スタッフごとに閲覧権限の違う「プライベートメモ」や、顧客タグ付け・検索機能なども実装してみたいです。
