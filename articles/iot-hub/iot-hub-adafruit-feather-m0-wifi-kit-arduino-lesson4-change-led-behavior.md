---
title: "将 Arduino (C) 连接到 Azure IoT - 第 4 课：修改应用 |Microsoft Docs"
description: "对消息进行自定义以更改 LED 的亮起和熄灭行为。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "使用 arduino 控制 led"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 3585dfbac8816140c0a62931920aff1a6bf7d540
ms.lasthandoff: 01/24/2017


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>更改 LED 的亮起和熄灭行为
## <a name="what-you-will-do"></a>执行的操作
对消息进行自定义以更改 LED 的亮起和熄灭行为。 如果有任何问题，请在适用于 Adafruit Feather M0 WiFi Arduino 开发板的[故障排除页](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md)查找解决方案。

## <a name="what-you-will-learn"></a>你要学习的知识
使用额外的 Arduino 函数更改 LED 的亮起和熄灭行为。

## <a name="what-you-need"></a>所需条件
必须已成功完成了[在 Arduino 开发板上运行示例应用程序来检索“云到设备”消息][receive-cloud-to-device-messages]的操作。

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>将函数添加到 main.c 和 gulpfile.js
1. 通过运行以下命令在 Visual Studio Code 中打开示例应用程序：

   ```bash
   cd Lesson4
   code .
   ```
2. 打开 `app.ino` 文件，然后在 blinkLED() 函数之后添加以下函数：

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![包含已添加函数的 app.ino 文件][app-ino-file]
3. 在 `receiveMessageCallback` 函数的 `else if` 块之前添加以下条件：

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
   {
       turnOffLED();
   }
   ```

   现在已将示例应用程序配置为通过消息响应更多指令。 “on”指令使 LED 亮起，“off”指令使 LED 熄灭。
4. 打开 gulpfile.js 文件，然后在函数 `sendMessage` 之前添加一个新函数：

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   };
   ```

   ![包含已添加函数的 Gulpfile.js 文件][gulp-file-js]
5. 在 `sendMessage` 函数中，将 `var message = buildMessage(sentMessageCount);` 行替换为以下代码片段中显示的新行：

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. 保存所有更改。

### <a name="deploy-and-run-the-sample-application"></a>部署并运行示例应用程序
通过运行以下命令在 Arduino 开发板上部署并运行示例应用程序：

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

你应当会看到 LED 亮起两秒，然后熄灭两秒。 最后的“stop”消息将停止运行示例应用程序。

![打开和关闭][on-and-off]

祝贺你！ 你已成功自定义了从 IoT 中心发送到 Arduino 开发板的消息。

### <a name="summary"></a>摘要
此可选部分展示了如何自定义消息，以便使示例应用程序能够以不同的方式控制 LED 的亮起和熄灭行为。

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png
