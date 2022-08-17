# skinAPI

## API概要
![a259c2e0-47b5-4f9c-9a24-0ae1ddcf7893](https://user-images.githubusercontent.com/33708804/94100177-dcefa180-fe67-11ea-9cee-c7e1ff5b55a1.png)

skinAPIはシンプルなAPIです。人物の顔写真を送るだけで「シミの状態」「シワの状態」などの肌診断の結果（肌スコア）を取得することができます。リクエストは、画像データをHTTPSを使用して送信するだけ、結果はJSON形式で返却されます。プラットフォームに制限はありません。

1. ユーザーが顔写真をAIサーバーに送信します。
2. AIサーバが判定した肌スコアを返します。

- 顔写真を無許可でサーバに保存することはありません
- 匿名情報（タイムスタンプ／アクセストークンID／肌スコア）をAPIコール時に保存します

## 肌スコア
以下肌スコアを取得することが出来ます
- シワ（0から1のスコア）
- キメ（0から1のスコア）
- シミ（0から1のスコア）
- 透明感（0から1のスコア）
- 潤い（0から1のスコア）
- 毛穴（0から1のスコア）
- 肌タイプ（乾燥肌、普通肌、油性肌）
- 肌年齢 (開発中)

## 診断モデル概要
<img width="893" alt="診断モデル概要" src="https://user-images.githubusercontent.com/103926148/184310881-eb992111-0ead-471f-97b5-492c75bd21ce.png">

## 開発
### APIのみを利用する場合
#### API URL
development base_URL: https://dev2.skin.api.viewty-platform.com/<br>
production base_URL: https://skin.api.viewty.jp/<br>
POST : /api/v2/skin-detect/

1. 診断する画像をPOSTし、skin_dataを取得する


###### POST : /api/v2/skin-detect/
```
#Request
{
  // 診断する画像
  facePict: イメージファイル(写真から顔部分のみをトリミング→長辺256pxになるようにリサイズ)
}

{'authorizationToken':'XXXXXX'}

イメージファイルサンプル
https://github.com/noveramirror/skinapi.github.io/blob/master/img_sample.zip

python
DETECTION_URL = f'https://dev2.skin.api.viewty-platform.com/api/2/skin-detect'
files=[('facePict',
          (img_path,open(img_path,'rb'),'image/jpeg'))
          ]
headers = {'authorizationToken':'XXXXX'} 
payload={}

res = requests.request('POST', DETECTION_URL, headers=headers, data=payload, files=files)

```

```
#Response
# HTTP Status: 200 OK

```
#Response
# HTTP Status: 200 OK
{
  "age": 38, #肌年齢 (int)
  "condition": "normal",　 #肌タイプ（"dry"（乾燥肌）/"oily"（脂性肌）/"normal"（普通肌）
  "details": {
    "age": 38.15592555201033 # 肌年齢 (float)
  },
  "scores": {
    "clarity": { #透明感
      "average": 0,　　# 現状常に0
      "details": {
        "detailValue": 0.51220703125 #診断結果
      },
      "rank": "D",　  #ランク
      "score": 0.42, #ランクスコア
      "title_en": "clarity", #項目(英語)
      "title_ja": "\u900f\u660e\u611f" #項目(日本語)
    },
    "moisture": {
      "average": 0,　　　　　　　# 現状常に0
      "details": {
        "detailValue": 0.673828125　#診断結果
      },
      "rank": "E",　　　　　　　　#ランク
      "score": 0.28,　　　　#ランクスコア
      "title_en": "moisture",　#項目(英語)
      "title_ja": "\u6f64\u3044"　#項目(日本語)
    },
    "pores": {
      "average": 0,　　　　　　　# 現状常に0
      "details": {
        "detailValue": 0.7265625　#診断結果
      },
      "rank": "C",　　　　　　　　#ランク
      "score": 0.56,　　　　#ランクスコア
      "title_en": "pores",　　　　　　　　　　　　#項目(英語)
      "title_ja": "\u6bdb\u7a74"　#項目(日本語)
    },
    "stain": {
      "average": 0,　　　　　　　　# 現状常に0
      "details": {
        "detailValue": 0.64599609375　　#診断結果
      },
      "rank": "C",　　　　　　　　#ランク
      "score": 0.56,　　　　#ランクスコア
      "title_en": "stain",　　　　　　　　　　　　　#項目(英語)
      "title_ja": "\u30b7\u30df"　#項目(日本語)
    },
    "texture": {
      "average": 0,　　　　　　　# 現状常に0
      "details": {
        "detailValue": 0.7001953125　#診断結果
      },
      "rank": "E",　　　　#ランク
      "score": 0.28,　　　　#ランクスコア
      "title_en": "texture",　　　　　　　　　#項目(英語)
      "title_ja": "\u30ad\u30e1"　#項目(日本語)
    },
    "wrinkle": {
      "average": 0,　　　　　　　　# 現状常に0
      "details": {
        "detailValue": 0.18168881702583814　#診断結果
      },
      "rank": "A",　　　　　　　　#ランク
      "score": 1,　　　　#ランクスコア
      "title_en": "wrinkle",　　　　　　　　#項目(英語)
      "title_ja": "\u30b7\u30ef"　#項目(日本語)
    }
  }
}```

