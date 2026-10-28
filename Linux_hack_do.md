# Linux 主機疑似遭入侵檢查

# Linux 伺服器遭駭必查的 17 個指令

## 一、登入與帳號調查

1. `who` — 查看目前登入的使用者與來源
2. `last -ai` — 檢查歷史登入紀錄與來源 IP
3. `sudo lastb` — 查找失敗登入與暴力破解跡象
4. `cat ~/.ssh/authorized_keys` — 檢查 SSH 金鑰後門

## 二、網路連線與程序調查

5. `sudo ss -tnp` — 查看 TCP 連線與對應程序
6. `sudo ss -tulpn` — 檢查監聽連接埠與異常服務
7. `sudo lsof -nP -i | grep ESTABLISHED` — 查找已建立的網路連線
8. `ps aux --sort=-%cpu` — 找出高 CPU 使用率的可疑程序
9. `sudo ls -l /proc/*/exe 2>/dev/null | grep deleted` — 尋找已刪除但仍在執行的程式

## 三、後門與權限調查

10. `crontab -l` — 檢查目前使用者的排程後門
11. `sudo find /root /home -name authorized_keys -ls` — 搜尋各使用者的 SSH 授權金鑰
12. `sudo getcap -r / 2>/dev/null` — 掃描可能被濫用的特殊權限
13. `cat /etc/ld.so.preload` — 檢查可疑的共享函式庫載入設定
14. `cat /proc/sys/kernel/tainted` — 查看 Linux 核心污染旗標

## 四、檔案與日誌調查

15. `sudo find / -xdev -type f -mtime -1 2>/dev/null` — 搜尋最近 24 小時內修改的檔案
16. `sudo journalctl -u ssh --since "24 hours ago"` — 檢查近期 SSH 服務日誌
17. `sudo dpkg -V` 或 `sudo rpm -Va` — 驗證系統套件完整性

## 五、事件應變原則

- 不要急著重開機，避免遺失即時鑑識證據
- 優先檢查網路連線、登入紀錄與可疑程序
- 留意 SSH 金鑰、排程任務與特殊權限後門
- 保存日誌與可疑檔案，避免直接刪除
- 必要時隔離主機並進行記憶體與離線鑑識
- 不要完全相信已遭入侵主機上的工具輸出

核心觀念：先保全證據，再隔離調查，最後清除與復原
