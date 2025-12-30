<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>考研复试助手 - 网页版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", sans-serif;
        }
        body {
            background-color: #f5f5f5;
            padding: 20px;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        .tab-header {
            display: flex;
            background-color: #2c3e50;
        }
        .tab-btn {
            flex: 1;
            text-align: center;
            padding: 15px;
            color: #fff;
            border: none;
            background: none;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        .tab-btn.active {
            background-color: #34495e;
            border-bottom: 3px solid #3498db;
        }
        .tab-content {
            padding: 25px;
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .title {
            font-size: 20px;
            margin-bottom: 20px;
            color: #2c3e50;
            font-weight: bold;
        }
        .select-box {
            margin-bottom: 20px;
        }
        .select-box select {
            width: 200px;
            padding: 8px 12px;
            border-radius: 5px;
            border: 1px solid #ddd;
            font-size: 14px;
        }
        .btn {
            padding: 10px 20px;
            background-color: #3498db;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px 0;
        }
        .btn:hover {
            background-color: #2980b9;
        }
        .question-card {
            border: 2px solid #3498db;
            border-radius: 8px;
            padding: 20px;
            margin: 20px 0;
        }
        .question-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: #2c3e50;
        }
        .question-content {
            font-size: 18px;
            line-height: 1.6;
        }
        .score-item {
            display: flex;
            align-items: center;
            margin: 15px 0;
        }
        .score-item label {
            width: 150px;
            font-size: 14px;
        }
        .score-item input {
            width: 80px;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 3px;
            margin-right: 10px;
        }
        .score-rule {
            font-size: 12px;
            color: #666;
            margin-left: 10px;
        }
        .total-score {
            font-size: 18px;
            font-weight: bold;
            margin: 20px 0;
            color: #e74c3c;
        }
        .textarea-box {
            margin: 20px 0;
        }
        .textarea-box textarea {
            width: 100%;
            height: 100px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            resize: vertical;
        }
        .record-item {
            border-bottom: 1px dashed #ddd;
            padding: 10px 0;
            margin: 10px 0;
        }
        .grid-box {
            display: flex;
            gap: 20px;
        }
        .grid-left {
            flex: 1;
        }
        .grid-right {
            flex: 1;
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            max-height: 600px;
            overflow-y: auto;
        }
        ul {
            padding-left: 20px;
            margin: 10px 0;
        }
        li {
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 顶部Tab按钮 -->
        <div class="tab-header">
            <button class="tab-btn active" data-tab="tab1">随机抽题</button>
            <button class="tab-btn" data-tab="tab2">问题库</button>
            <button class="tab-btn" data-tab="tab3">复试流程</button>
            <button class="tab-btn" data-tab="tab4">自定义问题</button>
            <button class="tab-btn" data-tab="tab5">标准化打分</button>
            <button class="tab-btn" data-tab="tab6">评分记录</button>
        </div>

        <!-- 1. 随机抽题 -->
        <div class="tab-content active" id="tab1">
            <div class="title">随机抽题</div>
            <div class="select-box">
                <select id="questionType">
                    <option value="全部">全部类型</option>
                    <option value="自我介绍类">自我介绍类</option>
                    <option value="专业知识类">专业知识类</option>
                    <option value="综合能力类">综合能力类</option>
                    <option value="英语问答类">英语问答类</option>
                </select>
                <button class="btn" onclick="drawQuestion()">抽取随机问题</button>
            </div>
            <div class="question-card" id="questionCard" style="display: none;">
                <div class="question-title">当前问题</div>
                <div class="question-content" id="currentQuestion"></div>
            </div>
        </div>

        <!-- 2. 问题库 -->
        <div class="tab-content" id="tab2">
            <div class="title">复试问题库</div>
            <div id="questionBank"></div>
        </div>

        <!-- 3. 复试流程 -->
        <div class="tab-content" id="tab3">
            <div class="title">考研复试通用流程</div>
            <ul>
                <li><strong>1. 报到</strong>：携带身份证、准考证、成绩单等材料到指定地点报到</li>
                <li><strong>2. 抽签</strong>：确定面试顺序和抽题顺序</li>
                <li><strong>3. 面试环节</strong>：
                    <ul>
                        <li>自我介绍（中文/英文，2-3分钟）</li>
                        <li>专业问题回答（抽题或老师随机提问）</li>
                        <li>综合问答（个人情况、科研潜力、职业规划等）</li>
                        <li>英文问答（部分院校要求）</li>
                    </ul>
                </li>
                <li><strong>4. 评分</strong>：面试官根据表现现场打分</li>
                <li><strong>5. 离场</strong>：完成所有环节后签字离场</li>
            </ul>
            <div class="title" style="margin-top: 20px;">复试注意事项</div>
            <ul>
                <li>着装整洁大方，避免过于随意</li>
                <li>回答问题逻辑清晰，语速适中</li>
                <li>遇到不会的问题如实说明，不要不懂装懂</li>
                <li>保持礼貌和自信，眼神交流自然</li>
            </ul>
        </div>

        <!-- 4. 自定义问题 -->
        <div class="tab-content" id="tab4">
            <div class="title">添加自定义问题</div>
            <div class="select-box">
                <select id="customType">
                    <option value="自我介绍类">自我介绍类</option>
                    <option value="专业知识类">专业知识类</option>
                    <option value="综合能力类">综合能力类</option>
                    <option value="英语问答类">英语问答类</option>
                </select>
            </div>
            <div class="textarea-box">
                <textarea id="customContent" placeholder="请输入问题内容"></textarea>
            </div>
            <button class="btn" onclick="addCustomQuestion()">添加问题</button>
        </div>

        <!-- 5. 标准化打分 -->
        <div class="tab-content" id="tab5">
            <div class="title">标准化打分（总分100分）</div>
            <div class="grid-box">
                <div class="grid-left">
                    <div class="question-card" id="scoreQuestionCard" style="display: none;">
                        <div class="question-title">当前回答问题</div>
                        <div class="question-content" id="scoreCurrentQuestion"></div>
                    </div>
                    <button class="btn" onclick="loadCurrentQuestion()">加载最新抽取的问题</button>
                    
                    <div class="score-item">
                        <label>语言表达（20分）：</label>
                        <input type="number" id="score1" min="0" max="20" value="0">
                    </div>
                    <div class="score-item">
                        <label>逻辑思维（20分）：</label>
                        <input type="number" id="score2" min="0" max="20" value="0">
                    </div>
                    <div class="score-item">
                        <label>专业知识（30分）：</label>
                        <input type="number" id="score3" min="0" max="30" value="0">
                    </div>
                    <div class="score-item">
                        <label>应变能力（15分）：</label>
                        <input type="number" id="score4" min="0" max="15" value="0">
                    </div>
                    <div class="score-item">
                        <label>态度礼仪（15分）：</label>
                        <input type="number" id="score5" min="0" max="15" value="0">
                    </div>

                    <div class="total-score" id="totalScore">本次总分：0 / 100分</div>
                    <button class="btn" onclick="calculateTotal()">计算总分</button>
                    <button class="btn" onclick="resetScore()">重置评分</button>

                    <div class="textarea-box">
                        <textarea id="scoreComment" placeholder="打分依据（必填：说明加分/扣分原因）"></textarea>
                    </div>
                    <button class="btn" onclick="saveScoreRecord()">保存评分记录</button>
                </div>
                <div class="grid-right">
                    <div class="title" style="font-size: 16px;">评分细则（对照打分）</div>
                    <div>
                        <p><strong>语言表达（20分）</strong></p>
                        <p>优秀(16-20)：流畅自然，逻辑清晰，用词准确</p>
                        <p>良好(12-15)：较流畅，偶有口误不影响理解</p>
                        <p>合格(8-11)：基本通顺，逻辑有瑕疵</p>
                        <p>不合格(0-7)：卡顿严重，逻辑混乱</p>
                        <hr>
                        <p><strong>逻辑思维（20分）</strong></p>
                        <p>优秀(16-20)：结构清晰，论点明确，论据充分</p>
                        <p>良好(12-15)：结构较清晰，有一定论据支撑</p>
                        <p>合格(8-11)：结构基本完整，论点模糊</p>
                        <p>不合格(0-7)：结构混乱，无有效论据</p>
                        <hr>
                        <p><strong>专业知识（30分）</strong></p>
                        <p>优秀(24-30)：概念精准，能结合实例/前沿拓展</p>
                        <p>良好(18-23)：概念准确，少量细节瑕疵</p>
                        <p>合格(12-17)：概念不扎实，回答片面</p>
                        <p>不合格(0-11)：概念混淆，答非所问</p>
                        <hr>
                        <p><strong>应变能力（15分）</strong></p>
                        <p>优秀(12-15)：不慌乱，能关联相关知识</p>
                        <p>良好(9-11)：较冷静，能给基本思路</p>
                        <p>合格(6-8)：略有怯场，勉强回答</p>
                        <p>不合格(0-5)：语无伦次，回避问题</p>
                        <hr>
                        <p><strong>态度礼仪（15分）</strong></p>
                        <p>优秀(12-15)：礼貌谦逊，眼神交流自然</p>
                        <p>良好(9-11)：礼貌规范，态度诚恳</p>
                        <p>合格(6-8)：基本礼貌，偶有小动作</p>
                        <p>不合格(0-5)：态度傲慢，缺乏礼貌</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 6. 评分记录 -->
        <div class="tab-content" id="tab6">
            <div class="title">历史评分记录</div>
            <button class="btn" onclick="refreshRecords()">刷新记录</button>
            <div id="recordList"></div>
        </div>
    </div>

    <script>
        // 预设问题库
        let questionData = {
            "自我介绍类": [
                "请做一下自我介绍",
                "你为什么选择跨考计算机专业？",
                "你的优缺点是什么？",
                "你研究生阶段的规划是什么？"
            ],
            "专业知识类": [
                "请简述计算机网络OSI七层模型",
                "什么是进程和线程？区别是什么？",
                "简述排序算法的分类及各自特点",
                "计算机组成原理中，CPU的主要功能是什么？"
            ],
            "综合能力类": [
                "你考研过程中遇到的最大挑战是什么？如何克服的？",
                "如果这次复试失败了，你会怎么办？",
                "你做过的最有成就感的一件事是什么？",
                "你有什么想问老师的？"
            ],
            "英语问答类": [
                "Please introduce yourself briefly",
                "Why do you choose our university?",
                "What are your strengths and weaknesses?",
                "What are your research plans for postgraduate study?"
            ]
        };

        // 本地存储：加载自定义问题
        if (localStorage.getItem('customQuestions')) {
            const custom = JSON.parse(localStorage.getItem('customQuestions'));
            for (let type in custom) {
                if (!questionData[type]) questionData[type] = [];
                questionData[type] = [...questionData[type], ...custom[type]];
            }
        }

        // 当前抽取的问题
        let currentQuestion = "";

        // Tab切换
        document.querySelectorAll('.tab-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                const tabId = this.dataset.tab;
                document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
                document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                this.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });

        // 初始化问题库
        function initQuestionBank() {
            const bank = document.getElementById('questionBank');
            bank.innerHTML = "";
            for (let type in questionData) {
                let typeHtml = `<div><h3 style="margin:10px 0;color:#3498db;">${type}</h3><ul>`;
                questionData[type].forEach(q => {
                    typeHtml += `<li>${q}</li>`;
                });
                typeHtml += `</ul></div><hr>`;
                bank.innerHTML += typeHtml;
            }
        }

        // 随机抽题
        function drawQuestion() {
            const type = document.getElementById('questionType').value;
            let allQuestions = [];
            
            if (type === "全部") {
                for (let t in questionData) {
                    allQuestions = [...allQuestions, ...questionData[t]];
                }
            } else {
                allQuestions = questionData[type] || [];
            }

            if (allQuestions.length === 0) {
                alert("暂无该类型问题！");
                return;
            }

            const randomIdx = Math.floor(Math.random() * allQuestions.length);
            currentQuestion = allQuestions[randomIdx];
            
            const card = document.getElementById('questionCard');
            document.getElementById('currentQuestion').textContent = currentQuestion;
            card.style.display = "block";
        }

        // 添加自定义问题
        function addCustomQuestion() {
            const type = document.getElementById('customType').value;
            const content = document.getElementById('customContent').value.trim();
            
            if (!content) {
                alert("请输入问题内容！");
                return;
            }

            if (!questionData[type]) questionData[type] = [];
            questionData[type].push(content);

            // 保存到本地存储
            let custom = JSON.parse(localStorage.getItem('customQuestions')) || {};
            if (!custom[type]) custom[type] = [];
            custom[type].push(content);
            localStorage.setItem('customQuestions', JSON.stringify(custom));

            alert("添加成功！");
            document.getElementById('customContent').value = "";
            initQuestionBank();
        }

        // 加载当前问题到打分页面
        function loadCurrentQuestion() {
            if (!currentQuestion) {
                alert("请先到随机抽题页面抽取问题！");
                return;
            }
            document.getElementById('scoreCurrentQuestion').textContent = currentQuestion;
            document.getElementById('scoreQuestionCard').style.display = "block";
        }

        // 计算总分
        function calculateTotal() {
            const s1 = parseInt(document.getElementById('score1').value) || 0;
            const s2 = parseInt(document.getElementById('score2').value) || 0;
            const s3 = parseInt(document.getElementById('score3').value) || 0;
            const s4 = parseInt(document.getElementById('score4').value) || 0;
            const s5 = parseInt(document.getElementById('score5').value) || 0;

            if (s1 > 20 || s2 > 20 || s3 > 30 || s4 > 15 || s5 > 15) {
                alert("分数不能超过各维度满分！");
                return;
            }

            const total = s1 + s2 + s3 + s4 + s5;
            document.getElementById('totalScore').textContent = `本次总分：${total} / 100分`;
        }

        // 重置评分
        function resetScore() {
            document.getElementById('score1').value = 0;
            document.getElementById('score2').value = 0;
            document.getElementById('score3').value = 0;
            document.getElementById('score4').value = 0;
            document.getElementById('score5').value = 0;
            document.getElementById('totalScore').textContent = "本次总分：0 / 100分";
            document.getElementById('scoreComment').value = "";
        }

        // 保存评分记录
        function saveScoreRecord() {
            const comment = document.getElementById('scoreComment').value.trim();
            const total = document.getElementById('totalScore').textContent.split('：')[1].split(' ')[0];
            const question = document.getElementById('scoreCurrentQuestion').textContent || "无";
            
            if (total === "0") {
                alert("请先计算总分！");
                return;
            }
            if (!comment) {
                alert("请填写打分依据！");
                return;
            }

            const record = {
                time: new Date().toLocaleString(),
                question: question,
                score1: document.getElementById('score1').value,
                score2: document.getElementById('score2').value,
                score3: document.getElementById('score3').value,
                score4: document.getElementById('score4').value,
                score5: document.getElementById('score5').value,
                total: total,
                comment: comment
            };

            let records = JSON.parse(localStorage.getItem('scoreRecords')) || [];
            records.unshift(record);
            localStorage.setItem('scoreRecords', JSON.stringify(records));

            alert("记录保存成功！");
            resetScore();
        }

        // 刷新评分记录
        function refreshRecords() {
            const records = JSON.parse(localStorage.getItem('scoreRecords')) || [];
            const list = document.getElementById('recordList');
            
            if (records.length === 0) {
                list.innerHTML = "<p>暂无评分记录</p>";
                return;
            }

            list.innerHTML = "";
            records.forEach(r => {
                const item = `
                <div class="record-item">
                    <p><strong>时间</strong>：${r.time}</p>
                    <p><strong>问题</strong>：${r.question}</p>
                    <p><strong>评分</strong>：语言表达(${r.score1}) 逻辑思维(${r.score2}) 专业知识(${r.score3}) 应变能力(${r.score4}) 态度礼仪(${r.score5})</p>
                    <p><strong>总分</strong>：${r.total}分</p>
                    <p><strong>打分依据</strong>：${r.comment}</p>
                </div>`;
                list.innerHTML += item;
            });
        }

        // 页面初始化
        window.onload = function() {
            initQuestionBank();
        };
    </script>
</body>
</html>
