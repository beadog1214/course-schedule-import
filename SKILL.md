---
name: course-schedule-import
description: 课表图片 OCR 识别 + 生成 iOS 日历 .ics 文件。拍一张课表截图，自动识别课程名/时间/教室，导出可导入 iPhone 日历的文件。
version: 1.0.0
metadata:
  openclaw:
    emoji: "📅"
    homepage: https://github.com/beadog1214/course-schedule-import
    requires:
      bins:
        - python3
---

# 课表导入 iOS 日历

把课表截图变成 iPhone 日历事件。OCR 用 macOS Vision 引擎，支持中英文混排。

## 触发

用户说「导入课表」「课表转日历」「把课表加到日历」等。

## 流程

1. 让用户提供课表图片路径
2. 运行 `python3 schedule2ics.py <图片路径>`
3. 脚本自动 OCR，显示识别文字
4. 交互输入每门课：`课程名,星期(一二三四五),节次,周次,教室`
5. 生成 `.ics` 文件到 Downloads
6. 用户 AirDrop 到 iPhone → 点开自动导入日历

## 自定义时间槽

**优先从课表图片中自动捕捉**节次时间（如识别到「1-2节 8:00-9:40」），捕捉不到才问用户。

自定义一次后自动保存到 `slots.json`，下次直接加载。

**三种设置方式**：

1. **交互式**（推荐）：运行后在提示处输入
   ```
   1-2=08:30-10:00;3-4=10:15-11:45;5-6=13:30-15:10
   ```

2. **命令行**：
   ```bash
   python3 schedule2ics.py 课表.png --slots "1-2=08:30-10:00;3-4=10:15-11:45"
   ```

3. **编辑 `slots.json`**：首次运行后自动生成，直接编辑

**默认值**（通用大学作息）：
| 节次 | 时间 |
|------|------|
| 1-2 | 08:00-09:40 |
| 3-4 | 10:00-11:40 |
| 5-6 | 13:30-15:10 |
| 7-8 | 15:20-17:00 |
| 9-11 | 18:30-21:00 |

## 手动输入格式

```
课程名,星期,节次,周次,教室
```

示例：
```
大学英语II,一二三,1-2,1-3周单7-9周11周,教B-302
民法（上）,一二三四,3-4,2-6周双7-11周单12-14周16-17周,模拟法庭
```

## 依赖

- Python 3.9+
- **tesseract** + 中文语言包（必须安装，跨平台统一 OCR 引擎）

### 安装 tesseract

| 平台 | 命令 |
|------|------|
| **Mac** | `brew install tesseract tesseract-lang` |
| **Linux** | `apt install tesseract-ocr tesseract-ocr-chi-sim` |
| **Windows** | [下载安装程序](https://github.com/UB-Mannheim/tesseract/wiki)，安装时勾选 Chinese Simplified |

安装后验证：`tesseract --list-langs | grep chi_sim`

如果图方便不想装 OCR，脚本会自动检测缺失并进入纯手动输入模式。
