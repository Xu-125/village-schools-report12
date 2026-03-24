@ -0,0 +1,1026 @@
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>故土与新途：乡村小学的撤并倒计时</title>
    <style>
        /* === 基础重置 === */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        :root {
            --color-primary: #c41e3a;
            --color-dark: #1a1a1a;
            --color-gray: #666;
            --color-light: #f5f5f0;
            --color-beige: #e8e4dc;
            --font-serif: "Noto Serif SC", "Source Han Serif SC", "SimSun", "Songti SC", serif;
            --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
        }
        
        html {
            scroll-behavior: smooth;
        }
        
        body {
            font-family: var(--font-serif);
            background: #fafafa;
            color: var(--color-dark);
            line-height: 1.8;
            overflow-x: hidden;
        }
        
        /* === 进度条 === */
        .progress-bar {
            position: fixed;
            top: 0;
            left: 0;
            height: 3px;
            background: var(--color-primary);
            z-index: 9999;
            transition: width 0.1s;
        }
        
        /* === 首屏 === */
        .hero {
            height: 100vh;
            background: linear-gradient(180deg, var(--color-light) 0%, var(--color-beige) 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
            position: relative;
        }
        
        .hero-label {
            font-family: var(--font-sans);
            font-size: 11px;
            letter-spacing: 4px;
            color: var(--color-gray);
            text-transform: uppercase;
            margin-bottom: 30px;
        }
        
        .hero h1 {
            font-size: clamp(32px, 8vw, 72px);
            font-weight: 700;
            color: var(--color-dark);
            margin-bottom: 20px;
            line-height: 1.2;
        }
        
        .hero-subtitle {
            font-size: clamp(16px, 3vw, 24px);
            color: var(--color-gray);
            font-style: italic;
            margin-bottom: 40px;
        }
        
        .hero-meta {
            font-family: var(--font-sans);
            font-size: 13px;
            color: #999;
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .scroll-hint {
            position: absolute;
            bottom: 40px;
            animation: bounce 2s infinite;
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }
        
        /* === 章节结构 === */
        .section {
            position: relative;
        }
        
        .sticky-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            min-height: 100vh;
        }
        
        .sticky-left {
            padding: 80px 60px;
            background: #fff;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        
        .sticky-right {
            position: sticky;
            top: 0;
            height: 100vh;
            background: var(--color-beige);
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        .chapter-number {
            font-family: var(--font-sans);
            font-size: 11px;
            color: var(--color-primary);
            letter-spacing: 3px;
            margin-bottom: 15px;
            text-transform: uppercase;
        }
        
        .chapter-title {
            font-size: 32px;
            font-weight: 700;
            margin-bottom: 30px;
            line-height: 1.3;
        }
        
        .chapter-text {
            font-size: 17px;
            color: #444;
            line-height: 2;
            margin-bottom: 25px;
        }
        
        .quote-block {
            border-left: 4px solid var(--color-primary);
            padding: 20px 25px;
            background: #fafafa;
            margin: 30px 0;
        }
        
        .quote-text {
            font-size: 20px;
            font-style: italic;
            color: var(--color-dark);
            line-height: 1.7;
            margin-bottom: 15px;
        }
        
        .quote-author {
            font-size: 14px;
            color: var(--color-gray);
            font-family: var(--font-sans);
        }
        
        /* === 可视化元素 === */
        .viz-card {
            background: #fff;
            padding: 50px;
            border-radius: 8px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.15);
            text-align: center;
            max-width: 400px;
        }
        
        .viz-number {
            font-size: 80px;
            font-weight: 700;
            color: var(--color-primary);
            line-height: 1;
            margin-bottom: 15px;
        }
        
        .viz-label {
            font-size: 20px;
            color: #555;
            margin-bottom: 10px;
        }
        
        .viz-context {
            font-size: 14px;
            color: #888;
            font-style: italic;
        }
        
        /* === 地图 === */
        .map-container {
            background: #fff;
            padding: 60px 40px;
            text-align: center;
        }
        
        .map-title {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 10px;
        }
        
        .map-subtitle {
            font-size: 16px;
            color: var(--color-gray);
            margin-bottom: 40px;
        }
        
        .china-map {
            max-width: 800px;
            margin: 0 auto;
        }
        
        .map-legend {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 30px;
            flex-wrap: wrap;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
            color: #555;
        }
        
        .legend-dot {
            width: 14px;
            height: 14px;
            border-radius: 50%;
            border: 2px solid #fff;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        
        /* === 数据图表 === */
        .data-section {
            background: var(--color-light);
            padding: 80px 40px;
        }
        
        .data-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
            max-width: 1000px;
            margin: 0 auto;
        }
        
        .data-card {
            background: #fff;
            padding: 35px;
            border-radius: 8px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            border-top: 4px solid var(--color-primary);
        }
        
        .data-card-number {
            font-size: 48px;
            font-weight: 700;
            color: var(--color-primary);
            line-height: 1;
            margin-bottom: 10px;
        }
        
        .data-card-unit {
            font-size: 16px;
            color: #888;
        }
        
        .data-card-title {
            font-size: 16px;
            color: #333;
            margin-top: 15px;
            font-weight: 600;
        }
        
        .data-card-desc {
            font-size: 13px;
            color: #666;
            margin-top: 8px;
            line-height: 1.6;
        }
        
        /* === 趋势图 === */
        .trend-chart {
            max-width: 800px;
            margin: 50px auto;
            background: #fff;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
        }
        
        .trend-title {
            font-size: 18px;
            font-weight: 600;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .bar-row {
            display: flex;
            align-items: center;
            margin: 18px 0;
        }
        
        .bar-label {
            width: 60px;
            font-size: 13px;
            color: #666;
            text-align: right;
            padding-right: 15px;
        }
        
        .bar-track {
            flex: 1;
            height: 32px;
            background: #f0f0f0;
            border-radius: 4px;
            position: relative;
            overflow: hidden;
        }
        
        .bar-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--color-primary) 0%, #d4576e 100%);
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 12px;
        }
        
        .bar-value {
            font-size: 13px;
            font-weight: 600;
            color: #fff;
            text-shadow: 0 1px 2px rgba(0,0,0,0.3);
        }
        
        /* === 对比表格 === */
        .compare-table {
            width: 100%;
            max-width: 900px;
            margin: 40px auto;
            border-collapse: collapse;
            background: #fff;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
        }
        
        .compare-table th {
            background: var(--color-primary);
            color: #fff;
            padding: 18px 15px;
            text-align: left;
            font-size: 13px;
            font-weight: 600;
            font-family: var(--font-sans);
        }
        
        .compare-table td {
            padding: 16px 15px;
            border-bottom: 1px solid #eee;
            font-size: 14px;
        }
        
        .compare-table tr:last-child td {
            border-bottom: none;
        }
        
        .compare-table tr:nth-child(even) {
            background: #fafafa;
        }
        
        .compare-table .highlight {
            color: var(--color-primary);
            font-weight: 600;
        }
        
        /* === 时间轴 === */
        .timeline {
            max-width: 600px;
            margin: 40px auto;
            padding: 30px;
            background: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
        }
        
        .timeline-item {
            display: flex;
            align-items: center;
            padding: 20px 0;
            border-bottom: 1px dashed #ddd;
        }
        
        .timeline-item:last-child {
            border-bottom: none;
        }
        
        .timeline-year {
            width: 80px;
            font-size: 24px;
            font-weight: 700;
            color: var(--color-primary);
        }
        
        .timeline-arrow {
            font-size: 24px;
            color: #ccc;
            margin: 0 20px;
        }
        
        .timeline-content {
            flex: 1;
        }
        
        .timeline-title {
            font-size: 16px;
            font-weight: 600;
            color: #333;
        }
        
        .timeline-desc {
            font-size: 13px;
            color: #666;
            margin-top: 5px;
        }
        
        /* === 页脚 === */
        .footer {
            background: var(--color-dark);
            color: #888;
            padding: 60px 40px;
            text-align: center;
        }
        
        .footer h2 {
            color: #fff;
            font-size: 28px;
            margin-bottom: 20px;
        }
        
        .footer p {
            max-width: 600px;
            margin: 0 auto 20px;
            line-height: 1.8;
        }
        
        /* === 响应式 === */
        @media (max-width: 900px) {
            .sticky-container {
                grid-template-columns: 1fr;
            }
            
            .sticky-right {
                position: relative;
                height: auto;
                min-height: 400px;
                order: -1;
            }
            
            .sticky-left {
                padding: 40px 25px;
            }
            
            .chapter-title {
                font-size: 24px;
            }
            
            .chapter-text {
                font-size: 16px;
            }
            
            .quote-text {
                font-size: 18px;
            }
            
            .viz-card {
                padding: 35px 25px;
                margin: 20px;
            }
            
            .viz-number {
                font-size: 60px;
            }
            
            .data-grid {
                grid-template-columns: 1fr;
            }
            
            .compare-table {
                font-size: 12px;
            }
            
            .compare-table th,
            .compare-table td {
                padding: 12px 8px;
            }
        }
        
        /* === 动画 === */
        .fade-in {
            opacity: 0;
            transform: translateY(30px);
            transition: opacity 0.8s, transform 0.8s;
        }
        
        .fade-in.visible {
            opacity: 1;
            transform: translateY(0);
        }
    </style>
</head>
<body>
    <!-- 进度条 -->
    <div class="progress-bar" id="progressBar"></div>
    
    <!-- 首屏 -->
    <header class="hero">
        <div class="hero-label">A Visual Report / 数据新闻可视化</div>
        <h1>故土与新途</h1>
        <p class="hero-subtitle">乡村小学的撤并倒计时</p>
        <div class="hero-meta">
            <span>六省实地采访</span>
            <span>·</span>
            <span>湖南 · 陕西 · 河北 · 河南 · 山东 · 广西</span>
            <span>·</span>
            <span>2026年3月</span>
        </div>
        <div class="scroll-hint">
            <svg width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="#666" stroke-width="2">
                <path d="M12 5v14M19 12l-7 7-7-7"/>
            </svg>
        </div>
    </header>

    <!-- 地图章节 -->
    <section class="map-container">
        <h2 class="map-title">消失的坐标</h2>
        <p class="map-subtitle">我们在六省记录下乡村小学的最后时光</p>
        
        <div class="china-map">
            <svg viewBox="0 0 700 500" style="width:100%;max-width:700px;">
                <!-- 简化的中国地图轮廓 -->
                <defs>
                    <linearGradient id="landGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" style="stop-color:#f5f5f0;stop-opacity:1" />
                        <stop offset="100%" style="stop-color:#e8e4dc;stop-opacity:1" />
                    </linearGradient>
                </defs>
                
                <!-- 中国轮廓 -->
                <path d="M180,80 
                         Q250,60 320,70 
                         L400,65 
                         Q450,60 480,80
                         L520,100
                         Q550,120 540,150
                         L530,180
                         Q520,220 540,250
                         L550,300
                         Q540,350 520,380
                         L480,420
                         Q450,450 400,440
                         L350,430
                         Q300,440 250,420
                         L200,400
                         Q160,380 140,340
                         L120,300
                         Q100,250 110,200
                         L120,150
                         Q130,100 180,80Z" 
                      fill="url(#landGradient)" 
                      stroke="#ccc" 
                      stroke-width="2"/>
                
                <!-- 省份标注 -->
                <!-- 黑龙江/东北 -->
                <text x="520" y="100" font-size="12" fill="#666">黑龙江</text>
                
                <!-- 河北 -->
                <circle cx="460" cy="180" r="8" fill="#d4a574" stroke="#fff" stroke-width="2"/>
                <text x="460" y="165" font-size="10" text-anchor="middle" fill="#333">河北</text>
                <text x="460" y="205" font-size="9" text-anchor="middle" fill="#888">衡水</text>
                
                <!-- 山东 -->
                <circle cx="490" cy="210" r="8" fill="#d4a574" stroke="#fff" stroke-width="2"/>
                <text x="490" y="195" font-size="10" text-anchor="middle" fill="#333">山东</text>
                <text x="490" y="230" font-size="9" text-anchor="middle" fill="#888">青岛</text>
                
                <!-- 河南 -->
                <circle cx="420" cy="220" r="8" fill="#d4a574" stroke="#fff" stroke-width="2"/>
                <text x="420" y="205" font-size="10" text-anchor="middle" fill="#333">河南</text>
                <text x="420" y="240" font-size="9" text-anchor="middle" fill="#888">开封</text>
                
                <!-- 陕西 - 主要 -->
                <circle cx="340" cy="210" r="12" fill="#c41e3a" stroke="#fff" stroke-width="3"/>
                <text x="340" y="185" font-size="12" text-anchor="middle" fill="#c41e3a" font-weight="bold">陕西</text>
                <text x="340" y="240" font-size="10" text-anchor="middle" fill="#666">丰仪镇</text>
                <text x="340" y="255" font-size="9" text-anchor="middle" fill="#888">15所→4所</text>
                
                <!-- 湖南 - 主要 -->
                <circle cx="420" cy="320" r="12" fill="#c41e3a" stroke="#fff" stroke-width="3"/>
                <text x="420" y="295" font-size="12" text-anchor="middle" fill="#c41e3a" font-weight="bold">湖南</text>
                <text x="420" y="350" font-size="10" text-anchor="middle" fill="#666">云山学校</text>
                <text x="420" y="365" font-size="9" text-anchor="middle" fill="#888">仅剩14人</text>
                
                <!-- 广西 -->
                <circle cx="380" cy="380" r="8" fill="#d4a574" stroke="#fff" stroke-width="2"/>
                <text x="380" y="365" font-size="10" text-anchor="middle" fill="#333">广西</text>
                <text x="380" y="400" font-size="9" text-anchor="middle" fill="#888">补充采访</text>
                
                <!-- 南海诸岛示意 -->
                <rect x="520" y="400" width="40" height="30" fill="#f0f0f0" stroke="#ccc" stroke-width="1" rx="3"/>
                <text x="540" y="420" font-size="10" text-anchor="middle" fill="#666">南海</text>
            </svg>
        </div>
        
        <div class="map-legend">
            <div class="legend-item">
                <div class="legend-dot" style="background:#c41e3a;"></div>
                <span>主采访地（深度报道）</span>
            </div>
            <div class="legend-item">
                <div class="legend-dot" style="background:#d4a574;"></div>
                <span>补充采访地</span>
            </div>
        </div>
    </section>

    <!-- 数据章节 -->
    <section class="data-section">
        <div style="text-align:center;margin-bottom:50px;">
            <div class="chapter-number">DATA OVERVIEW</div>
            <h2 class="chapter-title" style="margin-bottom:15px;">数字里的告别</h2>
            <p style="color:#666;max-width:600px;margin:0 auto;">13年间，超过4.6万所乡村小学消失。这不是抽象的数字，而是一个个具体童年记忆的终结。</p>
        </div>
        
        <div class="data-grid">
            <div class="data-card fade-in">
                <div class="data-card-number">12.4<span class="data-card-unit">万所</span></div>
                <div class="data-card-title">2011年全国乡村小学</div>
                <div class="data-card-desc">那时几乎每个村庄都有自己的小学</div>
            </div>
            <div class="data-card fade-in">
                <div class="data-card-number">7.8<span class="data-card-unit">万所</span></div>
                <div class="data-card-title">2024年全国乡村小学</div>
                <div class="data-card-desc">13年间减少4.6万所</div>
            </div>
            <div class="data-card fade-in">
                <div class="data-card-number">37<span class="data-card-unit">%</span></div>
                <div class="data-card-title">学校数量降幅</div>
                <div class="data-card-desc">同期学生减少40%</div>
            </div>
        </div>
        
        <!-- 趋势图 -->
        <div class="trend-chart">
            <div class="trend-title">全国乡村小学数量变化趋势（2011-2024）</div>
            <div class="bar-row">
                <div class="bar-label">2011</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width:100%;">
                        <span class="bar-value">12.4万所</span>
                    </div>
                </div>
            </div>
            <div class="bar-row">
                <div class="bar-label">2014</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width:81%;">
                        <span class="bar-value">10.1万所</span>
                    </div>
                </div>
            </div>
            <div class="bar-row">
                <div class="bar-label">2017</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width:74%;">
                        <span class="bar-value">9.2万所</span>
                    </div>
                </div>
            </div>
            <div class="bar-row">
                <div class="bar-label">2021</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width:65%;">
                        <span class="bar-value">8.0万所</span>
                    </div>
                </div>
            </div>
            <div class="bar-row">
                <div class="bar-label">2024</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width:63%;">
                        <span class="bar-value">7.8万所</span>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- 对比表格 -->
        <table class="compare-table">
            <thead>
                <tr>
                    <th>调研地点</th>
                    <th>时间节点</th>
                    <th>学生/学校规模</th>
                    <th>状态</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td class="highlight">湖南云山学校</td>
                    <td>2026春</td>
                    <td>14名学生，2个年级</td>
                    <td>2026下半年预计撤并</td>
                </tr>
                <tr>
                    <td class="highlight">陕西丰仪镇</td>
                    <td>2011→2026</td>
                    <td>15所→4所小学</td>
                    <td>15年撤并率73%</td>
                </tr>
                <tr>
                    <td>河北故城县</td>
                    <td>2000→2026</td>
                    <td>58村小→9所</td>
                    <td>持续萎缩</td>
                </tr>
                <tr>
                    <td>山东青岛青山</td>
                    <td>2022→2024</td>
                    <td>22人→0人</td>
                    <td>已改办幼儿园</td>
                </tr>
                <tr>
                    <td>河南开封杞县</td>
                    <td>2026</td>
                    <td>部分仅6名学生</td>
                    <td>1人也不停办</td>
                </tr>
                <tr>
                    <td>广西</td>
                    <td>2026</td>
                    <td>补充采访</td>
                    <td>持续调研中</td>
                </tr>
            </tbody>
        </table>
    </section>

    <!-- 故事章节1：湖南倒计时 -->
    <section class="section">
        <div class="sticky-container">
            <div class="sticky-left">
                <div class="chapter-number">Chapter 01</div>
                <h2 class="chapter-title">倒计时：14个学生的学校</h2>
                
                <p class="chapter-text">
                    2026年2月，21岁的尹佳琪打开了云山学校的大门。她是这所学校的校长，同时也教二年级语文、三年级英语、二三年级音乐，还是二年级班主任。
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"这个学期只有14个学生，上学期还有15个，转走了一个。应该是今年下半年就撤并了。"</p>
                    <p class="quote-author">—— 尹佳琪，05年出生，21岁校长</p>
                </div>
                
                <p class="chapter-text">
                    尹佳琪是2005年出生的。刚毕业的她，甚至不知道自己是怎么被选为校长的。"当时很震惊，也很害怕。"她的妈妈说："是不是这学校有什么贪污，让你来背锅？"
                </p>
                
                <p class="chapter-text">
                    "最开始还比较开心吧，因为就是能一下子就走到了巅峰。其实后面想想还是有点害怕，不知所措。"
                </p>
            </div>
            <div class="sticky-right">
                <div class="viz-card">
                    <div class="viz-number">14</div>
                    <div class="viz-label">名学生</div>
                    <div class="viz-context">这应该是学校的最后一个学期<br>2026年下半年即将撤并</div>
                </div>
            </div>
        </div>
    </section>

    <!-- 故事章节2：60公里 -->
    <section class="section">
        <div class="sticky-container">
            <div class="sticky-left" style="background:#f8f8f6;">
                <div class="chapter-number">Chapter 02</div>
                <h2 class="chapter-title">60公里：童年的距离</h2>
                
                <p class="chapter-text">
                    曾湘欧今年7岁，读二年级。当被问起家离学校有多远时，他说："60米。"然后又说："走路要一个小时。"最后改口："60公里。"
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"我妈妈跟别的男的跑了，我爸去长沙工作了，差不多一个月才能回来。"</p>
                    <p class="quote-author">—— 曾湘欧，7岁</p>
                </div>
                
                <p class="chapter-text">
                    实际上，他家离学校大约6公里。每天，他要么自己走路上学（1小时），要么爷爷骑车送他（50分钟）。下雨天，他就打着伞走。
                </p>
                
                <p class="chapter-text">
                    他的好朋友刘瑾宸、陈磊，都已经不在村里上学了——"他们去娄底，去贵州上学了，因为太远了。"
                </p>
            </div>
            <div class="sticky-right" style="background:#e8e4dc;">
                <div class="viz-card">
                    <div class="viz-number">60</div>
                    <div class="viz-label">公里</div>
                    <div class="viz-context">一个孩子对距离最夸张的想象<br>也是童年对远方的距离</div>
                </div>
            </div>
        </div>
    </section>

    <!-- 故事章节3：陕西变迁 -->
    <section class="section">
        <div class="sticky-container">
            <div class="sticky-left">
                <div class="chapter-number">Chapter 03</div>
                <h2 class="chapter-title">从15所到4所</h2>
                
                <p class="chapter-text">
                    陕西省兴平市丰仪镇，曾经是典型的"一村一校"布局。2011年，全镇有15所小学。如今，只剩下4所。
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"过去我们在丰仪村小，有380个娃，全乡才300个，我们村比全乡还多。"</p>
                    <p class="quote-author">—— 退休校长，1976-2015年任教</p>
                </div>
                
                <p class="chapter-text">
                    在策村小学，41岁的张娜老师已经教了16年书。她所在的学校现在有80多名学生，但一年级只有6个人，三年级也只有6个人。
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"其他周围的好像都撤并了，都撤了，就留了一个这个学校，还有一个中心校。家庭条件不太好的才在这上，条件稍微好一些的，娃子都在城里上学了。"</p>
                    <p class="quote-author">—— 张娜，策村小学教师</p>
                </div>
            </div>
            <div class="sticky-right">
                <div class="timeline">
                    <div class="timeline-item">
                        <div class="timeline-year">2011</div>
                        <div class="timeline-arrow">→</div>
                        <div class="timeline-content">
                            <div class="timeline-title">15所小学</div>
                            <div class="timeline-desc">"一村一校"鼎盛时期</div>
                        </div>
                    </div>
                    <div class="timeline-item">
                        <div class="timeline-year">2015</div>
                        <div class="timeline-arrow">→</div>
                        <div class="timeline-content">
                            <div class="timeline-title">开始大规模撤并</div>
                            <div class="timeline-desc">学生逐渐流失</div>
                        </div>
                    </div>
                    <div class="timeline-item">
                        <div class="timeline-year">2026</div>
                        <div class="timeline-arrow"></div>
                        <div class="timeline-content">
                            <div class="timeline-title">4所小学</div>
                            <div class="timeline-desc">73%的学校已消失</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 故事章节4：教师 -->
    <section class="section">
        <div class="sticky-container">
            <div class="sticky-left" style="background:#f8f8f6;">
                <div class="chapter-number">Chapter 04</div>
                <h2 class="chapter-title">一个人的全科</h2>
                
                <p class="chapter-text">
                    在乡村小学，"身兼多职"不是选择，而是生存方式。
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"老人基本上都辅导不了，农村又没有其他托管机构，所有责任全部压在学校。"</p>
                    <p class="quote-author">—— 张娜</p>
                </div>
                
                <p class="chapter-text">
                    尹佳琪，21岁，教二年级语文、三年级英语、二三年级音乐，还要当二年级班主任和校长。每周大约25节课。
                </p>
                
                <p class="chapter-text">
                    张娜，41岁，教英语+科学。陈志辉，50多岁，中文系毕业，教三年级数学、法治、信息。
                </p>
                
                <div class="quote-block">
                    <p class="quote-text">"我也尽力而为了，走了这么久，凭良心做事。"</p>
                    <p class="quote-author">—— 陈志辉，30多年教龄</p>
                </div>
            </div>
            <div class="sticky-right" style="background:#e8e4dc;">
                <div class="viz-card">
                    <div style="font-size:18px;color:#555;margin-bottom:20px;">一位乡村教师的日常</div>
                    <div style="background:#f5f5f5;padding:25px;border-radius:8px;margin-bottom:15px;">
                        <div style="font-size:48px;font-weight:700;color:var(--color-primary);">4</div>
                        <div style="color:#666;">门课程</div>
                    </div>
                    <div style="font-size:14px;color:#888;line-height:1.8;">
                        语文 · 英语 · 音乐<br>
                        + 班主任 + 校长<br>
                        每周25节课
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 故事章节5：尾声 -->
    <section class="section">
        <div class="sticky-container">
            <div class="sticky-left">
                <div class="chapter-number">Epilogue</div>
                <h2 class="chapter-title">回声</h2>
                
                <p class="chapter-text">
                    2026年春，某个周三下午，尹佳琪锁上了云山学校的校门。14个学生三三两两散去，廖雨辰留在宿舍——周末才回外婆家。
                </p>
                
                <p class="chapter-text">
                    在陕西，退休校长偶尔会路过高家小学旧址。那里门窗歪斜，荒草疯长。他曾在这里见证过380个学生的盛况。
               </p>
                
                <div class="quote-block">
                    <p class="quote-text">"刚开始可难受了，后来习惯了，反正大家都差不多，都是被学校赶出来的。"</p>
                    <p class="quote-author">—— 彭旭童，转学生</p>
                </div>
                
                <p class="chapter-text">
                    "习惯了"——这是最轻的词汇，也是最重的结语。
                </p>
            </div>
            <div class="sticky-right" style="background:#1a1a1a;color:#fff;">
                <div style="text-align:center;">
                    <div style="font-size:100px;font-weight:700;color:var(--color-primary);line-height:1;">60</div>
                    <div style="font-size:24px;margin:15px 0;">公里</div>
                    <div style="font-style:italic;opacity:0.8;line-height:1.8;">
                        "是真实的距离<br>也是童年的比例尺"
                    </div>
                    <div style="margin-top:30px;font-size:14px;opacity:0.5;">
                        ——曾湘欧，7岁
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 页脚 -->
    <footer class="footer">
        <h2>故土与新途</h2>
        <p>
            本报道基于2026年2月至3月在湖南、陕西、河北、河南、山东、广西六省的实地采访。<br>
            所有数据与引语均来自真实采访记录。
        </p>
        <p style="font-size:13px;opacity:0.6;margin-top:30px;">
            完全离线版本 · 无需网络 · 双击即用
        </p>
    </footer>

    <!-- 交互脚本 -->
    <script>
        // 进度条
        window.addEventListener('scroll', () => {
            const scrollTop = window.scrollY;
            const docHeight = document.documentElement.scrollHeight - window.innerHeight;
            const progress = (scrollTop / docHeight) * 100;
            document.getElementById('progressBar').style.width = progress + '%';
        });
        
        // 滚动动画
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };
        
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, observerOptions);
        
        document.querySelectorAll('.fade-in').forEach(el => {
            observer.observe(el);
        });
    </script>
</body>
</html>
