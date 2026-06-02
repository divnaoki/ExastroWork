# Movement: set_vm_cpu（vCPU数設定）

| 登録項目 | 値 |
|----------|----|
| Movement名 | set_vm_cpu |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/set_vm_cpu/` |
| 所属Conductor | A「仮想マシン構築」(2番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- `Set-VMProcessor -Count {{ item.cpu_count }}`。VMは停止中に設定。
- 冪等性: 現ProcessorCountと要求値が異なる場合のみ適用。前後取得＋assert。
