# 🔊 小闯闯语音课堂生成器
## 简介
根据用户需求，生成小学各学科知识点的卡通风格语音学习页面，支持6种卡通风格、6个学科，包含知识点语音朗读、趣味互动练习等功能。
## 使用方式
当用户需要生成某个学科知识点的语音学习页面时，使用以下格式调用：
**调用格式：**
@小闯闯语音课堂生成器 生成 [学科] [年级] [知识点] 的语音学习页面，风格选[风格]，难度[简单/中等/较难]
**参数说明：**
| 参数 | 说明 | 可选值 |
|------|------|--------|
| 学科 | 小学学科 | 语文、数学、英语、科学、美术、音乐 |
| 年级 | 年级 | 一年级~六年级 |
| 知识点 | 具体知识点 | 根据学科不同 |
| 风格 | 页面风格（可选，默认卡通冒险） | 卡通冒险、梦幻童话、太空探索、动物乐园、海洋世界、森林奇遇 |
| 难度 | 难度级别（可选，默认中等） | 简单、中等、较难 |
**调用示例：**
@小闯闯语音课堂生成器 生成 语文 一年级 拼音 的语音学习页面
@小闯闯语音课堂生成器 生成 数学 三年级 乘法口诀 的语音学习页面
## 执行流程
1. 解析用户输入，提取学科、年级、知识点、风格、难度
2. 调用 run() 函数生成HTML
3. 返回完整的HTML字符串
## 核心代码
```python
import os
import json
def generate_voice_html(subject, grade, topic, style="卡通冒险", difficulty="中等", content_data=None):
    """生成带语音播放功能的小学知识点学习页面"""
    styles = {
  "卡通冒险": {
    "primary": "#FF6B35",
    "secondary": "#F7C59F",
    "bg": "#FFF3E0",
    "accent": "#FFD700",
    "text": "#4A2800",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(255,107,53,0.2)",
    "gradient": "linear-gradient(135deg, #FF6B35 0%, #FFD700 100%)",
    "font_family": "'Comic Neue', 'ZCOOL KuaiLe', cursive",
    "description": "活力橙黄配色，适合冒险主题"
  },
  "梦幻童话": {
    "primary": "#9B59B6",
    "secondary": "#E8DAEF",
    "bg": "#F5E6FF",
    "accent": "#FF69B4",
    "text": "#4A235A",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(155,89,182,0.2)",
    "gradient": "linear-gradient(135deg, #9B59B6 0%, #FF69B4 100%)",
    "font_family": "'Ma Shan Zheng', 'ZCOOL KuaiLe', cursive",
    "description": "梦幻紫粉配色，适合童话主题"
  },
  "太空探索": {
    "primary": "#2E86C1",
    "secondary": "#AED6F1",
    "bg": "#E8F4FD",
    "accent": "#00FF88",
    "text": "#1B2631",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(46,134,193,0.2)",
    "gradient": "linear-gradient(135deg, #2E86C1 0%, #00FF88 100%)",
    "font_family": "'Orbitron', 'ZCOOL KuaiLe', sans-serif",
    "description": "科技蓝绿配色，适合太空主题"
  },
  "动物乐园": {
    "primary": "#27AE60",
    "secondary": "#A9DFBF",
    "bg": "#E8F8F0",
    "accent": "#F39C12",
    "text": "#1E3A2E",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(39,174,96,0.2)",
    "gradient": "linear-gradient(135deg, #27AE60 0%, #F39C12 100%)",
    "font_family": "'Fredoka One', 'ZCOOL KuaiLe', cursive",
    "description": "自然绿橙配色，适合动物主题"
  },
  "海洋世界": {
    "primary": "#1ABC9C",
    "secondary": "#D5F5E3",
    "bg": "#E8F6F3",
    "accent": "#3498DB",
    "text": "#0E4D3A",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(26,188,156,0.2)",
    "gradient": "linear-gradient(135deg, #1ABC9C 0%, #3498DB 100%)",
    "font_family": "'Pacifico', 'ZCOOL KuaiLe', cursive",
    "description": "清新蓝绿配色，适合海洋主题"
  },
  "森林奇遇": {
    "primary": "#8BC34A",
    "secondary": "#C8E6C9",
    "bg": "#F1F8E9",
    "accent": "#FF9800",
    "text": "#33691E",
    "card_bg": "#FFFFFF",
    "shadow": "rgba(139,195,74,0.2)",
    "gradient": "linear-gradient(135deg, #8BC34A 0%, #FF9800 100%)",
    "font_family": "'Patrick Hand', 'ZCOOL KuaiLe', cursive",
    "description": "森林绿橙配色，适合自然主题"
  }
}
    style_info = styles.get(style, styles["卡通冒险"])
    primary = style_info["primary"]
    secondary = style_info["secondary"]
    bg = style_info["bg"]
    accent = style_info["accent"]
    text_color = style_info["text"]
    card_bg = style_info["card_bg"]
    shadow = style_info["shadow"]
    gradient = style_info["gradient"]
    font_family = style_info["font_family"]
    
    if not content_data:
        content_data = {
            "title": subject + " · " + topic,
            "intro": "欢迎来到" + subject + "的" + topic + "学习课堂！",
            "sections": [
                {"icon": "\U0001f4da", "title": topic + "基础", "content": "这是" + grade + subject + "的" + topic + "知识点。"},
                {"icon": "\U0001f4a1", "title": "学习要点", "content": "掌握" + topic + "的核心概念和基本应用。"},
                {"icon": "\U0001f3af", "title": "重点提示", "content": "多练习、多思考，你一定能学好！"}
            ],
            "tips": "每天坚持学习，进步看得见！",
            "questions": [
                {"q": "你对" + topic + "了解多少？", "a": "继续学习", "options": ["继续学习", "已经掌握", "需要帮助", "不确定"]}
            ]
        }
    
    title = content_data.get("title", subject + " · " + topic)
    intro = content_data.get("intro", "")
    sections = content_data.get("sections", content_data.get("knowledge_points", []))
    tips = content_data.get("tips", "")
    questions = content_data.get("questions", content_data.get("practice", []))
    
    # 构建语音朗读文本
    voice_text = title + "。" + intro + "。"
    for sec in sections:
        voice_text += sec.get("title", "") + "：" + sec.get("content", "") + "。"
    if tips:
        voice_text += "学习小提示：" + tips
    
    # 构建知识点HTML
    sections_html = ""
    for sec in sections:
        icon = sec.get("icon", "\U0001f4d6")
        sec_title = sec.get("title", "")
        sec_content = sec.get("content", "")
        sections_html += """
        <div class="section-card" style="background:""" + card_bg + """;border-radius:16px;padding:20px;margin-bottom:16px;box-shadow:0 4px 15px """ + shadow + """;border-left:5px solid """ + primary + """;">
            <div style="display:flex;align-items:center;gap:12px;margin-bottom:12px;">
                <span style="font-size:32px;">""" + icon + """</span>
                <h3 style="margin:0;font-size:20px;color:""" + text_color + """;">""" + sec_title + """</h3>
            </div>
            <p style="margin:0;font-size:16px;line-height:1.8;color:""" + text_color + """;opacity:0.9;">""" + sec_content + """</p>
        </div>"""
    
    # 构建练习题HTML
    questions_html = ""
    for i, q in enumerate(questions):
        q_text = q.get("question", q.get("q", ""))
        q_answer = q.get("answer", q.get("a", ""))
        q_options = q.get("options", [])
        options_html = ""
        for j, opt in enumerate(q_options):
            letter = chr(65 + j)
            options_html += """
            <div class="option" data-answer=\"""" + q_answer + """\" data-value=\"""" + opt + """\" onclick="checkAnswer(this)" style="background:""" + secondary + """;border-radius:12px;padding:12px 16px;margin-bottom:8px;cursor:pointer;transition:all 0.3s;font-size:15px;color:""" + text_color + """;">
                <span style="display:inline-block;width:28px;height:28px;border-radius:50%;background:""" + primary + """;color:white;text-align:center;line-height:28px;margin-right:10px;font-weight:bold;">""" + letter + """</span>
                """ + opt + """
            </div>"""
        questions_html += """
        <div class="question-card" style="background:""" + card_bg + """;border-radius:16px;padding:20px;margin-bottom:16px;box-shadow:0 4px 15px """ + shadow + """;">
            <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
                <span style="font-size:20px;">\u270f\ufe0f</span>
                <span style="font-size:14px;color:""" + primary + """;font-weight:bold;">第""" + str(i+1) + """题</span>
            </div>
            <p style="margin:0 0 12px 0;font-size:16px;line-height:1.6;color:""" + text_color + """;">""" + q_text + """</p>
            <div class="options" style="margin-top:8px;">
                """ + options_html + """
            </div>
            <div class="feedback" style="margin-top:12px;padding:10px 14px;border-radius:10px;display:none;font-size:14px;"></div>
        </div>"""
    
    # 构建提示卡片
    tips_html = ""
    if tips:
        tips_html = """
        <div class="tips-card" style="background:linear-gradient(135deg, """ + accent + """22, """ + secondary + """44);border-radius:16px;padding:20px;margin-bottom:24px;border:2px dashed """ + accent + """;">
            <div style="font-size:18px;margin-bottom:8px;">\U0001f4a1 学习小提示</div>
            <p style="font-size:15px;line-height:1.6;color:""" + text_color + """;">""" + tips + """</p>
        </div>"""
    
    # 构建完整HTML
    html = """<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>""" + title + """ - 小闯闯语音课堂</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=ZCOOL+KuaiLe&display=swap');
        * { margin:0; padding:0; box-sizing:border-box; }
        body {
            font-family: 'ZCOOL KuaiLe', 'Comic Sans MS', cursive, sans-serif;
            background: """ + bg + """;
            min-height: 100vh;
            padding: 20px;
        }
        .container { max-width: 800px; margin: 0 auto; }
        .header {
            background: """ + gradient + """;
            border-radius: 24px;
            padding: 30px;
            text-align: center;
            margin-bottom: 24px;
            box-shadow: 0 8px 30px """ + shadow + """;
        }
        .header h1 { color: white; font-size: 28px; margin-bottom: 8px; text-shadow: 2px 2px 4px rgba(0,0,0,0.2); }
        .header p { color: rgba(255,255,255,0.9); font-size: 16px; }
        .voice-bar {
            background: """ + card_bg + """;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 24px;
            text-align: center;
            box-shadow: 0 4px 15px """ + shadow + """;
        }
        .voice-btn {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            background: """ + gradient + """;
            color: white;
            border: none;
            padding: 14px 32px;
            border-radius: 50px;
            font-size: 18px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 15px """ + shadow + """;
        }
        .voice-btn:hover { transform: scale(1.05); }
        .voice-btn:active { transform: scale(0.95); }
        .voice-btn.playing { animation: pulse 1.5s infinite; }
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 """ + primary + """66; }
            70% { box-shadow: 0 0 0 15px """ + primary + """00; }
            100% { box-shadow: 0 0 0 0 """ + primary + """00; }
        }
        .tips-card { }
        .section-title {
            font-size: 22px;
            color: """ + text_color + """;
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .option:hover { transform: translateX(5px); }
        .option.correct { background: #27AE60 !important; color: white !important; }
        .option.wrong { background: #E74C3C !important; color: white !important; }
        .feedback.correct { background: #D5F5E3; color: #1E8449; display:block !important; }
        .feedback.wrong { background: #FADBD8; color: #922B21; display:block !important; }
        @media (max-width: 600px) {
            .header h1 { font-size: 22px; }
            .voice-btn { padding: 12px 24px; font-size: 16px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>\U0001f50a """ + title + """</h1>
            <p>""" + intro + """</p>
        </div>
        
        <div class="voice-bar">
            <button class="voice-btn" onclick="playVoice()" id="voiceBtn">
                <span id="voiceIcon">\U0001f50a</span>
                <span id="voiceText">点击听小闯闯讲解</span>
            </button>
            <div style="margin-top:12px;font-size:13px;color:""" + text_color + """;opacity:0.6;">
                \U0001f3af 点击按钮，小闯闯为你朗读知识点
            </div>
        </div>
        
        <div class="section-title">\U0001f4d6 知识点讲解</div>
        """ + sections_html + """
        
        """ + tips_html + """
        
        <div class="section-title" style="margin-top:24px;">\U0001f914 趣味小练习</div>
        """ + questions_html + """
        
        <div style="text-align:center;padding:20px;color:""" + text_color + """;opacity:0.5;font-size:13px;">
            \U0001f308 小闯闯语音课堂 · 快乐学习每一天
        </div>
    </div>
    
    <script>
        var isPlaying = false;
        var utterance = null;
        
        function playVoice() {
            if (isPlaying) {
                window.speechSynthesis.cancel();
                stopPlaying();
                return;
            }
            
            var text = \"""" + voice_text + """\";
            
            if (!window.speechSynthesis) {
                alert("您的浏览器不支持语音播放，请使用Chrome浏览器！");
                return;
            }
            
            utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = "zh-CN";
            utterance.rate = 0.9;
            utterance.pitch = 1.1;
            utterance.volume = 1;
            
            var voices = window.speechSynthesis.getVoices();
            var zhVoice = voices.find(function(v) { return v.lang.startsWith("zh"); });
            if (zhVoice) utterance.voice = zhVoice;
            
            utterance.onstart = function() { startPlaying(); };
            utterance.onend = function() { stopPlaying(); };
            utterance.onerror = function() { stopPlaying(); };
            
            window.speechSynthesis.speak(utterance);
        }
        
        function startPlaying() {
            isPlaying = true;
            document.getElementById("voiceBtn").classList.add("playing");
            document.getElementById("voiceIcon").textContent = "\u23f9\ufe0f";
            document.getElementById("voiceText").textContent = "点击停止播放";
        }
        
        function stopPlaying() {
            isPlaying = false;
            document.getElementById("voiceBtn").classList.remove("playing");
            document.getElementById("voiceIcon").textContent = "\U0001f50a";
            document.getElementById("voiceText").textContent = "点击听小闯闯讲解";
        }
        
        function checkAnswer(el) {
            var answer = el.getAttribute("data-answer");
            var value = el.getAttribute("data-value");
            var feedback = el.closest(".question-card").querySelector(".feedback");
            var options = el.closest(".options").querySelectorAll(".option");
            
            options.forEach(function(opt) { opt.style.pointerEvents = "none"; });
            
            if (value === answer) {
                el.classList.add("correct");
                feedback.className = "feedback correct";
                feedback.innerHTML = "\u2705 回答正确！太棒了！\U0001f389";
            } else {
                el.classList.add("wrong");
                feedback.className = "feedback wrong";
                feedback.innerHTML = "\u274c 再想想哦～正确答案是：" + answer;
                options.forEach(function(opt) {
                    if (opt.getAttribute("data-value") === answer) {
                        opt.classList.add("correct");
                    }
                });
            }
        }
        
        if (window.speechSynthesis) {
            window.speechSynthesis.getVoices();
            window.speechSynthesis.onvoiceschanged = function() {
                window.speechSynthesis.getVoices();
            };
        }
    </script>
</body>
</html>"""
    
    return html
def run(subject, grade, topic, style="卡通冒险", difficulty="中等", content_data=None):
    """桂教通平台调用入口"""
    return generate_voice_html(subject, grade, topic, style, difficulty, content_data)

```