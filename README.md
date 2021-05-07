## 本モバイルアプリケーションの概要
<div style="text-align: left;">
  <img width="300" height="200" alt="bench-map-4-mobile" src="https://user-images.githubusercontent.com/54522567/115953948-454f5000-a529-11eb-84fd-3c49bce2fb86.png">
</div>

##### [Web アプリケーション版のBenchMap](https://github.com/nakano0518/benchmap-README) をモバイルアプリケーション化しました。
##### ※iOSモバイルアプリとしてApple Storeにリリースしました。 
より『周辺のベンチを探す』という目的にフォーカスし、  
Map の操作性の向上、および Web 版における機能過多な部分 (フォロー機能など) を削除しスリム化しました。  
本アプリをスマートフォンにインストールしておくことで、素早く手軽に周辺のベンチを探すことができます。


## 機能一覧および使用技術一覧

- ### フロント (ReactNative ( React / Typescript ))

  - ︎Google Map 関連機能
    - 現在地取得・表示機能
    - 地名検索機能
    - ベンチの位置に対するマーカー (以後、マーカー) 表示・登録機能
    - マーカークリックによるベンチ詳細情報表示機能 (コールアウト)
    - 200 件を超えるマーカーの表示に 15 秒かかる問題への対応 (下記)
      - 全てのマーカーを取得するのではなく、画面に表示される地図の範囲プラスアルファの範囲 (以後、取得マーカー範囲(※)) に限定して取得
      - ズームアウト (縮小) しすぎの場合、アラートを出しマーカーの取得をしない。
  - ベンチ詳細表示・編集機能
  - ユーザー登録・編集機能
  - いいね登録・解除機能
  - コメント表示・投稿・編集・削除機能
  - ユーザーの登録ベンチ、いいね、コメントの一覧表示・検索・編集・削除機能
  - Twitter、Facebook、Google での登録・ログイン機能
  - ローディング機能
  - モーダル表示機能 (下記)
    - ログイン、ログアウトなど処理の完了通知機能
    - サーバーサイドエラーの通知機能
  - ネットワークエラーに対する通知とリロードのページ表示機能
  - ベンチでない不適切な画像に対するバリデーション機能 ([機械学習](https://github.com/nakano0518/benchImageClassifier))

  - Redux 関連の実装
    - ミドルウェアに Redux-Thunk を使用
    - store のstateをイミュータブルな構造として保存 (immutable.js)
    - store の domain state を正規化して保存 (normalizr)
    - store で管理する認証情報に関しては永続化 (redux-persist)
    - フォルダは、app state、domain state および ui state に分け、各対象を re-ducks パターンで整理
    - useSelector における余計なレンダリングを抑制 (reselector)
- ### サーバーサイド (Rails)
  [こちらのページのAPI機能の項目](https://github.com/nakano0518/benchmap-README)をご参照ください。

## 今後の課題
- ### ベンチのデータ数を増やしていく必要性
  - 本アプリ完成段階でベンチのデータ数 70 件
- ### 地図のパフォーマンス低下問題 (下記) 
  - アプリのデータ数が増え、(※) の取得マーカー範囲に 200 件以上のマーカーが表示されるようになった場合にパフォーマンスが低下することが想定される　(下記、対応)
    - ~Google Map のクラスタリングライブラリの利用を検討~  
      ⇒ 一旦全てのマーカーを地図上に表示しクラスタリングしなおすライブラリであり、使用によりパフォーマンスは向上しないとわかったため断念。
    - reselectorを使用し表示前にマーカーをクラスタリングしたものを地図に表示する方法 (自前実装)で検討

## 参考画像  

- ### ログイン (例. Facebookによるログイン)  
<img width="320" height="570" alt="sns-login" src="https://user-images.githubusercontent.com/54522567/115959385-fadccc00-a546-11eb-9f7b-80961d8e8bc2.gif">

- ### いいね処理  
<img width="320" height="570" alt="like" src="https://user-images.githubusercontent.com/54522567/115959452-4000fe00-a547-11eb-8e50-4e2c92a27e20.gif">

- ### コメント投稿・編集・削除処理  
<img width="320" height="570" alt="comment" src="https://user-images.githubusercontent.com/54522567/115959467-4e4f1a00-a547-11eb-99d1-94ded8983305.gif">

- ### ベンチ登録 (機械学習によるバリデーション)  
<img width="320" height="570" alt="bench-created" src="https://user-images.githubusercontent.com/54522567/115959494-6cb51580-a547-11eb-880e-9cabef0b103e.gif">
<img width="320" height="570" alt="bench-not-created" src="https://user-images.githubusercontent.com/54522567/115959500-79396e00-a547-11eb-9e59-aae7a8fba916.gif">

- ### ユーザーの登録ベンチ・いいね・コメント一覧およびユーザー詳細情報表示  
<img width="320" height="570" alt="users-related-display" src="https://user-images.githubusercontent.com/54522567/115959509-82c2d600-a547-11eb-87e8-6586c4a89ab5.gif">
