# Movement: configure_vm_network（仮想スイッチ接続）

| 登録項目 | 値 |
|----------|----|
| Movement名 | configure_vm_network |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/configure_vm_network/` |
| 所属Conductor | A「仮想マシン構築」(4番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- `Connect-VMNetworkAdapter -SwitchName {{ item.switch_name }}`、`item.vlan_id` 指定時は `Set-VMNetworkAdapterVlan`。
- 冪等性: 現接続先スイッチ・VLANと要求値が異なる場合のみ適用。前後取得＋assert。
