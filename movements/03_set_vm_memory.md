# Movement: set_vm_memory（メモリ設定）

| 登録項目 | 値 |
|----------|----|
| Movement名 | set_vm_memory |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/set_vm_memory/` |
| 所属Conductor | A「仮想マシン構築」(3番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- `Set-VMMemory`（起動メモリ／動的メモリ）。`item.memory_dynamic` で静的/動的を分岐。
- 冪等性: 現StartupBytes・DynamicMemoryEnabledと要求値が異なる場合のみ適用。前後取得＋assert。
