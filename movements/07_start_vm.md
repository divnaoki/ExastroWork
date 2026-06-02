# Movement: start_vm（VM起動）

| 登録項目 | 値 |
|----------|----|
| Movement名 | start_vm |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/start_vm/` |
| 所属Conductor | A「仮想マシン構築」(7番目・最終) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- `Start-VM`。全HW構成完了後に起動し、`until` で State=Running を待機（retries:30 / delay:10）。
- 冪等性: Running でない場合のみ起動。前後取得＋assert。
