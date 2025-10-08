# Excel 工作表說明（OUTPUT_SHEETS）

## Per_VLAN_Global(Clarity)
- `VLAN_ID`：單值或範圍（如 `1041-1044`）
- `Color`：Green / White / Yellow / Red
- `Usage_Category`：Described / In_Use / Verify / Delete_Candidate
- `Description`：VLAN 名稱或原因說明
- `Devices_*`：覆蓋裝置統計（含角色分類）
- `Devices_with_*`：列出實際涉及該 VLAN 的裝置
- `SVI_IPs`：所有 SVI 的 IP 列表

## Per_Device_Summary
- `VLANs_Defined`：該裝置涉及的 VLAN 總數（SVI/Access/顯式 trunk）
- `SVI_Count`：SVI 介面數量
- `Tagged_Trunk_VLANs(Count)`：若存在 **ALL trunk** → `ALL`；若顯式列表很大（>2000）也視為 `ALL`。

## Per_Device_VLAN_Detail
- `SVI?` / `SVI_IPs`
- `Tagged on Trunk/Hybrid?`
- `VLAN_Name`

## VLAN x Device Matrix
- 每列為一個 **在用 VLAN**
- 每欄為 `裝置 [角色]`
- 值為 `S` / `A` / `T` 的組合（如 `S+A+T`）
