# Hugo-AsciiDoc-Pdf_DevCon
Hugo と Asciidoc で静的サイトの構築を行うためのスケルトンプロジェクトです。  
日本語を扱う設定といろいろと制約のある Hugo との連携をそれっぽく実現しています。

AsciiDocから、PlantUMLやLatexの画像を含むサイトの生成、Github Pagesでの公開までを Github Actions で行います。

また、ローカルでの環境を VS Code + Remotecontainers で簡単に作れたり、PDFの生成も簡単に作成できます。

### 🏃‍♂️💨とりあえず動かしてみたい方向け
VS Code + Remote Containers でコンテナとして開いた後で、以下のコマンドを実行してください。
```bash
cd example
asciidoctor-pdf example.adoc
hugo server -D
```
- http://localhost:1313/ で Hugo の結果が確認できます。
- PDFファイルが、`example/example.pdf` に作成されます。


## HTMLの生成（Hugo）について
### 🔧使い方
VS Code + Remote Containers でコンテナとして開いた後で、以下のコマンドを実行してください。
``` bash
cd (生成したいサイトのディレクトリ)
hugo
```
HTMLのスタイル付けとして、静的サイトジェネレータの [Hugo](https://gohugo.io/) を使用しています。  
hugo コマンドの使い方やサイトの構成などは、Hugo の情報を参照してください。

### ⚠️注意
#### 初期設定（hugo.toml）について
Hugoのバージョンアップでセキュリティが強化され、標準ではAsciiDocの変換ができなくなりました。

設定ファイル（hugo.toml）に以下のような設定を追加してください。

- `[security.exec]` の `allow` に `"^asciidoctor$"` を追加
- `[security.exec]` の `osEnv` に `'^HUGO_'` を追加

https://gohugo.io/about/security-model/#security-policy
``` toml
[security]
  enableInlineShortcodes = false
  [security.exec]
    allow = ['^(dart-)?sass(-embedded)?$', '^go$', '^npx$', '^postcss$', "^asciidoctor$"]
    osEnv = ['(?i)^((HTTPS?|NO)_PROXY|PATH(EXT)?|APPDATA|TE?MP|TERM|GO\w+|(XDG_CONFIG_)?HOME|USERPROFILE|SSH_AUTH_SOCK|DISPLAY|LANG|SYSTEMDRIVE)$', '^HUGO_']
  [security.funcs]
    getenv = ['^HUGO_', '^CI$']
  [security.http]
    methods = ['(?i)GET|POST']
    urls = ['.*']
```

#### PlantUMLなどの画像が表示がされない場合
PlantUMLや数式などの画像を正しく出力するためには、`hugo` コマンドを2回実行してください。

Hugoでは公開フォルダ（Public）に、①画像などのファイルのコピーと、②AsciiDocからHTMLへの変換を行います。  
PlantUMLなどの画像化は②で行いますが、この時作られた画像は公開フォルダにコピーされないため表示できません。

２回実行することでこれらの画像も公開フォルダにコピーされ、表示が可能となります。

### ⚙️カスタマイズ
環境変数で以下を変更することができます。

#### HUGO_AD_HTML_ROUGE_ARGS
シンタックスハイライトの引数。

`rouge-style`にて、テーマを選択できます。

Hugo では、テーマ自体にシンタックスハイライトが組み込まれていることが多いため、
不要な場合は値を空にしてください。

デフォルト値
```bash
-a source-highlighter=rouge -a rouge-css=style -a rouge-style=molokai
```

構文解析だけしたい場合
```bash
-a source-highlighter=rouge
```

#### HUGO_AD_HTML_BASE_DIR
Webサイトのベースディレクトリ。

ドメイン直下でない場合はそのディレクトリを指定してください。

> https://someuser.github.io/someRepos/

上記URLの場合、 `someRepos/` 部分を設定してください。

デフォルト値
```bash
(なし)
```


#### HUGO_AD_HTML_IMG_OUT_DIR
HTML 作成時の画像生成ディレクトリ（static配下を指定）

デフォルト値
```bash
images/generated
```


## PDFの生成（AsciiDoctor-PDF）について
### 🔧使い方
VS Code + Remote Containers でコンテナとして開いた後で、以下のコマンドを実行してください。
``` bash
asciidoctor-pdf (PDF化対象のAsciiDocファイル)
```

### ⚙️カスタマイズ
環境変数で以下を変更することができます。

#### HUGO_AD_PDF_IMG_DIR
PDF 作成時の画像パス

デフォルト値
```bash
static/images
```

#### HUGO_AD_PDF_IMG_OUT_DIR
PDF 作成時の画像生成先パス

デフォルト値
```bash
static/images/generated
```

#### HUGO_AD_PDF_THEME
PDF 作成時のテーマファイル。

```bash
default-with-fallback-font
```

#### HUGO_AD_PDF_ROUGE_STYLE
PDF 作成時の rouge （シンタックスハイライト）のスタイル

デフォルト値
```bash
molokai
```

スタイルを github に変更
```bash
github
```

#### HUGO_AD_PDF_DEFAULT_FONT
PDF 作成時のフォントファミリー

デフォルト値
```bash
M+ 1p Fallback
```

フォントファミリーをRobotoに変更
```bash
Roboto Mono
```
