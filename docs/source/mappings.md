# マッピング

半角と全角のマッピングを説明します。

## 半角から全角へのマッピング

Unicode 標準の ["Halfwidth and Fullwidth Forms"](https://www.unicode.org/charts/PDF/UFF00.pdf) チャートで定義されている半角カタカナを対応する全角カタカナへ変換します。

半角カタカナとは、次の範囲の文字とします。

- "Halfwidth and Fullwidth Forms" チャート
  - "Halfwidth CJK punctuation" (0xFF61-0xFF64)
  - "Halfwidth Katakana variants" (0xFF65-0xFF9F)

上記範囲には、カタカナ文字以外に句点、読点、鉤括弧、中黒、長音記号、濁点、半濁点といった記号を含みます。日本語には、ひらがなや漢字の半角文字はないため、これらの記号もカタカナの部分とします。

濁点、半濁点は、直前の文字と組み合わせることで成る文字が全角カタカナの合成済み文字として定義されている場合、直前の文字と組み合わせた合成済み文字へ変換します。合成済み文字が定義されていない場合、全角の濁点、半濁点へ変換します。

| 半角                       | 全角                  |
| -------------------------- | --------------------- |
| ﾀﾞ (0xFF80, 0xFF9E)         | ダ (0x30C0)           |
| ｱﾞ (0xFF71, 0xFF9E)         | ア゛ (0x30A2, 0x309B) |
| ﾀﾞﾞ (0xFF80, 0xFF9E, 0xFF9E) | ダ゛ (0x30C0, 0x309B) |

合成済み文字として Unicode で未定義の新しい文字を作り出さないようにするため、結合文字の濁点 (0x3099)、半濁点 (0x309A) へは変換しません。

句点、読点、鉤括弧、中黒、長音記号は、一括で変換するかしないかを選択できます。

## 全角から半角へのマッピング

Unicode 標準の ["Katakana"](https://www.unicode.org/charts/PDF/U30A0.pdf) チャートで定義されている全角カタカナを対応する半角カタカナへ変換します。又、句点、読点、鉤括弧、濁点、半濁点といった全角カタカナのチャートには定義されていませんが、カタカナ表記で使用する記号も変換します。

- "Katakana" チャート
  - "Katakana letters" (0x30A1-0x30FA)
  - "Conjunction and length marks" (0x30FB-0x30FC)
- ["Hiragana"](https://www.unicode.org/charts/PDF/U3040.pdf) チャート
  - "Voicing marks" (0x3099-0x309C)
- ["CJK Symbols and Punctuation"](https://www.unicode.org/charts/PDF/U3000.pdf) チャート
  - "CJK corner brackets" (0x300C-0x300D のみ)

半角カタカナが定義されていないワ行の "ヰ" と "ヱ"、それぞれの濁音は変換しません。同様に小文字の "ヵ" と "ヶ"、"ヮ" は変換しません。

"Katakana punctuation" として定義されているダブルハイフン "゠" は、相互変換できないため、変換しません。同様に "Iteration marks" や "Katakana digraph" に定義されている文字も変換しません。

濁音、半濁音は、合成済み文字か結合文字かに関わらず、半角カタカナの清音と濁点、半濁点へ変換します。単独の濁点、半濁点は、合成済み文字か結合文字かに関わらず、カタカナに続くとみなせる場合は、変換します。

| 半角                  | 全角                       |
| --------------------- | -------------------------- |
| ダ   (0x30C0)         | ﾀﾞ  (0xFF80, 0xFF9E)        |
| ダ   (0x30BF, 0x3099) | ﾀﾞ  (0xFF80, 0xFF9E)        |
| ダ゛ (0x30C0, 0x309B) | ﾀﾞﾞ (0xFF80, 0xFF9E, 0xFF9E) |
| ダ゛ (0x30C0, 0x3099) | ﾀﾞﾞ (0xFF80, 0xFF9E, 0xFF9E) |
| ア゛ (0x30A2, 0x309B) | ｱﾞ  (0xFF71, 0xFF9E)        |
| だ゛ (0x3060, 0x309B) | だ゛(0x3060, 0x309B)       |

:::{note}
HTML で表示が崩れるため、半角に例示している (0x30C0, 0x3099) は、結合文字ではなく全角カタカナの濁音で表示しています。 
:::

句点、読点、鉤括弧、中黒、長音記号は、ひらがなかカタカナのいずれとして扱うかを判断できないため、一括で変換するかしないかを選択できます。

## 制御文字の取り扱い

半角から全角、全角から半角のいずれも変換では、不可視の制御文字も 1 文字として処理します。

濁音、半濁音とカタカナの間に制御文字がある場合、カタカナに続くとみなしません。

| 半角                       | 全角                          |
| -------------------------- | ----------------------------- |
| ﾀﾞ (0xFF80, 0x0000, 0xFF9E) | タ゛ (0x30C0, 0x0000, 0x309B) |

## 半角と全角のマップ

半角と全角のマップを示します。

### カタカナ文字 (清音)

| 半角       | 全角        |
| ---------- | ----------- |
| ｱ (0xFF71) | ア (0x30A2) |
| ｲ (0xFF72) | イ (0x30A4) |
| ｳ (0xFF73) | ウ (0x30A6) |
| ｴ (0xFF74) | エ (0x30A8) |
| ｵ (0xFF75) | オ (0x30AA) |
| ｶ (0xFF76) | カ (0x30AB) |
| ｷ (0xFF77) | キ (0x30AD) |
| ｸ (0xFF78) | ク (0x30AF) |
| ｹ (0xFF79) | ケ (0x30B1) |
| ｺ (0xFF7A) | コ (0x30B3) |
| ｻ (0xFF7B) | サ (0x30B5) |
| ｼ (0xFF7C) | シ (0x30B7) |
| ｽ (0xFF7D) | ス (0x30B9) |
| ｾ (0xFF7E) | セ (0x30BB) |
| ｿ (0xFF7F) | ソ (0x30BD) |
| ﾀ (0xFF80) | タ (0x30BF) |
| ﾁ (0xFF81) | チ (0x30C1) |
| ﾂ (0xFF82) | ツ (0x30C4) |
| ﾃ (0xFF83) | テ (0x30C6) |
| ﾄ (0xFF84) | ト (0x30C8) |
| ﾅ (0xFF85) | ナ (0x30CA) |
| ﾆ (0xFF86) | ニ (0x30CB) |
| ﾇ (0xFF87) | ヌ (0x30CC) |
| ﾈ (0xFF88) | ネ (0x30CD) |
| ﾉ (0xFF89) | ノ (0x30CE) |
| ﾊ (0xFF8A) | ハ (0x30CF) |
| ﾋ (0xFF8B) | ヒ (0x30D2) |
| ﾌ (0xFF8C) | フ (0x30D5) |
| ﾍ (0xFF8D) | ヘ (0x30D8) |
| ﾎ (0xFF8E) | ホ (0x30DB) |
| ﾏ (0xFF8F) | マ (0x30DE) |
| ﾐ (0xFF90) | ミ (0x30DF) |
| ﾑ (0xFF91) | ム (0x30E0) |
| ﾒ (0xFF92) | メ (0x30E1) |
| ﾓ (0xFF93) | モ (0x30E2) |
| ﾔ (0xFF94) | ヤ (0x30E4) |
| ﾕ (0xFF95) | ユ (0x30E6) |
| ﾖ (0xFF96) | ヨ (0x30E8) |
| ﾗ (0xFF97) | ラ (0x30E9) |
| ﾘ (0xFF98) | リ (0x30EA) |
| ﾙ (0xFF99) | ル (0x30EB) |
| ﾚ (0xFF9A) | レ (0x30EC) |
| ﾛ (0xFF9B) | ロ (0x30ED) |
| ﾜ (0xFF9C) | ワ (0x30EF) |
| ｦ (0xFF66) | ヲ (0x30F2) |
| ﾝ (0xFF9D) | ン (0x30F3) |

### カタカナ文字 (小文字)

| 半角       | 全角        |
| ---------- | ----------- |
| ｬ (0xFF6C) | ャ (0x30E3) |
| ｭ (0xFF6D) | ュ (0x30E5) |
| ｮ (0xFF6E) | ョ (0x30E7) |
| ｯ (0xFF6F) | ッ (0x30C3) |
| ｧ (0xFF67) | ァ (0x30A1) |
| ｨ (0xFF68) | ィ (0x30A3) |
| ｩ (0xFF69) | ゥ (0x30A5) |
| ｪ (0xFF6A) | ェ (0x30A7) |
| ｫ (0xFF6B) | ォ (0x30A9) |

### カタカナ文字 (濁音)

半角から全角へは、合成済み文字のみへの変換になります。全角から半角は、合成済み文字か結合文字の両方を変換します。

| 半角               | 全角                          |
| ------------------ | ----------------------------- |
| ｳﾞ (0xFF73, 0xFF9E) | ヴ (0x30F4 or 0x30A6, 0x3099) |
| ｶﾞ (0xFF76, 0xFF9E) | ガ (0x30AC or 0x30AB, 0x3099) |
| ｷﾞ (0xFF77, 0xFF9E) | ギ (0x30AE or 0x30AD, 0x3099) |
| ｸﾞ (0xFF78, 0xFF9E) | グ (0x30B0 or 0x30AF, 0x3099) |
| ｹﾞ (0xFF79, 0xFF9E) | ゲ (0x30B2 or 0x30B1, 0x3099) |
| ｺﾞ (0xFF7A, 0xFF9E) | ゴ (0x30B4 or 0x30B3, 0x3099) |
| ｻﾞ (0xFF7B, 0xFF9E) | ザ (0x30B6 or 0x30B5, 0x3099) |
| ｼﾞ (0xFF7C, 0xFF9E) | ジ (0x30B8 or 0x30B7, 0x3099) |
| ｽﾞ (0xFF7D, 0xFF9E) | ズ (0x30BA or 0x30B9, 0x3099) |
| ｾﾞ (0xFF7E, 0xFF9E) | ゼ (0x30BC or 0x30BB, 0x3099) |
| ｿﾞ (0xFF7F, 0xFF9E) | ゾ (0x30BE or 0x30BD, 0x3099) |
| ﾀﾞ (0xFF80, 0xFF9E) | ダ (0x30C0 or 0x30BF, 0x3099) |
| ﾁﾞ (0xFF81, 0xFF9E) | ヂ (0x30C2 or 0x30C1, 0x3099) |
| ﾂﾞ (0xFF82, 0xFF9E) | ヅ (0x30C5 or 0x30C4, 0x3099) |
| ﾃﾞ (0xFF83, 0xFF9E) | デ (0x30C7 or 0x30C6, 0x3099) |
| ﾄﾞ (0xFF84, 0xFF9E) | ド (0x30C9 or 0x30C8, 0x3099) |
| ﾊﾞ (0xFF8A, 0xFF9E) | バ (0x30D0 or 0x30CF, 0x3099) |
| ﾋﾞ (0xFF8B, 0xFF9E) | ビ (0x30D3 or 0x30D2, 0x3099) |
| ﾌﾞ (0xFF8C, 0xFF9E) | ブ (0x30D6 or 0x30D5, 0x3099) |
| ﾍﾞ (0xFF8D, 0xFF9E) | ベ (0x30D9 or 0x30D8, 0x3099) |
| ﾎﾞ (0xFF8E, 0xFF9E) | ボ (0x30DC or 0x30DB, 0x3099) |
| ﾜﾞ (0xFF9C, 0xFF9E) | ヷ (0x30F7 or 0x30EF, 0x3099) |
| ｦﾞ (0xFF66, 0xFF9E) | ヺ (0x30FA or 0x30F2, 0x3099) |

### カタカナ文字 (半濁音)

半角から全角へは、合成済み文字のみへの変換になります。全角から半角は、合成済み文字か結合文字の両方を変換します。

| 半角               | 全角                          |
| ------------------ | ----------------------------- |
| ﾊﾟ (0xFF8A, 0xFF9F) | パ (0x30D1 or 0x30CF, 0x309A) |
| ﾋﾟ (0xFF8B, 0xFF9F) | ピ (0x30D4 or 0x30D2, 0x309A) |
| ﾌﾟ (0xFF8C, 0xFF9F) | プ (0x30D7 or 0x30D5, 0x309A) |
| ﾍﾟ (0xFF8D, 0xFF9F) | ペ (0x30DA or 0x30D8, 0x309A) |
| ﾎﾟ (0xFF8E, 0xFF9F) | ポ (0x30DD or 0x30DB, 0x309A) |

### カタカナ記号 (濁点、半濁点)

| 半角       | 全角        |
| ---------- | ----------- |
| ﾞ (0xFF9E) | ゛ (0x309B) |
| ﾟ (0xFF9F) | ゜ (0x309C) |

### カタカナ記号 (句読点)

| 半角       | 全角        |
| ---------- | ----------- |
| ､ (0xFF64) | 、 (0x3001) |
| ｡ (0xFF61) | 。 (0x3002) |

### カタカナ記号 (鉤括弧)

| 半角       | 全角        |
| ---------- | ----------- |
| ｢ (0xFF62) | 「 (0x300C) |
| ｣ (0xFF63) | 」 (0x300D) |

### カタカナ記号 (中黒)

| 半角       | 全角        |
| ---------- | ----------- |
| ･ (0xFF65) | ・ (0x30FB) |

### カタカナ記号 (長音記号)

| 半角       | 全角        |
| ---------- | ----------- |
| ｰ (0xFF70) | ー (0x30FC) |