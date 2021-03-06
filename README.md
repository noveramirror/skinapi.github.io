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
- 肌タイプ（乾燥肌、油性肌、混合肌、敏感肌、普通肌）

## 開発
### API
base_URL: https://skin.api.viewty-platform.com/<br>
POST : /api/v1/face-detect/

1. 診断する画像をPOSTし、skin_dataを取得する

### 認証
ヘッダに弊社発行のアクセストークンIDの情報を付与してください  
※弊社発行のアクセストークンIDは、契約後に配布いたします。

###### POST : /api/v1/face-detect/
```
#Request
{
  // 診断する画像
  facePict: File(ImageBinary)
}
```

```
#Response
# HTTP Status: 200 OK
{
    //肌タイプ（"dry"（乾燥肌）/"oily"（脂性肌）/"mix"（混合肌）/"sensitive"（敏感肌）/"normal"（普通肌）)
    skin: 'String',
    //肌スコア
    scores: [
        {
          //日本語タイトル（"シワ"/"キメ"/"シミ"/"透明感"/"潤い"/"毛穴"）
            title_ja: 'String',
            //英語タイトル（"wrinkle"/"texture"/"stain"/"clarity"/"moisture"/"pores")
            title_en: 'String',
            //0~1のスコア（min=0, max=1）
            score: 'integer',
            //同世代の平均値（未入力の場合0）※テストURLの場合0
            average: 'integer',
            //A~Gのスコアに連動したランク(min=G, max=A)
            rank: 'String',
            //項目が"毛穴","シワ"の場合、値が来る
            details: {
                //詳細の識別子（"pores"（毛穴）/"wrinkle"（シワ）)
                detailType: 'String',
                //個数
                detailValue: 'integer',
                //"個"などの単位
                detailUnit": 'String'
            }
        },
        
        ...
        
        }
    ]
}
