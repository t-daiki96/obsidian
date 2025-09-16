# Obsidian × Claude Code やさしいローカル接続ガイド

**元記事:** https://zenn.dev/kalubi/articles/b696138474c488
**著者:** 上かるび
**公開日:** 2025/08/18

## はじめに

普段ゆる〜くデイリーノート程度にObsidianを使っているのですが、Claude Codeと連携できると知り、早速試しました（無事、接続・連携できました✌️）

主に参考にさせていただいた記事↓  
[https://zenn.dev/oikon/articles/obsidian-claude-code](https://zenn.dev/oikon/articles/obsidian-claude-code)

しかし肝心の**接続方法部分があまり詳しく書かれているものがなかった**ので、自分が詰まったポイントなどを紹介しながらできるだけわかりやすく接続方法をまとめました。

**注意事項:**
- 筆者のPCは**Mac（Apple Silicon）** 環境です。
- Obsidian Desktop, Claude Code はセットアップ済みとします。
  - [Obsidian のインストール](https://help.obsidian.md/install)
  - [Claude Code のインストール](https://docs.anthropic.com/ja/docs/claude-code/overview)
- この記事は、AIが書いた下書きを筆者が推敲したものです。

## ステップ1: Obsidian 側の準備

### コミュニティプラグインを有効化

初めてObsidianを使う場合、最初はコミュニティプラグインというサードパーティーのプラグインが使えない状態なので、有効化します。

### 1-1. Local REST API をインストール

1. 有効化したら、閲覧ボタンから検索画面へ
2. 「Local REST API」で検索 → インストール → 有効化
3. 設定画面で **API Key** が表示される（このAPI Keyは後で使います）

### 1-2. MCP Tools をインストール

1. 同じくコミュニティプラグインから **MCP Tools** をインストール
2. そのまま「オプション」ボタンをクリック or プラグイン画面から「**Install server**」をクリック

これで Vault（ノートを保存する金庫のようなもの）内に `mcp-server` バイナリ（実行ファイル）が配置されます。

## ステップ2: パスをコピーする

### Finder でのやり方

1. MCP Tools の画面で「Server install folder」をクリック
2. 開いた Finder の `bin` を右クリックでパスをどこかにメモっておく

**重要な注意点:**
「Server install folder」をクリックすると `bin/` フォルダが開きますが、MCP Serverがインストールされているはずなのに私の場合はここに **`mcp-server` が表示されませんでした。**（そういうものなのかも？or 隠しファイル？）

なので、`bin`フォルダのパスをコピーしたら、最後に手動で `/mcp-server` を追記してください。

例：  
`/Users/jookalubi/Documents/Obsidian Vault/.obsidian/plugins/mcp-tools/bin/mcp-server`

**スペースを含むパスの注意:**
パスの中にスペースがあるフォルダ名（例: `Obsidian Vault`）の場合、ターミナルに貼り付けるときには必ず `" "` で囲みます。

（私の場合の例）  
`"/Users/jookalubi/Documents/Obsidian Vault/.obsidian/plugins/mcp-tools/bin/mcp-server"`

## ステップ3: Claude Code に登録する

### 3-1. CLI で追加する方法

ターミナルで以下を実行します：

```bash
# パスはステップ2でコピーしたもの
claude mcp add obsidian-mcp-tools -s user -- "/path/to/vault/.obsidian/plugins/mcp-tools/bin/mcp-server" \
  --env OBSIDIAN_API_KEY=YOUR_API_KEY
```

- `-s user` → 全プロジェクトで共通利用
- `YOUR_API_KEY` → 1-1. Local REST API をインストール で控えたキー

### 3-2. 設定ファイルに追記する方法

`~/.claude.json`（ユーザーごとの設定ファイル）に以下を追記：

```json
{
  "mcpServers": {
    "obsidian-mcp-tools": {
      "command": "/Users/username/Documents/Obsidian Vault/.obsidian/plugins/mcp-tools/bin/mcp-server",
      "args": [],
      "env": {
        "OBSIDIAN_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

**Tip:** `~/.claude.json`を開くコマンド

```bash
# エディタ（Cursorなど）で開く
code ~/.claude.json
```

## ステップ4: 接続確認

ターミナルで以下を叩きます。

```bash
claude mcp list
```

接続が成功している場合、以下のように表示されます。

```
obsidian-mcp-tools: ... - ✓ Connected
```

## ステップ5: 最小テスト

連携確認ができたら、Claude に指示をして動作確認します。

```bash
Obsidianにあるノート一覧を取得して
```

## トラブルシューティング（つまづきポイント）

- **「Failed to connect」と出る**  
  → API Key が渡っていない可能性。`~/.claude.json` を最小構成で修正し、`"env": { "OBSIDIAN_API_KEY": "..." }` を明示します。

私の場合も、最初に参考サイトを元に接続コマンドだけ叩いただけだと失敗していて、`claude.json`に手打ちで設定をしてから再度試したらうまくいきました。

- **バイナリが実行できない**  
  → 実行権限を確認

```bash
ls -la mcp-server
```

権限が付いていない場合は追加：

```bash
chmod +x mcp-server
```

## おわりに

この連携によってObsidianのノートを自動でカテゴリに振り分けることが可能になるので、Webの記事やXのポストなどを雑にObsidianにクリッピングしておいて、いい感じにカテゴリー分けさせたり紐づけたりして体系的に知識をインプットできるようにしていきたいと考えています。

良い感じの運用方法などわかってきたらまた記事を書きたいと思います。

## 参考文献

https://zenn.dev/oikon/articles/obsidian-claude-code