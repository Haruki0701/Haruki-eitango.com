<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>プレミアム版 英単語サプリ100</title>
    <style>
        :root {
            --bg-0: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            --correct-flash: rgba(40, 167, 69, 0.15);
            --incorrect-flash: rgba(220, 53, 69, 0.15);
        }

        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background: var(--bg-0);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
            transition: background 0.4s ease;
        }

        /* 画面フラッシュ演出用 */
        body.flash-correct { background: var(--correct-flash); }
        body.flash-incorrect { background: var(--incorrect-flash); }

        /* --- マイページ（学習記録） --- */
        .mypage-box {
            background: #f8fafc;
            border-radius: 16px;
            padding: 15px;
            margin-bottom: 25px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            border: 1px solid #e2e8f0;
        }
        .stat-card {
            background: white;
            padding: 10px;
            border-radius: 12px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
        }
        .stat-val {
            font-size: 20px;
            font-weight: bold;
            color: #1e3c72;
        }
        .stat-lbl {
            font-size: 11px;
            color: #64748b;
            margin-top: 2px;
        }

        /* --- カテゴリセレクター --- */
        .category-select-box {
            margin-bottom: 25px;
            text-align: left;
        }
        .select-label {
            font-size: 13px;
            font-weight: bold;
            color: #475569;
            margin-bottom: 6px;
            display: block;
        }
        .custom-select {
            width: 100%;
            padding: 12px;
            border-radius: 12px;
            border: 2px solid #e2e8f0;
            font-size: 14px;
            font-weight: bold;
            color: #334155;
            background-color: white;
            cursor: pointer;
            outline: none;
        }
        .custom-select:focus {
            border-color: #1a73e8;
        }

        /* --- メニューコンテナ --- */
        .menu-container {
            background-color: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(10px);
            padding: 35px 25px;
            border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.25);
            text-align: center;
            width: 100%;
            max-width: 460px;
            box-sizing: border-box;
            animation: fadeIn 0.4s ease forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .menu-title {
            font-size: 30px;
            font-weight: 800;
            margin: 0 0 5px 0;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .menu-subtitle {
            font-size: 13px;
            color: #666;
            margin: 0 0 25px 0;
            font-weight: bold;
        }
        .menu-grid {
            display: grid;
            gap: 14px;
            margin-bottom: 25px;
        }
        .menu-item {
            position: relative;
            display: flex;
            align-items: center;
            padding: 16px 18px;
            background: #ffffff;
            border: 2px solid #e2e8f0;
            border-radius: 16px;
            cursor: pointer;
            transition: all 0.25s ease;
            text-align: left;
        }
        .menu-item:hover {
            transform: translateY(-2px);
            border-color: #1a73e8;
            box-shadow: 0 8px 16px rgba(26, 115, 232, 0.08);
            background: #f8faff;
        }
        .menu-item.item-recom { border-color: #34a853; background: #fbfcfa; }
        .menu-item.item-recom:hover { border-color: #34a853; background: #f4faf5; }
        .menu-item.item-weak { border-color: #dc3545; background: #fffdfd; }
        .menu-item.item-weak:hover { border-color: #dc3545; background: #fff5f5; }
        
        .menu-item:disabled, .menu-item.disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none !important;
            box-shadow: none !important;
            background: #f1f5f9 !important;
            border-color: #e2e8f0 !important;
        }

        .menu-badge {
            position: absolute;
            top: -9px;
            right: 15px;
            background: #34a853;
            color: white;
            font-size: 10px;
            font-weight: bold;
            padding: 2px 7px;
            border-radius: 10px;
        }
        .menu-badge.badge-weak { background: #dc3545; }

        .menu-icon {
            font-size: 24px;
            margin-right: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 42px;
            height: 42px;
            background: #f1f5f9;
            border-radius: 12px;
        }
        .menu-info { display: flex; flex-direction: column; }
        .menu-item-title { font-size: 16px; font-weight: bold; color: #1e293b; }
        .menu-item-desc { font-size: 12px; color: #64748b; margin-top: 1px; }

        /* --- クイズコンテナ --- */
        .quiz-container {
            background-color: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            text-align: center;
            width: 100%;
            max-width: 460px;
            box-sizing: border-box;
            animation: fadeIn 0.3s ease;
        }
        .mode-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            background-color: #e8f0fe;
            color: #1a73e8;
        }
        .progress-box {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 13px;
            color: #64748b;
            margin: 12px 0 6px 0;
        }
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #e2e8f0;
            border-radius: 3px;
            margin-bottom: 20px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background-color: #34a853;
            width: 0%;
            transition: width 0.3s ease;
        }
        .word-container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
            margin: 20px 0;
        }
        .word {
            font-size: 36px;
            font-weight: bold;
            color: #1a73e8;
            cursor: pointer;
        }
        .audio-btn { background: none; border: none; font-size: 22px; cursor: pointer; }
        .options { display: grid; gap: 10px; }
        
        button.option-btn {
            background-color: #ffffff;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            padding: 14px;
            font-size: 15px;
            font-weight: 600;
            color: #334155;
            cursor: pointer;
            transition: all 0.15s ease;
            text-align: left;
            padding-left: 18px;
        }
        button.option-btn:hover:not(:disabled) {
            background-color: #f8fafc;
            border-color: #cbd5e1;
        }

        /* 解答アニメーション演出用クラス */
        button.option-btn.btn-correct-ans {
            border-color: #28a745 !important;
            color: #28a745 !important;
            background-color: #f4faf5 !important;
            animation: pulse-green 0.4s ease;
        }
        button.option-btn.btn-incorrect-ans {
            border-color: #dc3545 !important;
            color: #dc3545 !important;
            background-color: #fff5f5 !important;
            animation: shake 0.3s ease;
        }

        @keyframes pulse-green {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-6px); }
            75% { transform: translateX(6px); }
        }

        #result { margin-top: 15px; font-size: 18px; font-weight: bold; }
        .correct { color: #28a745; }
        .incorrect { color: #dc3545; }

        .explanation-card {
            display: none;
            background-color: #f8fafc;
            border-left: 5px solid #cbd5e1;
            border-radius: 8px;
            padding: 15px;
            margin-top: 15px;
            text-align: left;
            font-size: 14px;
            color: #334155;
        }
        .goro {
            background-color: #fef9c3;
            padding: 6px 10px;
            border-radius: 6px;
            font-weight: bold;
            margin: 8px 0;
            border-left: 3px solid #eab308;
            color: #713f12;
        }
        .example-box {
            margin-top: 8px;
            color: #475569;
            background: #f1f5f9;
            padding: 10px;
            border-radius: 6px;
        }
        
        .action-btn {
            margin-top: 20px;
            background-color: #1a73e8;
            color: white;
            border: none;
            border-radius: 12px;
            padding: 14px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            display: none;
        }
        .action-btn:hover { background-color: #1557b5; }
    </style>
</head>
<body>

<div class="menu-container" id="menu-card">
    <h1 class="menu-title">英単語サプリ 100</h1>
    <p class="menu-subtitle">高3上位・難関大レベル機能拡張版</p>
    
    <div class="mypage-box">
        <div class="stat-card">
            <div id="stat-total" class="stat-val">0</div>
            <div class="stat-lbl">総解答数</div>
        </div>
        <div class="stat-card">
            <div id="stat-streak" class="stat-val">0</div>
            <div class="stat-lbl">🔥 現在の連勝</div>
        </div>
    </div>

    <div class="category-select-box">
        <label class="select-label">📚 出題カテゴリ選択</label>
        <select id="category-filter" class="custom-select">
            <option value="all">すべての単語 (全50語)</option>
            <option value="verb">動詞のみ</option>
            <option value="adj">形容詞・その他</option>
            <option value="difficult">難関重要レベル</option>
        </select>
    </div>

    <div class="menu-grid">
        <div class="menu-item" onclick="startQuizMode(10)">
            <div class="menu-icon">🎯</div>
            <div class="menu-info">
                <span class="menu-item-title">10問チャレンジ</span>
                <span class="menu-item-desc">スキマ時間にサクッと復習</span>
            </div>
        </div>
        
        <div class="menu-item item-recom" onclick="startQuizMode(30)">
            <div class="menu-badge">おすすめ</div>
            <div class="menu-icon">⚡</div>
            <div class="menu-info">
                <span class="menu-item-title">30問実践演習</span>
                <span class="menu-item-desc">標準ボリュームで実力診断</span>
            </div>
        </div>

        <div class="menu-item item-weak" id="weak-mode-btn" onclick="startWeakQuizMode()">
            <div class="menu-badge badge-weak" id="weak-count-badge">0語</div>
            <div class="menu-icon">❌</div>
            <div class="menu-info">
                <span class="menu-item-title">弱点克服モード</span>
                <span class="menu-item-desc">間違えた単語だけを集中的に撃破</span>
            </div>
        </div>
    </div>

    <div class="menu-footer">※結果はブラウザのストレージに自動保存されます</div>
</div>

<div class="quiz-container" id="quiz-card" style="display: none;">
    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
        <button onclick="backToMenu()" style="background:none; border:none; color:#1a73e8; cursor:pointer; font-size:14px; font-weight:bold; padding:0;">🏠 中断してメニューへ</button>
        <div id="mode-text" class="mode-badge">通常モード</div>
    </div>
    <div class="progress-box">
        <span id="progress-text">第 1 問</span>
        <span id="score-text">スコア: 0</span>
    </div>
    <div class="progress-bar">
        <div id="progress-fill" class="progress-fill"></div>
    </div>

    <div class="word-container">
        <div id="word" class="word" onclick="speakWord(document.getElementById('word').innerText)">---</div>
        <button class="audio-btn" onclick="speakWord(document.getElementById('word').innerText)">🔊</button>
    </div>
    
    <div id="options" class="options"></div>
    <div id="result"></div>

    <div id="explanation" class="explanation-card">
        <div class="exp-title" id="exp-badge" style="font-weight:bold; margin-bottom:5px;">💡 解説</div>
        <div id="exp-text">解説テキスト</div>
        <div id="exp-goro" class="goro">語呂合わせ</div>
        <div class="example-box">
            <span style="font-weight:bold;">例文:</span> 
            <span id="exp-ex-en">Example</span> <button style="background:none; border:none; cursor:pointer;" id="speaker-ex-btn">🔊</button><br>
            <span id="exp-ex-ja" style="font-size:13px; color:#64748b;">(和訳)</span>
        </div>
    </div>
    
    <button id="next-btn" class="action-btn" onclick="nextQuestion()">次の問題へ</button>
</div>

<script>
    // 拡張データベース（品詞・タグ情報つき）
    const baseVocabulary = [
        { word: "abolish", options: ["廃止する", "設立する", "許可する", "誇張する"], answer: "廃止する", hint: "法律や制度などを完全に無くすこと。", goro: "「暴利し」た悪徳制度を廃止する", ex_en: "The government decided to abolish the tax.", ex_ja: "政府はその税金を廃止することを決定した。", type: "verb", tag: "difficult" },
        { word: "acquire", options: ["獲得する", "要求する", "放棄する", "調査する"], answer: "獲得する", hint: "努力して言語や技術、財産を手に入れること。", goro: "「あ、これ」いいな！努力で技術を獲得する", ex_en: "She managed to acquire a new language.", ex_ja: "彼女はどうにか新しい言語を獲得した。", type: "verb", tag: "standard" },
        { word: "alternative", options: ["代わりの・選択肢", "伝統的な", "一時的な", "人工的な"], answer: "代わりの・選択肢", hint: "今までのものに代わる新しい選択案。", goro: "「洗ったネタ、他」に代わりの選択肢はない", ex_en: "Is there any alternative plan?", ex_ja: "何か代わりの計画はありますか？", type: "adj", tag: "standard" },
        { word: "analyze", options: ["分析する", "麻痺させる", "合成する", "批判する"], answer: "分析する", hint: "データを細かく分けて原因を調べること。", goro: "「あ、穴開ず」にデータを細かく分析する", ex_en: "We need to analyze the test results.", ex_ja: "私たちはテスト結果を分析する必要がある。", type: "verb", tag: "standard" },
        { word: "annual", options: ["年一回の・毎年の", "月一回の", "世紀の", "永遠の"], answer: "年一回の・毎年の", hint: "ann（年）が語源。毎年の恒例行事など。", goro: "「あん」にゅある毎年恒例、年一回のイベント", ex_en: "The company holds an annual meeting in Tokyo.", ex_ja: "その会社は東京で年一回の総会を開く。", type: "adj", tag: "standard" },
        { word: "cooperate", options: ["協力する", "競争する", "衝突する", "妥協する"], answer: "協力する", hint: "共に力を合わせて作業を行うこと。", goro: "「子をオペレート」するために親が協力する", ex_en: "Everyone must cooperate to finish this project.", ex_ja: "このプロジェクトを終えるために全員が協力しなければならない。", type: "verb", tag: "standard" },
        { word: "decline", options: ["衰退する・断る", "承認する", "傾く", "拡大する"], answer: "衰退する・断る", hint: "下向きに傾く、または誘いを丁寧に拒否する。", goro: "「でかいライン」が右肩下がりに衰退する", ex_en: "He chose to decline the invitation.", ex_ja: "彼は招待を断ることを選んだ。", type: "verb", tag: "standard" },
        { word: "distinguish", options: ["区別する", "消滅させる", "尊敬する", "がっかりさせる"], answer: "区別する", hint: "2つのものの違いを見分けること。", goro: "「ディスる」チンパンジーと人間を区別する", ex_en: "It is hard to distinguish the twins.", ex_ja: "その双子を区別するのは難しい。", type: "verb", tag: "difficult" },
        { word: "emphasize", options: ["強調する", "同情する", "批判する", "無視する"], answer: "強調する", hint: "大事であることを強く主張すること。", goro: "「縁をファサ」っと広げて重要性を強調する", ex_en: "The teacher emphasized the importance of review.", ex_ja: "先生は復習の重要性を強調した。", type: "verb", tag: "standard" },
        { word: "exaggerate", options: ["誇張する", "展示する", "疲れ果てさせる", "除外する"], answer: "誇張する", hint: "話を大きく盛って大げさに言うこと。", goro: "「育った」ジャガイモを大げさに誇張する", ex_en: "Don't exaggerate. It wasn't that big.", ex_ja: "誇張するなよ。そんなに大きくなかったろ。", type: "verb", tag: "difficult" },
        { word: "ambiguous", options: ["あいまいな", "明確な", "野心的な", "不吉な"], answer: "あいまいな", hint: "複数の解釈ができて、はっきりしない様子。", goro: "「あんびな」誘惑はいつもあいまいな態度から", ex_en: "His ambiguous answer confused everyone.", ex_ja: "彼のあいまいな返事はみんなを混乱させた。", type: "adj", tag: "difficult" },
        { word: "appropriate", options: ["適切な・ふさわしい", "高価な", "違法な", "否定的な"], answer: "適切な・ふさわしい", hint: "目的や状況にぴったり合っている状態。", goro: "「アップロードに適切な」画像を選ぶ", ex_en: "Please wear appropriate clothes for the interview.", ex_ja: "面接に適切な服装をしてください。", type: "adj", tag: "standard" },
        { word: "artificial", options: ["人工的な", "芸術的な", "誠実な", "原始的な"], answer: "人工的な", hint: "自然ではなく、人間の手によって作られたもの。", goro: "「味、不自然」な人工的な甘味料", ex_en: "The flowers are made of artificial silk.", ex_ja: "その花は人工のシルクで作られている。", type: "adj", tag: "standard" },
        { word: "crucial", options: ["決定的な・極めて重要な", "些細な", "残酷な", "退屈な"], answer: "決定的な・極めて重要な", hint: "運命を分けるほど極めて重要不可欠な様子。", goro: "「狂う、猿」にはバナナが極めて重要だ", ex_en: "Vitamin C plays a crucial role in health.", ex_ja: "ビタミン
