# OnsenSearch
このサービスは温泉を検索する際、AIを用いることによって短時間で探すことができるサービスです。
  
## 背景
温泉を探す際に重要なことは、癒されに行きたい。友人と話したい。また、サウナに行きたい。など、その時の気分であると考えています。しかし、現在その目的を達成できるサービスでは温泉を探す時に時間がかかってしまいます。私は、検索に費やす無駄な時間を温泉で癒される時間に変えたいと考え、AIを用いることによって最適な温泉を検索するサービスを開発しました。
## 概要
AIを用いることにより、温泉を探す際に対話形式で検索を行うことができます。短時間で自分に合った温泉を探すことができるだけでなく、他のサービスにない「スタンプラリー機能」により達成感や楽しさの付加価値を提供します。　

## 技術選定
*Frontend*
- React.js

*Backend*
- Ruby on Rails

*DB*
-  MySQL

## 機能要件

### 温泉検索機能
#### 機能概要
<!-- ユーザーがこの機能を通じて何ができるのかを端的に表現しましょう -->

ユーザーが今の気分を入力することで、LLMが適切な温泉リストを推薦してくれる

#### ユーザーストーリー
<!-- ユーザーがこの機能を使うのアクションを番号付きリストで表現しましょう -->
1. 今の気分を入力する
2. LLMが気分から重視したい温泉の特性を下記の５種類から特定する
    - 温泉の種類と効能
    - ロケーションと景観
    - 設備とサービスの良さ
    - アクセスの良さ
    - 周辺の観光スポット
3. 分類されたカテゴリーに当てはまる温泉の一覧が表示される

#### 基本設計
<!-- 少し具体的に使用する技術やデータの構造などを踏まえて詳細に機能について表現しましょう -->

1. LLMにより、気分→温泉の特徴を抽出する
2. 温泉情報DBに「設備の綺麗さ」や「サービスの質」、「温泉の種類」などの情報を設ける
3. 抽出された温泉の特徴に合致する変数をDBから、最大値を示す温泉を表示する

▽ プロンプト例
```
入力された情報の感情にマッチする温泉を以下の5つの選択肢のうちどれにあたるか、数字のみで回答してください。
1. 温泉の種類と効能
2. ロケーションと景観
3. 設備とサービスの良さ
4. アクセスの良さ
5. 周辺の観光スポット
```

▽ 例
「友人と楽しく話ながら温泉にはいりたい！」  
↓
`3.設備とサービスの良さ`

`3.設備とサービスの良さ` に合致する変数は「綺麗さ」、「サービスの質」である  
→「綺麗さ」、「サービスの質」の変数(doble型)を温泉情報DBから参照し、それぞれの変数を足した値の平均値が最大を示す温泉を推薦する

### ユーザ登録機能

#### 機能概要
ユーザが入力したユーザ名、パスワードを元にアカウントの登録を行う

#### 基本設計
ユーザ情報DBを設け、ユーザが入力した「ユーザ名」、「パスワード」をDBに追加する

- ユーザ名:string型、1文字以上
- パスワード:string型、6文字以上,英数字を含むこと

### ログイン機能

#### 機能概要
ユーザが入力した「ユーザ名」、「パスワード」からログインを行う

#### 基本設計
- ユーザが入力した「ユーザ名」、「パスワード」がユーザ情報DBに存在する場合  
→ ログイン完了のメッセージを出力

- ユーザ情報DBに存在しない場合  
→ エラー文を出力

### 温泉お気に入り登録機能
#### 機能概要
プロフィールから、お気に入り登録した温泉を選ぶことで、「場所」や「料金」などの情報を短時間で確認できる

#### 基本設計
ユーザ情報DBに、お気に入り登録した温泉を表すカラムを設け、温泉情報DBと紐づける

### スタンプラリー機能
ユーザが訪れた温泉を記録し、可視化する

#### 必須
日本地図のイラスト(白地図)をプロフィールに表示させ、それぞれの都道府県をタップまたはクリックすることにより、色を塗りつぶすことができる

#### 出来ればよい
Geolocation APIなどの位置情報を取得できるAPIを用いて、位置情報によりその温泉が位置する都道府県の色を塗りつぶすことができる