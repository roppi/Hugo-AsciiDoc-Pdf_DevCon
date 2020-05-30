# rc_hugo
Hugo と Asciidoc で静的サイトの構築を行うためのスケルトンプロジェクトです。  
日本語を含んだ PlantUML, latexmath などの画像生成とPDF化も可能です。

また、Remote-Containers 導入済みの VS Code で開くだけで環境構築ができます。

## 使い方

### とりあえずの試し方
```bash
cd sample
cp "$(dirname $(dirname $(gem which asciidoctor-pdf-cjk-kai_gen_gothic)))/data/themes/KaiGenGothicJP-theme.yml" .
asciidoctor-pdf sample.adoc
hugo server -D -t nothing
```

`sample` 配下に、 `sample.pdf` が作成されます。
また、http://localhost:1313/ で Hugo の結果が確認できます。

### Hugo の使い方

#### サイトのスケルトン作成
```bash
hugo new site . --force
```

#### 作成したサイトを確認
```bash
hugo server -Dw
```

#### HTMLを生成
```bash
hugo
```

http://localhost:1313/ にて確認できます。

### Asciidoctor-PDF の使い方

#### PDF の生成
```bash
asciidoctor-pdf target.adoc
```

## カスタマイズ
`.devcontainer/.devcontainer.json` の `build: args:` で見た目が変えられます。

### HTML_ROUGE_ARGS
シンタックスハイライトの引数。  
（ただの asciidoctor への引数なので、その他の使い方もできます。）

Hugo では、テーマ自体にシンタックスハイライトが組み込まれていることが多いため、
テーマの作りによって変更してください。

デフォルト値
```bash
-a source-highlighter=rouge -a rouge-css=style -a rouge-style=molokai
```

構文解析だけしたい場合
```bash
-a source-highlighter=rouge
```

色もつけたい場合（ rouge のスタイルを github に変更）
```bash
-a source-highlighter=rouge -a rouge-css=style -a rouge-style=github
```
### HTML_BASE_DIR
Webサイトのベースディレクトリ

> https://someuser.github.io/someRepos/
上記URLの場合、 `someRepos\\/` を設定してください 


### HTML_IMG_OUT_DIR
HTML 作成時の画像生成先パス（static配下を指定）

デフォルト値
```bash
images
```

### PDF_IMG_OUT_DIR
PDF 作成時の画像生成先パス

デフォルト値
```bash
static/images
```

### PDF_DEFAULT_FONT
PDF 作成時のフォントファミリー

デフォルト値
```bash
KaiGen Gothic JP
```

フォントファミリーをRobotoに変更
```bash
Roboto Mono
```


### PDF_ROUGE_STYLE
PDF 作成時の rouge のスタイル

デフォルト値
```bash
molokai
```

スタイルを github に変更
```bash
github
```

### PDF_STYLE
PDF 作成時のテーマファイル

```
KaiGenGothicJP-theme.yml
```


## 注意点

### Hugoで生成画像が表示されない場合は、Hugoを再実行する
Asciidoctor で生成した画像は、`static/images` 内に保存されますが、
公開用ディレクトリへの同期に間に合っていないことがあります。

再実行することで、公開用ディレクトリへの同期を行います。

### PDF 作成時はスタイルファイルを用意する
`asciidoctor-pdf` を実行するディレクトリに `KaiGenGothicJP-theme.yml` を用意するか、
以下のコマンドで標準のファイルを取得してください。

```bash
$ cp "$(dirname $(dirname $(gem which asciidoctor-pdf-cjk-kai_gen_gothic)))/data/themes/KaiGenGothicJP-theme.yml" .
```

### asciidoctor-diagram で作成する図のフォーマットを SVG にする
フォーマットがSVGでない場合日本語が出力できないため、フォーマットに SVG を指定してください.

#### OK
```adoc
[plantuml, format=svg]
----
花子 -> 太郎: Authentication Request
----
```

#### NG
```adoc
[plantuml]
----
花子 -> 太郎: Authentication Request
----
```
