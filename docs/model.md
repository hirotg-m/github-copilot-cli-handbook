# GitHub Copilot CLI のモデルを確認・変更する

GitHub Copilot CLI では、対話セッションで使用する AI モデルを選択できます。利用できるモデルは、Copilot のプラン、組織のポリシー、CLI のバージョンによって異なるため、モデル名を固定した手順ではなく、CLI に表示される候補を基準に選びます。

## 現在使用しているモデルを確認する

対話セッションで `/context` を実行します。先頭行に、現在のモデル名と文脈ウィンドウの使用量が表示されます。

```text
/context
```

たとえば、次の表示では現在のモデルは `gpt-5.6-terra` です。

```text
gpt-5.6-terra · 35k/400k tokens (9%)
```

モデル選択画面を開いて、現在選択されているモデルを確認することもできます。

```text
/model
```

## 使用できるモデルを確認する

`/model` を実行すると、この環境で選択できるモデルの一覧が表示されます。

```text
/model
```

表示される候補だけが、現在のアカウント、プラン、組織ポリシーで使用できます。GitHub Copilot がサポートするモデルの全体一覧は、次の公式ドキュメントを参照してください。

- [GitHub Copilot でサポートされている AI モデル](https://docs.github.com/ja/copilot/reference/ai-models/supported-models)

## サポートモデルと料金

以下は、公式の料金表に掲載されているモデルと、100 万トークンあたりの料金です。料金は米ドルで、`Input` は入力、`Cached input` はキャッシュ済み入力、`Output` は出力を表します。実際に選択できるモデルは `/model` の一覧で確認してください。

長い文脈を扱えるモデルでは、入力トークン数がしきい値を超えると `Long context` の料金が適用されます。Anthropic モデルでは、キャッシュの書き込み料金も別途かかります。

### OpenAI

| モデル | 区分 | Input | Cached input | Output |
| --- | --- | ---: | ---: | ---: |
| GPT-5 mini | Default | $0.25 | $0.025 | $2.00 |
| GPT-5.3-Codex | Default | $1.75 | $0.175 | $14.00 |
| GPT-5.4 | Default（入力 272k 以下） | $2.50 | $0.25 | $15.00 |
| GPT-5.4 | Long context（入力 272k 超） | $5.00 | $0.50 | $22.50 |
| GPT-5.4 mini | Default | $0.75 | $0.075 | $4.50 |
| GPT-5.4 nano | Default | $0.20 | $0.02 | $1.25 |
| GPT-5.5 | Default（入力 272k 以下） | $5.00 | $0.50 | $30.00 |
| GPT-5.5 | Long context（入力 272k 超） | $10.00 | $1.00 | $45.00 |
| GPT-5.6 Luna | Default（入力 200k 以下） | $1.00 | $0.10 | $6.00 |
| GPT-5.6 Luna | Long context（入力 200k 超） | $2.00 | $0.20 | $9.00 |
| GPT-5.6 Sol | Default（入力 272k 以下） | $5.00 | $0.50 | $30.00 |
| GPT-5.6 Sol | Long context（入力 272k 超） | $10.00 | $1.00 | $45.00 |
| GPT-5.6 Terra | Default（入力 272k 以下） | $2.50 | $0.25 | $15.00 |
| GPT-5.6 Terra | Long context（入力 272k 超） | $5.00 | $0.50 | $22.50 |

### Anthropic

| モデル | Input | Cached input | Cache write | Output |
| --- | ---: | ---: | ---: | ---: |
| Claude Haiku 4.5 | $1.00 | $0.10 | $1.25 | $5.00 |
| Claude Sonnet 4.5 | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Sonnet 4.6 | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Sonnet 5 | $2.00 | $0.20 | $2.50 | $10.00 |
| Claude Opus 4.5 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.6 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.7 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.8 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.8（fast mode） | $10.00 | $1.00 | $12.50 | $50.00 |
| Claude Opus 5 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Fable 5 | $10.00 | $1.00 | $12.50 | $50.00 |

### Google

| モデル | 区分 | Input | Cached input | Output |
| --- | --- | ---: | ---: | ---: |
| Gemini 2.5 Pro | Default | $1.25 | $0.125 | $10.00 |
| Gemini 3 Flash | Default | $0.50 | $0.05 | $3.00 |
| Gemini 3.1 Pro | Default（入力 200k 以下） | $2.00 | $0.20 | $12.00 |
| Gemini 3.1 Pro | Long context（入力 200k 超） | $4.00 | $0.40 | $18.00 |
| Gemini 3.5 Flash | Default | $1.50 | $0.15 | $9.00 |
| Gemini 3.6 Flash | Default | $1.50 | $0.15 | $7.50 |

### GitHub、Microsoft、Moonshot AI

| モデル | Input | Cached input | Output |
| --- | ---: | ---: | ---: |
| Raptor mini | $0.25 | $0.025 | $2.00 |
| MAI-Code-1-Flash | $0.75 | $0.075 | $4.50 |
| Kimi K2.7 Code | $0.95 | $0.19 | $4.00 |

### 料金の読み方

料金は、選択したモデルと実際に消費した入力・出力・キャッシュトークンから計算されます。`/context` の使用量は現在の文脈量を示すもので、請求額そのものではありません。

プランに含まれる AI Credits の範囲内では、その枠から利用量が消費されます。枠を超えた利用は、上記の単価に基づき AI Credits として請求されます。1 AI Credit は $0.01 USD です。

モデル、料金、利用可能なモデルは変更される可能性があります。最新の情報は、必ず次の公式料金表で確認してください。

- [GitHub Copilot のモデルと料金](https://docs.github.com/ja/copilot/reference/copilot-billing/models-and-pricing)

## モデルを変更する

対話セッションで `/model` を実行し、一覧から使用したいモデルを選びます。選択後、このセッション以降の応答にそのモデルが使われます。

```text
/model
```

自動選択に戻す場合は、`auto` を指定します。自動選択では、Copilot がタスクに適した利用可能なモデルを選びます。

```text
/model auto
```

## 既定のモデルを設定する

`/model --repo` は現在のリポジトリの既定モデルを、`/model --local` はローカル環境の既定モデルを設定します。どちらも実行後に一覧からモデルを選びます。

```text
/model --repo
```

```text
/model --local
```

計画モードだけ別のモデルを使うには、`plan` または `--plan` を指定します。

```text
/model --plan
```

既定モデルの設定範囲を確認してから選びたい場合は、まず引数なしの `/model` を実行してください。共有リポジトリでは、リポジトリ設定を変更する前にチームの運用ルールも確認します。

## モデルが一覧にない

目的のモデルが `/model` に表示されない場合、そのモデルは現在の環境では使用できません。Copilot のプラン、組織管理者が設定した利用可能モデル、CLI の更新状況を確認してください。

```text
/update
```

組織で Copilot を利用している場合は、モデルの利用可否を組織管理者に確認します。

## 関連ガイド

- [GitHub Copilot CLI のセッションを管理する](./session.md)
- [GitHub Copilot CLI の使用](https://docs.github.com/ja/copilot/how-tos/copilot-cli/use-copilot-cli/overview)
