## このプロジェクトの動かし方（Contract + Backend + Frontend 連動）

### 環境変数による設定

フロントエンド（React）は `.env` ファイルでコントラクトアドレス・API URL を管理できます。

`app/.env` 例:

```
REACT_APP_CONTRACT_ADDRESS=0x5FbDB2315678afecb367f032d93F642f64180aa3
REACT_APP_API_URL=http://127.0.0.1:5000
```

（デプロイ時やアドレス変更時はここを書き換えてください）

このプロジェクトは以下 3 つを同時に起動して動かします。

- Contract: Hardhat ローカルチェーン + `VoiceKeyRecover` デプロイ
- Backend: Flask API（音声特徴量計算 + 証明生成）
- Frontend: React アプリ

### 0. 前提

- Rust / Cargo
- Python 3.10+ / Poetry
- Node.js / npm
- MetaMask（ブラウザ）

### 1. 初回セットアップ（ルートディレクトリ）

```bash
# Python 依存
poetry install

# Rust(Python 拡張)ビルド
poetry shell
cd voice_recovery_python
maturin develop
cd ..

# ZK proving 用ファイル生成（初回または設定変更時）
mkdir -p build
cargo run --release --bin voice_recovery -- GenParams --k 22 --params-path ./build/params
cargo run --release --bin voice_recovery -- GenKeys
cargo run --release --bin voice_recovery -- GenEvmVerifier

# 生成した Verifier を hardhat 側へ反映
cp ./build/Verifier.sol ./hardhat/contracts/Verifier.sol
```

### 2. Contract 起動（ターミナル A）

```bash
cd hardhat
npm install
npx hardhat node
```

別ターミナル（ターミナル B）でデプロイ:

```bash
cd hardhat
npx hardhat run scripts/deploy.ts --network localhost
```

- 出力された `VoiceKeyRecover deployed to: ...` を確認
- 必要なら `app/src/App.js` の `contractAddress` をデプロイ先アドレスに合わせる
- MetaMask は `http://127.0.0.1:8545` / Chain ID `1337` に接続

### 3. Backend 起動（ターミナル C）

```bash
cd backend
mkdir -p storage
poetry run python app.py
```

- API は `http://127.0.0.1:5000` で待受

### 4. Frontend 起動（ターミナル D）

```bash
cd app
npm install
npm start
```

- ブラウザで `http://localhost:3000`
- `app/src/App.js` の `apiUrl` は `http://127.0.0.1:5000` を利用

### 5. 動作確認フロー

1. Frontend で録音して `register`
2. 続けて録音して `recover`
3. Backend の `/api/feature-vector`, `/api/gen-proof` と Contract 呼び出しが通ることを確認

---

Follow this document to call rust from python
https://pyo3.rs/v0.18.3/getting_started

How to rebuild

```
poetry install
poetry shell
cd voice_recovery_python
maturin develop
```

Setup

GenParams -> GenKeys -> GenEvmVerifier

見るべきファイルはこちらを参考に

```
EvmProve {
        /// setup parameter file
        #[arg(short, long, default_value = "./build/params")]
        params_dir: String,
        /// circuit configure file
        #[arg(
            short,
            long,
            default_value = "./eth_voice_recovery/configs/test1_circuit.config"
        )]
        app_circuit_config: String,
        #[arg(
            short,
            long,
            default_value = "./eth_voice_recovery/configs/agg_circuit.config"
        )]
        agg_circuit_config: String,
        /// proving key file path
        #[arg(long, default_value = "./build/pks")]
        pk_dir: String,
        /// input file path
        #[arg(long, default_value = "./build/input.json")]
        input_path: String,
        /// proof file path
        #[arg(long, default_value = "./build/evm_proof.hex")]
        proof_path: String,
        /// public input file path
        #[arg(long, default_value = "./build/evm_public_input.json")]
        public_input_path: String,
    },
```

Public Inputの中身

```
pub struct DefaultVoiceRecoverCircuitPublicInput {
    commitment: String,
    commitment_hash: String,
    message: String,
    feature_hash: String,
    message_hash: String,
    // acc: String,
}
```

Secret Inputの中身

```
{
    "features": "0x52ad6993e8ed48b87023fa32cb416c49b4e0b87c2c63a8ea8e68818c776d9e7f8efc64a1f3b96e806ec2bc9fb4301ce7c9b47ac29ca143d25ca3b082b8f76c207dcaa671ca4df240d277ffde7d4d37887266e923cc51910039f485823dba94dc02da01bca68bbb7b79695b693341eca4bbd955714e6155d2eb641762a307c2c7e0c021fabb817da4f720f9f9",
    "errors": "0x02000401080080007000040030000110000000000090090010024000000c08008000000000040000000080002000000001c044001800000000080100440100000a00000000004000000000001298a400000049800004900080400000000000000031080110004800200801608410040020000000801000018212000022100000002401160380200080010000",
    "commitment": "0xb577b007bd3c08abfb74aa19c01395a00788c7862913953def64406050c7322ab5a393558c081b143a325c1de06856846f80704900df75be381216f53d7f6651d2557dae5c029b3cc0fbb671f53e11a1745466b4b1d4f39cf52c9e389bfe58796013ee4031816e839041cde2dc8d212ddf488834de1de7164e7f87b0e5ac1af210374da374ca572751247a8a",
    "message": "0xf39fd6e51aad88f6f4ce6ab8827279cfffb9226670997970c51812dc3a010c7d01b50e0d17dc79c8"
}
```
