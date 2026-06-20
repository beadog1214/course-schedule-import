# 📅 课表 → iOS 日历

每学期开学第一件事就是对着课表往手机日历里敲，实在不想再干了。写个脚本代劳一下。

给张课表截图，OCR 认出来，吐个 .ics 文件，AirDrop 到 iPhone 点开就全加进去了。

## 安装

依赖 tesseract 做 OCR，三平台都支持：

**Mac**
```bash
brew install tesseract tesseract-lang
```

**Linux**
```bash
apt install tesseract-ocr tesseract-ocr-chi-sim
```

**Windows**  
[下载安装包](https://github.com/UB-Mannheim/tesseract/wiki)，安装时记得勾 Chinese Simplified。

完了验证一下：
```bash
tesseract --list-langs | grep chi_sim
```

## 使用

```bash
git clone https://github.com/beadog1214/course-schedule-import.git
cd course-schedule-import
python3 schedule2ics.py 课表.jpg --name "课表" --start 2026-02-23
```

脚本会显示 OCR 认出来的字，然后按提示输课程：

```
课程名,星期,节次,周次,教室
大学英语,一二三,1-2,1-18周,教B-302
```

输完回车，Downloads 里就能看到 .ics 了。

## 节次时间

大部分课表图片里标了时间（比如「1-2节 8:00-9:40」），脚本会自动抓。抓不到就问你，输一次下次记住。

也可以直接命令行指定：
```bash
python3 schedule2ics.py 课表.png --slots "1-2=08:30-10:00;3-4=10:15-11:45"
```

不改的话默认用这个：
```
1-2  08:00-09:40
3-4  10:00-11:40
5-6  13:30-15:10
7-8  15:20-17:00
9-11 18:30-21:00
```

## 导入手机

.ics 文件 AirDrop 到 iPhone，点一下自动弹日历，选「添加全部」就行。

## 顺手也支持

- 单双周：`1-18周(单)` 自动只加奇数周
- 周次组合：`1-4周,6-8周,15周` 逗号拼一起
- 多人同文件：每次 `--name` 换个人名就行

---

不想折腾 OCR 的话，脚本也会提示切到纯手动模式，照样能用。
