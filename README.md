# BLE 设备扫描器

使用 Web Bluetooth API 检测环境中 BLE 设备的广播信号，显示 RSSI、地址、名字和完整的广播数据信息。

## ✨ 功能特性

- 📡 扫描附近的 BLE 设备广播信号
- 📊 显示设备 RSSI（信号强度）
- 🏷️ 显示设备地址和名称
- 📦 **显示完整的广播数据信息（十六进制格式）**
  - Service UUIDs
  - Manufacturer Data（制造商数据，包含公司 ID）
  - Service Data（服务数据）
  - TX Power（发射功率）
  - Appearance（外观值）
- 🔢 **完整的十六进制数据显示**
  - 显示每个字段的字节长度
  - 十六进制格式化显示（XX XX XX）
  - 大数据自动使用 Hex Dump 格式（16 字节/行，带 ASCII）
  - 原始数据汇总和总字节数统计
- 📋 **一键复制十六进制数据**
- 🎨 现代化的响应式界面设计
- 🔄 实时更新设备信息

## 📋 使用要求

### 1. 浏览器要求
- Chrome 90+ 或 Edge 90+
- 其他浏览器暂不支持

### 2. 启用实验性功能

Web Bluetooth LE Scan API 是实验性功能，需要手动启用：

1. 在 Chrome 地址栏输入：`chrome://flags/#enable-experimental-web-platform-features`
2. 找到 "Experimental Web Platform features"
3. 设置为 **Enabled**
4. 点击 **Relaunch** 重启浏览器（必须重启！）

### 3. 运行环境

必须在安全上下文中运行：
- `https://` 网站
- `localhost` 或 `127.0.0.1`

## 🚀 快速开始

### 方法 1：使用 Python（推荐）

```bash
# Python 3
python3 -m http.server 8000

# 或 Python 2
python -m SimpleHTTPServer 8000
```

### 方法 2：使用 Node.js

```bash
# 安装 http-server（如果还没安装）
npm install -g http-server

# 启动服务器
http-server -p 8000
```

### 访问网页

在浏览器中打开：`http://localhost:8000/index.html`

## 📖 使用步骤

1. **启动本地服务器**（见上方快速开始）

2. **在 Chrome 浏览器中打开网页**
   - 访问 `http://localhost:8000/index.html`

3. **打开开发者工具**（可选但推荐）
   - 按 `F12` 或右键 → 检查
   - 切换到 Console 标签页查看日志

4. **开始扫描**
   - 点击 **"开始扫描"** 按钮
   - 在弹出的权限对话框中点击 **"允许"**

5. **查看设备信息**
   - 发现的设备会自动显示在列表中
   - 实时更新 RSSI 和广播数据
   - 按信号强度排序显示

6. **管理设备列表**
   - 点击 **"停止扫描"** 停止搜索新设备
   - 点击 **"清除列表"** 清空设备列表

## 🐛 故障排查

### 问题 1：点击"允许"后没有发现设备

**解决方案：**

1. **检查实验性功能是否启用**
   - 访问 `chrome://flags/#enable-experimental-web-platform-features`
   - 确保设置为 **Enabled**
   - **必须重启浏览器**

2. **检查浏览器控制台**
   - 按 F12 打开开发者工具
   - 查看 Console 是否有错误信息
   - 正常情况应该看到 "Scan started successfully" 日志

3. **确认蓝牙已开启**
   - 确保电脑的蓝牙适配器已开启
   - 确保附近有 BLE 设备在广播

4. **尝试其他测试设备**
   - 用手机开启蓝牙作为测试设备
   - 使用 BLE Beacon 或智能手环
   - 有些设备可能不持续广播

5. **检查运行环境**
   - 确保在 localhost 或 HTTPS 环境
   - 不要直接双击打开 HTML 文件（file:// 协议不支持）

### 问题 2：提示不支持 Web Bluetooth LE Scan

**解决方案：**
- 确保使用 Chrome 90+ 或 Edge 90+
- 确保已启用实验性功能（见上方"使用要求"）
- 重启浏览器使设置生效

### 问题 3：扫描很慢或设备很少

**可能原因：**
- BLE 设备的广播间隔不同（从几十毫秒到几秒不等）
- 有些设备只在特定时间广播
- 信号弱的设备可能无法检测到

**解决方案：**
- 多等待一段时间
- 将设备靠近电脑
- 确保附近有活跃的 BLE 设备

### 问题 4：在 HTTPS 网站上仍然无法使用

**解决方案：**
- 检查证书是否有效（不能是自签名证书）
- 检查是否被浏览器安全策略阻止
- 尝试在 localhost 上测试

### 问题 5：只显示了 SCAN_RSP 的数据

- Web Bluetooth API 的设计限制，只返回解析后的 Event

## 📊 显示信息说明

### RSSI（信号强度指示）
- **单位**：dBm（分贝毫瓦）
- **范围**：通常在 -100 dBm 到 0 dBm 之间
- **信号强度**：
  - -60 dBm 及以上：强信号（绿色）
  - -60 ~ -80 dBm：中等信号（橙色）
  - -80 dBm 以下：弱信号（红色）

### 设备信息
- **设备 ID/地址**：设备的唯一标识符
- **名称**：设备广播的名称（如果有）
- **TX Power**：发射功率，用于计算距离

### 广播数据（十六进制格式）
- **Service UUIDs**：设备提供的蓝牙服务列表
- **Manufacturer Data**：厂商自定义数据
  - 显示公司 ID（Company ID）的十六进制和十进制值
  - 显示数据长度（字节数）
  - 完整的十六进制数据（格式：XX XX XX）
  - 超过 8 字节自动使用 Hex Dump 格式显示
- **Service Data**：与特定服务相关的数据
  - 显示服务 UUID
  - 显示数据长度（字节数）
  - 完整的十六进制数据
  - 超过 8 字节自动使用 Hex Dump 格式显示
- **Appearance**：设备类型标识（十六进制格式）

## 🔧 技术细节

### 使用的 API
- `navigator.bluetooth.requestLEScan()` - 请求 BLE 扫描
- `advertisementreceived` 事件 - 接收广播数据

### 浏览器兼容性

| 浏览器 | 支持状态 |
|--------|---------|
| Chrome 90+ | ✅ 需启用实验性功能 |
| Edge 90+ | ✅ 需启用实验性功能 |
| Firefox | ❌ 不支持 |
| Safari | ❌ 不支持 |
| Opera | ⚠️ 基于 Chromium 的版本可能支持 |

### 安全限制
- 必须在用户手势（点击按钮）后才能请求扫描
- 必须在安全上下文（HTTPS 或 localhost）
- 需要用户明确授权蓝牙访问权限

## 💡 使用建议

1. **调试时打开控制台**
   - 可以看到详细的扫描日志
   - 方便排查问题

2. **信号强度参考**
   - RSSI 值会随设备移动实时变化
   - 可用于粗略估计设备距离

3. **广播数据解析**
   - Manufacturer Data 中的公司 ID 可以查询蓝牙 SIG 官网
   - Service UUID 可参考蓝牙标准服务列表

4. **十六进制数据使用**
   - 点击 **"📋 复制"** 按钮复制所有数据
   - 可以保存到文本文件进行离线分析
   - 使用专业工具（如 Wireshark）分析协议
   - 对比不同时间点的数据变化

5. **数据格式理解**
   - 每两个十六进制字符代表一个字节（8 位）
   - 例如：`4C 00` = 公司 ID 0x004C（Apple Inc.）
   - 大端序（Big-Endian）和小端序（Little-Endian）要注意区分

6. **性能优化**
   - 不需要时及时停止扫描
   - 定期清理设备列表

## 📚 相关资源

- [Web Bluetooth API 规范](https://webbluetoothcg.github.io/web-bluetooth/)
- [Chrome Web Bluetooth 实现状态](https://github.com/WebBluetoothCG/web-bluetooth/blob/main/implementation-status.md)
- [蓝牙 SIG - 公司标识符](https://www.bluetooth.com/specifications/assigned-numbers/company-identifiers/)
- [蓝牙 GATT 服务](https://www.bluetooth.com/specifications/gatt/services/)

## ⚠️ 注意事项

1. **实验性 API**
   - Web Bluetooth LE Scan API 是实验性功能
   - API 可能在未来版本中发生变化
   - 不建议用于生产环境的关键功能

2. **隐私保护**
   - BLE 扫描会暴露附近设备信息
   - 请遵守相关隐私法规

3. **性能影响**
   - 持续扫描会增加系统负担
   - 建议按需扫描，不需要时停止

## 📄 许可证

MIT License

## 🤝 贡献

欢迎提交问题和改进建议！

---

**提示**：如果遇到问题无法解决，请检查：
1. ✅ 实验性功能已启用
2. ✅ 浏览器已重启
3. ✅ 在 localhost 环境运行
4. ✅ 蓝牙已开启且有设备在广播
5. ✅ 查看控制台日志

