title: Mac OSX - 使用 ifconfig 指令查詢與設定網路介面卡
date: 2015-11-22 04:34
category:
    - Internet
tag:
    - Internet
    - Mac OSX
----

`ifconfig` 指令是 Interface Configuration 的縮寫，為 Linux/Unix 系統中用來查詢與控制網路介面卡的指令。

<!-- more -->

## 在 Mac OSX 底下的查詢網路介面卡的狀態

``` bash
usage: ifconfig [-C] [-L] interface address_family [address [dest_address]]
                [parameters]
       ifconfig interface create
       ifconfig -a [-C] [-L] [-d] [-m] [-u] [-v] [address_family]
       ifconfig -l [-d] [-u] [address_family]
       ifconfig [-C] [-L] [-d] [-m] [-u] [-v]

$ ifconfig -a
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=3<RXCSUM,TXCSUM>
	inet6 ::1 prefixlen 128
	inet 127.0.0.1 netmask 0xff000000
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
	nd6 options=1<PERFORMNUD>
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether 60:f8:1d:ba:9e:00
	inet6 fe80::62f8:1dff:feba:9e00%en0 prefixlen 64 scopeid 0x4
	inet 192.168.1.103 netmask 0xffffff00 broadcast 192.168.1.255
	nd6 options=1<PERFORMNUD>
	media: autoselect
	status: active
en1: flags=963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX> mtu 1500
	options=60<TSO4,TSO6>
	ether 72:00:06:25:98:80
	media: autoselect <full-duplex>
	status: active
ppp0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1492
	inet 122.121.181.203 --> 168.95.98.254 netmask 0xff000000
```

單純使用 `ifconfig [-a]` 查詢目前所有的網路介面資訊時會看到上述的資訊，較為重要的說明如下:

1. `en0` : 代表第一張網路介面卡， 而 `en1` 則是第二張網路介面卡，以此類推…
    - `lo0` 為系統內建的遞迴網路。
    - `eth0` 為乙太網路介面卡。
    - `ppp0` 為 PPPoE 連線介面。
2. `mtu` : 為每個資料原包的最大傳輸單位 (Maximum Transmission Unit)。
3. `ether` : 網路介面卡的 MAC (Media Access Control Address) 卡號。
4. `inet`, `inet addr` : 網際網路位址 (Internet Address)，即此台電腦的 IP 位址。
5. `inet6`,  `inet6 addr` : IPv6 使用的位址。
6. `mask`, `netmask` : 網路遮罩。
7. `HWaddr` : 硬體位址。前3碼是網路炩生產廠商的代號，後3碼是該網路卡的生產序號。
8. `bcast` 或 `broadcast` 為廣播 (Broadcast) 的位址，用來同時發送訊息給相同網域的其他電腦。
9. `RX/TX packets` 分別為已接收 (Received) / 已輸出 (Transmitted) 的封包總數。
10. `status` 標示該網路介面卡的狀態為啟用 (Active) 或停用 (Inactive) 。

## `ifconfig` 的常用查詢與設定指令

下列有些修改設定相關的指令可能需要 `sudo` 管理權限才能操作。

1. 查詢 `ifconfig` 說明手冊。

    ``` bash
    $ man ifconfig
    ```

2. 查詢所有網路介面卡的名稱。

   ``` bash
   $ ifconfig -l
   lo0 en0 en1 ppp0
   ```

3. 查詢網路介面卡的狀態。

   ``` bash
   $ ifconfig en0
   en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
   	ether ab:09:87:65:43:21
   	inet6 fe80::62f8:1dff:feba:9e00%en0 prefixlen 64 scopeid 0x4
   	inet 192.168.1.103 netmask 0xffffff00 broadcast 192.168.1.255
   	nd6 options=1<PERFORMNUD>
   	media: autoselect
   	status: active
   ```

4. 啟動網路介面卡

   ``` bash
   $ ifconfig en0 up
   ```

5. 停用網路介面卡

   ``` bash
   $ ifconfig en0 down
   ```

6. 手動設定網路介面卡的 IP 位址

   ``` bash
   $ ifconfig en0 127.0.0.1
   ```

7. 修改網路介面卡的 MAC Address ，需要管理權限請謹慎操作。

    ``` bash
    $ sudo ifconfig en0 ether 12:34:56:78:90:AB
    Password:
    $ ifconfig en0
    en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
    	ether 12:34:56:78:90:ab
    	inet6 fe80::62f8:1dff:feba:9e00%en0 prefixlen 64 scopeid 0x4
    	inet 192.168.1.103 netmask 0xffffff00 broadcast 192.168.1.255
    	nd6 options=1<PERFORMNUD>
    	media: autoselect
    	status: active
    ```

### References

- [鳥哥的 Linux 私房菜 - 常用網路指令](http://linux.vbird.org/linux_server/0140networkcommand.php#ifconfig)
- [Linux ifconfig 實用例子](http://www.phpini.com/linux/linux-ifconfig)
