以下は、アプリがダウンロードするJSONファイルの仕様書（例）です。  
このJSONは、2ちゃんねるのアスキーアート長篇作品のデータを格納しており、各作品はディレクトリ単位で管理され、ダウンロード時にアプリ側でファイルURLを組み立てます。

---

# JSON仕様書: YaRU_Viewer JSON Data Format

## 1. ルートオブジェクト

JSONファイルのルートはオブジェクト形式で、以下のキーを含みます。

| キー      | 型              | 説明 |
|-----------|-----------------|------|
| **name**  | string          | コレクション全体のタイトル（例：「やる夫まとめ」） |
| **rootURL** | string        | 作品ファイルのダウンロード時のベースURL。各ディレクトリ内のファイル名と組み合わせて完全なURLを構築する。 |
| **tags**  | array of string | コレクション全体に関連するタグのリスト。作品の分類やフィルタリングに使用される。 |
| **dirs**  | array of object | 作品ディレクトリ（カテゴリ）ごとの情報を格納する配列。 |

### サンプル（ルートオブジェクト）

```json
{
  "name": "やる夫まとめ",
  "rootURL": "https://yaruojson.github.io/yaruojson/",
  "tags": ["まとめ", "アスキーアート"],
  "dirs": [ ... ]
}
```

## 2. dirs 配列の各オブジェクト

`dirs` 配列は、各ディレクトリ（カテゴリ・作品群）を表すオブジェクトのリストです。各オブジェクトは以下のキーを持ちます。

| キー       | 型              | 説明 |
|------------|-----------------|------|
| **name**   | string          | ディレクトリの名称。各カテゴリや作品集のタイトル。 |
| **tags**   | array of string | このディレクトリに関連するタグ。複数の分類やジャンルを設定可能。 |
| **files**  | array of string | このディレクトリ内に含まれる各作品ファイルの名前。各ファイルはテキストデータ（アスキーアート）のファイル名を示す。 |

※アプリ側では、JSON内の各ディレクトリオブジェクトからファイル名（文字列）のリストを受け取り、`LocalFile` 型（初期値 readNum=0、fav=false など）として内部管理します。また、ディレクトリの `rootName` および `rootURL` は、ルートオブジェクトの `name` と `rootURL` を継承して設定されます。

### サンプル（ディレクトリオブジェクト）

```json
{
  "name": "作品カテゴリ1",
  "tags": ["タグA", "タグB"],
  "files": [
    "作品1.txt",
    "作品2.txt",
    "作品3.txt"
  ]
}
```

## 3. 全体のサンプルJSON

以下は、上記仕様に沿ったサンプルJSONです。

```json
{
  "name": "やる夫まとめ",
  "rootURL": "https://yaruojson.github.io/yaruojson/",
  "tags": ["まとめ", "アスキーアート"],
  "dirs": [
    {
      "name": "作品カテゴリ1",
      "tags": ["タグA", "タグB"],
      "files": [
        "作品1.txt",
        "作品2.txt"
      ]
    },
    {
      "name": "作品カテゴリ2",
      "tags": ["タグC"],
      "files": [
        "作品3.txt",
        "作品4.txt",
        "作品5.txt"
      ]
    }
  ]
}
```

## 4. 注意事項

- **キーの大文字・小文字**  
  キー名は必ず上記のとおりに記述してください。  
- **オプション項目**  
  もしルートオブジェクトや各ディレクトリオブジェクト内のキーが存在しない場合、アプリ側で空文字や空配列などのデフォルト値が使用されます。  
- **ファイルパスの組み立て**  
  アプリは、`rootURL` に各ディレクトリの `name` とファイル名（`files` 内の文字列）を連結することで、ファイルの完全なURLを構築します。  
- **内部プロパティとの違い**  
  JSONには各ファイルの「既読数」や「お気に入り状態」などの情報は含まれていません。これらはアプリ起動後に内部で管理・更新されます。

---

以上が、YaRU_Viewerアプリが使用するJSONファイルの仕様書です。  
この仕様に基づいて、アプリは指定されたURLからJSONを取得し、ディレクトリごとに分類されたアスキーアートテキストデータをダウンロード・表示します。