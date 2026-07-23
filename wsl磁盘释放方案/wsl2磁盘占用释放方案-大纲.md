# wsl2磁盘占用释放方案

* wsl2很香。

* wsl2 + docker更香

* docker加加减减的情况下 wsl2的磁盘占用空间只会变大不会减小

* 如今磁盘价格贵的离谱

* 缩容的方式有两种
    * Sparse VHD
        * 适用于新版本的wsl，新功能，社区反馈不稳定,这里不做说明

    * 手动diskpart(稳定可靠，亲测可用)
        1. 在Linux 里面擦除已删文件的痕迹（防止碎片）
            ```bash
            sudo fstrim -av
            ```
        2. 彻底关闭 WSL
            ```powershell
            wsl --shutdown
            ```
        3. 找到你的wsl的ext4.vhdx文件
        4. 用 Windows 自带的 diskpart 压缩（管理员身份运行 PowerShell）
            ```powershell
            diskpart
            select vdisk file="你的ext4.vhdx路径"
            compact vdisk
            exit
            ```
* 磁盘释放了感觉钱又回来了真好