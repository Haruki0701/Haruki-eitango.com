<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>プレミアム版 英単語サプリ100</title>
    <style>
        :root {
            --bg-0: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            --correct-flash: #e2f4e8;
            --incorrect-flash: #fde8e8;
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
            transition: background 0.3s ease;
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
            <option value="all">すべての単語</option>
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
    // 拡張データベース（全100語：高3上位・難関大レベル）
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
        { word: "crucial", options: ["決定的な・極めて重要な", "些細な", "残酷な", "退屈な"], answer: "決定的な・極めて重要な", hint: "運命を分けるほど極めて重要不可欠な様子。", goro: "「狂う、猿」にはバナナが極めて重要だ", ex_en: "Vitamin C plays a crucial role in health.", ex_ja: "ビタミンCは健康に決定的な役割を果たす。", type: "adj", tag: "difficult" },
        // --- 15語目以降の追加単語 ---
        { word: "abandon", options: ["放棄する・見捨てる", "維持する", "収集する", "歓迎する"], answer: "放棄する・見捨てる", hint: "途中で諦めて置き去りにすること。", goro: "「あ、番犬」をその場に放棄する", ex_en: "They had to abandon the sinking ship.", ex_ja: "彼らは沈みゆく船を放棄しなければならなかった。", type: "verb", tag: "standard" },
        { word: "accumulate", options: ["蓄積する", "分配する", "消費する", "計算する"], answer: "蓄積する", hint: "長期間かけて徐々に集まり、積み重なること。", goro: "「開く見え、冷凍」庫に氷が蓄積する", ex_en: "Dust began to accumulate on the old books.", ex_ja: "古い本の上に埃が蓄積し始めた。", type: "verb", tag: "difficult" },
        { word: "advocate", options: ["主張する・擁護する", "非難する", "延期する", "誤解する"], answer: "主張する・擁護する", hint: "ある考えや権利を公に支持・推薦すること。", goro: "「アド（助言）をボート」の上で強く主張する", ex_en: "Many doctors advocate a low-sugar diet.", ex_ja: "多くの医師が低糖質の食事を主張している。", type: "verb", tag: "difficult" },
        { word: "afford", options: ["〜する余裕がある", "〜を拒絶する", "〜を求める", "〜を誇る"], answer: "〜する余裕がある", hint: "金銭的、時間的にそれを行う力があること。", goro: "「絵札、買（お）うど」それくらい買う余裕がある", ex_en: "I cannot afford to buy a new car right now.", ex_ja: "今は新しい車を買う余裕がありません。", type: "verb", tag: "standard" },
        { word: "anticipate", options: ["予期する・楽しみに待つ", "後悔する", "無視する", "終了する"], answer: "予期する・楽しみに待つ", hint: "将来起こることをあらかじめ見込んでおくこと。", goro: "「アンチ、失敗」を早くも予期する", ex_en: "We anticipate that prices will rise next month.", ex_ja: "来月、物価が上がると私たちは予期している。", type: "verb", tag: "difficult" },
        { word: "attribute", options: ["〜のせいにする・帰する", "〜に貢献する", "〜を分配する", "〜を補償する"], answer: "〜のせいにする・帰する", hint: "attribute A to B で「AをBの原因に帰する」の意味。", goro: "「後（あ）取り、ビューッと」事故ったのはアイツのせいにする", ex_en: "She attributes her success to hard work.", ex_ja: "彼女は自分の成功を努力のおかげだとしている。", type: "verb", tag: "difficult" },
        { word: "bewilder", options: ["当惑させる", "喜ばせる", "納得させる", "失望させる"], answer: "当惑させる", hint: "人を混乱させ、どうしていいか分からなくさせること。", goro: "「美、ビルだー！」とあまりの巨大さに当惑させる", ex_en: "The complex instructions bewildered the tourists.", ex_ja: "複雑な指示が観光客を当惑させた。", type: "verb", tag: "difficult" },
        { word: "cherish", options: ["心に抱く・大切にする", "破壊する", "非難する", "見捨てる"], answer: "心に抱く・大切にする", hint: "思い出や友情、希望などを愛情を持って守ること。", goro: "「チェリー、主（しゅ）」が大切にする果物", ex_en: "You should cherish your childhood memories.", ex_ja: "子供の頃の思い出を大切にするべきだ。", type: "verb", tag: "standard" },
        { word: "coincide", options: ["同時に起きる・一致する", "衝突する", "遅れる", "消滅する"], answer: "同時に起きる・一致する", hint: "2つの出来事が偶然同じタイミングで重なること。", goro: "「コイン、再度」投げたら偶然2人の意見が一致する", ex_en: "My holiday coincides with the festival.", ex_ja: "私の休みがちょうどお祭りと同時に起きる。", type: "verb", tag: "difficult" },
        { word: "compensate", options: ["補償する・埋め合わせる", "要求する", "競う", "計算する"], answer: "補償する・埋め合わせる", hint: "損失や不足に対して、別のもので埋め合わせをすること。", goro: "「今（こん）ペン探せ」なくした分を補償する", ex_en: "The company will compensate for your loss.", ex_ja: "会社があなたの損失を補償します。", type: "verb", tag: "difficult" },
        { word: "comply", options: ["従う・応じる", "文句を言う", "拒絶する", "競争する"], answer: "従う・応じる", hint: "comply with の形で、規則や要求を満たすこと。", goro: "「来い、プライ（ド）」を捨てて規則に従う", ex_en: "All students must comply with the school rules.", ex_ja: "全生徒が校則に従わなければならない。", type: "verb", tag: "difficult" },
        { word: "compromise", options: ["妥協する", "主張する", "誤解する", "約束する"], answer: "妥協する", hint: "お互いが譲り合って、中間の解決策を見出すこと。", goro: "「混むプロ、まず」は話し合って妥協する", ex_en: "They finally managed to compromise on the price.", ex_ja: "彼らはついに価格について妥協することができた。", type: "verb", tag: "standard" },
        { word: "conceal", options: ["隠す", "明らかにする", "歓迎する", "製造する"], answer: "隠す", hint: "事実や感情、物を他人の目から見えなくすること。", goro: "「懇親（こんしん）のシール」で傷を隠す", ex_en: "He tried to conceal his disappointment with a smile.", ex_ja: "彼は笑顔で落胆を隠そうとした。", type: "verb", tag: "difficult" },
        { word: "confront", options: ["直面する・立ち向かう", "避ける", "慰める", "調査する"], answer: "直面する・立ち向かう", hint: "困難や敵から目を背けず、正面から向き合うこと。", goro: "「来ん、フロント」に誰もいなくて問題に直面する", ex_en: "We must confront the problem immediately.", ex_ja: "私たちは直ちにその問題に直面し、立ち向かわねばならない。", type: "verb", tag: "standard" },
        { word: "contradict", options: ["矛盾する・反論する", "証明する", "同意する", "予測する"], answer: "矛盾する・反論する", hint: "相手の言うことと自分の言う内容が食い違うこと。", goro: "「今、虎、ディスコ（にいる）」という話は現実に矛盾する", ex_en: "The witness's statement contradicts the evidence.", ex_ja: "目撃者の供述は証拠と矛盾している。", type: "verb", tag: "difficult" },
        { word: "cultivate", options: ["耕す・栽培する・育む", "破壊する", "無視する", "集める"], answer: "耕す・栽培する・育む", hint: "土地を耕すだけでなく、才能や関係を磨き育てること。", goro: "「軽いチバ（千葉）の畑」をトラクターで耕す", ex_en: "She tries to cultivate good reading habits.", ex_ja: "彼女は良い読書習慣を育もうとしている。", type: "verb", tag: "standard" },
        { word: "deprive", options: ["奪う", "与える", "保護する", "禁止する"], answer: "奪う", hint: "deprive A of B で「AからB（権利や自由など）を奪う」。", goro: "「デップリ太ったライブ」の主催者から自由を奪う", ex_en: "The war deprived many children of their homes.", ex_ja: "戦争が多くの子供たちから家を奪った。", type: "verb", tag: "difficult" },
        { word: "derive", options: ["由来する・引き出す", "減少する", "運転する", "到着する"], answer: "由来する・引き出す", hint: "derive from で「〜から生じる、由来する」。", goro: "「出（で）ライブ」からヒット曲のインスピレーションを引き出す", ex_en: "Many English words are derived from Latin.", ex_ja: "多くの英単語はラテン語に由来している。", type: "verb", tag: "difficult" },
        { word: "deteriorate", options: ["悪化する", "改善する", "決定する", "拡大する"], answer: "悪化する", hint: "状態や質がどんどん悪くなっていくこと。", goro: "「出た、料理、えっと…」時間が経って質が悪化する", ex_en: "The weather conditions began to deteriorate rapidly.", ex_ja: "天候の状態が急速に悪化し始めた。", type: "verb", tag: "difficult" },
        { word: "diminish", options: ["減少する・小さくする", "増加する", "消去する", "区別する"], answer: "減少する・小さくする", hint: "数量、価値、影響力などが目減りすること。", goro: "「地味に一種」類ずつ、在庫が減少する", ex_en: "The drug helped to diminish the pain.", ex_ja: "その薬は痛みを減少させるのに役立った。", type: "verb", tag: "standard" },
        { word: "discard", options: ["捨てる・放棄する", "保管する", "トランプをする", "発見する"], answer: "捨てる・放棄する", hint: "不要になったカードやゴミなどをポイと捨てること。", goro: "「ディスったカード」をゴミ箱へ捨てる", ex_en: "Please discard old receipts you no longer need.", ex_ja: "もう必要のない古い領収書は捨ててください。", type: "verb", tag: "standard" },
        { word: "disrupt", options: ["混乱させる・中断させる", "建設する", "紹介する", "解決する"], answer: "混乱させる・中断させる", hint: "交通網や会議、平穏な進行をガタガタに乱すこと。", goro: "「ディス、ラップ」で会議の進行を混乱させる", ex_en: "The heavy snow disrupted the train schedule.", ex_ja: "大雪が電車のダイヤを混乱させた。", type: "verb", tag: "difficult" },
        { word: "distort", options: ["歪める", "真っ直ぐにする", "修復する", "支援する"], answer: "歪める", hint: "事実や形、音などを不正確な状態にねじ曲げること。", goro: "「ディス、通報（とーと）」して事実を歪める", ex_en: "The newspaper distorted the facts of the incident.", ex_ja: "その新聞は事件の事実を歪めて伝えた。", type: "verb", tag: "difficult" },
        { word: "dominate", options: ["支配する", "従う", "訪問する", "寄付する"], answer: "支配する", hint: "圧倒的な力で市場や国、ゲームを牛耳ること。", goro: "「ドミノで、えっと」世界を支配する作戦", ex_en: "The smartphone giant continues to dominate the market.", ex_ja: "そのスマートフォン巨頭が市場を支配し続けている。", type: "verb", tag: "standard" },
        { word: "eliminate", options: ["排除する・除去する", "追加する", "評価する", "維持する"], answer: "排除する・除去する", hint: "不要なものや競争相手を完全に取り除くこと。", goro: "「襟、見ねえ、って」無駄な部分を排除する", ex_en: "We must eliminate all unnecessary costs.", ex_ja: "私たちはすべての不要なコストを排除しなければならない。", type: "verb", tag: "standard" },
        { word: "encounter", options: ["遭遇する", "避ける", "計算する", "励ます"], answer: "遭遇する", hint: "偶然、困難や見知らぬ人にバッタリ出会うこと。", goro: "「縁、カウンター」席で偶然の人物に遭遇する", ex_en: "I encountered an old friend on my way home.", ex_ja: "家に帰る途中で古い友人に遭遇した。", type: "verb", tag: "standard" },
        { word: "endure", options: ["耐える・持ちこたえる", "楽しむ", "拒絶する", "消滅する"], answer: "耐える・持ちこたえる", hint: "苦しみや厳しい状況にじっと長期間耐えること。", goro: "「円、じゅるり（上昇）」となるまで貧乏に耐える", ex_en: "The runners had to endure extreme heat.", ex_ja: "ランナーたちは猛烈な暑さに耐えなければならなかった。", type: "verb", tag: "standard" },
        { word: "enhance", options: ["高める・強化する", "下げる", "魅了する", "模倣する"], answer: "高める・強化する", hint: "質、能力、魅力などをさらに一段と向上させること。", goro: "「円、半（はん）す」れば価値が高まる？いや逆か", ex_en: "Good lighting can enhance the beauty of a room.", ex_ja: "良い照明は部屋の美しさを高めることができる。", type: "verb", tag: "standard" },
        { word: "ensure", options: ["確実にする・保証する", "危険にさらす", "疑う", "保険をかける"], answer: "確実にする・保証する", hint: "間違いが起きないように安全や結果をしっかりと担保すること。", goro: "「円、正（しゅあ）に」手に入るよう確実にする", ex_en: "Please ensure that all windows are closed.", ex_ja: "すべての窓が閉まっていることを確実にして（確かめて）ください。", type: "verb", tag: "standard" },
        { word: "exert", options: ["（力などを）行使する・出す", "リラックスする", "消費する", "吸収する"], answer: "（力などを）行使する・出す", hint: "exert pressure（圧力をかける）や exert influence（影響力を行使する）。", goro: "「エキサイト、ずっと」して影響力を行使する", ex_en: "He had to exert all his strength to move the rock.", ex_ja: "彼はその岩を動かすためにすべての力を出さねばならなかった。", type: "verb", tag: "difficult" },
        { word: "flourish", options: ["繁栄する・栄える", "衰退する", "枯れる", "平らにする"], answer: "繁栄する・栄える", hint: "文化やビジネス、植物などが勢いよく元気に育つこと。", goro: "「風呂、利（り）、主（しゅ）」のおかげで温泉街が繁栄する", ex_en: "The dynamic tech industry continues to flourish.", ex_ja: "活発なハイテク産業は繁栄し続けている。", type: "verb", tag: "standard" },
        { word: "foster", options: ["育成する・育む", "無視する", "妨げる", "養子に出す"], answer: "育成する・育む", hint: "子供や才能、関心などを大切に育て上げること。", goro: "「ホスター（ポスター）貼って」アイドルを育成する", ex_en: "The club aims to foster young musical talent.", ex_ja: "そのクラブは若い音楽の才能を育成することを目指している。", type: "verb", tag: "difficult" },
        { word: "frustrate", options: ["イライラさせる・挫折させる", "満足させる", "楽しませる", "許可する"], answer: "イライラさせる・挫折させる", hint: "計画を邪魔して、思い通りにいかず焦らせること。", goro: "「フラスト（レーション）、売って」余計にイライラさせる", ex_en: "The continuous delays frustrated the passengers.", ex_ja: "度重なる遅延が乗客たちをイライラさせた。", type: "verb", tag: "standard" },
        { word: "guarantee", options: ["保証する", "否定する", "推測する", "借金する"], answer: "保証する", hint: "製品の品質や結果に責任を持ち、確約すること。", goro: "「ガランとしたティー」だけど品質は保証する", ex_en: "The shop guarantees that the product is authentic.", ex_ja: "その店は商品が本物であることを保証している。", type: "verb", tag: "standard" },
        { word: "hinder", options: ["妨げる", "助ける", "隠す", "加速する"], answer: "妨げる", hint: "進行や成長の邪魔をして遅れさせること。", goro: "「ヒンズー、だー」と道を塞いで進行を妨げる", ex_en: "Heavy traffic hindered us from arriving on time.", ex_ja: "激しい交通渋滞が、私たちの時間通りの到着を妨げした。", type: "verb", tag: "difficult" },
        { word: "ignore", options: ["無視する", "気づく", "尊敬する", "探検する"], answer: "無視する", hint: "知っていながらわざと見ないふり、聞かないふりをすること。", goro: "「生（い）ぐ、のあ」という誘いを無視する", ex_en: "It is rude to ignore someone's question.", ex_ja: "誰かの質問を無視するのは失礼だ。", type: "verb", tag: "standard" },
        { word: "implement", options: ["実行する・実施する", "計画する", "延期する", "批判する"], answer: "実行する・実施する", hint: "取り決めた政策や計画を、具体的に動かしていくこと。", goro: "「犬、プレ、面と」向かって計画を実行する", ex_en: "The committee decided to implement the new policy.", ex_ja: "委員会はその新政策を実行することを決定した。", type: "verb", tag: "difficult" },
        { word: "imply", options: ["ほのめかす・暗示する", "はっきり言う", "否定する", "翻訳する"], answer: "ほのめかす・暗示する", hint: "直接言葉にせず、遠回しに意味を伝えること。", goro: "「今、プライ（ド）」が傷ついたと態度でほのめかす", ex_en: "His words imply that he wants to resign.", ex_ja: "彼の言葉は、彼が辞職したがっていることをほのめかしている。", type: "verb", tag: "standard" },
        { word: "impose", options: ["課す・押し付ける", "免除する", "提案する", "購入する"], answer: "課す・押し付ける", hint: "税金やルール、自分の意見を強引に相手に背負わせること。", goro: "「芋、ポーズ」を強制的に課す", ex_en: "The council decided to impose a new tax on plastic.", ex_ja: "議会はプラスチックに新しい税を課すことを決定した。", type: "verb", tag: "difficult" },
        { word: "induce", options: ["引き起こす・誘発する", "減らす", "禁止する", "説明する"], answer: "引き起こす・誘発する", hint: "ある行動や状態になるよう、変化を誘導して生じさせること。", goro: "「院でニュース」がパニックを引き起こす", ex_en: "High stress can induce chemical changes in the body.", ex_ja: "高いストレスは体内に化学変化を引き起こすことがある。", type: "verb", tag: "difficult" },
        { word: "inherit", options: ["相続する・受け継ぐ", "放棄する", "購入する", "投資する"], answer: "相続する・受け継ぐ", hint: "親から財産、遺伝子、伝統などを引き継ぐこと。", goro: "「犬、ヘリ、ト」マト畑の遺産を相続する", ex_en: "She inherited a large fortune from her grandmother.", ex_ja: "彼女は祖母から多額の財産を相続した。", type: "verb", tag: "standard" },
        { word: "inhibit", options: ["抑制する・妨げる", "促進する", "展示する", "居住する"], answer: "抑制する・妨げる", hint: "感情の動きや、化学反応などの進行を押さえつけること。", goro: "「イン、日々、と」閉じこもって成長を抑制する", ex_en: "Cold weather can inhibit the growth of plants.", ex_ja: "冷涼な気候は植物の成長を抑制することがある。", type: "verb", tag: "difficult" },
        { word: "initiate", options: ["始める・創始する", "終わらせる", "真似する", "評価する"], answer: "始める・創始する", hint: "新しい事業、改革、手続きなどの最初の一歩を踏み出すこと。", goro: "「犬、石、あっと」驚くプロジェクトを始める", ex_en: "The government plans to initiate a new program.", ex_ja: "政府は新しいプログラムを始める計画を立てている。", type: "verb", tag: "difficult" },
        { word: "inspect", options: ["検査する・点検する", "無視する", "尊敬する", "見捨てる"], answer: "検査する・点検する", hint: "不具合がないか、書類や機械をじっくり調べること。", goro: "「イン、スペック」を隅々まで検査する", ex_en: "Engineers arrived to inspect the damaged bridge.", ex_ja: "エンジニアたちが、損傷した橋を検査するために到着した。", type: "verb", tag: "standard" },
        { word: "inspire", options: ["奮起させる・霊感を与える", "がっかりさせる", "息を吐く", "陰謀を企てる"], answer: "奮起させる・霊感を与える", hint: "人の心にやる気を起こさせたり、アイデアをひらめかせること。", goro: "「イン、スパイ、あ（そこに）」スパイの映画が彼を奮起させる", ex_en: "His speech inspired many young people to take action.", ex_ja: "彼のスピーチは多くの若者を奮起させ、行動を起こさせた。", type: "verb", tag: "standard" },
        { word: "justify", options: ["正当化する", "批判する", "証明する", "判決を下す"], answer: "正当化する", hint: "自分の行動や意見が「正しい」とする理由を説明すること。", goro: "「ジャスト、ふぁい（と）」したからと暴力を正当化する", ex_en: "You cannot justify your bad behavior.", ex_ja: "自分の悪い行動を正当化することはできない。", type: "verb", tag: "standard" },
        { word: "maintain", options: ["維持する・主張する", "破壊する", "変更する", "見落とす"], answer: "維持する・主張する", hint: "建物や健康を同じ状態でキープする、または意見を崩さないこと。", goro: "「メイン、体重」をずっと同じに維持する", ex_en: "It is important to maintain a healthy lifestyle.", ex_ja: "健康的なライフスタイルを維持することは重要だ。", type: "verb", tag: "standard" },
        { word: "manifest", options: ["（形や色で）明らかにする・表す", "隠す", "破壊する", "製造する"], answer: "（形や色で）明らかにする・表す", hint: "感情や兆候が、目に見える形でハッキリ現れること。", goro: "「マニア、フェスト（お祭り）」で熱狂のサインを明らかにする", ex_en: "The symptoms of the disease manifest within a week.", ex_ja: "その病気の症状は1週間以内に明らかになる（現れる）。", type: "verb", tag: "difficult" },
        { word: "modify", options: ["（部分的に）修正する・変更する", "そのままにする", "破壊する", "認証する"], answer: "（部分的に）修正する・変更する", hint: "計画やデザインを、より良くするために少し作り変えること。", goro: "「窓、ふぁい（と）」で少しサイズを修正する", ex_en: "We need to modify our travel plans slightly.", ex_ja: "私たちは旅行の計画を少し変更する必要がある。", type: "verb", tag: "standard" },
        { word: "negligible", options: ["無視できるほどの・些細な", "非常に重要な", "不注意な", "明確な"], answer: "無視できるほどの・些細な", hint: "あまりに小さくて、計算や考慮に入れなくても問題ないレベルの様子。", goro: "「ねじ、グリグリ、ぶる（震える）」程度の誤差は無視できるほどだ", ex_en: "The difference in price is negligible.", ex_ja: "価格の差は無視できるほど（ごくわずか）だ。", type: "adj", tag: "difficult" },
        { word: "notify", options: ["通知する・知らせる", "秘密にする", "拒絶する", "認識する"], answer: "通知する・知らせる", hint: "公式にメッセージを送って、相手に出来事を伝えること。", goro: "「脳にファイル」が届いたと通知する", ex_en: "Please notify us of any changes in your address.", ex_ja: "住所に変更があれば、私たちに通知してください。", type: "verb", tag: "standard" },
        { word: "obliterate", options: ["完全に消し去る・抹消する", "建設する", "保護する", "目立たせる"], answer: "完全に消し去る・抹消する", hint: "記憶や文字、痕跡を跡形もなく消してしまうこと。", goro: "「オブリ（ありがとう）、お寺、エイト」の記録を完全に消し去る", ex_en: "The heavy rain obliterated the footprints on the sand.", ex_ja: "大雨が砂の上の足跡を完全に消し去った。", type: "verb", tag: "difficult" },
        { word: "obstacle", options: ["障害（物）", "目標", "支援", "加速"], answer: "障害（物）", hint: "前進を阻む、道路上の障害物や人生の壁のこと。", goro: "「お、ブタ、狂（くる）う」障害物を飛び越える", ex_en: "Fear of failure can be a big obstacle to success.", ex_ja: "失敗への恐れは、成功への大きな障害になり得る。", type: "adj", tag: "standard" }, // 名詞だがadj枠で処理可能
        { word: "obtain", options: ["手に入れる・獲得する", "失う", "維持する", "探す"], answer: "手に入れる・獲得する", hint: "お役所の手続きや努力を通じて、必要な情報や権利を得ること。", goro: "「お、ぶっ飛ぶイン」フォメーションを手に入れる", ex_en: "You can obtain a visa at the embassy.", ex_ja: "大使館でビザを手に入れることができます。", type: "verb", tag: "standard" },
        { word: "occupy", options: ["占める・占領する", "解放する", "移動する", "空ける"], answer: "占める・占領する", hint: "時間や空間、または席を自分の存在でいっぱいにすること。", goro: "「お、牛（おすう）、パイ」の店を占領する", ex_en: "Reading copies occupies most of her free time.", ex_ja: "読書が彼女の自由時間の大部分を占めている。", type: "verb", tag: "standard" },
        { word: "overlook", options: ["見落とす・大目に見る", "見つめる", "調査する", "過大評価する"], answer: "見落とす・大目に見る", hint: "うっかりチェックし忘れる、または小さなミスを許してあげること。", goro: "「オーバーなルック（視線）」で逆に重要な点を見落とす", ex_en: "He tended to overlook small spelling mistakes.", ex_ja: "彼は小さな綴りのミスを見落とす傾向があった。", type: "verb", tag: "standard" },
        { word: "overwhelm", options: ["圧倒する・困惑させる", "元気づける", "敗北する", "退屈させる"], answer: "圧倒する・困惑させる", hint: "あまりに大量の仕事や感情が押し寄せ、キャパオーバーにさせること。", goro: "「大場（おおば）、ウェルカム！」と大歓声で圧倒する", ex_en: "She was overwhelmed by the sudden workload.", ex_ja: "彼女は突然の仕事量に圧倒された。", type: "verb", tag: "difficult" },
        { word: "perceive", options: ["知覚する・気づく・理解する", "誤解する", "見落とす", "騙す"], answer: "知覚する・気づく・理解する", hint: "五感（特に目や頭脳）を使って、状況を正しく察知すること。", goro: "「パー、シー、部（ぶ）」のサインだと直感で知覚する", ex_en: "Animals can perceive sounds that humans cannot hear.", ex_ja: "動物は人間には聞こえない音を知覚することができる。", type: "verb", tag: "difficult" },
        { word: "persist", options: ["固執する・しつこく続く", "諦める", "変化する", "消滅する"], answer: "固執する・しつこく続く", hint: "周囲が反対しても意見を曲げない、または雨などがダラダラ続くこと。", goro: "「パー、シスト（アシスト）」を出すことに固執する", ex_en: "If the pain persists, you must see a doctor.", ex_ja: "もし痛みがしつこく続くなら、医者に診てもらわねばならない。", type: "verb", tag: "difficult" },
        { word: "postpone", options: ["延期する", "前倒しする", "中止する", "開催する"], answer: "延期する", hint: "予定を後ろの日時にずらすこと（＝put off）。", goro: "「ポストにポン、と」手紙を入れて提出を延期する", ex_en: "The game was postponed due to heavy rain.", ex_ja: "試合は大雨のため延期された。", type: "verb", tag: "standard" },
        { word: "precede", options: ["〜に先行する・先立つ", "〜に続く", "〜を妨げる", "〜と一致する"], answer: "〜に先行する・先立つ", hint: "時間や順番が、他のものよりも前に来ること。", goro: "「プロ、シード（選手）」は予選に先立つ", ex_en: "A dark cloud preceded the violent storm.", ex_ja: "激しい嵐に先立って黒い雲が現れた。", type: "verb", tag: "difficult" },
        { word: "predict", options: ["予測する・予言する", "思い出す", "記録する", "修正する"], answer: "予測する", hint: "データなどをもとに、未来に何が起こるかを事前に言うこと。", goro: "「プリ（プリン）、ディクト（言う）」次に食べる味を予測する", ex_en: "It is difficult to predict the earthquake exactly.", ex_ja: "地震を正確に予測することは難しい。", type: "verb", tag: "standard" },
        { word: "prohibit", options: ["（法律等で）禁止する", "許可する", "推奨する", "免除する"], answer: "（法律等で）禁止する", hint: "公式なルールとして「やってはいけない」と定めること（＝forbid）。", goro: "「プロ、日々、ト」レーニング以外は禁止する", ex_en: "The law prohibits smoking in public areas.", ex_ja: "法律は公共の場での喫煙を禁止している。", type: "verb", tag: "standard" },
        { word: "prosper", options: ["繁栄する・成功する", "破産する", "減少する", "競争する"], answer: "繁栄する・成功する", hint: "ビジネスや国がお金持ちになり、絶好調に発展すること。", goro: "「プロのパー、素晴らしい」とゴルフ店が繁栄する", ex_en: "The small town began to prosper after the new highway opened.", ex_ja: "新しい高速道路が開通した後、その小さな町は繁栄し始めた。", type: "verb", tag: "difficult" },
        { word: "pursue", options: ["追求する・追跡する", "諦める", "避ける", "満足する"], answer: "追求する・追跡する", hint: "夢やキャリア、逃げる犯人をどこまでも追いかけること。", goro: "「パス、言う（ゆう）」権利を追い求め追求する", ex_en: "She moved to New York to pursue her career in art.", ex_ja: "彼女はアートのキャリアを追求するためにニューヨークへ移住した。", type: "verb", tag: "standard" },
        { word: "reconcile", options: ["和解させる・調和させる", "喧嘩させる", "拒絶する", "分離する"], answer: "和解させる・調和させる", hint: "対立する2つの意見や、喧嘩した友達の間を取り持つこと。", goro: "「離婚（りこん）、再度、いる？」話し合って意見を和解させる", ex_en: "It is hard to reconcile the two different theories.", ex_ja: "その2つの異なる理論を調和させるのは難しい。", type: "verb", tag: "difficult" },
        { word: "refuse", options: ["拒否する・断る", "受け入れる", "招待する", "延期する"], answer: "拒否する・断る", hint: "命令やオファーに対して「嫌です」とはねつけること。", goro: "「理不尽（りふ）なユーズ（使用）」をきっぱり拒否する", ex_en: "They refuse to accept the unfair terms.", ex_ja: "彼らは不公平な条件を受け入れることを拒否している。", type: "verb", tag: "standard" },
        { word: "regulate", options: ["規制する・調整する", "自由にする", "破壊する", "無視する"], answer: "規制する・調整する", hint: "ルールを作って、スピードや温度、活動をコントロールすること。", goro: "「レギュラー、えっと」人数を厳格に規制する", ex_en: "The government needs to regulate traffic in the city.", ex_ja: "政府は都市の交通を規制する必要がある。", type: "verb", tag: "standard" },
        { word: "reinforce", options: ["補強する・強化する", "弱める", "解体する", "無視する"], answer: "補強する・強化する", hint: "壁やチーム、軍隊に力を足して、より頑丈にすること。", goro: "「レイン（雨）、ホース」で濡れた堤防を補強する", ex_en: "The army built structures to reinforce the wall.", ex_ja: "軍は壁を補強するための構造物を建てた。", type: "verb", tag: "difficult" },
        { word: "relieve", options: ["（苦痛等を）和らげる・安心させる", "強める", "がっかりさせる", "引き起こす"], answer: "（苦痛等を）和らげる・安心させる", hint: "ストレスや頭痛などを取り除き、ホッとさせること。", goro: "「リ（再）、ライブ」を見てストレスを和らげる", ex_en: "This medicine will help relieve your headache.", ex_ja: "この薬はあなたの頭痛を和らげるのに役立つでしょう。", type: "verb", tag: "standard" },
        { word: "reluctant", options: ["気乗りしない・嫌がる", "熱心な", "準備ができた", "誇り高い"], answer: "気乗りしない・嫌がる", hint: "あまり気が進まないけれど、渋々やるような心理状態。", goro: "「リラックマ、タント（たくさん）」動くのを気乗りしない様子", ex_en: "He was reluctant to talk about his past mistakes.", ex_ja: "彼は過去のミスについて話すのを気乗りしない様子だった。", type: "adj", tag: "standard" },
        { word: "resolve", options: ["解決する・決意する", "悪化させる", "延期する", "溶解する"], answer: "解決する・決意する", hint: "複雑な問題の答えを見出す、または固く心に誓うこと。", goro: "「利、祖父（そぶ）、解決」してくれたお小遣い問題", ex_en: "We need to resolve this conflict peacefully.", ex_ja: "私たちはこの紛争を平和的に解決する必要がある。", type: "verb", tag: "standard" },
        { word: "restrict", options: ["制限する", "拡大する", "許可する", "無視する"], answer: "制限する", hint: "行動範囲や予算、人数に上限を設けて縛ること。", goro: "「リスト、陸（りく）、と」行動エリアを制限する", ex_en: "The school restricts the use of mobile phones.", ex_ja: "学校は携帯電話の使用を制限している。", type: "verb", tag: "standard" },
        { word: "retain", options: ["保持する・維持する", "失う", "変更する", "返却する"], answer: "保持する・維持する", hint: "古い記憶や水分、権利をそのまま逃さずに持ち続けること。", goro: "「利、手（て）に、イン」したまま権利を保持する", ex_en: "The soil can retain moisture for a long time.", ex_ja: "その土壌は水分を長い間保持することができる。", type: "verb", tag: "difficult" },
        { word: "reveal", options: ["明らかにする・漏らす", "隠す", "保護する", "偽造する"], answer: "明らかにする・漏らす", hint: "秘密の事実やカーテンの奥のものを、みんなに見せること（＝disclose）。", goro: "「リ（再）、ビール」を飲むと本音を明らかにする", ex_en: "The investigation revealed the truth behind the case.", ex_ja: "調査はその事件の背後にある真実を明らかにした。", type: "verb", tag: "standard" },
        { word: "scrutinize", options: ["詳細に調べる・吟味する", "チラ見する", "無視する", "破壊する"], answer: "詳細に調べる・吟味する", hint: "間違いや怪しい点がないか、虫眼鏡で見るように徹底チェックすること。", goro: "「スク（塾）、ルート、ナイス」かを詳細に調べる", ex_en: "The accountant will scrutinize every expense report.", ex_ja: "会計士はすべての経費報告書を詳細に調べるだろう。", type: "verb", tag: "difficult" },
        { word: "simulate", options: ["シミュレーションする・〜のふりをする", "刺激する", "計算する", "同時に起きる"], answer: "シミュレーションする・〜のふりをする", hint: "本物そっくりの状況を仮想空間で作って試してみること。", goro: "「シミ、売れ、あっと」驚く実験状況を模倣する", ex_en: "Computer software can simulate rocket flights.", ex_ja: "コンピュータソフトウェアはロケットの飛行をシミュレーションできる。", type: "verb", tag: "standard" },
        { word: "speculate", options: ["推測する・投機する", "証明する", "目撃する", "無視する"], answer: "推測する・投機する", hint: "確実な証拠がない中で「こうではないか」と考えを巡らせること。", goro: "「スペック、売れ、あっと」価格の変動を推測する", ex_en: "Journalists began to speculate about the election result.", ex_ja: "ジャーナリストたちは選挙結果について推測し始めた。", type: "verb", tag: "difficult" },
        { word: "stimulate", options: ["刺激する・活気づける", "麻痺させる", "真似する", "シミュレーションする"], answer: "刺激する・活気づける", hint: "脳細胞や経済活動に刺激を与えて、活発にさせること。", goro: "「市見舞（し・みま）う、冷え」た経済を刺激する", ex_en: "The lower tax rates will stimulate economic growth.", ex_ja: "減税は経済成長を刺激するだろう。", type: "verb", tag: "standard" },
        { word: "substitute", options: ["代わりにする・代用する", "設置する", "引く", "購入する"], answer: "代わりにする・代用する", hint: "バターの代わりにマーガリンを使うように、別のものを当てること。", goro: "「サブ（補佐）に、立つ、ちゅと（ちょっと）」メインの代わりにする", ex_en: "You can substitute honey for sugar in this recipe.", ex_ja: "このレシピでは砂糖の代わりにハチミツを代用できます。", type: "verb", tag: "standard" },
        { word: "surpass", options: ["上回る・超える", "下回る", "追いつく", "パスを回す"], answer: "上回る・超える", hint: "ライバルの記録や、事前の期待値を大きく追い抜くこと。", goro: "「さあ、パス！」と先輩の記録を上回る", ex_en: "This year's profits surpassed all expectations.", ex_ja: "今年の利益はすべての予想を大きく上回った。", type: "verb", tag: "difficult" },
        { word: "sustain", options: ["維持する・持ちこたえる", "破壊する", "中断する", "無視する"], answer: "維持する・持ちこたえる", hint: "SDGsのsustainableの動詞形。生命や経済活動を長く持続させること。", goro: "「さすが、イン」ドア派、体力を長く維持する", ex_en: "The planet can no longer sustain this level of pollution.", ex_ja: "地球はもはやこのレベルの汚染には持ちこたえられない。", type: "verb", tag: "difficult" },
        { word: "terminate", options: ["終わらせる・終結する", "始める", "継続する", "接続する"], answer: "終わらせる・終結する", hint: "契約や雇用の期間が来て、ピリオドを打つこと（＝end）。", goro: "「ターミネーター」が敵の息の根を終わらせる", ex_en: "The company decided to terminate the contract.", ex_ja: "会社はその契約を終わらせることを決定した。", type: "verb", tag: "difficult" },
        { word: "transform", options: ["変形させる・変える", "維持する", "輸送する", "翻訳する"], answer: "変形させる・変える", hint: "元の姿や性質を、劇的に別の素晴らしいものへガラリと変えること。", goro: "「トランス、フォーム（形）」をガラリと変形させる", ex_en: "The old factory was transformed into an art gallery.", ex_ja: "古い工場はアートギャラリーへと姿を変えられた。", type: "verb", tag: "standard" },
        { word: "undergo", options: ["（試験や手術を）受ける・経験する", "避ける", "拒絶する", "下に潜る"], answer: "（試験や手術を）受ける・経験する", hint: "変化や辛い治療、厳しい検査を身に受けて通ること。", goro: "「安藤、ゴー！」と大手術を受けに行く", ex_en: "The patient needs to undergo emergency surgery.", ex_ja: "その患者は緊急手術を受ける必要がある。", type: "verb", tag: "difficult" },
        { word: "undertake", options: ["（仕事などを）引き受ける・着手する", "断る", "見落とす", "追い抜く"], answer: "（仕事などを）引き受ける・着手する", hint: "重い責任のあるタスクや調査を「私がやります」と引き受けること。", goro: "「アンダー、テイク（取る）」と責任ある仕事を床下から引き受ける", ex_en: "The university will undertake a major research project.", ex_ja: "大学は大規模な研究プロジェクトを引き受ける予定だ。", type: "verb", tag: "difficult" },
        { word: "utilize", options: ["利用する・活用する", "無駄にする", "破壊する", "製造する"], answer: "利用する・活用する", hint: "ある目的のために、道具や限られた資源を有効に使いこなすこと。", goro: "「家（うち）、利、来ず」資源を有効に利用する", ex_en: "We should utilize solar energy more efficiently.", ex_ja: "私たちは太陽エネルギーをもっと効率的に利用すべきだ。", type: "verb", tag: "standard" },
        { word: "verify", options: ["証明する・確かめる", "疑う", "偽造する", "否定する"], answer: "証明する・確かめる", hint: "それが事実であるか、または本物であるかを証拠を用いてチェックすること。", goro: "「ベリー、ファイン、いい」データか本当かを確かめる", ex_en: "Please verify your email address to complete registration.", ex_ja: "登録を完了するためにメールアドレスが正しいか確かめてください。", type: "verb", tag: "difficult" },
        { word: "vulnerable", options: ["脆弱な・傷つきやすい", "頑丈な", "貴重な", "攻撃的な"], answer: "脆弱な・傷つきやすい", hint: "サイバー攻撃や病気、災害に対して守りが弱く、被害を受けやすい様子。", goro: "「ばあさん、並ぶ、ぶる（震える）」台風に脆弱な古い家", ex_en: "Old wooden houses are vulnerable to earthquakes.", ex_ja: "古い木造住宅は地震に脆弱だ（弱い）。", type: "adj", tag: "difficult" },
        { word: "withstand", options: ["耐える・抵抗する", "降伏する", "避ける", "立つ"], answer: "耐える・抵抗する", hint: "強い風圧や攻撃に対して、壊れずにじっと耐えること（＝resist）。", goro: "「水、スタンド」で押し寄せる波に耐える", ex_en: "The new skyscraper can withstand violent typhoons.", ex_ja: "新しい超高層ビルは猛烈な台風に耐えることができる。", type: "verb", tag: "difficult" },
        { word: "yield", options: ["産出する・（利益を）生む・屈する", "消費する", "拒絶する", "隠す"], answer: "産出する・（利益を）生む・屈する", hint: "畑が作物を実らせる、または交差点で道を譲る・降伏すること。", goro: "「言う（いーる）、ド（ど）」っと利益を産出する畑", ex_en: "The investment is expected to yield high returns.", ex_ja: "その投資は高い利益を生むと期待されている。", type: "verb", tag: "difficult" },
        { word: "unprecedented", options: ["前例のない・未曾有の", "過去の", "典型的な", "平凡な"], answer: "前例のない・未曾有の", hint: "歴史上、これまで一度も起きたことがないような驚くべき事態。", goro: "「あん、プレ、社長、出（で）た」前例のない大事件", ex_en: "The country is facing an unprecedented economic crisis.", ex_ja: "その国は前例のない経済危機に直面している。", type: "adj", tag: "difficult" },
        { word: "anonymous", options: ["匿名の・名前を伏せた", "有名な", "偽名の", "危険な"], answer: "匿名の・名前を伏せた", hint: "誰が書いたか、誰が寄付したか名前が隠されている状態。", goro: "「あの兄貴、マスコミ、ミス」匿名の通報者だった", ex_en: "An anonymous donor gave one million dollars to the school.", ex_ja: "ある匿名の寄付者が学校に100万ドルを寄付した。", type: "adj", tag: "standard" },
        { word: "coherent", options: ["首尾一貫した・筋の通った", "矛盾した", "複雑な", "伝統的な"], answer: "首尾一貫した・筋の通った", hint: "話の前後がしっかりと噛み合っていて、論理的な様子。", goro: "「子、平和、えん（縁）」と筋の通った説明をする", ex_en: "He failed to give a coherent explanation for his absence.", ex_ja: "彼は欠席について筋の通った説明をすることができなかった。", type: "adj", tag: "difficult" },
        { word: "compulsory", options: ["義務的な・強制的な", "自由選択の", "推奨される", "禁止された"], answer: "義務的な・強制的な", hint: "法律などで、全員が必ずやらなければならないと決まっていること（＝mandatory）。", goro: "「混む、プロ、さらに」と義務的な講習に参加する", ex_en: "Primary education is compulsory in Japan.", ex_ja: "日本では初等教育は義務的（義務教育）である。", type: "adj", tag: "standard" },
        { word: "diligent", options: ["勤勉な・熱心な", "怠惰な", "頭の良い", "失礼な"], answer: "勤勉な・熱心な", hint: "サボらずにコツコツと真面目に努力を続ける様子。", goro: "「尻、ジリジリ、あっと」驚くほど勤勉な勉強家", ex_en: "She is a diligent student who never misses class.", ex_ja: "彼女は授業を絶対に休まない勤勉な学生だ。", type: "adj", tag: "standard" },
        { word: "exquisite", options: ["非常に美しい・洗練された", "醜い", "安っぽい", "巨大な"], answer: "非常に美しい・洗練された", hint: "職人の技が細部まで行き届いた、極上の美術品などを褒める言葉。", goro: "「エキサイト、食い、ずっと」非常に美しい料理を堪能する", ex_en: "The museum displayed an exquisite piece of gold jewelry.", ex_ja: "博物館には非常に美しい金製の宝飾品が展示されていた。", type: "adj", tag: "difficult" },
        { word: "genuine", options: ["本物の・偽りのない", "偽物の", "人工的な", "高価な"], answer: "本物の・偽りのない", hint: "ニセモノではなく、純粋な本物の革や、嘘偽りのない感情のこと。", goro: "「銭、院（いん）」に入れるのは本物の硬貨だけ", ex_en: "Is this jacket made of genuine leather?", ex_ja: "このジャケットは本物の革で作られていますか？", type: "adj", tag: "standard" },
        { word: "hostile", options: ["敵意のある・不都合な", "友好的な", "無関心な", "安全な"], answer: "敵意のある・不都合な", hint: "相手に対してトゲトゲした態度や、攻撃的な気持ちを持っている様子。", goro: "「干したイルカ」に牙を剥く敵意のあるクジラ", ex_en: "The locals were hostile to the new tourists.", ex_ja: "地元の人々は新しい観光客に対して敵意を持っていた。", type: "adj", tag: "difficult" },
        { word: "innovative", options: ["革新的な・画期的な", "保守的な", "古い", "退屈な"], answer: "革新的な・画期的な", hint: "これまでにない全く新しいアイデアや技術を導入している様子。", goro: "「犬、場、テレビ」を融合した革新的なアイデア", ex_en: "The company is famous for its innovative technology.", ex_ja: "その会社は革新的な技術で有名だ。", type: "adj", tag: "standard" },
        { word: "legitimate", options: ["合法的な・正当な", "違法な", "怪しい", "伝統的な"], answer: "合法的な・正当な", hint: "法律やルールに適っており、誰から見ても文句のつけようがない状態。", goro: "「理事、地味、メイト」だけど手続きは完全に合法的だ", ex_en: "The business has a legitimate reason for the delay.", ex_ja: "その企業には遅延に対する正当な理由がある。", type: "adj", tag: "difficult" },
        { word: "naive", options: ["世間知らずな・うぶな", "洗練された", "意地悪な", "賢い"], answer: "世間知らずな・うぶな", hint: "日本語の「ナイーブ（繊細）」とは違い、騙されやすく経験が浅いマイナスの意味で使うことが多い。", goro: "「無い、胃部（いぶ）」の知識、世間知らずな若者", ex_en: "It was naive of you to believe his story so easily.", ex_ja: "彼の話をそんなに簡単に信じるなんて、君もうぶ（世間知らず）だったね。", type: "adj", tag: "standard" },
        { word: "obscure", options: ["無名の・はっきりしない", "有名な", "明るい", "重要な"], answer: "無名の・はっきりしない", hint: "世間に知られていない、または暗くてよく見えない様子。", goro: "「お、ブス、今日、あ（そこに）」無名の役者だけど演技がすごい", ex_en: "He was born in an obscure village in the mountains.", ex_ja: "彼は山奥の無名な村で生まれた。", type: "adj", tag: "difficult" }
    ];

    // 状態管理変数
    let quizQueue = [];
    let currentQuestionIndex = 0;
    let score = 0;
    let isAnswered = false;

    // ローカルストレージ用キー
    const STORAGE_TOTAL = "vocab_total_answers";
    let currentStreak = 0; // セッション連勝
    let weakWords = JSON.parse(localStorage.getItem("vocab_weak_words")) || [];

    // 初期化処理
    window.onload = function() {
        updateStatsDisplay();
        updateCategoryOptionCount();
    };

    function updateCategoryOptionCount() {
        const select = document.getElementById("category-filter");
        select.options[0].text = `すべての単語 (全${baseVocabulary.length}語)`;
    }

    function updateStatsDisplay() {
        const total = localStorage.getItem(STORAGE_TOTAL) || 0;
        document.getElementById("stat-total").innerText = total;
        document.getElementById("stat-streak").innerText = currentStreak;
        
        const weakBadge = document.getElementById("weak-count-badge");
        weakBadge.innerText = `${weakWords.length}語`;
        
        const weakBtn = document.getElementById("weak-mode-btn");
        if(weakWords.length === 0) {
            weakBtn.classList.add("disabled");
        } else {
            weakBtn.classList.remove("disabled");
        }
    }

    // クイズ開始処理（通常モード）
    function startQuizMode(limit) {
        const filterValue = document.getElementById("category-filter").value;
        let filtered = baseVocabulary;

        if (filterValue === "verb") {
            filtered = baseVocabulary.filter(item => item.type === "verb");
        } else if (filterValue === "adj") {
            filtered = baseVocabulary.filter(item => item.type === "adj");
        } else if (filterValue === "difficult") {
            filtered = baseVocabulary.filter(item => item.tag === "difficult");
        }

        if(filtered.length === 0) {
            alert("該当する単語がありません。");
            return;
        }

        document.getElementById("mode-text").innerText = `通常モード (${limit}問)`;
        setupQueue(filtered, limit);
    }

    // クイズ開始処理（弱点克服モード）
    function startWeakQuizMode() {
        if(weakWords.length === 0) return;
        
        const filtered = baseVocabulary.filter(item => weakWords.includes(item.word));
        document.getElementById("mode-text").innerText = "弱点克服モード";
        setupQueue(filtered, filtered.length);
    }

    function setupQueue(wordsArray, limit) {
        // シャッフル
        let shuffled = [...wordsArray].sort(() => 0.5 - Math.random());
        quizQueue = shuffled.slice(0, limit);
        
        currentQuestionIndex = 0;
        score = 0;
        
        document.getElementById("menu-card").style.display = "none";
        document.getElementById("quiz-card").style.display = "block";
        
        showQuestion();
    }

    // 問題表示
    function showQuestion() {
        isAnswered = false;
        document.body.className = ""; // フラッシュリセット
        document.getElementById("result").innerText = "";
        document.getElementById("explanation").style.display = "none";
        document.getElementById("next-btn").style.display = "none";

        const currentData = quizQueue[currentQuestionIndex];
        
        // 進捗アップデート
        document.getElementById("progress-text").innerText = `第 ${currentQuestionIndex + 1} 問 / 全 ${quizQueue.length} 問`;
        document.getElementById("score-text").innerText = `スコア: ${score}`;
        const percent = ((currentQuestionIndex) / quizQueue.length) * 100;
        document.getElementById("progress-fill").style.width = `${percent}%`;

        // 単語セット
        document.getElementById("word").innerText = currentData.word;
        
        // オプション生成
        const optionsContainer = document.getElementById("options");
        optionsContainer.innerHTML = "";
        
        currentData.options.forEach(opt => {
            const btn = document.createElement("button");
            btn.className = "option-btn";
            btn.innerText = opt;
            btn.onclick = () => checkAnswer(btn, opt, currentData.answer);
            optionsContainer.appendChild(btn);
        });

        // 自動音声読み上げ
        speakWord(currentData.word);
    }

    // 判定処理
    function checkAnswer(selectedBtn, selectedOpt, correctOpt) {
        if(isAnswered) return;
        isAnswered = true;

        const currentData = quizQueue[currentQuestionIndex];
        const optionButtons = document.querySelectorAll(".option-btn");
        
        // 総解答数のインクリメント
        let total = parseInt(localStorage.getItem(STORAGE_TOTAL) || 0);
        localStorage.setItem(STORAGE_TOTAL, total + 1);

        if(selectedOpt === correctOpt) {
            score++;
            currentStreak++;
            selectedBtn.classList.add("btn-correct-ans");
            document.getElementById("result").innerHTML = "<span class='correct'>⭕ 正解！</span>";
            document.body.classList.add("flash-correct");
            
            // 弱点リストから削除（一度正解したら克服とみなす場合）
            weakWords = weakWords.filter(w => w !== currentData.word);
        } else {
            currentStreak = 0;
            selectedBtn.classList.add("btn-incorrect-ans");
            document.getElementById("result").innerHTML = "<span class='incorrect'>❌ 不正解...</span>";
            document.body.classList.add("flash-incorrect");
            
            // 正解ボタンをハイライト
            optionButtons.forEach(btn => {
                if(btn.innerText === correctOpt) {
                    btn.classList.add("btn-correct-ans");
                }
            });

            // 弱点リストに追加
            if(!weakWords.includes(currentData.word)) {
                weakWords.push(currentData.word);
            }
        }

        // 弱点を保存
        localStorage.setItem("vocab_weak_words", JSON.stringify(weakWords));
        updateStatsDisplay();

        // ボタンの無効化
        optionButtons.forEach(btn => btn.disabled = true);

        // 解説の表示
        showExplanation(currentData);
    }

    // 解説カードの中身を埋めて表示
    function showExplanation(data) {
        document.getElementById("exp-text").innerText = data.hint;
        document.getElementById("exp-goro").innerText = data.goro;
        document.getElementById("exp-ex-en").innerText = data.ex_en;
        document.getElementById("exp-ex-ja").innerText = data.ex_ja;
        
        document.getElementById("speaker-ex-btn").onclick = () => speakWord(data.ex_en);
        document.getElementById("explanation").style.display = "block";
        
        // ボタン文言変更
        const nextBtn = document.getElementById("next-btn");
        if(currentQuestionIndex === quizQueue.length - 1) {
            nextBtn.innerText = "結果を見る";
        } else {
            nextBtn.innerText = "次の問題へ";
        }
        nextBtn.style.display = "block";
    }

    // 次の目的地（問題 or 結果）への遷移
    function nextQuestion() {
        if(currentQuestionIndex < quizQueue.length - 1) {
            currentQuestionIndex++;
            showQuestion();
        } else {
            // プログレスバーを100%にする演出
            document.getElementById("progress-fill").style.width = "100%";
            alert(`チャレンジ終了！\nあなたのスコア: ${score} / ${quizQueue.length}`);
            backToMenu();
        }
    }

    // メニューへ戻る
    function backToMenu() {
        document.body.className = "";
        document.getElementById("quiz-card").style.display = "none";
        document.getElementById("menu-card").style.display = "block";
        updateStatsDisplay();
    }

    // 音声合成機能
    function speakWord(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'en-US';
            utterance.rate = 0.9;
            window.speechSynthesis.speak(utterance);
        }
    }
</script>

</body>
</html>
