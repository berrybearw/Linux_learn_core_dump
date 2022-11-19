# Linux_learn_core_dump
core dump 設定

參考 : 

* [https://www.ltsplus.com/linux/linux-disable-core-dump](https://www.ltsplus.com/linux/linux-disable-core-dump)

* [https://www.cyberciti.biz/faq/disable-core-dumps-in-linux-with-systemd-sysctl/](https://www.cyberciti.biz/faq/disable-core-dumps-in-linux-with-systemd-sysctl/)

```
📖

Core Dump 的作用是診斷及除錯 Linux 系統發生的錯誤, 
也有一些別名是 memory dump, crash dump, system dump 等。
但 core dump 會包括有一個較敏感的資訊, 例如密碼, 使用者的 PAN, SSN 等, 對於開發環境, 
這些除錯的資訊十分有用, 但基於保安理由, 在生產環境中建議關閉 core dump.

預設的情況下, Linux 會把 core dump 的資訊儲存在 /var/crash/ 目錄, 
而透過 systemd 啟動的服務, 則會儲存在 /var/lib/systemd/coredump/ 目錄.

limits.conf 及 sysctl 關閉 core dumpDisabling core dumps on Linux

1. 開啟 /etc/security/limits.conf 檔案, 加入以下 2 行:

  hard core 0
  soft core 0

2. 開啟 /etc/sysctl.d/9999-disable-core-dump.conf 或 /etc/sysctl.conf, 加入以下 2 行:

  fs.suid_dumpable=0kernel.core_pattern=|/bin/false

3. 儲存檔案後, 執行以下指令使以上變更生效:

  $ sudo sysctl -p /etc/sysctl.d/9999-disable-core-dump.conf

關閉 systemd 服務 core dump

透過 systemd 啟動的服務會忽略 limites.conf, 所以需要以下設定來關閉。

1. 建立 /etc/systemd/coredump.conf.d/ 目錄:

  $ sudo mkdir /etc/systemd/coredump.conf.d/

2. 建立 /etc/systemd/coredump.conf.d/custom.conf 檔案:

  $ sudo vim /etc/systemd/coredump.conf.d/custom.conf

3. 撰寫以下設定內容:

  [Coredump]Storage=noneProcessSizeMax=0

4. 最後執行以下指令重新載入 systemctl:

  $ sudo systemctl daemon-reload
```
