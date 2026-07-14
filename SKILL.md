# 🎨 卡通学习网页生成器
## 简介
根据用户需求，自动生成小学各学科知识点的卡通风格学习网页，支持6种卡通风格、6个学科，包含知识点讲解、趣味互动练习等功能。
## 使用方式
当用户需要生成某个学科知识点的卡通学习网页时，使用以下格式调用：
**调用格式：**
@卡通学习网页生成器 生成 [学科] [年级] [知识点] 的卡通学习网页，风格选[风格]，难度[简单/中等/较难]
**参数说明：**
| 参数 | 说明 | 可选值 |
|------|------|--------|
| 学科 | 小学学科 | 语文、数学、英语、科学、美术、音乐 |
| 年级 | 年级 | 一年级~六年级 |
| 知识点 | 具体知识点 | 根据学科不同 |
| 风格 | 页面风格（可选，默认卡通冒险） | 卡通冒险、梦幻童话、太空探索、动物乐园、海洋世界、森林奇遇 |
| 难度 | 难度级别（可选，默认中等） | 简单、中等、较难 |
**调用示例：**
@卡通学习网页生成器 生成 语文 一年级 拼音 的卡通学习网页
@卡通学习网页生成器 生成 数学 三年级 乘法口诀 的卡通学习网页
## 执行流程
1. 解析用户输入，提取学科、年级、知识点、风格、难度
2. 调用 run() 函数生成HTML
3. 返回完整的HTML网页字符串
## 核心代码
```python
import os
import json
import webbrowser
def generate_learning_html(subject, grade, topic, style="卡通冒险", difficulty="中等"):
    """
    生成卡通风格的小学学科知识点学习网页
    
    Args:
        subject: 学科
        grade: 年级
        topic: 知识点主题
        style: 页面风格
        difficulty: 难度级别
    
    Returns:
        str: 生成的HTML文件路径
    """
    # 加载风格配置
    # 内联样式配置
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
    
    # 生成知识点内容
    content = generate_content(subject, grade, topic, difficulty)
    
    # 生成HTML
    html = build_html(subject, grade, topic, style_info, content, difficulty)
    
    # 保存文件

    os.makedirs(output_dir, exist_ok=True)
    
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = f"{subject}_{grade}_{topic}_{timestamp}.html"
    filepath = os.path.join(output_dir, filename)
    
    with open(filepath, "w", encoding="utf-8") as f:
        f.write(html)
    
    return filepath
def generate_content(subject, grade, topic, difficulty):
    """根据学科、年级、知识点生成学习内容"""
    # 知识点内容模板库
    content_templates = {
        "语文": {
            "拼音": {
                "简单": {
                    "title": "🎈 拼音小乐园",
                    "intro": "小朋友，让我们一起认识拼音宝宝吧！",
                    "knowledge_points": [
                        {"icon": "🔤", "title": "声母", "content": "b p m f d t n l g k h j q x zh ch sh r z c s y w"},
                        {"icon": "🎵", "title": "韵母", "content": "a o e i u ü ai ei ui ao ou iu ie üe er an en in un ün ang eng ing ong"},
                        {"icon": "🎯", "title": "整体认读音节", "content": "zhi chi shi ri zi ci si yi wu yu ye yue yuan yin yun ying"}
                    ],
                    "tips": "💡 小提示：拼音就像积木，声母和韵母拼在一起就能读出汉字啦！",
                    "practice": [
                        {"question": "b + a = ?", "answer": "ba", "options": ["ba", "pa", "ma", "da"]},
                        {"question": "m + a = ?", "answer": "ma", "options": ["ma", "ba", "pa", "fa"]},
                        {"question": "d + a = ?", "answer": "da", "options": ["da", "ta", "na", "la"]}
                    ]
                },
                "中等": {
                    "title": "📚 拼音大冒险",
                    "intro": "欢迎来到拼音王国，今天我们要学习更多拼音知识！",
                    "knowledge_points": [
                        {"icon": "🔤", "title": "声母", "content": "b p m f d t n l g k h j q x zh ch sh r z c s y w"},
                        {"icon": "🎵", "title": "韵母", "content": "a o e i u ü ai ei ui ao ou iu ie üe er an en in un ün ang eng ing ong"},
                        {"icon": "🎯", "title": "声调", "content": "一声（¯）二声（ˊ）三声（ˇ）四声（ˋ）"},
                        {"icon": "📖", "title": "拼读规则", "content": "两拼音节：声母+韵母；三拼音节：声母+介母+韵母"}
                    ],
                    "tips": "💡 小提示：j q x 和 ü 相拼时，ü 要写成 u，但还读 ü 的音哦！",
                    "practice": [
                        {"question": "j + ü = ?", "answer": "ju", "options": ["ju", "jü", "ji", "ja"]},
                        {"question": "q + ü = ?", "answer": "qu", "options": ["qu", "qü", "qi", "qa"]},
                        {"question": "x + üe = ?", "answer": "xue", "options": ["xue", "xüe", "xie", "xia"]}
                    ]
                },
                "较难": {
                    "title": "🏆 拼音小达人",
                    "intro": "挑战更高难度的拼音知识，你准备好了吗？",
                    "knowledge_points": [
                        {"icon": "🔤", "title": "声母", "content": "b p m f d t n l g k h j q x zh ch sh r z c s y w"},
                        {"icon": "🎵", "title": "韵母", "content": "a o e i u ü ai ei ui ao ou iu ie üe er an en in un ün ang eng ing ong"},
                        {"icon": "🎯", "title": "声调标调规则", "content": "有a标a上，没a找o e，i u并列标在后"},
                        {"icon": "📖", "title": "特殊规则", "content": "1. y和w的使用规则 2. ü上两点的省略 3. 隔音符号的使用"}
                    ],
                    "tips": "💡 小提示：标调时记住'有a找a，没a找o e'这个口诀！",
                    "practice": [
                        {"question": "下面哪个拼音的声调标对了？", "answer": "huā", "options": ["huā", "hua", "huà", "huǎ"]},
                        {"question": "'月亮'的拼音是？", "answer": "yuè liang", "options": ["yuè liang", "yüè liàng", "yue liang", "yuè liàng"]},
                        {"question": "'游泳'的拼音是？", "answer": "yóu yǒng", "options": ["yóu yǒng", "yóu yóng", "yòu yǒng", "yóu yōng"]}
                    ]
                }
            },
            "汉字": {
                "简单": {
                    "title": "🖌️ 汉字小课堂",
                    "intro": "让我们一起认识汉字的笔画和结构吧！",
                    "knowledge_points": [
                        {"icon": "✏️", "title": "基本笔画", "content": "横（一）竖（丨）撇（丿）捺（㇏）点（丶）提（㇀）"},
                        {"icon": "📐", "title": "笔画顺序", "content": "先横后竖，先撇后捺，从上到下，从左到右"},
                        {"icon": "🔢", "title": "汉字笔顺", "content": "十：一横一竖；人：一撇一捺；大：一横一撇一捺"}
                    ],
                    "tips": "💡 小提示：写字要记住'横平竖直'，这样写出来的字才漂亮！",
                    "practice": [
                        {"question": "'十'字共有几画？", "answer": "2", "options": ["2", "3", "1", "4"]},
                        {"question": "'人'字的第一笔是？", "answer": "撇", "options": ["撇", "捺", "横", "竖"]},
                        {"question": "'大'字的笔顺是？", "answer": "横、撇、捺", "options": ["横、撇、捺", "撇、横、捺", "横、捺、撇", "撇、捺、横"]}
                    ]
                }
            },
            "古诗": {
                "简单": {
                    "title": "📜 古诗小乐园",
                    "intro": "让我们一起走进古诗的美妙世界！",
                    "knowledge_points": [
                        {"icon": "🌙", "title": "静夜思", "content": "床前明月光，疑是地上霜。举头望明月，低头思故乡。——李白"},
                        {"icon": "🌿", "title": "春晓", "content": "春眠不觉晓，处处闻啼鸟。夜来风雨声，花落知多少。——孟浩然"},
                        {"icon": "🏔️", "title": "登鹳雀楼", "content": "白日依山尽，黄河入海流。欲穷千里目，更上一层楼。——王之涣"}
                    ],
                    "tips": "💡 小提示：读古诗时想象画面，能帮助你更好地理解和记忆！",
                    "practice": [
                        {"question": "'床前明月光'的下一句是？", "answer": "疑是地上霜", "options": ["疑是地上霜", "低头思故乡", "举头望明月", "处处闻啼鸟"]},
                        {"question": "'春眠不觉晓'的作者是？", "answer": "孟浩然", "options": ["孟浩然", "李白", "王之涣", "杜甫"]},
                        {"question": "《登鹳雀楼》中'欲穷千里目'的下一句是？", "answer": "更上一层楼", "options": ["更上一层楼", "黄河入海流", "白日依山尽", "花落知多少"]}
                    ]
                }
            }
        },
        "数学": {
            "乘法口诀": {
                "简单": {
                    "title": "✖️ 乘法口诀小乐园",
                    "intro": "让我们一起来学习乘法口诀吧！",
                    "knowledge_points": [
                        {"icon": "📊", "title": "1的乘法", "content": "一一得一\n一二得二\n一三得三\n一四得四\n一五得五\n一六得六\n一七得七\n一八得八\n一九得九"},
                        {"icon": "📊", "title": "2的乘法", "content": "二二得四\n二三得六\n二四得八\n二五一十\n二六十二\n二七十四\n二八十六\n二九十八"},
                        {"icon": "📊", "title": "3的乘法", "content": "三三得九\n三四十二\n三五十五\n三六十八\n三七二十一\n三八二十四\n三九二十七"}
                    ],
                    "tips": "💡 小提示：乘法口诀就是快速计算的秘诀，背熟了口诀，计算就会又快又准！",
                    "practice": [
                        {"question": "2 × 3 = ?", "answer": "6", "options": ["5", "6", "7", "8"]},
                        {"question": "3 × 4 = ?", "answer": "12", "options": ["10", "11", "12", "13"]},
                        {"question": "2 × 5 = ?", "answer": "10", "options": ["8", "9", "10", "11"]}
                    ]
                },
                "中等": {
                    "title": "✖️ 乘法口诀大闯关",
                    "intro": "挑战更难的乘法口诀，看看你能闯到第几关！",
                    "knowledge_points": [
                        {"icon": "📊", "title": "4的乘法", "content": "四四十六\n四五二十\n四六二十四\n四七二十八\n四八三十二\n四九三十六"},
                        {"icon": "📊", "title": "5的乘法", "content": "五五二十五\n五六三十\n五七三十五\n五八四十\n五九四十五"},
                        {"icon": "📊", "title": "6的乘法", "content": "六六三十六\n六七四十二\n六八四十八\n六九五十四"}
                    ],
                    "tips": "💡 小提示：5的乘法口诀结果末尾不是0就是5，记住这个规律能帮你快速检查！",
                    "practice": [
                        {"question": "6 × 7 = ?", "answer": "42", "options": ["42", "48", "36", "54"]},
                        {"question": "5 × 8 = ?", "answer": "40", "options": ["35", "40", "45", "50"]},
                        {"question": "4 × 9 = ?", "answer": "36", "options": ["32", "36", "40", "45"]}
                    ]
                },
                "较难": {
                    "title": "✖️ 乘法口诀小达人",
                    "intro": "终极挑战！混合乘法口诀大比拼！",
                    "knowledge_points": [
                        {"icon": "📊", "title": "7的乘法", "content": "七七四十九\n七八五十六\n七九六十三"},
                        {"icon": "📊", "title": "8的乘法", "content": "八八六十四\n八九七十二"},
                        {"icon": "📊", "title": "9的乘法", "content": "九九八十一"},
                        {"icon": "🎯", "title": "混合练习", "content": "任意两个一位数相乘，快速说出结果！"}
                    ],
                    "tips": "💡 小提示：9的乘法有个小窍门——积的十位数和个位数相加等于9！",
                    "practice": [
                        {"question": "7 × 8 = ?", "answer": "56", "options": ["56", "63", "48", "64"]},
                        {"question": "9 × 9 = ?", "answer": "81", "options": ["72", "81", "90", "64"]},
                        {"question": "6 × 9 = ?", "answer": "54", "options": ["54", "56", "48", "63"]}
                    ]
                }
            },
            "认识分数": {
                "简单": {
                    "title": "🍕 认识分数小课堂",
                    "intro": "把一块蛋糕分成几份，每一份就是分数！",
                    "knowledge_points": [
                        {"icon": "🍕", "title": "什么是分数", "content": "分数表示把一个整体平均分成若干份，取其中的一份或几份"},
                        {"icon": "📐", "title": "分数的组成", "content": "分数由三部分组成：分子（上面的数）、分数线、分母（下面的数）"},
                        {"icon": "🎯", "title": "几分之一", "content": "1/2（二分之一）、1/3（三分之一）、1/4（四分之一）"}
                    ],
                    "tips": "💡 小提示：分数就像切蛋糕，分母表示切成了几块，分子表示拿了几块！",
                    "practice": [
                        {"question": "把一块蛋糕平均分给2个人，每人得到多少？", "answer": "1/2", "options": ["1/2", "1/3", "1/4", "1"]},
                        {"question": "1/4表示把一个整体分成了几份？", "answer": "4", "options": ["2", "3", "4", "5"]},
                        {"question": "下面哪个分数最大？", "answer": "1/2", "options": ["1/2", "1/3", "1/4", "1/5"]}
                    ]
                }
            }
        },
        "英语": {
            "单词": {
                "简单": {
                    "title": "🐶 英语单词小乐园",
                    "intro": "Let's learn some fun English words!",
                    "knowledge_points": [
                        {"icon": "🐱", "title": "Animals 动物", "content": "cat 猫 🐱 dog 狗 🐶 fish 鱼 🐟 bird 鸟 🐦 rabbit 兔子 🐰"},
                        {"icon": "🎨", "title": "Colors 颜色", "content": "red 红色 🔴 blue 蓝色 🔵 green 绿色 🟢 yellow 黄色 🟡"},
                        {"icon": "🍎", "title": "Food 食物", "content": "apple 苹果 🍎 banana 香蕉 🍌 bread 面包 🍞 milk 牛奶 🥛"}
                    ],
                    "tips": "💡 小提示：每天记住3个新单词，一年就能记住1000多个单词哦！",
                    "practice": [
                        {"question": "猫的英文是？", "answer": "cat", "options": ["cat", "dog", "fish", "bird"]},
                        {"question": "红色的英文是？", "answer": "red", "options": ["red", "blue", "green", "yellow"]},
                        {"question": "苹果的英文是？", "answer": "apple", "options": ["apple", "banana", "bread", "milk"]}
                    ]
                }
            }
        }
    }
    
    # 尝试获取对应内容，如果没有则使用通用模板
    subject_content = content_templates.get(subject, {})
    topic_content = subject_content.get(topic, {})
    difficulty_content = topic_content.get(difficulty, topic_content.get("中等", {}))
    
    if not difficulty_content:
        # 生成通用内容
        difficulty_content = {
            "title": f"📖 {subject} · {topic}",
            "intro": f"欢迎来到{subject}的{topic}学习课堂！",
            "knowledge_points": [
                {"icon": "📚", "title": f"{topic}基础", "content": f"这是{grade}{subject}的{topic}知识点，难度为{difficulty}。"},
                {"icon": "💡", "title": "学习要点", "content": f"掌握{topic}的核心概念和基本应用。"},
                {"icon": "🎯", "title": "重点提示", "content": "多练习、多思考，你一定能学好！"}
            ],
            "tips": "💡 小提示：每天坚持学习，进步看得见！",
            "practice": [
                {"question": f"你对{topic}了解多少？", "answer": "继续学习", "options": ["继续学习", "已经掌握", "需要帮助", "不确定"]}
            ]
        }
    
    return difficulty_content
def build_html(subject, grade, topic, style_info, content, difficulty):
    """构建完整的HTML页面"""
    
    primary = style_info["primary"]
    secondary = style_info["secondary"]
    bg = style_info["bg"]
    accent = style_info["accent"]
    text_color = style_info["text"]
    card_bg = style_info["card_bg"]
    shadow = style_info["shadow"]
    gradient = style_info["gradient"]
    font_family = style_info["font_family"]
    
    # 构建知识点HTML
    knowledge_html = ""
    for kp in content["knowledge_points"]:
        knowledge_html += f"""
        <div class="knowledge-card">
            <div class="card-icon">{kp['icon']}</div>
            <div class="card-content">
                <h3>{kp['title']}</h3>
                <p>{kp['content'].replace(chr(10), '<br>')}</p>
            </div>
        </div>
        """
    
    # 构建练习题HTML
    practice_html = ""
    for i, q in enumerate(content["practice"]):
        options_html = ""
        for opt in q["options"]:
            options_html += f"""
            <label class="option-label">
                <input type="radio" name="q{i}" value="{opt}">
                <span class="option-text">{opt}</span>
            </label>
            """
        
        practice_html += f"""
        <div class="practice-card" data-answer="{q['answer']}">
            <div class="question-number">第{i+1}题</div>
            <div class="question-text">{q['question']}</div>
            <div class="options">
                {options_html}
            </div>
            <div class="feedback"></div>
        </div>
        """
    
    # 构建完整HTML
    html = f"""<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{subject} · {grade} · {topic}</title>
    <link href="https://fonts.googleapis.com/css2?family=ZCOOL+KuaiLe&family=Ma+Shan+Zheng&family=Comic+Neue:wght@400;700&family=Fredoka+One&family=Pacifico&family=Patrick+Hand&family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
    <style>
        * {{
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }}
        
        body {{
            font-family: {font_family};
            background-color: {bg};
            color: {text_color};
            min-height: 100vh;
            overflow-x: hidden;
        }}
        
        /* 背景装饰 */
        body::before {{
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(circle at 10% 20%, {secondary}40 0%, transparent 50%),
                radial-gradient(circle at 90% 80%, {secondary}30 0%, transparent 50%);
            z-index: -1;
        }}
        
        /* 浮动装饰元素 */
        .floating-shapes {{
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }}
        
        .shape {{
            position: absolute;
            border-radius: 50%;
            opacity: 0.1;
            animation: float 8s ease-in-out infinite;
        }}
        
        .shape:nth-child(1) {{
            width: 100px;
            height: 100px;
            background: {primary};
            top: 10%;
            left: 5%;
            animation-delay: 0s;
        }}
        
        .shape:nth-child(2) {{
            width: 150px;
            height: 150px;
            background: {accent};
            top: 60%;
            right: 10%;
            animation-delay: 2s;
        }}
        
        .shape:nth-child(3) {{
            width: 80px;
            height: 80px;
            background: {primary};
            bottom: 20%;
            left: 30%;
            animation-delay: 4s;
        }}
        
        .shape:nth-child(4) {{
            width: 120px;
            height: 120px;
            background: {accent};
            top: 30%;
            right: 30%;
            animation-delay: 6s;
        }}
        
        @keyframes float {{
            0%, 100% {{ transform: translateY(0) rotate(0deg); }}
            50% {{ transform: translateY(-30px) rotate(180deg); }}
        }}
        
        /* 头部 */
        .header {{
            background: {gradient};
            padding: 40px 20px 60px;
            text-align: center;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 20px {shadow};
        }}
        
        .header::after {{
            content: '';
            position: absolute;
            bottom: -20px;
            left: 0;
            right: 0;
            height: 40px;
            background: {bg};
            border-radius: 50% 50% 0 0;
        }}
        
        .header-badge {{
            display: inline-block;
            background: rgba(255,255,255,0.2);
            padding: 8px 20px;
            border-radius: 20px;
            font-size: 14px;
            color: white;
            margin-bottom: 15px;
            backdrop-filter: blur(5px);
        }}
        
        .header h1 {{
            font-size: 2.5em;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
            margin-bottom: 10px;
        }}
        
        .header p {{
            color: rgba(255,255,255,0.9);
            font-size: 1.1em;
        }}
        
        .header .difficulty-badge {{
            display: inline-block;
            margin-top: 15px;
            padding: 6px 18px;
            border-radius: 15px;
            font-size: 14px;
            background: rgba(255,255,255,0.25);
            color: white;
            border: 2px solid rgba(255,255,255,0.3);
        }}
        
        /* 主内容 */
        .container {{
            max-width: 900px;
            margin: 0 auto;
            padding: 30px 20px;
            position: relative;
            z-index: 1;
        }}
        
        /* 知识点区域 */
        .section-title {{
            font-size: 1.8em;
            color: {primary};
            text-align: center;
            margin: 40px 0 25px;
            position: relative;
        }}
        
        .section-title::after {{
            content: '';
            display: block;
            width: 60px;
            height: 4px;
            background: {gradient};
            margin: 10px auto;
            border-radius: 2px;
        }}
        
        .knowledge-grid {{
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }}
        
        .knowledge-card {{
            background: {card_bg};
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 8px 25px {shadow};
            transition: all 0.3s ease;
            border: 2px solid transparent;
            position: relative;
            overflow: hidden;
        }}
        
        .knowledge-card::before {{
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: {gradient};
        }}
        
        .knowledge-card:hover {{
            transform: translateY(-5px);
            box-shadow: 0 12px 35px {shadow};
            border-color: {primary}40;
        }}
        
        .card-icon {{
            font-size: 2.5em;
            margin-bottom: 10px;
        }}
        
        .card-content h3 {{
            font-size: 1.3em;
            color: {primary};
            margin-bottom: 10px;
        }}
        
        .card-content p {{
            font-size: 0.95em;
            line-height: 1.6;
            color: {text_color};
        }}
        
        /* 小提示 */
        .tips-box {{
            background: {gradient};
            border-radius: 15px;
            padding: 20px 25px;
            margin: 30px 0;
            color: white;
            font-size: 1.05em;
            line-height: 1.6;
            box-shadow: 0 4px 15px {shadow};
        }}
        
        /* 练习题区域 */
        .practice-section {{
            background: {card_bg};
            border-radius: 25px;
            padding: 30px;
            margin: 30px 0;
            box-shadow: 0 8px 25px {shadow};
        }}
        
        .practice-card {{
            background: {bg};
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            border: 2px solid {secondary};
            transition: all 0.3s ease;
        }}
        
        .practice-card.correct {{
            border-color: #4CAF50;
            background: #E8F5E9;
        }}
        
        .practice-card.wrong {{
            border-color: #f44336;
            background: #FFEBEE;
        }}
        
        .question-number {{
            display: inline-block;
            background: {gradient};
            color: white;
            padding: 4px 12px;
            border-radius: 10px;
            font-size: 0.9em;
            margin-bottom: 10px;
        }}
        
        .question-text {{
            font-size: 1.1em;
            font-weight: bold;
            margin-bottom: 15px;
        }}
        
        .options {{
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }}
        
        .option-label {{
            display: flex;
            align-items: center;
            padding: 10px 15px;
            background: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 2px solid transparent;
        }}
        
        .option-label:hover {{
            background: {secondary};
            border-color: {primary};
            transform: scale(1.02);
        }}
        
        .option-label input[type="radio"] {{
            margin-right: 10px;
            accent-color: {primary};
        }}
        
        .option-text {{
            font-size: 1em;
        }}
        
        .feedback {{
            margin-top: 10px;
            padding: 8px 12px;
            border-radius: 8px;
            font-weight: bold;
            display: none;
        }}
        
        .practice-card.correct .feedback {{
            display: block;
            background: #C8E6C9;
            color: #2E7D32;
        }}
        
        .practice-card.wrong .feedback {{
            display: block;
            background: #FFCDD2;
            color: #C62828;
        }}
        
        /* 提交按钮 */
        .submit-btn {{
            display: block;
            width: 200px;
            margin: 30px auto 0;
            padding: 15px 30px;
            background: {gradient};
            color: white;
            border: none;
            border-radius: 50px;
            font-size: 1.2em;
            font-family: {font_family};
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px {shadow};
        }}
        
        .submit-btn:hover {{
            transform: translateY(-3px);
            box-shadow: 0 6px 20px {shadow};
        }}
        
        .submit-btn:active {{
            transform: translateY(0);
        }}
        
        /* 分数显示 */
        .score-display {{
            text-align: center;
            margin: 20px 0;
            padding: 20px;
            background: {gradient};
            border-radius: 15px;
            color: white;
            display: none;
        }}
        
        .score-display.show {{
            display: block;
            animation: scoreIn 0.5s ease;
        }}
        
        @keyframes scoreIn {{
            # from {{
                transform: scale(0.5);
                opacity: 0;
            }}
            to {{
                transform: scale(1);
                opacity: 1;
            }}
        }}
        
        .score-number {{
            font-size: 3em;
            font-weight: bold;
        }}
        
        .score-text {{
            font-size: 1.2em;
            margin-top: 10px;
        }}
        
        /* 页脚 */
        .footer {{
            text-align: center;
            padding: 30px 20px;
            color: {primary}80;
            font-size: 0.9em;
        }}
        
        /* 响应式 */
        @media (max-width: 600px) {{
            .header h1 {{ font-size: 1.8em; }}
            .options {{ grid-template-columns: 1fr; }}
            .knowledge-grid {{ grid-template-columns: 1fr; }}
        }}
        
        /* 星星动画 */
        .star {{
            position: fixed;
            pointer-events: none;
            font-size: 20px;
            animation: starFall 1s ease-out forwards;
            z-index: 100;
        }}
        
        @keyframes starFall {{
            0% {{
                transform: translateY(0) scale(1);
                opacity: 1;
            }}
            100% {{
                transform: translateY(-100px) scale(0);
                opacity: 0;
            }}
        }}
    </style>
</head>
<body>
    <!-- 浮动装饰 -->
    <div class="floating-shapes">
        <div class="shape"></div>
        <div class="shape"></div>
        <div class="shape"></div>
        <div class="shape"></div>
    </div>
    
    <!-- 头部 -->
    <header class="header">
        <div class="header-badge">📚 {grade} · {subject}</div>
        <h1>{content['title']}</h1>
        <p>{content['intro']}</p>
        <span class="difficulty-badge">🎯 难度：{difficulty}</span>
    </header>
    
    <!-- 主内容 -->
    <div class="container">
        <!-- 知识点 -->
        <h2 class="section-title">📖 知识点学习</h2>
        <div class="knowledge-grid">
            {knowledge_html}
        </div>
        
        <!-- 小提示 -->
        <div class="tips-box">
            {content['tips']}
        </div>
        
        <!-- 练习题 -->
        <h2 class="section-title">✏️ 趣味练习</h2>
        <div class="practice-section" id="practiceSection">
            {practice_html}
            <button class="submit-btn" onclick="checkAnswers()">✅ 提交答案</button>
            <div class="score-display" id="scoreDisplay">
                <div class="score-number" id="scoreNumber">0/0</div>
                <div class="score-text" id="scoreText"></div>
            </div>
        </div>
    </div>
    
    <!-- 页脚 -->
    <div class="footer">
        🎓 快乐学习，健康成长！<br>
        Generated by AiPy · 本地数据处理，不上传任何信息
    </div>
    
    <script>
        function checkAnswers() {{
            const cards = document.querySelectorAll('.practice-card');
            let correct = 0;
            let total = cards.length;
            
            cards.forEach((card, index) => {{
                const selected = card.querySelector('input[type="radio"]:checked');
                const correctAnswer = card.dataset.answer;
                const feedback = card.querySelector('.feedback');
                
                card.classList.remove('correct', 'wrong');
                
                if (selected) {{
                    if (selected.value === correctAnswer) {{
                        card.classList.add('correct');
                        feedback.textContent = '✅ 回答正确！太棒了！🌟';
                        correct++;
                        createStars(selected);
                    }} else {{
                        card.classList.add('wrong');
                        feedback.textContent = '❌ 再想想哦，正确答案是：' + correctAnswer;
                    }}
                }} else {{
                    card.classList.add('wrong');
                    feedback.textContent = '⚠️ 请选择一个答案！正确答案是：' + correctAnswer;
                }}
            }});
            
            // 显示分数
            const scoreDisplay = document.getElementById('scoreDisplay');
            const scoreNumber = document.getElementById('scoreNumber');
            const scoreText = document.getElementById('scoreText');
            
            scoreDisplay.classList.add('show');
            scoreNumber.textContent = correct + '/' + total;
            
            if (correct === total) {{
                scoreText.textContent = '🎉 全部答对！你是小天才！🏆';
            }} else if (correct >= total * 0.7) {{
                scoreText.textContent = '👏 表现不错！继续加油！💪';
            }} else if (correct >= total * 0.5) {{
                scoreText.textContent = '💪 还可以，多复习一下知识点哦！';
            }} else {{
                scoreText.textContent = '📚 别灰心，再看一遍知识点再来挑战吧！';
            }}
        }}
        
        function createStars(element) {{
            for (let i = 0; i < 5; i++) {{
                const star = document.createElement('div');
                star.className = 'star';
                star.textContent = ['⭐', '🌟', '✨', '💫', '🎉'][Math.floor(Math.random() * 5)];
                star.style.left = (element.getBoundingClientRect().left + Math.random() * 100) + 'px';
                star.style.top = (element.getBoundingClientRect().top + Math.random() * 50) + 'px';
                star.style.animationDelay = (Math.random() * 0.3) + 's';
                document.body.appendChild(star);
                setTimeout(() => star.remove(), 1500);
            }}
        }}
    </script>
</body>
</html>"""
    
    return html
```