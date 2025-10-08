# 解析演算法（ALGORITHM）

## 資料抽取
- **Declared VLANs**：
  - `The VLANs include ...` 連行解析 → 單值與範圍
  - `vlan a to b` / `vlan a-b` / `vlan n`
- **名稱/描述**：`vlan N` 區塊內 `name` 或 `description`
- **SVI**：
  - `interface Vlan-interfaceX` 區塊的 `ip address X Y`
  - `display interface brief` 的 `VlanX ... IP`
- **Access(PVID/Untagged)**：
  - `port access/default vlan N`、`port hybrid pvid vlan N`
  - `port hybrid untagged vlan ...`
  - 橋接簡表的 **PVID**（且介面 Type ≠ T）
- **Trunk/Hybrid Tagged**：
  - `port trunk permit vlan ...`（含單值/逗號/範圍/`to`）
  - `undo port trunk permit vlan ...`（作為**排除**）
  - `port hybrid tagged vlan ...`
  - 遇到 `permit vlan all` 或 `permit vlan 2 to 4094` → 視為 **ALL trunk**

## 分類與著色
1. **In-Use（綠/白）**：若某 VLAN 在任何裝置上被偵測到 **SVI** 或 **Access(PVID/Untagged)** → 列為在用：  
   - 有名稱/描述 → **綠（Described）**
   - 沒有名稱/描述 → **白（In_Use）**
2. **Transit-only/Verify（黃）**：若某 VLAN **僅**出現在 Trunk/Hybrid（顯式 tagged 或存在 ALL trunk）但**沒有** SVI/Access → 合併為**連續範圍**並標 **黃**。
3. **Delete Candidate（紅）**：若某 VLAN 僅出現在宣告清單，且：  
   - 全網**沒有** ALL trunk，且
   - 在任何裝置上**未**出現 SVI/Access/顯式 trunk  
   → 合併為範圍並標 **紅**。

## 群組化
- 單點集合會先轉為**區間**（連續值合併）。
- 使用 `subtract_points()` 從宣告區間扣除「已使用」與「顯式 trunk」點。
