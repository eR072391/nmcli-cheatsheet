nmcliはNetworkManagerを制御するコマンドラインツール  

```
Usage: nmcli [OPTIONS] OBJECT { COMMAND | help }

OPTIONS
  -a, --ask                                ask for missing parameters
  -c, --colors auto|yes|no                 whether to use colors in output
  -e, --escape yes|no                      escape columns separators in values
  -f, --fields <field,...>|all|common      specify fields to output
  -g, --get-values <field,...>|all|common  shortcut for -m tabular -t -f
  -h, --help                               print this help
  -m, --mode tabular|multiline             output mode
  -o, --overview                           overview mode
  -p, --pretty                             pretty output
  -s, --show-secrets                       allow displaying passwords
  -t, --terse                              terse output
  -v, --version                            show program version
  -w, --wait <seconds>                     set timeout waiting for finishing operations

OBJECT
  g[eneral]       NetworkManager's general status and operations
  n[etworking]    overall networking control
  r[adio]         NetworkManager radio switches
  c[onnection]    NetworkManager's connections
  d[evice]        devices managed by NetworkManager
  a[gent]         NetworkManager secret agent or polkit agent
  m[onitor]       monitor NetworkManager changes
```

## 1. 一般的な情報の取得  

NetworkManagerの状態を確認    
`nmcli general status`  

---

デバイスの一覧を表示  
`nmcli device status`  

---

ネットワーク接続の一覧を表示  
`nmcli connection show`  

---

アクティブな接続のみ表示  
`nmcli connection show --active`  

---

## 2. ネットワークデバイスの管理  

デバイスの状態を確認  
`nmcli device show`  

---

デバイスの詳細情報を確認  
`nmcli device show <デバイス名>`  

---

デバイスの有効化  
`nmcli device connect <デバイス名>`  

---

デバイスの無効化  
`nmcli device disconnect <デバイス名>`  

## 3. ネットワーク接続の管理  

既存の接続を有効化  
`nmcli connection up <接続名>`  

---

既存の接続を無効化  
`nmcli connection down <接続名>`  

---

既存の接続を削除  
`nmcli connection delete <接続名>`  

---

既存の接続を編集（対話式）  
`nmcli connection edit <接続名>`  

---

既存の接続を変更（例:IPアドレスを変更）  
`nmcli connection modify <接続名> ipv4.addresses <IPアドレス/プレフィックス>`  


## 4. Wi-Fiの管理  

近くのWi-Fiネットワークをスキャン  
`nmcli device wifi list`  

---

Wi-Fiに接続  
`nmcli device wifi connect <SSID> password <パスワード>`  
or  
`nmcli device wifi connect <SSID> --ask`  

---

既存のWi-Fi接続を切断  
`nmcli device disconnect <Wi-Fiデバイス名>`  

---

Wi-Fi接続を作成（静的IP）  
`nmcli connection add type wifi ifname <Wi-Fiデバイス名> ssid <SSID> ipv4.addresses <IPアドレス/プレフィックス>`  

## 5. 有線接続の管理  

DHCPで有線接続を追加  
`nmcli connection add type ethernet ifname <インターフェイス名> autoconnect yes`  

---

静的IPで有線接続を追加  
`nmcli connection add type ethernet ifname <インターフェイス名> ipv4.addresses <IPアドレス/プレフィックス>`  


## 6. VPNの設定 

VPN接続を追加（OpenVPN）  
`nmcli connection add type vpn ifname <VPN名> vpn-type openvpn`  

---

VPN接続を有効化  
`nmcli connection up <VPN接続名>`  

---

VPN接続を無効化  
`nmcli connection down <VPN接続名>`  


## 7. ホットスポットの作成  

Wi-Fiホットスポットを作成  
`nmcli device wifi hotspot ifname <Wi-Fiデバイス名> ssid <SSID> password <パスワード>`  

---

Wi-Fiホットスポットを停止  
`nmcli connection down Hotspot`  


## 8. プロキシの設定  

プロキシの追加  
`nmcli connection modify <接続名> proxy.method manual`  
`nmcli connection modify <接続名> proxy.http "http://proxy.example.com:8080"`  
`nmcli connection modify <接続名> proxy.https "https://proxy.example.com:8080"`  

---

プロキシの無効化  
`nmcli connection modify <接続名> proxy.method none`  


## 9. IPv6の設定  

IPv6を有効化  
`nmcli connection modify <接続名> ipv6.method auto`  

---

IPv6を無効化  
`nmcli connection modify <接続名> ipv6.method ignore`  


## 10. DNSの設定  

静的DNSを追加  
`nmcli connection modify <接続名> ipv4.dns <DNSアドレス>`  

---

DNSをリセット（デフォルトに戻す）  
`nmcli connection modify <接続名> ipv4.ignore-auto-dns no`  


## 11. MACアドレスの変更  

ランダムMACアドレスを設定  
`nmcli connection modify <接続名> 802-11-wireless.mac-address-randomization yes`  

---

固定MACアドレスを設定  
`nmcli connection modify <接続名> 802-3-ethernet.cloned-mac-address <MACアドレス>`  


## 12. トラブルシューティング  

ログを表示  
`journalctl -u NetworkManager --since "1 hour ago"`  

---

NetworkManagerのリロード  
`systemctl restart NetworkManager`  

---

NetworkManagerの状態確認  
`systemctl status NetworkManager`  


END.
