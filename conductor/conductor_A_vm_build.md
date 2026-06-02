# Conductor A「仮想マシン構築」フロー（ロールパッケージ: vm_build）

実行対象: Hyper-Vホスト（WinRM）。各Movementは `loop: VAR_vm` で全VMを処理。
失敗時方針: **即時中断**。HW設定は**逐次**。

```mermaid
flowchart TD
  S((開始)) --> A1["import_template_vm<br/>pkg: vm_build / 対象: 全VM<br/>Op: Hyper-V_VM構築_初回"]
  A1 --> A2["set_vm_cpu<br/>pkg: vm_build / 対象: 全VM"]
  A2 --> A3["set_vm_memory<br/>pkg: vm_build / 対象: 全VM"]
  A3 --> A4["configure_vm_network<br/>pkg: vm_build / 対象: 全VM"]
  A4 --> A5["configure_vm_firmware<br/>pkg: vm_build / 対象: 全VM"]
  A5 --> A6["enable_time_sync<br/>pkg: vm_build / 対象: 全VM"]
  A6 --> A7["start_vm<br/>pkg: vm_build / 対象: 全VM"]
  A7 --> E((終了))
```

## 設計根拠
- HW構成（CPU/メモリ/NW/ファームウェア）はVM**停止中**に行うため `start_vm` より前。
- 時刻同期（統合サービス）も起動前に有効化。
- 全7ロールはロールパッケージ **vm_build** に同梱。
