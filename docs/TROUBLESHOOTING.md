# 疑難排除（TROUBLESHOOTING）

- **輸出幾乎都是 Yellow 範圍？**  
  代表輸入檔多為 L2 transit（大量 trunk all/範圍宣告），而缺少 SVI 與 access PVID 證據。請補上 core/aggregation 的 `display interface brief` 或完整 running-config。
- **VLAN 1 是否算 tagged？**  
  若出現 `undo port trunk permit vlan 1`，該介面視為**排除 VLAN 1**，不算 tagged。
- **裝置角色判定不準？**  
  使用 `--core-names` 與 `--agg-prefix` 或 `--role-map` 明確指定。
