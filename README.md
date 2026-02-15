# macproxy

macOS 系統代理管理工具，一鍵切換 HTTP/HTTPS/SOCKS 代理，適用於 ClashX / Clash Verge / Surge 等代理軟體。

## 特性

- 自動偵測活躍網路介面（無需手動指定 Wi-Fi 或 Ethernet）
- 同時設定 HTTP、HTTPS、SOCKS 代理
- 支援自訂代理位址，設定持久化儲存
- 自動產生環境變數檔案供終端使用

## 安裝

將 `macproxy` 複製到 PATH 中的任意位置：

```bash
cp macproxy /usr/local/bin/
chmod +x /usr/local/bin/macproxy
```

## 用法

```bash
macproxy on                          # 啟用系統代理
macproxy off                         # 關閉系統代理
macproxy status                      # 查看代理狀態
macproxy config                      # 查看當前設定
macproxy config <host> [port]        # 設定代理位址與連接埠
macproxy config <host>:<port>        # 設定代理位址與連接埠
```

### 範例

```bash
# 設定代理伺服器位址
macproxy config 192.168.1.100 7890

# 也支援 host:port 格式
macproxy config 192.168.1.100:1080

# 啟用代理
macproxy on

# 讓當前終端也走代理
source ~/.macproxy_env

# 查看狀態
macproxy status

# 關閉代理
macproxy off
```

## 設定檔

| 檔案 | 說明 |
|------|------|
| `~/.macproxy.conf` | 代理位址設定（由 `macproxy config` 管理） |
| `~/.macproxy_env` | 環境變數檔案（由 `macproxy on` 產生，`macproxy off` 刪除） |
