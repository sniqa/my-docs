### 创建nat网络
- 创建内部交换机
```powershell
New-VMSwitch -SwitchName "SwitchName" -SwitchType Internal
```
- 创建nat网关
```powershell
New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex 24
```
- 配置Nat网络
```powershell
New-NetNat -Name MyNATnetwork -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```
