title: 學習筆記 - Vagrant 基本入門
date: 2015-11-28 05:30
category:
    - Vagrant
tag:
    - DevOps
    - Mac OSX
    - Vagrant
    - Virtual Machine
    - VirtualBox

----

根據 Mitchell Hashimoto 最初在 [GitHub][GitHub] 首次釋出 [Vagrant 0.1.0](https://github.com/mitchellh/vagrant/tree/v0.1.0)  的專案說明:

> Vagrant is a tool for building and distributing virtualized development environments.
> By providing automated creation and provisioning of virtual machines using [Sun’s VirtualBox][VirtualBox], Vagrant provides the tools to create and configure lightweight, reproducible, and portable virtual environments.

[Vagrant][Vagrant] 為一套工具用來協助建立與散佈虛擬化的開發環境，主要提供下列特點:

- 自動化建立、設定與啟動 Virtual Machines。
- 提供建立與配置輕量化、可重覆使用及可攜式的虛擬環境。

最初的 [Vagrant][Vagrant] 是架構在 [VirtualBox][VirtualBox] 的基礎之上，使用 Ruby 所開發而成的一套用來管理 Virtual Machine 且可程式化的工具，在經過幾年發展後便不再只侷限於 [VirtualBox][VirtualBox]，現今已經能支援各式主流的 Virtual Machine 軟體，且安裝方式也簡化不少， 直接到 [Vagrant 官方網站][Vagrant] 根據作業系統下載適合的標準安裝檔來安裝即可。

<!-- more -->

## 在 Mac OSX 裡安裝 [Vagrant][Vagrant]

如前述所說，[Vagrant][Vagrant] 的基礎是架構在 [VirtualBox][VirtualBox] 等虛擬機軟體之上，因此系統環境除了要安裝 [Vagrant][Vagrant] 以外，同時也需要安裝 [VirtualBox][VirtualBox] 等軟體之一來運作 Virtual Machine。

1. 至 [Vagrant 官方網站][Vagrant] 下載適合的安裝檔並執行安裝: [下載頁面][Vagrant Download]
   - 使用 [Vagrant][Vagrant] 1.7.4 Version。
2. 至 [VirtualBox 官方網站][VirtualBox] 下載適合的安裝檔並執行安裝:
   - 使用 [VirtualBox][VirtualBox] 5.0.10 Version。

另一方面也可以透過 [Homebrew Cask][Homebrew Cask] 來安裝:

``` bash
$ brew cask install vagrant --appdir=/Applications
$ brew cask install virtualbox --appdir=/Applications
```

## 使用 [Vagrant][Vagrant] 快速安裝與建立 Virtual Machine

只需要簡單幾行指令就能利用 [Vagrant][Vagrant] 建立出一個新的 Virtual Machine:

``` bash
$ mkdir vagrant-demo
$ cd vagrant-demo
$ vagrant init ubuntu/trusty64
$ vagrant up
$ vagrant ssh
vagrant@vagrant-ubuntu-trusty-64:~$ lsb_release -a
```

上述指令會在本機 (**Host OS**) 裡建立並啟動一個新的 Virtual Machine (**Guest OS**):

1. 首先建立與切換一個新的工作目錄。
2. 執行 `vagrant init ubuntu/trusty64` 會產生一份 **VagrantFile** ，是用來建立 Virtual Machine 的設定檔。
   - [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64) 是用 Ubuntu 14.04 LTS (Trusty Tahr) 64-bit 打包好的一個作業系統，
   - 一個打包好的作業系統稱作 `box` 表示為 [Vagrant][Vagrant] 虛擬機。
3. 執行 `vagrant up` 後即可根據 **VagrantFile** 來建立並啟動 Virtual Machine。
   - 當 box 第一次在 Host OS 上被建立時, [Vagrant][Vagrant] 會先行向 [Vagrant Cloud][Vagrant Cloud] 查詢並下載 box 檔案完成後才會開始建立。
   - 當 box 曾經在 Host OS 上建立過，則 [Vagrant][Vagrant] 會直接使用之前下載的 box 檔案來建立。注意: 此 box 檔案由於是之前下載時的版本，因此不一定是最新版本。
   - 最後會建立出一個 Virtual Machine (稱為 Guest OS) 並運作在背景裡，且它會佔用 Host OS 的部份 CPU 與 Memory 等資源。
4. 執行  `vagrant ssh` 後即可透過 ssh 登入至 Guest OS 裡面並開始使用。
5. 最後在 Guest OS 裡執行 `lsb_release -a` 就可以看到它的系統資訊。

在使用的過程中登出、重新連線以及關閉 Virtual Machine 的方式為:

``` bash
vagrant@vagrant-ubuntu-trusty-64:~$ exit
$ vagrant ssh
vagrant@vagrant-ubuntu-trusty-64:~$ exit
$ vagrant halt
```

1. 利用 `exit` 來登出 Guest OS。
2. 在該工作目錄內可隨時使用 `vagrant ssh` 再次登入至 Guest OS。
3. 使用 `vagrant halt` 則可將 Guest OS 關閉，並將佔用的 CPU, Memory 等資源歸還給 Host OS。

## [Vagrant Cloud][Vagrant Cloud] 介紹

前述範例中所使用的 [ubuntu/trusty64](https://vagrantcloud.com/ubuntu/boxes/trusty64), 其下載來源為 [Vagrant Cloud (ATLAS)][Vagrant Clode],  是 [Vagrant][Vagrant] 發明人創辦的公司所經營的服務，其用途是作為 box 的 Repository 中央儲存庫。

因此你可以在 [Vagrant Cloud][Vagrant Cloud] 上面搜尋與下載 box 來直接建立對應的 Virtual Machine，例如以下為目前 Ubuntu 比較常用的 box:

- [ubuntu/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64): Official Ubuntu Server 14.04 LTS (Trusty Tahr) builds.
- [hashicorp/precise64](https://atlas.hashicorp.com/hashicorp/boxes/precise64): A standard Ubuntu 12.04 LTS 64-bit box.
- [hashicorp/precise32](https://atlas.hashicorp.com/hashicorp/boxes/precise32) A standard Ubuntu 12.04 LTS 32-bit box.

### References

- [Vagrant][Vagrant]
- [Vagrant Cloud (ATLAS)][Vagrant Cloud]
- [Vagrant Tutorial（2）跟著流浪漢把玩虛擬機](http://www.codedata.com.tw/social-coding/vagrant-tutorial-2-playing-vm-with-vagrant/)

[Vagrant]: https://www.vagrantup.com/
[Vagrant Download]: https://www.vagrantup.com/downloads.html
[Vagrant Cloud]: https://vagrantcloud.com
[GitHub]: https://github.com
[VirtualBox]: https://www.virtualbox.org/
[Homebrew Cask]: http://caskroom.io/
