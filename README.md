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

## 開発
### APIのみを利用する場合
#### API URL
base_URL: https://dev.skin.api.viewty-platform.com/<br>
POST : /api/v2/skin-detect/

1. 診断する画像をPOSTし、skin_dataを取得する


###### POST : /api/v2/skin-detect/
```
#Request
{
  // 診断する画像
  facePict: Base64
}
```

```
#Response
# HTTP Status: 200 OK
{
　　  //肌年齢
    age: number,
    //肌タイプ（"dry"（乾燥肌）/"oily"（脂性肌）/"normal"（普通肌）)
    condition: string,

    //肌スコア
    scores:{
        //英語タイトル（"wrinkle"/"texture"/"stain"/"clarity"/"moisture"/"pores")
        SkinDetectKey:{
            // 現状常に0
            average: number
            details: {
                        detailValue: number
                      }
            // 項目評価
            rank: SkinRank
            score: number
            title_en: string
            title_ja: string
        }
    }
}
```
