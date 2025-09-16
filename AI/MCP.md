# MCP (Model Context Protocol)

## MCPとは
MCP (Model Context Protocol) は、AIアプリケーションがローカルおよびリモートのリソースに安全にアクセスするための標準プロトコル。Claude CodeなどのAIツールが外部システムと連携するために設計されている。

## MCPサーバーの役割

### 1. リソースへの安全なアクセス
- ファイルシステム
- データベース
- Web API
- その他の外部システム

### 2. 標準化されたインターフェース
- 統一されたAPIを通じてAIがリソースにアクセス
- プロトコルレベルでのセキュリティ制御
- 明確な権限管理

### 3. 拡張性
- カスタムサーバーの実装が可能
- 既存システムとの柔軟な連携
- モジュラー設計

## 主要なMCPサーバー

### Obsidian MCP Server
- **機能**: Obsidianノート管理システムとの連携
- **提供サービス**: ノートの読み書き、検索機能、ファイル管理、テンプレート実行

### File System MCP Server
- **機能**: ローカルファイルシステムへのアクセス
- **提供サービス**: ファイルの読み込み・書き込み、ディレクトリ操作、ファイル検索

### Database MCP Server
- **機能**: データベースとの連携
- **提供サービス**: SQL実行、データの読み取り・更新、スキーマ情報の取得

## MCPサーバーの設定

### Claude Code での設定方法
1. **CLI コマンドによる追加**
```bash
claude mcp add <server-name> -s user -- <server-path> --env <ENVIRONMENT_VARIABLES>
```

2. **設定ファイル (~/.claude.json) による管理**
```json
{
  "mcpServers": {
    "server-name": {
      "command": "/path/to/server/binary",
      "args": [],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

### 接続確認
```bash
claude mcp list
```

## トラブルシューティング

### よくある問題
1. **接続エラー**: 設定ファイルの確認、環境変数の検証
2. **認証エラー**: API キーの有効性確認、権限設定の見直し
3. **パフォーマンス問題**: リクエスト頻度の最適化、キャッシュ活用

## 参考リンク
- [MCPサーバー公式一覧](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#%EF%B8%8F-official-integrations)
- [[2025-09-16_Obsidian × Claude Code やさしいローカル接続ガイド]]