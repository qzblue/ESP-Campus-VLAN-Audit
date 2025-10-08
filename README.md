# ESP-VLAN-Audit-Tool

> H3C 組態/日誌解析 → 產出 **CSW 模板**風格的 VLAN 稽核 Excel（含**綠/白/黃/紅**顏色標記與群組化）。

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](#license)
[![python](https://img.shields.io/badge/python-3.9%2B-brightgreen)](#requirements)
[![status](https://img.shields.io/badge/status-production-ready)](#)

---

## ✨ 功能特點
- 解析 H3C 匯出（`.cfg/.log/.txt`）的：
  - `display vlan` / `The VLANs include ...`
  - `vlan` 區塊中的 `name/description`、`vlan a-b` / `a to b`
  - `interface Vlan-interfaceX` 與 `display interface brief`（SVI IP）
  - 實體介面 `port link-type trunk/hybrid/access`、`permit vlan ...`、`undo permit vlan ...`、`hybrid tagged/untagged`、`access/default vlan`、`hybrid pvid`
  - 橋接簡表（**PVID** 與介面 **Type A/H/T**）
- 匯總出：
  - **Per_VLAN_Global(Clarity)**（群組化的 VLAN 範圍 + 顏色：綠/白/黃/紅）
  - **Per_Device_Summary**、**Per_Device_VLAN_Detail**、**VLAN x Device Matrix**
- 完整遵循你在 CSW 稽核中採用的規則（含 `permit vlan 2 to 4094` 視為 **ALL trunk**，`undo ... vlan 1` 代表**排除** VLAN 1）。

---

## 📦 安裝 & 需求 <a id="requirements"></a>

```bash
pip install -r requirements.txt
# 或最小安裝：
pip install pandas openpyxl
```

- Python 3.9+
- 僅本地檔案解析，不需網路/設備連線。

---

## 🚀 快速開始

```bash
python h3c_vlan_audit.py \\
  --input /path/to/h3c/logs \\
  --output ESP_CSW03_VLAN_Audit_FINAL_GROUPED_COLOR_2025-10-08.xlsx \\
  --core-names espcsw03 \\
  --agg-prefix espac
```

更多用法、參數與範例，見 **[docs/USAGE.md](docs/USAGE.md)**。

---

## 🧠 顏色與判定規則（重點）
| 顏色 | 類別 | 判定 | 含義 |
|---|---|---|---|
| 綠（Green） | Described | VLAN 在用（有 **SVI** 或 **Access/PVID**），且有 `name/description` | 已定義且在用 |
| 白（White） | In_Use | VLAN 在用，但沒有名稱/描述 | 在用但未命名 |
| 黃（Yellow） | Verify | 僅見於 **Trunk/Hybrid**（顯式 `permit` 或存在 **ALL trunk**），無 SVI/Access；以**連續範圍**群組 | 需人工確認是否僅轉送 |
| 紅（Red） | Delete_Candidate | 僅在宣告清單出現，**沒有** SVI/Access/顯式 trunk，且全網**沒有** ALL trunk | 刪除候選 |

完整演算法細節請見 **[docs/ALGORITHM.md](docs/ALGORITHM.md)** 與 **[docs/OUTPUT_SHEETS.md](docs/OUTPUT_SHEETS.md)**。

---

## 📁 專案結構
```
esp-vlan-audit/
├─ h3c_vlan_audit.py            # 主程式（可直接運行）
├─ README.md
├─ CONTRIBUTING.md
├─ LICENSE
├─ docs/
│  ├─ USAGE.md
│  ├─ ALGORITHM.md
│  ├─ OUTPUT_SHEETS.md
│  └─ TROUBLESHOOTING.md
└─ .github/
   └─ workflows/
      └─ ci.yml                 # 基本 CI：安裝依賴+語法檢查
```

---

## 🤝 貢獻
歡迎 PR / Issue！請先閱讀 **[CONTRIBUTING.md](CONTRIBUTING.md)**。

---

## 🔒 隱私 / 安全
- 本工具僅處理**靜態文本檔**；不會向外發送任何資料。
- 若在公開 Repo：請確保已**匿名化** IP、主機名與敏感描述。

---

## 📜 授權 <a id="license"></a>
本專案採用 **MIT License**，詳見 [LICENSE](LICENSE)。
