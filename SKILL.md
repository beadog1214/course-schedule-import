---
name: course-schedule-import
description: 拍课表截图 → OCR → .ics 日历文件，AirDrop 到 iPhone 直接用。全平台 tesseract 引擎。
version: 1.2.0
metadata:
  openclaw:
    emoji: "📅"
    homepage: https://github.com/beadog1214/course-schedule-import
    requires:
      bins:
        - python3
---

# 课表转 iOS 日历

每次开学对着课表截图手动建日历事件太烦了。这个脚本就是干这事的——给张课表图片，吐个 .ics，AirDrop 到手机点一下完事。

## 什么时候用

用户给了课表图片/PDF/Word 文档，说想导进日历。

## 怎么跑

```bash
python3 schedule2ics.py 文件路径 --name "张三 课表" --start 2026-02-23
```

支持 PNG JPG PDF DOCX，脚本自动识别类型。PDF 优先提取文本层，扫面件自动转图 OCR。

然后按提示输课程，格式：

```
课程名,星期,节次,周次,教室
大学英语,一二三,1-2,1-18周,教B-302
```

输完回车两次，.ics 文件就在 Downloads 了。

## 节次时间

脚本会试着从图片里读时间（大部分课表都有标注），读到就用，读不到就问你。改一次就记住，下次不用重设。

或者一开始就指定：
```bash
python3 schedule2ics.py 课表.png --slots "1-2=08:30-10:00;3-4=10:15-11:45"
```

默认是通用的大学作息：
| 节次 | 时间 |
|------|------|
| 1-2 | 08:00-09:40 |
| 3-4 | 10:00-11:40 |
| 5-6 | 13:30-15:10 |
| 7-8 | 15:20-17:00 |
| 9-11 | 18:30-21:00 |

## 依赖

- tesseract + 中文包（图片 OCR 必须）
- 处理 PDF/Word 的话额外装：
  ```bash
  pip install pdfplumber python-docx PyMuPDF
  ```
  不装也能跑——PDF 会回退到 pdftotext，扫面件转图 OCR，Word 文本直接读不着就提示。
