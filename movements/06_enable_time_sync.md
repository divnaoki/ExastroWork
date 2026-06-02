# Movement: enable_time_sync（時刻同期統合サービス有効化）

| 登録項目 | 値 |
|----------|----|
| Movement名 | enable_time_sync |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/enable_time_sync/` |
| 所属Conductor | A「仮想マシン構築」(6番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- `Enable-VMIntegrationService -Name 'Time Synchronization'`。全VM対象。Hyper-Vホスト時刻にゲストを同期。
- 冪等性: 現Enabled状態が無効の場合のみ有効化。前後取得＋assert。
