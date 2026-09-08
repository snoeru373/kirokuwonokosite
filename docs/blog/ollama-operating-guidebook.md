---
created: 2026-07-09 03:00(Asia/Tokyo)
updated: 2026-07-09 03:00(Asia/Tokyo)
type: concept
status: active
tags:
  - Ollama
  - ローカルLLM
  - MLX
  - ROCm
  - GPUバックエンド
  - 量子化
  - 運用ガイド
title: "Ollama運用ガイドブック — MLX/ROCm/Q4量子化最適設定集"
description: "Brain3 Vaultで安定したローカルLLM推論を実現するための運用指針。MLXエンジン最新動向、GPU互換性、量子化フォーマット完全比較、バックエンド選択、ベストプラクティスを含む。"
related_sources:
  - "[[Ollama最新情報2026 — MLXエンジン更新、GPU互換性Calculator]]"
  - "[[GPUバックエンドを変えたら、temp 0でもAIの答えが変わった — CUDA vs Vulkan、18本勝負で一致ゼロ【検証第3回】(更新)｜zephel01.md]]"
  - "[[2026年中のローカルLLM推論プラットフォーム比較 — Ollama、vLLM、llama.cpp]]"
  - "[[2026年中のローカルLLMベンチ]]"
---

# Ollama運用ガイドブック — MLX/ROCm/Q4量子化最適設定集

## 目的

Brain3 Vault（GMKtec EVO-X2 + RTX5090 32GB）で安定したローカルLLM推論を実現するための運用指針。MLXエンジン最新動向、GPU互換性Calculator活用、量子化フォーマット完全比較、バックエンド選択基準を体系化する。

---

## 1. Ollamaの現状と最新の更新（2026年6月以降）

### MLXエンジン最高パフォーマンス（Apple Silicon向け）

- Apple Silicon向けMLXエンジンを最新アップデートで**最大の性能**を達成
- モデル出力品質が向上
- Metal GPUオフロードをマルチモーダルモデルにサポート拡大
- Brain3環境には直接適用不可だが、ユーザーのMacデバイス併用時価値が高い

### GPU互換性Calculator新機能

- 最新のApple GPUモデルをサポート
- RAMとCPU計算アルゴリズムを更新
- **AMD GPUのVRAM相当するNVIDIA GPU参照可能** → マシンアップグレード判断に活用可

### NVIDIA GPUサポート要件

| 項目 | 要件 |
|---:|---|
| Compute Capability | **5.0+** |
| ドライババージョン | **550以降** |
| Brain3環境 | RTX5090 (compute capability **9.0+**) → **完全サポート** ✓ |

### AMD GPUサポート（ROCm）

- ROCmプラットフォームを介してOllamaがAMD GPUsをサポート
- AMD Radeon 32GB+ GPUでローカルAIワークロード可能
- BIOSアップデートからAMDファームウェア更新で性能アップ
- Windows上でもCustom Buildガイド公開（680M/780M/890M対応、ROCm不要ケースあり）

---

## 2. モデル選択とVRAM要件

### Q4量子化のVRAM目安

| モデルサイズ | 必要VRAM(Q4_K_M) | 代表モデル | Brain3環境での位置づけ |
|---:|---|:---|:---|
| 7B | 4-5GB | Llama 4 Scout, Qwen 3.5 7B | ✅ 余裕あり、95+ tok/s |
| 14B | 8-10GB | Qwen 3.5 14B, Mistral Small 3 | ✅ 余裕あり |
| 32B | 16-20GB | Qwen 3.6 32B, DeepSeek V3 | ✅ RTX5090快適、~45 tok/s |
| **70B** | **35-40GB** | Llama 3.3 70B, Mixtral 8x22B | ⚠️ **VRAM上限ぎりぎり**（RTX5090 32GBでは部分的OFFLOAD） |
| 405B | 220+GB | Llama 3.1 405B | ❌ マルチGPU必要 |

### Brain3 Vaultの位置づけ: **「黄金スペック」**

- GMKtec EVO-X2 (96GB RAM + RTX5090 32GB VRAM)
- Q4量子化で**~32B モデルをVRAM内に完結**させて快適運用可能
- 70Bモデルは部分的OFFLOADのため速度低下あり（~12 tok/s）

---

## 3. GPUバックエンド選択ガイド

### CUDA vs Vulkan の違い（zephel01検証第3回より）

| パラメータ | CUDA | Vulkan |
|---:|:---|:---|
| **出力再現性** | temp 0 + seed固定 → バイト単位一致 | 同じ条件でも全パターン不一致 |
| **生成速度** | ベースライン | ベースラインから -31% |
| **プロンプト処理** | ベースライン | ベースラインから最大2倍遅い |
| **ハルシネーション内容** | 「量子化で低下した精度を推論時に補正」 | 「llama-quantizeのオプション」と別の嘘 |

### 核心発見: temp 0 + seed固定でもバックエンド跨ぎ = 18/18件不一致

浮動小数点演算の順序依存性により、GPUバックエンドが変われば出力も変わる。ハルシネーションの「嘘の内容」さえ環境依存する。

### Brain3での推奨選択

- **デフォルト:** `CUDA`（最適化優先）
- **サブ/移植性:** Vulkan（Mac等で併用時）
- **重要判断:** 複数バックエンドで確認
- cron自動化には `--repeat-penalty` 必須追加

---

## 4. 量子化フォーマット完全比較表

| レベル | サイズ(70B) | 品質影響 | Brain3での推奨 |
|---:|---:|:---|:---|
| FP16 | ~140GB | ベースライン | ❌ VRAM超える |
| Q8_0 | ~70GB | ほぼ損失なし | ⚠️ RTX5090超える |
| Q6_K | ~54GB | 最小限の損失 | ⚠️ 余裕が必要 |
| **Q4_K_M** | **~40GB** | **✅ 品質/サイズの最適帯域** | **★ デフォルト推奨** |
| Q3_K_M | ~33GB | 目に見える低下 | △ 速度優先時 |
| Q2_K | ~25GB | 大きな低下 | ❌ 非推奨 |

### Q4_K_Mが最適な理由

70BモデルでVRAM内に収まりつつ、品質低下が最小限。RTX5090(32GB)では部分的OFFLOADとなるが、6~12 tok/sの運用可能速度を確保できる。

---

## 5. 最適パラメータ設定ベストプラクティス

### プロンプト・推論パラメータ

| パラメータ | 推奨値 | 用途 |
|---:|:---|:---|
| `--repeat-penalty` | **1.1-1.3** | 無限ループ防止（必須） |
| `--max-tokens` | コンテキストの80%程度 | 長時間無人運用保護 |
| `temperature` (温度) | 0 (決定論), 0.7 (創造性) | タスク別使い分け |
| `n-gpu-layers` | **-1** (全レイヤーGPU) | RTX5090完結時 |
| `num-ctx` | **4096** (デフォルト) / **8192+** (長文処理) | コンテキスト拡張 |

### ハルシネーション対策チートシート

1. temp=0 + seed固定 → 同一バックエンド内なら完璧再現
2. `--repeat-penalty` を必須設定に含める（vLLM検証でも1/3で再発）
3. **同じ質問→別バックエンド→答えが食い違い→両方疑う**
4. 重要判断時は複数バックエンド結果を比較

---

## 6. Platform選択: Ollama vs vLLM vs llama.cpp vs TensorRT-LLM

### Brain3 Vaultでの役割分担

| レベル | ツール | 用途 |
|---:|:---|:---|
| **プロトタイピング** | Ollama (CLI第一) | Cron自動化との親和性最高 |
| **本番サービング** | vLLM (PagedAttention) | 高スループット必要時に移行 |
| **カスタム/エッジ** | llama.cpp | ネイティブGGUF対応 |
| **NVIDIA専用最適化** | TensorRT-LLM | vLLM比最大2倍高速（RTX50シリーズ） |

### Brain3環境での推奨戦略

```
単一ユーザー用途 → Ollama (CLI第一 + OpenAI互換API)
    ↓ (スケール必要時)
vLLM → TensorRT-LLM (NVIDIA RTX5090で最大性能)
```

---

## 7. Health Check / デバッグコマンドコレクション

### Ollama常用コマンド

```bash
# モデル一覧
ollama list

# モデル削除
ollama rm <model-name>

# モデル情報確認
ollama show <model-name> --verbose

# サーバー状態確認
systemctl status ollama

# テスト推論（動作確認）
ollama run <model-name> "テスト: 1+1=?"

# リストのモデルをバックエンド確認
ollama list | grep cuda
```

### VRAM使用量確認

```bash
nvidia-smi
nvtop    # インストール必要: apt install nvtop
```

---

## 8. Brain3 Vaultへの示唆（運用方針）

| 項目 | 方針 |
|---:|---|
| **デフォルトバックエンド** | CUDA（最適化優先） |
| **量子化フォーマット** | Q4_K_M（品質/サイズ最適帯域） |
| **推奨モデル** | Qwen 3.6 32B Q4（VRAM内完結、~45 tok/s） |
| **cron追加設定** | `--repeat-penalty=1.1` 必須 |
| **並列推論** | vLLM/TensorRT-LLMでスケール |
| **AMD GPU併用時** | GPU互換性CalculatorでVRAM相当GPUを参照 |
| **Mac運用時** | MLXエンジン最新状態を注視（出力品質向上中） |

---

## 9. Loopバッドケース一覧と回避方法

| バッドケース | 原因 | 回避策 |
|---:|---|:---|
| 無限ループ | repeat-penalty未設定 | `--repeat-penalty=1.1` 必須追加 |
| temp0でも出力変化 CUDA→Vulkan | 浮動小数点演算順序依存性 | バックエンド固定 + seed固定 |
| ハルシネーションの環境依存 | VRAM不足による部分的OFFLOAD | GPU完結（`n-gpu-layers=-1`） |
| 長時間推論のメモリリーク | コンテキスト肥大 | `--max-tokens` 上限設定 |

---

## 更新履歴

| 日付 | 内容 |
|-----|---|
| 2026-07-09 | 初版作成。Ollama最新情報 + GPUバックエンド検証 + プラットフォーム比較から統合 |
