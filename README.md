# あざとい後輩女子にJavaScriptを教えて欲しいと言われたんだけど (kouhai-js Novel)

[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://python.org)
[![MkDocs](https://img.shields.io/badge/mkdocs-material-green.svg)](https://squidfunk.github.io/mkdocs-material/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

[本文はこちら](https://kmizu.github.io/kouhai-js/)

## キャッチフレーズ

ひょっとしたらJavaScriptがわかるかも？

## 📖 作品概要

嵐山隆弘（あらしやまたかひろ）はプログラミング部に所属する高校二年生。
ある日、部内で新作ゲームを作っていたところ、「JavaScriptを教えてください！」と
いう後輩にして幼馴染の女子、河内美久が現れたのだった。
「プログラミングとか食いっぱぐれなさそうとかいう理由なら挫折するぞ？」
「私は真剣にJavaScriptをやりたいんです！」
熱意に絆された隆弘は美久に渋々プログラミングを教える羽目に。
これは、プログラミングを出汁にして二人の高校生が仲を深め合う、ただそれだけのラブコメである。

- **舞台**: 京都市内の中高一貫高校（進学校）
- **主人公**: 嵐山隆弘（あらしやまたかひろ）
- **ヒロイン**:河内美久（かわちみく）
- **ジャンル**: 学園ラブコメ+プログラミングもちょっとわかるかも？なエンタメ小説
- **構成**: 第1巻30話完結（約10万文字）

## 🎯 コンセプト

幼馴染で、方やプログラミングが大好きな理系男子、かたや文学や哲学が大好きな文系オタク女子。先輩後輩の間柄で幼馴染でもある二人が、プログラミングを通じて「あと一歩」の距離を縮めていく物語。メインがラブコメでありながら、プログラミング、あるいは計算機科学の入口も読者が体験できる欲張りなお話。舞台は京都市の中心部である中京区。京都で育った二人が、ときにプログラミングについて語り、ときに哲学について語ったり、あるいはオタクなお話をしたりする、そんなお話。イチャイチャもたっぷりあるよ！

## 🚀 クイックスタート

### 1. インストール

```bash
# リポジトリをクローン
git clone https://github.com/kmizu/kouhai-js.git
cd kouhai-js

# 依存関係をインストール
pip install -e .

# または開発環境セットアップ
pip install -e ".[dev]"
```

### 2. サイトプレビュー

```bash
# 開発サーバー起動
kouhai-js-serve

# または
mkdocs serve
```

ブラウザで http://localhost:8000 にアクセスして小説サイトをプレビューできます。

### 3. サイトビルド

```bash
# 静的サイト生成
kouhai-js-build

# または
mkdocs build
```

## 📁 プロジェクト構成

```
kouhai-js/
├── docs/                   # 小説コンテンツ
│   ├── index.md            # トップページ
│   └── volume1/            # 第1巻
│       ├── chapter01.md    # 第1話
│       ├── chapter02.md    # 第2話
│       └── ...             # 第30話まで
├── src/kouhai-js/        # Pythonパッケージ
│   ├── __init__.py         # パッケージ初期化
│   ├── cli.py              # CLIコマンド
│   └── utils.py            # ユーティリティ
├── tests/                  # テストコード
├── mkdocs.yml              # MkDocs設定
├── pyproject.toml          # Python設定
└── requirements.txt        # 依存関係
```

## 🛠️ CLIコマンド

プロジェクトには専用のCLIツールが付属しています：

```bash
# プロジェクト状態確認
kouhai-js-serve status

# 開発サーバー起動
kouhai-js-serve --host 0.0.0.0 --port 8080

# サイトビルド
kouhai-js-build --clean

# GitHub Pagesデプロイ
kouhai-js-deploy
```

## 📊 進捗状況

- ✅ **第1巻完結** (30話)
- ✅ **総文字数**: 約90,000文字
- ✅ **MkDocsサイト**: 構築済み
- ✅ **Pythonパッケージ**: 機能完備

### 章構成

| 章 | 話数 | テーマ | 状況 |
|---|-----|-------|------|
| 第1章 | 1-5話 | 春の始まり | ✅ 完成 |
| 第2章 | 6-10話 | 日常の中で | ✅ 完成 |
| 第3章 | 11-15話 | 気づき始める想い | ✅ 完成 |
| 第4章 | 16-20話 | 文化祭 | ✅ 完成 |
| 第5章 | 21-25話 | 夏の終わり | ✅ 完成 |
| 第6章 | 26-30話 | 心を繋ぐ | ✅ 完成 |

## 🎨 特徴

### 文体・スタイル
- 一人称（拓海視点）での心理描写重視
- 久野真一作品の文体を忠実に再現
- 温かみのある日常描写

### 技術スタック
- **MkDocs**: 静的サイト生成
- **Material Design**: モダンなテーマ
- **Python**: CLIツールとユーティリティ
- **GitHub Pages**: デプロイ対応

### 地域設定
- 京都市中京区の実在地域
- 錦市場、新京極、鴨川などの名所
- 桜月堂（和菓子店）などの詳細設定

## 📚 技術仕様

### 文字数設定
- **1話**: 約3,000文字
- **1巻**: 約90,000文字（30話）
- **カクヨム投稿**: 対応可能

### 品質保証
- 自動テスト完備
- 文字数チェック機能
- 章構成検証機能

## 🔧 開発

### テスト実行

```bash
# 全テスト実行
pytest

# カバレッジ付き
pytest --cov=src/kouhai-js

# 特定テスト
pytest tests/test_utils.py
```

### コード品質

```bash
# フォーマット
black src/ tests/

# インポートソート
isort src/ tests/

# 型チェック
mypy src/kouhai-js

# リント
flake8 src/ tests/
```

## 📝 ライセンス

MIT License - 詳細は [LICENSE](LICENSE) ファイルを参照

## 🤝 貢献

プルリクエストやイシューは大歓迎です！

1. このリポジトリをフォーク
2. フィーチャーブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

## 📞 サポート

- Issues: [GitHub Issues](https://github.com/kmizu/kouhai-js/issues)
- Documentation: [プロジェクトサイト](https://kmizu.github.io/kouhai-js/)
