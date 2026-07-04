# ============================================================
# Next.js (開発用) コンテナ
#   - 開発はソースを bind mount + ホットリロード
#   - 本番は multi-stage build（npm ci && npm run build）に差し替え推奨
# ============================================================
FROM node:22-bookworm

WORKDIR /app

# 依存(node_modules)はビルド時に入れず、
#   docker compose run --rm web npm install
# でボリュームへインストールする（scaffold手順を参照）。

EXPOSE 3000

# -H 0.0.0.0 でコンテナ外（ホスト）からアクセス可能にする
CMD ["npm", "run", "dev", "--", "-H", "0.0.0.0"]
