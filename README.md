# Zenn CLI

Zennの記事・本を管理するリポジトリです。

## コマンド一覧

| コマンド | 説明 |
|---------|------|
| `npm run preview` | ローカルでプレビュー (http://localhost:8000) |
| `npm run new:article` | 新しい記事を作成 |
| `npm run new:book` | 新しい本を作成 |
| `npm run help` | ヘルプを表示 |

## よく使うオプション

### 記事作成時のオプション

```bash
# slugを指定して作成
npm run new:article -- --slug my-article-slug

# 絵文字とタイトルも指定
npm run new:article -- --slug my-article --title "記事タイトル" --emoji "🎉"
```

### プレビュー時のオプション

```bash
# ポートを指定
npm run preview -- --port 3000
```

## ディレクトリ構成

```
.
├── articles/     # 記事を格納
├── books/        # 本を格納
└── images/       # 画像を格納
```

## 参考リンク

- [Zenn CLIの使い方](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [Zenn記事の書き方](https://zenn.dev/zenn/articles/markdown-guide)
