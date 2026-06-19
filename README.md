<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>高3上位・難関大レベル 英単語サプリ100</title>
    <style>
        :root {
            --bg-0: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            --bg-1: linear-gradient(135deg, #243b55 0%, #141e30 100%);
            --bg-2: linear-gradient(135deg, #134e5e 0%, #71b280 100%);
            --bg-3: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            --bg-4: linear-gradient(135deg, #414d0b 0%, #727a17 100%);
            --bg-5: linear-gradient(135deg, #0f2027 0%, #203a43 100%);
            --bg-6: linear-gradient(135deg, #3d7eaa 0%, #6d7f9c 100%);
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
            transition: background 0.5s ease;
        }
        .quiz-container {
            background-color: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            text-align: center;
            width: 100%;
            max-width: 460px;
        }
        .mode-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            margin-bottom: 15px;
            background-color: #e8f0fe;
            color: #1a73e8;
        }
        .progress-box {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 14px;
            color: #555;
            margin-bottom: 10px;
        }
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #e0e0e0;
            border-radius: 3px;
            margin-bottom: 25px;
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
            margin: 25px 0;
        }
        .word {
            font-size: 38px;
            font-weight: bold;
            color: #1a73e8;
            letter-spacing: 0.5px;
            cursor: pointer;
        }
        .audio-btn {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            padding: 5px;
        }
        .options {
            display: grid;
            gap: 12px;
        }
        button.option-btn {
            background-color: #ffffff;
            border: 2px solid #e0e2e6;
            border-radius: 10px;
            padding: 14px;
            font-size: 16px;
            font-weight: 500;
            color: #333;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: left;
            padding-left: 20px;
        }
        button.option-btn:hover:not(:disabled) {
            background-color: #f1f7fe;
            border-color: #1a73e8;
            color: #1a73e8;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
        }
        .correct { color: #28a745; }
        .incorrect { color: #dc3545; }

        .explanation-card {
            display: none;
            background-color: #f8f9fa;
            border-left: 5px solid #dc3545;
            border-radius: 6px;
            padding: 15px;
            margin-top: 15px;
            text-align: left;
            font-size: 14px;
            color: #333;
        }
        .exp-title {
            font-weight: bold;
            color: #dc3545;
            margin-bottom: 5px;
        }
        .goro {
            background-color: #fff3cd;
            padding: 6px 10px;
            border-radius: 4px;
            font-weight: bold;
            margin: 8px 0;
            border-left: 3px solid #ffc107;
        }
        .example-box {
            margin-top: 8px;
            font-style: italic;
            color: #555;
            background: #edf2f7;
            padding: 8px;
            border-radius: 4px;
        }
        
        .action-btn {
            margin-top: 25px;
            background-color: #1a73e8;
            color: white;
            border: none;
            border-radius: 10px;
            padding: 14px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            display: none;
        }
        .action-btn:hover {
            background-color: #1557b5;
        }
    </style>
</head>
<body>

<div class="quiz-container" id="quiz-card">
    <div id="mode-text" class="mode-badge">通常モード</div>
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
        <div class="exp-title">💡 不正解解説</div>
        <div id="exp-text">ここに一言解説が入ります。</div>
        <div id="exp-goro" class="goro">ここに語呂合わせが入ります。</div>
        <div class="example-box">
            <span style="font-weight:bold; font-style:normal;">例文:</span> 
            <span id="exp-ex-en">English example.</span> <button style="background:none; border:none; cursor:pointer;" id="speaker-ex-btn">🔊</button><br>
            <span id="exp-ex-ja" style="font-size:13px; color:#666; font-style:normal;">(日本語訳)</span>
        </div>
    </div>
    
    <button id="next-btn" class="action-btn" onclick="nextQuestion()">次の問題へ</button>
</div>

<script>
    // 厳選100単語（高3上位〜難関私大・国公立二次レベル）
    const baseVocabulary = [
        { word: "abolish", options: ["廃止する", "設立する", "許可する", "誇張する"], answer: "廃止する", hint: "法律や制度、奴隷制などを完全に無くすときに使われます。", goro: "「暴利し」た悪徳制度を廃止する", ex_en: "The government decided to abolish the tax.", ex_ja: "政府はその税金を廃止することを決定した。" },
        { word: "acquire", options: ["獲得する", "要求する", "放棄する", "調査する"], answer: "獲得する", hint: "時間をかけて努力し、言語や技術、財産などを手に入れること。", goro: "「あ、これ」いいな！努力で技術を獲得する", ex_en: "She managed to acquire a new language.", ex_ja: "彼女はどうにか新しい言語を獲得（習得）した。" },
        { word: "alternative", options: ["代わりの・選択肢", "伝統的な", "一時的な", "人工的な"], answer: "代わりの・選択肢", hint: "alter（変える）が語源。今までのものに代わる新しい案のこと。", goro: "「洗ったネタ、他」に代わりの選択肢はない", ex_en: "Is there any alternative plan?", ex_ja: "何か代わりの計画（代替案）はありますか？" },
        { word: "analyze", options: ["分析する", "麻痺させる", "合成する", "批判する"], answer: "分析する", hint: "データを細かく分けて、原因や構造を調べることです。", goro: "「あ、穴開ず」にデータを細かく分析する", ex_en: "We need to analyze the test results.", ex_ja: "私たちはテスト結果を分析する必要がある。" },
        { word: "annual", options: ["年一回の・毎年の", "月一回の", "世紀の", "永遠の"], answer: "年一回の・毎年の", hint: "ann（年）が語源。記念日の anniversary と仲間です。", goro: "「あん」にゅある毎年恒例、年一回のイベント", ex_en: "The company holds an annual meeting in Tokyo.", ex_ja: "その会社は東京で年一回の総会を開く。" },
        { word: "cooperate", options: ["協力する", "競争する", "衝突する", "妥協する"], answer: "協力する", hint: "co（共に）+ operate（作動する）＝力を合わせる。", goro: "「子をオペレート」するために親が協力する", ex_en: "Everyone must cooperate to finish this project.", ex_ja: "このプロジェクトを終えるために全員が協力しなければならない。" },
        { word: "decline", options: ["衰退する・断る", "承認する", "傾く", "拡大する"], answer: "衰退する・断る", hint: "下向きに傾くことから、数などが「減る」、誘いを「丁寧に断る」の意味に。", goro: "「でかいライン」が右肩下がりに衰退する", ex_en: "He chose to decline the invitation.", ex_ja: "彼は招待を断ることを選んだ。" },
        { word: "distinguish", options: ["区別する", "消滅させる", "尊敬する", "がっかりさせる"], answer: "区別する", hint: "2つのものの違いを見分ける、はっきり分けるという意味。", goro: "「ディスる」チンパンジーと人間を区別する", ex_en: "It is hard to distinguish the twins.", ex_ja: "その双子を区別するのは難しい。" },
        { word: "emphasize", options: ["強調する", "同情する", "批判する", "無視する"], answer: "強調する", hint: "そこにスポットライトを当てて、大事であることを強く主張する。", goro: "「縁をファサ」っと広げて重要性を強調する", ex_en: "The teacher emphasized the importance of review.", ex_ja: "先生は復習の重要性を強調した。" },
        { word: "exaggerate", options: ["誇張する", "展示する", "疲れ果てさせる", "除外する"], answer: "誇張する", hint: "話を盛る、大げさに言うという意味の重要単語です。", goro: "「育った」ジャガイモを大げさに誇張する", ex_en: "Don't exaggerate. It wasn't that big.", ex_ja: "誇張する（話を盛る）なよ。そんなに大きくなかったろ。" },
        
        // 11-25
        { word: "abandon", options: ["捨てる・放棄する", "維持する", "抱きしめる", "降伏する"], answer: "捨てる・放棄する", hint: "途中で諦めて、見捨てたり放置したりすること。", goro: "「あ、番犬」をその場に捨てて放棄する", ex_en: "They had to abandon the sinking ship.", ex_ja: "彼らは沈没しかけた船を放棄しなければならなかった。" },
        { word: "accumulate", options: ["蓄積する", "分配する", "消費する", "計算する"], answer: "蓄積する", hint: "お金や知識、ゴミなどがどんどん積み重なって増えること。", goro: "「悪見え」るほど裏金を蓄積する", ex_en: "Dust began to accumulate on the old books.", ex_ja: "古い本の上にほこりが蓄積し始めた。" },
        { word: "advocate", options: ["主張する・支持者", "批判する", "延期する", "だます"], answer: "主張する・支持者", hint: "公共の場で特定の意見やポリシーを強く訴えること。", goro: "「あ、ぶっ壊」せと改革を強く主張する", ex_en: "Many doctors advocate a low-sugar diet.", ex_ja: "多くの医師が低糖質の食事を主張（推奨）している。" },
        { word: "ambiguous", options: ["あいまいな", "明確な", "野心的な", "不吉な"], answer: "あいまいな", hint: "解釈がいくつもできて、はっきりしない様子。", goro: "「あんび（甘美）な」誘惑はいつもあいまいな態度から", ex_en: "His ambiguous answer confused everyone.", ex_ja: "彼のあいまいな返事はみんなを混乱させた。" },
        { word: "anticipate", options: ["予期する・楽しみに待つ", "後悔する", "参加する", "無視する"], answer: "予期する・楽しみに待つ", hint: "起こることを予測して、それなりの準備や覚悟をすること。", goro: "「あんちゃん、手」に汗握って未来を予期する", ex_en: "We anticipate that prices will rise next year.", ex_ja: "私たちは来年、物価が上昇すると予期している。" },
        { word: "appropriate", options: ["適切な・ふさわしい", "高価な", "違法な", "否定的な"], answer: "appropriate", hint: "目的や状況にぴったり合っている状態を指します。", goro: "「アップろ（アップロード）に適切な」画像を選ぶ", ex_en: "Please wear appropriate clothes for the interview.", ex_ja: "面接に適切な服装をしてください。" },
        { word: "artificial", options: ["人工的な", "芸術的な", "誠実な", "原始的な"], answer: "人工的な", hint: "自然のものではなく、人間の手によって作られたもの。", goro: "「味、不自然」な人工的な甘味料", ex_en: "The flowers are made of artificial silk.", ex_ja: "その花は人工のシルクで作られている。" },
        { word: "attribute", options: ["〜のせいにする・属性", "貢献する", "分配する", "避ける"], answer: "〜のせいにする・属性", hint: "結果の原因が、特定の場所や人にあると考えること。A to Bの形で頻出。", goro: "「あとりび（後日々）」の体調不良を寝不足のせいにする", ex_en: "She attributes her success to hard work.", ex_ja: "彼女は自分の成功を猛勉強のおかげだとしている（〜に帰している）。" },
        { word: "benefit", options: ["利益・恩恵", "損害", "偏見", "負債"], answer: "利益・恩恵", hint: "福祉や健康、お金の面でプラスになること。", goro: "「紅（べに）引くと」見た目に大きな利益・恩恵がある", ex_en: "The new system will benefit everyone.", ex_ja: "新しいシステムは全員に利益をもたらすだろう。" },
        { word: "bureaucracy", options: ["官僚政治・官僚機構", "民主主義", "貴族社会", "無政府状態"], answer: "官僚政治・官僚機構", hint: "お役所仕事や、手続きが複雑なピラミッド型の組織のこと。", goro: "「びゅー（ビュー）と楽してし」まう官僚政治", ex_en: "We need to reduce unnecessary bureaucracy.", ex_ja: "私たちは不必要な官僚政治（お役所手続き）を減らす必要がある。" },
        { word: "chronological", options: ["年代順の", "論理的な", "地理的な", "心理的な"], answer: "年代順の", hint: "chronoは「時間」。出来事が起きた順番に並んでいること。", goro: "「苦労乗っける」歴史の教科書の年代順の記述", ex_en: "The history list is in chronological order.", ex_ja: "その歴史のリストは年代順に並んでいる。" },
        { word: "cognitive", options: ["認知の・認識の", "秘密の", "協力的な", "巨大な"], answer: "認知の・認識の", hint: "脳が物事を理解したり記憶したりするプロセスに関すること。", goro: "「焦ぐ、ニート」が現実を認知の歪みで捉える", ex_en: "Reading helps children's cognitive development.", ex_ja: "読書は子供の認知発達を助ける。" },
        { word: "coincide", options: ["同時に起きる・一致する", "衝突する", "消滅する", "孤立する"], answer: "同時に起きる・一致する", hint: "偶然、2つの出来事が同じタイミングや同じ場所で重なること。", goro: "「恋、インサイド（内側）」で彼女の予定と同時に起きる", ex_en: "My holiday coincides with the festival.", ex_ja: "私の休暇はちょうどその祭りと同時に起きる。" },
        { word: "compel", options: ["強いる・無理に〜させる", "説得する", "禁止する", "許す"], answer: "強いる・無理に〜させる", hint: "力やルールを使って、人にやりたくないことを強制すること。", goro: "「混むペル（ペルー）」のバスが乗客に起立を強いる", ex_en: "Bad health compelled him to resign.", ex_ja: "健康悪化が彼に辞職を強いた。" },
        { word: "compensate", options: ["補償する・埋め合わせる", "賞賛する", "計算する", "競争する"], answer: "補償する・埋め合わせる", hint: "損失やミスに対して、お金や別の良いことで穴埋めすること。", goro: "「混んでん、銭湯」の代わりに入浴剤で補償する", ex_en: "The company will compensate for your loss.", ex_ja: "会社はあなたの損失を補償します。" },

        // 26-50
        { word: "compromise", options: ["妥協する", "約束する", "批判する", "誤解する"], answer: "妥協する", hint: "お互いが少しずつ譲り合って、中間の解決策に落ち着くこと。", goro: "「混むプロ、まずい」と折れて妥協する", ex_en: "They finally reached a compromise.", ex_ja: "彼らはついに妥協（妥協案の合意）に達した。" },
        { word: "consequence", options: ["結果・影響", "原因", "出発", "自信"], answer: "結果・影響", hint: "ある行動や出来事から「必然的に導かれる」重大な結果。", goro: "「今（こん）週件数（けんすう）」が増えた結果、大問題になる", ex_en: "You must accept the consequences of your actions.", ex_ja: "あなたは自分の行動の結果を受け入れなければならない。" },
        { word: "contemporary", options: ["現代の・同世代の", "古代の", "一時的な", "恒久的な"], answer: "現代の・同世代の", hint: "con（共に）＋tempor（時間）。「今まさに同じ時代を生きている」という意味。", goro: "「今、天ぷら（てんぽらり）食う」現代の若者", ex_en: "I prefer contemporary art to classic paint.", ex_ja: "私は古典的な絵画より現代アートの方が好きだ。" },
        { word: "contradict", options: ["矛盾する・反論する", "裏付ける", "翻訳する", "予測する"], answer: "矛盾する・反論する", hint: "contra（反対に）＋dict（言う）。前の話と辻褄が合わないこと。", goro: "「今トラ出た」とさっきの話に矛盾する", ex_en: "His actions contradict his words.", ex_ja: "彼の行動は言葉と矛盾している。" },
        { word: "contribute", options: ["貢献する・寄付する", "配布する", "邪魔する", "収集する"], answer: "貢献する・寄付する", hint: "con（共に）＋tribute（与える）。社会や組織のために力やお金を出すこと。", goro: "「今（こん）取り分（とりびゅーと）」をボランティアに貢献する", ex_en: "Many factors contribute to global warming.", ex_ja: "多くの要因が地球温暖化に貢献（影響）している。" },
        { word: "crucial", options: ["決定的な・極めて重要な", "些細な", "残酷な", "退屈な"], answer: "決定的な・極めて重要な", hint: "運命を分けるほど、ものすごく重要で不可欠な局面。", goro: "「狂う、猿（くるーしゃる）」にはバナナが極めて重要だ", ex_en: "Vitamin C plays a crucial role in health.", ex_ja: "ビタミンCは健康に極めて重要な役割を果たしている。" },
        { word: "cynical", options: ["冷笑的な・ひねくれた", "情熱的な", "楽天的な", "無邪気な"], answer: "冷笑的な・ひねくれた", hint: "「どうせ綺麗事だろ」と、物事を冷めた目で見ること。", goro: "「死に（しに）かかる」人を見て冷笑的な態度をとるな", ex_en: "He is cynical about the new project's success.", ex_ja: "彼は新しいプロジェクトの成功について冷笑的（懐疑的）だ。" },
        { word: "deprive", options: ["奪う・剥奪する", "与える", "救う", "描写する"], answer: "奪う・剥奪する", hint: "de（下へ・分離）＋prive（個人の）。大切な権利や自由を取り上げること。", goro: "「出っ歯（でぷら）いぶ」る権利を奪う", ex_en: "The war deprived many children of homes.", ex_ja: "戦争は多くの子供たちから家を奪った。" },
        { word: "deteriorate", options: ["悪化する", "改善する", "決定する", "遅らせる"], answer: "悪化する", hint: "状況や健康状態、質がどんどん悪い方向に落ちていくこと。", goro: "「出た、お料理（でてりおりえいと）」を放置して品質が悪化する", ex_en: "The weather conditions deteriorated rapidly.", ex_ja: "天候の状況が急速に悪化した。" },
        { word: "devastate", options: ["壊滅させる", "建設する", "開発する", "洗練する"], answer: "壊滅させる", hint: "災害や戦争などが、街や地域をボロボロに破壊し尽くすこと。", goro: "「出っ歯、捨て（でばすていと）」られた怒りで街を壊滅させる", ex_en: "The earthquake devastated the small town.", ex_ja: "地震はその小さな町を壊滅させた。" },
        { word: "diminish", options: ["減少する・小さくする", "拡大する", "維持する", "消去する"], answer: "減少する・小さくする", hint: "力や影響力、数、価値などがだんだん小さく、薄れていくこと。", goro: "「地味に死（じみにし）に向かって」元気が減少する", ex_en: "Our resources will diminish over time.", ex_ja: "私たちの資源は時間の経過とともに減少するだろう。" },
        { word: "discipline", options: ["規律・しつけ・学問分野", "混乱", "自由", "犯罪"], answer: "規律・しつけ・学問分野", hint: "自分をコントロールするための厳しいルールや訓練のこと。", goro: "「弟子プリン」食うなと規律を叩き込む", ex_en: "Yoga requires strict physical discipline.", ex_ja: "ヨガには厳格な肉体的規律（自己管理）が必要だ。" },
        { word: "discriminate", options: ["差別する・区別する", "歓迎する", "統合する", "保護する"], answer: "差別する・区別する", hint: "人種や性別などで、不当に不公平な扱いをすること。", goro: "「ディス（です）くり、見ないと」偏見で差別する", ex_en: "It is illegal to discriminate against workers.", ex_ja: "労働者を差別することは違法である。" },
        { word: "diverse", options: ["多様な", "同一の", "単純な", "退屈な"], answer: "多様な", hint: "di（離れて）。いろいろな種類や異なる性質のものが混ざっていること。", goro: "「台場（だいばーす）には」多様な国籍の人がいる", ex_en: "We need to hear diverse opinions.", ex_ja: "私たちは多様な意見を聞く必要がある。" },
        { word: "domestic", options: ["国内の・家庭の", "海外の", "野蛮な", "商業的な"], answer: "国内の・家庭の", hint: "dome（家）が語源。国際（international）の対義語として頻出。", goro: "「ドメ（どめ）さんチックな」国内の政治ニュース", ex_en: "The domestic flight was delayed by snow.", ex_ja: "国内線のフライトが雪で遅れた。" },
        { word: "drastic", options: ["徹底的な・劇的な", "穏やかな", "伝統的な", "疑わしい"], answer: "徹底的な・劇的な", hint: "やり方や変化がのんびりしておらず、過激で効果がすぐ出る様子。", goro: "「どらす（どらすてぃっく）と」徹底的な大掃除をする", ex_en: "We need to make a drastic change in strategy.", ex_ja: "私たちは戦略を徹底的に（ドラスティックに）変更する必要がある。" },
        { word: "efficient", options: ["効率的な", "無駄な", "不十分な", "一時的な"], answer: "効率的な", hint: "時間やエネルギー、お金を無駄にせず、成果を出せること。", goro: "「いいフィッシュ（えふぃしゃんと）」を効率的な網で捕る", ex_en: "This new machine is very efficient.", ex_ja: "この新しい機械はとても効率的だ。" },
        { word: "eliminate", options: ["排除する・除去する", "追加する", "歓迎する", "模倣する"], answer: "排除する・除去する", hint: "e（外に）＋limin（境界）。不要なものを完全に外に放り出すこと。", goro: "「襟見（えりみね）っと」汚れを完全に排除する", ex_en: "The team wants to eliminate mistakes.", ex_ja: "チームはミスを排除したいと考えている。" },
        { word: "emerge", options: ["現れる・明らかになる", "消え去る", "沈む", "結合する"], answer: "現れる・明らかになる", hint: "隠れていたものが、水面や表面にスーッと姿を現すこと。", goro: "「今（いま）おじ」さんが霧の中から現れる", ex_en: "The sun emerged from behind the clouds.", ex_ja: "太陽が雲の後ろから現れた。" },
        { word: "encounter", options: ["遭遇する・予期せぬ出会い", "避ける", "計画する", "訪問する"], answer: "遭遇する・予期せぬ出会い", hint: "偶然、危険なことや知らない人にばったり出くわすこと。", goro: "「縁（えん）買うた（かうた）ら」未知の生物に遭遇する", ex_en: "I encountered a strange problem yesterday.", ex_ja: "昨日、奇妙な問題に遭遇した。" },
        { word: "endure", options: ["耐える・持ちこたえる", "楽しむ", "拒絶する", "崩壊する"], answer: "耐える・持ちこたえる", hint: "dur（硬い・続く）。苦痛や試練を、じっと我慢して生き残ること。", goro: "「円、じゅ（えんでゅあ）っと」熱い鉄板の上で耐える", ex_en: "She had to endure heavy pain.", ex_ja: "彼女は激しい痛みに耐えなければならなかった。" },
        { word: "enhance", options: ["高める・強化する", "下げる", "無視する", "破壊する"], answer: "高める・強化する", hint: "すでに良い状態のものの価値や魅力を、さらに引き上げること。", goro: "「縁（えん）はん（はんす）を」結んで魅力を高める", ex_en: "Good sleep will enhance your memory.", ex_ja: "質の良い睡眠はあなたの記憶力を高めるだろう。" },
        { word: "enormous", options: ["巨大な・莫大な", "微小な", "平凡な", "奇妙な"], answer: "巨大な・莫大な", hint: "e（外に）＋norm（標準）。普通の基準をはるかに飛び越えて大きいこと。", goro: "「絵が、飲む（えのーます）」巨大なクジラ", ex_en: "The project required an enormous amount of money.", ex_ja: "そのプロジェクトには莫大なお金が必要だった。" },
        { word: "equivalent", options: ["同等の・相当するもの", "異なる", "不公平な", "一時的な"], answer: "同等の・相当するもの", hint: "equi（同じ）＋vale（価値）。価値や量が全く同じレベルであること。", goro: "「行く、いば（いきゅいばれんと）ら」の道でも同等の価値がある", ex_en: "One glass of wine is equivalent to a beer shot.", ex_ja: "ワイングラス1杯はビール1つと同等だ。" },
        { word: "evaluate", options: ["評価する・見極める", "無視する", "批判する", "安く売る"], answer: "評価する・見極める", hint: "e（外に）＋value（価値）。テストやデータを見て、価値をじっくり判断すること。", goro: "「いば（いばりゅえいと）る」生徒の能力を正しく評価する", ex_en: "We need to evaluate the new policy.", ex_ja: "私たちは新しい政策を評価する必要がある。" },

        // 51-75
        { word: "evident", options: ["明白な・明らかな", "隠された", "疑わしい", "複雑な"], answer: "明白な・明らかな", hint: "証拠（evidence）から推測できるように、見ればすぐに分かること。", goro: "「海老、田ん（えびでんと）ぼ」にいるのは明白だ", ex_en: "It was evident that she was lying.", ex_ja: "彼女が嘘をついていることは明白だった。" },
        { word: "exert", options: ["（力などを）行使する・出す", "隠す", "怠ける", "リラックスする"], answer: "（力などを）行使する・出す", hint: "自分の持っている影響力やプレッシャーを、対象にぐっと働かせること。", goro: "「行く、ざ（いくざーと）っと」現場で影響力を行使する", ex_en: "You should exert your influence to stop this.", ex_ja: "これを止めるためにあなたの影響力を行使すべきだ。" },
        { word: "exploit", options: ["利用する・搾取する・開発する", "保護する", "寄付する", "謝罪する"], answer: "利用する・搾取する・開発する", hint: "（悪い意味で）人をタダ働きさせて甘い汁を吸う、または資源を有効利用する。", goro: "「行く、スプロ（いくすぷろいと）」の技術を不当に利用する", ex_en: "The company was accused of exploiting workers.", ex_ja: "その会社は労働者を搾取しているとして非難された。" },
        { word: "extinct", options: ["絶滅した・消えた", "生存している", "繁栄している", "危険な"], answer: "絶滅した・消えた", hint: "恐竜や昔の動物が、この地球上から完全にいなくなってしまった状態。", goro: "「行く、筋（いくすてぃんくと）肉」が絶滅した未来", ex_en: "Dinosaurs became extinct millions of years ago.", ex_ja: "恐竜は何百万年も前に絶滅した。" },
        { word: "fluctuate", options: ["変動する・上下する", "安定する", "停止する", "上昇する"], answer: "変動する・上下する", hint: "株価や気温、感情のグラフが波のように上がったり下がったりすること。", goro: "「降る、口開（ふるくちゅえいと）けて」株価が激しく変動する", ex_en: "Oil prices fluctuate every day.", ex_ja: "原油価格は毎日変動する。" },
        { word: "foster", options: ["促進する・育成する", "邪魔する", "無視する", "拒絶する"], answer: "促進する・育成する", hint: "子供を育てる（里親）、または良い関係やアイデアを大事に育むこと。", goro: "「干す、た（ほすたー）おる」を片付けて子供の自立を育成する", ex_en: "The school aims to foster creativity.", ex_ja: "学校は創造性を育む（促進する）ことを目指している。" },
        { word: "frustrate", options: ["挫折させる・イライラさせる", "満足させる", "励ます", "安心させる"], answer: "挫折させる・イライラさせる", hint: "計画を邪魔して、相手に「もう嫌だ！」とストレスを感じさせること。", goro: "「ふ（ふらすとれいと）らっと」立ち寄った店が閉まっていてイライラさせる", ex_en: "The lack of progress frustrated the team.", ex_ja: "進捗の遅れがチームをイライラさせた。" },
        { word: "genuine", options: ["本物の・純粋な", "偽の", "高価な", "危険な"], answer: "本物の・純粋な", hint: "フェイク（偽物）ではなく、中身が混じり気なしのリアルなもの。", goro: "「銭（ぜにゅいん）が」すべて本物の金貨だ", ex_en: "Is this leather jacket genuine?", ex_ja: "このレザージャケットは本物ですか？" },
        { word: "hostile", options: ["敵意のある・不都合な", "友好的な", "無関心な", "歓迎する"], answer: "敵意のある・不都合な", hint: "相手に対して攻撃的で、全く受け入れる気がない様子。", goro: "「干す、平（ほすたいる）気で」敵意のある態度を隠さない", ex_en: "The crowd was hostile to the speaker.", ex_ja: "聴衆はスピーカー（演者）に対して敵意を含んでいた。" },
        { word: "hypocrisy", options: ["偽善・猫かぶり", "誠実", "親切", "傲慢"], answer: "偽善・猫かぶり", hint: "心の中では悪いことを考えているのに、表面だけ立派に見せること。", goro: "「引っ張（ひぽくりし）りだす」政治家の偽善を暴く", ex_en: "I cannot stand his hypocrisy.", ex_ja: "彼の偽善には耐えられない。" },
        { word: "immense", options: ["巨大な・計り知れない", "些細な", "軽い", "狭い"], answer: "巨大な・計り知れない", hint: "im（否定）＋mense（測る）。測定できないほどスケールが大きいこと。", goro: "「胃、めん（いめんす）どくせえ」ほど巨大な大盛りカレー", ex_en: "The project was an immense success.", ex_ja: "そのプロジェクトは計り知れないほど巨大な成功を収めた。" },
        { word: "impede", options: ["邪魔する・遅らせる", "促進する", "許可する", "加速する"], answer: "邪魔する・遅らせる", hint: "進行中の物事の前に立ちふさがって、足止めをすること。", goro: "「淫（いんぴーど）に」耽って勉強の進捗を邪魔する", ex_en: "Storms impeded the rescue dynamic.", ex_ja: "嵐が救助活動を邪魔した（遅らせた）。" },
        { word: "implement", options: ["実行する・道具", "計画する", "延期する", "放棄する"], answer: "実行する・道具", hint: "決まった計画やポリシーを、実際に形にしてスタートさせること。", goro: "「犬、プリン（いんぷりめんと）食う」計画を実行する", ex_en: "We will implement the new rules next week.", ex_ja: "私たちは来週、新しい規則を実行（導入）します。" },
        { word: "imply", options: ["暗に意味する・ほのめかす", "はっきり言う", "否定する", "翻訳する"], answer: "暗に意味する・ほのめかす", hint: "言葉でダイレクトに言わず、ニュアンスや態度で「察せさせる」こと。", goro: "「犬、ぷら（いんぷらい）い」どを捨てておねだりをほのめかす", ex_en: "His silence implied consent.", ex_ja: "彼の沈黙は同意を暗に意味していた。" },
        { word: "indispensable", options: ["不可欠な・絶対必要な", "不要な", "安価な", "一時的な"], answer: "不可欠な・絶対必要な", hint: "in（否定）＋dispense（無しで済ます）。それを取り除いたら成り立たないこと。", goro: "「委員、田んぼ（いんでぃすぺんさぶる）に」不可欠なリーダーだ", ex_en: "Computers are indispensable to modern work.", ex_ja: "コンピューターは現代の仕事に不可欠だ。" },
        { word: "inevitable", options: ["避けられない・必然の", "偶然の", "回避できる", "疑わしい"], answer: "避けられない・必然の", hint: "in（否定）＋evitable（避けられる）。どうあがいても絶対に起こること。", goro: "「犬、エビ食べる（いねびたぶる）」のは避けられない運命だ", ex_en: "Changes are an inevitable part of life.", ex_ja: "変化は人生の避けられない一部である。" },
        { word: "infer", options: ["推測する", "決定する", "証明する", "拒絶する"], answer: "推測する", hint: "事実やデータという「手がかり」を元に、結論を導き出すこと。", goro: "「イン（いんふぁー）ドア」な情報から犯人を推測する", ex_en: "What can you infer from this passage?", ex_ja: "この文章からあなたは何を推測できますか？" },
        { word: "infrastructure", options: ["社会的基盤・インフラ", "上部構造", "自然環境", "商業都市"], answer: "社会的基盤・インフラ", hint: "道路、水道、電気など、生活を支える土台となる設備のこと。", goro: "「インフラ（いんふらすとらくちゃー）」が壊れると生活できない", ex_en: "The government invested in new infrastructure.", ex_ja: "政府は新しい社会的基盤（インフラ）に投資した。" },
        { word: "inherent", options: ["固有の・生まれつきの", "後天的な", "一時的な", "人工的な"], answer: "固有の・生まれつきの", hint: "あとから付いたものではなく、その存在自体に最初から組み込まれている性質。", goro: "「犬、平和（いんへれんと）に」生きる固有の権利がある", ex_en: "Every choice has its inherent risks.", ex_ja: "すべての選択には、それに固有の（つきものの）リスクがある。" },
        { word: "inhibit", options: ["抑制する・妨げる", "促進する", "奨励する", "開始する"], answer: "抑制する・妨げる", hint: "プレッシャーや不安が、行動や成長にブレーキをかけること。", goro: "「犬、日々（いんひびっと）の」ストレスで行動を抑制する", ex_en: "Fear can inhibit kids' growth.", ex_ja: "恐怖は子供たちの成長を妨げる（抑制する）ことがある。" },
        { word: "innovative", options: ["革新的な・画期的な", "保守的な", "古い", "退屈な"], answer: "革新的な・画期的な", hint: "これまでの常識をガラリと変えるような、全く新しいアイデア。", goro: "「胃の、鍋（いのべいてぃぶ）」という革新的な料理", ex_en: "The tech startup designed an innovative app.", ex_ja: "そのテック系スタートアップは革新的なアプリを設計した。" },
        { word: "insight", options: ["洞察力・見通し", "視力", "盲目", "過去の回想"], answer: "洞察力・見通し", hint: "in（中を）＋sight（見る）。物事の本質をパッと見抜く深い理解力。", goro: "「イン（いんさいと）ドア」派ほど鋭い洞察力を持つ", ex_en: "The book offers sharp insights into human nature.", ex_ja: "その本は人間の本質に対する鋭い洞察力を与えてくれる。" },
        { word: "integrate", options: ["統合する・溶け込ませる", "分離する", "排除する", "破壊する"], answer: "統合する・溶け込ませる", hint: "別々のパーツやバラバラのグループを、1つの大きな塊にまとめること。", goro: "「いい、インテ（いんてぐれいと）リ」が集まって会社を統合する", ex_en: "The plan tries to integrate the two systems.", ex_ja: "その計画は2つのシステムを統合しようとしている。" },
        { word: "intense", options: ["激しい・強烈な", "穏やかな", "退屈な", "薄い"], answer: "激しい・強烈な", hint: "光や熱、プレッシャー、感情などが、ギリギリまで張り詰めて強い状態。", goro: "「陰、点（いんてんす）ける」と激しい光が差し込む", ex_en: "He felt intense pressure to pass the test.", ex_ja: "彼はテストに合格しなければという激しいプレッシャーを感じていた。" },
        { word: "interfere", options: ["干渉する・邪魔する", "協力する", "無視する", "許可する"], answer: "干渉する・邪魔する", hint: "inter（間に）＋fere（打つ）。他人の問題に首を突っ込んで邪魔をすること。", goro: "「インター、古い（いんたふぃあ）」からネットの通信を邪魔する", ex_en: "Don't interfere with my private life.", ex_ja: "私の私生活に干渉しないでくれ。" },

        // 76-100
        { word: "jeopardize", options: ["危険にさらす", "保護する", "保証する", "避ける"], answer: "危険にさらす", hint: "間違った行動のせいで、安全や将来の計画を台無しにする危機に追い込むこと。", goro: "「邪魔（じゃぱだいず）する」男が計画を危険にさらす", ex_en: "A scandal could jeopardize his political career.", ex_ja: "スキャンダルは彼の政治生命を危険にさらしかねない。" },
        { word: "justify", options: ["正当化する・言い訳をする", "非難する", "謝罪する", "証明する"], answer: "正当化する・言い訳をする", hint: "just（正しい）にする。自分の行動が間違っていないと、理由を並べて主張すること。", goro: "「邪推（じゃすてぃふぁい）」を必死に正当化する", ex_en: "You cannot justify your violent behavior.", ex_ja: "あなたは自分の暴力的な行為を正当化することはできない。" },
        { word: "legitimate", options: ["合法的な・正当な", "違法の", "怪しい", "不公平な"], answer: "合法的な・正当な", hint: "法律（legal）に則っており、世間からも「筋が通っている」と認められること。", goro: "「路地（れじてぃめっと）裏」の店だが合法的な営業だ", ex_en: "He has a legitimate reason for being late.", ex_ja: "彼が遅刻したのには正当な理由がある。" },
        { word: "liability", options: ["法的責任・負債・足手まとい", "資産", "特権", "信頼"], answer: "法的責任・負債・足手まとい", hint: "トラブルが起きた時に背負うマイナスの義務、またはチームの重荷。", goro: "「来い（らいあびりてぃ）、ビリ」の奴はチームの足手まといだ", ex_en: "The company admitted liability for the accident.", ex_ja: "会社はその事故に対する法的責任を認めた。" },
        { word: "literal", options: ["文字通りの・誇張なしの", "比喩的な", "架空の", "退屈な"], answer: "文字通りの・誇張なしの", hint: "比喩ではなく、書いてある文字をそのままストレートに受け取る意味。", goro: "「リ（りてらる）んご、樽」ごと文字通りの意味で受け取る", ex_en: "The literal meaning of the word is different.", ex_ja: "その単語の文字通りの意味は異なっている。" },
        { word: "lucrative", options: ["利益の上がる・儲かる", "大赤字の", "無料の", "質素な"], answer: "利益の上がる・儲かる", hint: "lucre（利益）。やればやるほどガッポリお金が入ってくるビジネスを指す。", goro: "「ルクラ（るーくらてぃぶ）ト」で大金が手に入る儲かるビジネス", ex_en: "The store proved to be a lucrative business.", ex_ja: "その店は利益の上がる（儲かる）ビジネスであることが分かった。" },
        { word: "manifest", options: ["明らかにする・示す・明白な", "隠す", "否定する", "偽造する"], answer: "明らかにする・示す・明白な", hint: "選挙の「マニフェスト（政権公約）」の語源。心の中の意図をはっきり外に示すこと。", goro: "「マニ（まにふぇすと）フェスト」で自分の意志を明らかにする", ex_en: "He manifested his displeasure with a frown.", ex_ja: "彼は顔をしかめて不快感を明らかにした（示した）。" },
        { word: "marginal", options: ["わずかな・辺境の", "膨大な", "中心的な", "重要な"], answer: "わずかな・辺境の", hint: "ノートの端の余白（margin）が語源。中心ではなく、ギリギリおまけ程度の量。", goro: "「まー、地（まーじなる）の」果てにあるわずかな畑", ex_en: "There was only a marginal improvement in sales.", ex_ja: "売上にはわずかな改善しかなかった。" },
        { word: "mutual", options: ["相互の・共通の", "一方的な", "独立した", "個人的な"], answer: "相互の・共通の", hint: "片思いではなく、お互いが同じ感情や関係を持っている状態。", goro: "「ミュー（みゅーちゅある）タント」同士の相互の理解", ex_en: "Mutual respect is key to a good marriage.", ex_ja: "相互の尊重が良い結婚生活の鍵である。" },
        { word: "naive", options: ["うぶな・世間知らずな", "洗練された", "残酷な", "賢い"], answer: "うぶな・世間知らずな", hint: "日本では「繊細」の意味で使われますが、英語では「騙されやすい素人」というネガティブな意味。", goro: "「内（ないーぶ）部」の事情を知らない世間知らずな新人", ex_en: "It was naive of you to believe his promise.", ex_ja: "彼の約束を信じるなんて君もうぶ（世間知らず）だったね。" },
        { word: "negligible", options: ["無視できるほどの・些細な", "重大な", "不可欠な", "危険な"], answer: "無視できるほどの・些細な", hint: "neglect（無視する）＋able。小さすぎて、計算や考慮に入れなくていいレベル。", goro: "「ねぎ（ねぐりじぶる）じる」の量は全体から見れば無視できるほどだ", ex_en: "The cost difference is negligible.", ex_ja: "コストの差は無視できるほど（ごくわずか）だ。" },
        { word: "notable", options: ["注目すべき・著名な", "平凡な", "悪名高い", "一時的な"], answer: "注目すべき・著名な", hint: "ノート（note）に書き留めるに値するほど、素晴らしくて有名なこと。", goro: "「脳、た（のーたぶる）っぷり」ある注目すべき天才", ex_en: "This city is notable for its historic castle.", ex_ja: "この街は歴史的な城があることで注目すべき（有名だ）である。" },
        { word: "obstacle", options: ["障害（物）", "目標", "援助", "通路"], answer: "障害（物）", hint: "ゴールに向かって進む道をふさいでいる、邪魔な壁や困難。", goro: "「おば（おぶすたくる）さん、乗る車」が最大の障害物だ", ex_en: "A lack of money is the main obstacle to studying.", ex_ja: "お金の不足が留学への最大の障害だ。" },
        { word: "orthodox", options: ["正統派の・伝統的な", "異端の", "過激な", "最新の"], answer: "正統派の・伝統的な", hint: "広く認められた正しいルールや、王道のやり方に従っている様子。", goro: "「大（おーそどっくす）どくしい」伝統的な儀式", ex_en: "He took an orthodox approach to solve the math problem.", ex_ja: "彼はその数学の問題を解くために正統派の（王道の）アプローチをとった。" },
        { word: "paradox", options: ["逆説・パラドックス", "真実", "嘘", "一致"], answer: "逆説・パラドックス", hint: "一見すると間違っているように見えるが、実は正しい（またはその逆の）奇妙な論理。", goro: "「パラ（ぱらどっくす）ダイス」なのに地獄という逆説", ex_en: "It is a paradox that richer people sometimes feel poorer.", ex_ja: "金持ちの方が時に貧しさを感じるというのは逆説（矛盾）である。" },
        { word: "pessimistic", options: ["悲観的な", "楽天的な", "積極的な", "現実的な"], answer: "悲観的な", hint: "「どうせ失敗するに決まっている」と、未来を悪くばかり考えること。", goro: "「ペット、死（ぺしみすてぃっく）ぬ」と起きてもいない未来に悲観的な人", ex_en: "Don't be so pessimistic about the interview.", ex_ja: "面接についてそんなに悲観的になるなよ。" },
        { word: "plausible", options: ["もっともらしい・妥当な", "信じがたい", "偽の", "不快な"], answer: "もっともらしい・妥当な", hint: "本当に真実かどうかは別として、説明を聞くと「確かに筋が通っている」と思えること。", goro: "「プロ、う（ぷろーじぶる）まい」言い訳はもっともらしく聞こえる", ex_en: "Her story sounds plausible, but I doubt it.", ex_ja: "彼女の話はもっともらしく聞こえるが、私は疑っている。" },
        { word: "prejudice", options: ["偏見・先入観", "公平", "同情", "事実"], answer: "偏見・先入観", hint: "pre（事前に）＋judice（判断する）。事実を知る前に、勝手に思い込む歪んだ視点。", goro: "「ぷり（ぷれじゅでぃす）ぷり」怒る人は偏見に満ちている", ex_en: "We must eliminate racial prejudice.", ex_ja: "私たちは人種的偏見をなくさなければならない。" },
        { word: "prohibit", options: ["禁止する", "許可する", "推奨する", "免除する"], answer: "禁止する", hint: "法律や公式なルールによって、「やってはいけない」と指定すること。", goro: "「プロ、日々（ぷろひびっと）の」飲酒を禁止する", ex_en: "Smoking is prohibited inside the building.", ex_ja: "建物内での喫煙は禁止されている。" },
        { word: "prosper", options: ["繁栄する・成功する", "衰退する", "破産する", "消滅する"], answer: "繁栄する・成功する", hint: "ビジネスや街、国が、お金や人が集まってどんどん豊かになること。", goro: "「プロ、スーパー（ぷろすぱー）」を開いて大いに繁栄する", ex_en: "The online shopping company continues to prosper.", ex_ja: "そのオンラインショッピング会社は繁栄を続けている。" },
        { word: "radical", options: ["根本的な・過激な", "表面的な", "伝統的な", "穏やかな"], answer: "根本的な・過激な", hint: "枝葉の修正ではなく、根っこ（root）からガラリと全てを引っくり返す様子。", goro: "「裸（らでぃかる）になる」という根本的な解決策", ex_en: "The economy needs radical reform.", ex_ja: "経済には根本的な改革が必要だ。" },
        { word: "reluctant", options: ["気が進まない・嫌がる", "熱心な", "準備万端の", "誠実な"], answer: "気が進まない・嫌がる", hint: "気が進まないけれど、プレッシャーなどのせいで「しぶしぶ」やる態度。", goro: "「リ（りらくたんと）ラックス中」だから仕事に行くのが気が進まない", ex_en: "He was reluctant to share his secret.", ex_ja: "彼は秘密を話すのを嫌がった（気が進まなかった）。" },
        { word: "scrutinize", options: ["細かく調べる・精査する", "見落とす", "一瞥する", "破壊する"], answer: "細かく調べる・精査する", hint: "ミスや不正を見逃さないように、目を皿のようにして隅々までチェックすること。", goro: "「スク（すくるーてぃないず）りーんで」証拠の映像を細かく調べる", ex_en: "The bank will scrutinize your financial record.", ex_ja: "銀行はあなたの財政記録を細かく調べる（精査する）。" },
        { word: "skeptical", options: ["懐疑的な・疑い深い", "信じやすい", "確信している", "熱狂的な"], answer: "懐疑的な・疑い深い", hint: "「それって本当に本当？」と、簡単には信用しない構えを見せること。", goro: "「助っ人、軽（すけぷてぃかる）い」口を叩くから懐疑的な目で見る", ex_en: "I am skeptical about his excuse.", ex_ja: "私は彼の言い訳に対して懐疑的だ（疑っている）。" },
        { word: "subtle", options: ["微妙な・かすかな・繊細な", "明白な", "派手な", "不快な"], answer: "微妙な・かすかな・繊細な", hint: "言われないと気づかないほど、変化や違いがごくわずかで上品な様子。bは発音しません。", goro: "「里（さとる）の」空気の微妙な変化に気づく", ex_en: "There is a subtle difference between the two flavors.", ex_ja: "その2つの味の間には微妙な違いがある。" }
    ];

    let quizData = [];
    let currentQuestionIndex = 0;
    let score = 0;
    let isReviewMode = false;

    function shuffle(array) {
        return array.sort(() => Math.random() - 0.5);
    }

    function getTodayString() {
        const today = new Date();
        return today.toISOString().split('T')[0];
    }

    function addDays(dateStr, days) {
        const date = new Date(dateStr);
        date.setDate(date.getDate() + days);
        return date.toISOString().split('T')[0];
    }

    function setupReviewSystem() {
        const todayStr = getTodayString();
        const reviewSchedule = JSON.parse(localStorage.getItem("reviewSchedule100")) || {};
        
        let reviewWords = baseVocabulary.filter(item => {
            return reviewSchedule[item.word] && reviewSchedule[item.word] <= todayStr;
        });

        if (reviewWords.length > 0) {
            quizData = shuffle([...reviewWords]);
            isReviewMode = true;
            document.getElementById("mode-text").innerText = "⏰ 復習モード (本日分)";
            document.getElementById("mode-text").style.backgroundColor = "#fce8e6";
            document.getElementById("mode-text").style.color = "#c5221f";
        } else {
            quizData = shuffle([...baseVocabulary]);
            isReviewMode = false;
            document.getElementById("mode-text").innerText = "📖 通常モード";
            document.getElementById("mode-text").style.backgroundColor = "#e8f0fe";
            document.getElementById("mode-text").style.color = "#1a73e8";
        }
    }

    function saveWrongAnswerReview(word) {
        const reviewSchedule = JSON.parse(localStorage.getItem("reviewSchedule100")) || {};
        const todayStr = getTodayString();
        
        let currentStage = parseInt(localStorage.getItem(`stage100_${word}`)) || 0;
        let nextDays = 1;

        if (currentStage === 1) nextDays = 7;
        else if (currentStage >= 2) nextDays = 30;
        
        reviewSchedule[word] = addDays(todayStr, nextDays);
        localStorage.setItem(`stage100_${word}`, currentStage + 1);
        localStorage.setItem("reviewSchedule100", JSON.stringify(reviewSchedule));
    }

    function clearCorrectAnswerReview(word) {
        const reviewSchedule = JSON.parse(localStorage.getItem("reviewSchedule100")) || {};
        if (reviewSchedule[word]) {
            delete reviewSchedule[word];
            localStorage.removeItem(`stage100_${word}`);
            localStorage.setItem("reviewSchedule100", JSON.stringify(reviewSchedule));
        }
    }

    function setDayBackground() {
        const day = new Date().getDay();
        document.body.style.background = `var(--bg-${day})`;
    }

    function speakWord(text) {
        if ('speechSynthesis' in window && text !== "---") {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'en-US';
            utterance.rate = 0.85;
            window.speechSynthesis.speak(utterance);
        }
    }

    function initGame() {
        setDayBackground();
        setupReviewSystem();
        currentQuestionIndex = 0;
        score = 0;
        loadQuestion();
    }

    function loadQuestion() {
        const currentQuestion = quizData[currentQuestionIndex];
        
        document.getElementById("progress-text").innerText = `第 ${currentQuestionIndex + 1} 問 / 全 ${quizData.length} 問`;
        document.getElementById("score-text").innerText = `スコア: ${score}`;
        const progressPercent = ((currentQuestionIndex) / quizData.length) * 100;
        document.getElementById("progress-fill").style.width = `${progressPercent}%`;

        document.getElementById("word").innerText = currentQuestion.word;
        document.getElementById("explanation").style.display = "none";

        setTimeout(() => speakWord(currentQuestion.word), 100);

        const optionsContainer = document.getElementById("options");
        optionsContainer.innerHTML = "";
        document.getElementById("result").innerText = "";
        document.getElementById("next-btn").style.display = "none";

        const shuffledOptions = shuffle([...currentQuestion.options]);

        shuffledOptions.forEach((option, index) => {
            const button = document.createElement("button");
            button.className = "option-btn";
            button.innerText = `${index + 1}. ${option}`;
            button.onclick = () => checkAnswer(option, button);
            optionsContainer.appendChild(button);
        });
    }

    function checkAnswer(selected, selectedButton) {
        const currentQuestion = quizData[currentQuestionIndex];
        const correctAnswer = currentQuestion.answer;
        const resultDiv = document.getElementById("result");
        
        const buttons = document.querySelectorAll(".options button");
        buttons.forEach(btn => btn.disabled = true);

        if (selected === correctAnswer) {
            resultDiv.innerText = "⭕ 正解！";
            resultDiv.className = "correct";
            selectedButton.style.backgroundColor = "#e6f4ea";
            selectedButton.style.borderColor = "#34a853";
            selectedButton.style.color = "#137333";
            score++;
            document.getElementById("score-text").innerText = `スコア: ${score}`;
            
            if (isReviewMode) {
                clearCorrectAnswerReview(currentQuestion.word);
            }
        } else {
            resultDiv.innerText = `❌ 不正解... (正解: ${correctAnswer})`;
            resultDiv.className = "incorrect";
            selectedButton.style.backgroundColor = "#fce8e6";
            selectedButton.style.borderColor = "#ea4335";
            selectedButton.style.color = "#c5221f";
            
            buttons.forEach(btn => {
                if (btn.innerText.includes(correctAnswer)) {
                    btn.style.backgroundColor = "#e6f4ea";
                    btn.style.borderColor = "#34a853";
                }
            });

            document.getElementById("exp-text").innerText = currentQuestion.hint;
            document.getElementById("exp-goro").innerText = `💡 語呂合わせ: ${currentQuestion.goro}`;
            document.getElementById("exp-ex-en").innerText = currentQuestion.ex_en;
            document.getElementById("exp-ex-ja").innerText = currentQuestion.ex_ja;
            
            document.getElementById("speaker-ex-btn").onclick = () => speakWord(currentQuestion.ex_en);
            document.getElementById("explanation").style.display = "block";

            saveWrongAnswerReview(currentQuestion.word);
        }

        const nextBtn = document.getElementById("next-btn");
        if (currentQuestionIndex === quizData.length - 1) {
            nextBtn.innerText = "結果を見る";
        } else {
            nextBtn.innerText = "次の問題へ";
        }
        nextBtn.style.display = "block";
    }

    function nextQuestion() {
        currentQuestionIndex++;
        if (currentQuestionIndex < quizData.length) {
            loadQuestion();
        } else {
            document.getElementById("progress-fill").style.width = "100%";
            
            let finishTitle = isReviewMode ? "🎉 今日の復習完了！" : "🎉 100語マスタークイズ終了！";
            let comment = "圧倒的な語彙力が身についてきています！";
            if (score / quizData.length < 0.6) {
                comment = "難関大レベルなので、最初は間違えて当然。明日自動で出題されるので語呂合わせで今覚えちゃいましょう！";
            }

            document.getElementById("quiz-card").innerHTML = `
                <h1 style="font-size: 24px; margin-bottom: 10px;">${finishTitle}</h1>
                <p style="font-size: 18px; color: #333; margin-top: 20px;">
                    成果: <strong>${quizData.length} 問中 ${score} 問正解</strong>
                </p>
                <p style="font-size: 14px; color: #666; margin-bottom: 30px;">${comment}</p>
                <button class="action-btn" style="display:block;" onclick="location.reload()">もう一度トップに戻る</button>
            `;
        }
    }

    initGame();
</script>

</body>
</html
