# 使用說明（USAGE）

> 以 H3C 組態/日誌為輸入，輸出符合 CSW 稽核格式的 Excel。

## 基本用法
```bash
python h3c_vlan_audit.py \\
  --input /path/to/logs \\
  --output ESP_CSW03_VLAN_Audit_FINAL_GROUPED_COLOR.xlsx
```

### 參數
- `--input`：放置 `.cfg/.log/.txt` 的資料夾（**必要**）。
- `--output`：輸出 Excel 檔名（**必要**）。
- `--core-names`：把指定裝置名視為 **Core**（可多個）。例：`--core-names espcsw03 espcsw01`。
- `--agg-prefix`：裝置名包含該字串者視為 **Aggregation**。預設：`espac`。
- `--role-map`：自訂裝置角色 JSON 檔（`{"dev":"Core"}`）。

## 範例
```bash
# 僅以 CSW03 為 Core，所有 sysname 含 espac 視為 Aggregation
python h3c_vlan_audit.py \\
  --input ./samples/csw03_scope \\
  --output ESP_CSW03_VLAN_Audit_FINAL_GROUPED_COLOR.xlsx \\
  --core-names espcsw03 \\
  --agg-prefix espac

# 指定角色對照檔
python h3c_vlan_audit.py \\
  --input ./logs \\
  --output audit.xlsx \\
  --role-map ./roles.json
```

## 解析到的關鍵輸出
- `Per_VLAN_Global(Clarity)`：群組化 + 顏色（綠/白/黃/紅）
- `Per_Device_Summary`：每裝置統計與 `Tagged_Trunk_VLANs(Count)`（遇 **ALL trunk** 顯示 `ALL`）
- `Per_Device_VLAN_Detail`：每裝置/每 VLAN 的 `SVI?`、`Tagged on Trunk/Hybrid?`、`VLAN_Name`
- `VLAN x Device Matrix`：S=SVI、A=Access、T=Trunk

## 建議的輸入來源（可任選）
- `display vlan`、`display interface brief`
- 完整 running-config（含 `vlan`、`interface ...` 區塊）
- 橋接介面簡表（Type/PVID）
