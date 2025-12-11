<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>三寶媽咪記帳本</title>
    <link href="https://fonts.googleapis.com/css2?family=Varela+Round&display=swap" rel="stylesheet">
    <style>
        /* --- 基礎設定 --- */
        :root {
            --bg-color: #ffe6ee;
            --card-white: #ffffff;
            --main-color: #ff9ebb;
            --gradient-start: #ff9ebb;
            --gradient-end: #ffc8dd;
            --text-dark: #5a4a4a;
            --income-green: #4ecdc4;
            --expense-red: #ff6b6b;
        }

        body {
            font-family: 'Varela Round', 'Microsoft JhengHei', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-dark);
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            padding: 15px;
            min-height: 100vh;
            transition: background-color 0.5s ease;
            -webkit-tap-highlight-color: transparent;
            /* 防止iOS回彈效果 */
            overscroll-behavior-y: none;
        }

        /* --- 分頁切換按鈕區 --- */
        .tabs-container {
            display: flex;
            justify-content: center;
            width: 100%;
            max-width: 450px;
            margin-bottom: 20px;
            gap: 12px;
        }

        .tab-btn {
            flex: 1;
            padding: 15px 10px; /* 加大點擊範圍 */
            border: none;
            border-radius: 25px;
            font-size: 1.1rem; /* 字體加大 */
            font-weight: bold;
            color: white;
            opacity: 0.5;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            position: relative;
            top: 0;
        }

        .tab-btn.active {
            opacity: 1;
            top: -2px;
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
            border: 3px solid #fff;
        }

        .btn-lele { background-color: #ff9ebb; }
        .btn-yueyue { background-color: #f1c40f; }
        .btn-yaoyao { background-color: #58cc7e; }

        /* --- 主容器 --- */
        .container {
            width: 100%;
            max-width: 450px;
            background: var(--card-white);
            padding: 25px 20px;
            border-radius: 30px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
            border: 5px solid #fff;
            box-sizing: border-box;
        }

        h2 {
            text-align: center;
            color: var(--main-color);
            margin-top: 5px;
            margin-bottom: 20px;
            font-size: 1.8em;
            transition: color 0.5s ease;
            letter-spacing: 1px;
        }

        /* --- 餘額顯示區 --- */
        .balance-box {
            background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
            color: white;
            padding: 25px 20px;
            border-radius: 25px;
            text-align: center;
            margin-bottom: 25px;
            box-shadow: 0 8px 15px rgba(0,0,0,0.1);
            transition: background 0.5s ease;
        }

        .balance-box h4 { margin: 0; opacity: 0.9; font-size: 1rem;}
        .balance-box h1 { margin: 10px 0 0; font-size: 2.5em; letter-spacing: 1px;}

        .inc-exp-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            gap: 15px;
        }

        .inc-exp-box {
            flex: 1;
            background-color: #fff;
            border-radius: 20px;
            padding: 15px;
            text-align: center;
            border: 2px solid #f5f5f5;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
        }
        
        .inc-exp-box h4 { margin: 0 0 5px; color: #888; font-size: 0.9rem;}

        .money.plus { color: var(--income-green); font-weight: bold; font-size: 1.2em;}
        .money.minus { color: var(--expense-red); font-weight: bold; font-size: 1.2em;}

        /* --- 列表區 --- */
        h3 { margin-bottom: 15px; border-bottom: 2px dashed #eee; padding-bottom: 10px; color: #666;}

        .list {
            list-style-type: none;
            padding: 0;
            margin-bottom: 30px;
            max-height: 350px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        .list li {
            background-color: #fff;
            border-radius: 18px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 18px;
            margin-bottom: 12px;
            border: 2px solid #fbfbfb;
            box-shadow: 0 3px 8px rgba(0,0,0,0.03);
            transition: transform 0.1s;
        }
        
        .list li:active { transform: scale(0.98); }

        .list li.plus { border-left: 8px solid var(--income-green); }
        .list li.minus { border-left: 8px solid var(--expense-red); }

        .delete-btn {
            background-color: #ffecec;
            border: none;
            color: #ff6b6b;
            font-weight: bold;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            margin-left: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            cursor: pointer;
        }

        /* --- 表單區 --- */
        input, select {
            border: 2px solid #f0f0f0;
            border-radius: 15px;
            font-size: 17px; /* 16px以上防止iOS縮放 */
            padding: 15px;
            width: 100%;
            box-sizing: border-box;
            margin-bottom: 15px;
            outline: none;
            -webkit-appearance: none;
            background-color: #fafafa;
            color: #333;
        }
        
        input:focus { border-color: var(--main-color); background: #fff; }
        
        label { display: block; margin-bottom: 8px; font-weight: bold; color: var(--text-dark); margin-left: 5px;}

        .btn {
            background-color: var(--main-color);
            color: #fff;
            border: 0;
            font-size: 1.2rem;
            padding: 15px;
            width: 100%;
            border-radius: 50px;
            box-shadow: 0 5px 15px rgba(255, 158, 187, 0.3);
            font-weight: bold;
            margin-top: 10px;
            cursor: pointer;
            -webkit-appearance: none; 
            transition: background-color 0.5s ease;
        }
        
        .btn:active { transform: scale(0.97); opacity: 0.9; }

    </style>
</head>
<body>

    <div class="tabs-container">
        <button id="btn-lele" class="tab-btn btn-lele active">樂樂 🌸</button>
        <button id="btn-yueyue" class="tab-btn btn-yueyue">悅悅 ⭐</button>
        <button id="btn-yaoyao" class="tab-btn btn-yaoyao">曜曜 🍀</button>
    </div>

    <div class="container">
        <h2 id="app-title">🎀 樂樂的記帳本</h2>
        
        <div class="balance-box">
            <h4>目前持有現金</h4>
            <h1 id="balance">$0</h1>
        </div>

        <div class="inc-exp-container">
            <div class="inc-exp-box">
                <h4>總收入 💰</h4>
                <p id="money-plus" class="money plus">+$0</p>
            </div>
            <div class="inc-exp-box">
                <h4>總支出 💸</h4>
                <p id="money-minus" class="money minus">-$0</p>
            </div>
        </div>

        <h3>📝 新增一筆</h3>
        <form id="form">
            <div class="form-control">
                <label>項目名稱</label>
                <input type="text" id="text" placeholder="例如：零用錢..." autocomplete="off">
            </div>
            <div class="form-control">
                <label>金額與類別</label>
                <div style="display: flex; gap: 10px;">
                    <select id="type" style="width: 40%;">
                        <option value="expense">支出 (-)</option>
                        <option value="income">收入 (+)</option>
                    </select>
                    <input type="number" id="amount" placeholder="金額" style="width: 60%;" inputmode="decimal">
                </div>
            </div>
            <button type="submit" class="btn">✨ 記下來 ✨</button>
        </form>

        <h3>🌸 最近的紀錄</h3>
        <ul id="list" class="list"></ul>
    </div>

    <script>
        // --- 核心邏輯 (手機版優化) ---
        const balanceEl = document.getElementById('balance');
        const money_plusEl = document.getElementById('money-plus');
        const money_minusEl = document.getElementById('money-minus');
        const listEl = document.getElementById('list');
        const formEl = document.getElementById('form');
        const textEl = document.getElementById('text');
        const amountEl = document.getElementById('amount');
        const typeEl = document.getElementById('type');
        const titleEl = document.getElementById('app-title');

        const themes = {
            lele: { title: '🎀 樂樂的記帳本', bg: '#ffe6ee', main: '#ff9ebb', gradStart: '#ff9ebb', gradEnd: '#ffc8dd' },
            yueyue: { title: '⭐ 悅悅的記帳本', bg: '#fff5d7', main: '#f1c40f', gradStart: '#f1c40f', gradEnd: '#fce38a' },
            yaoyao: { title: '🍀 曜曜的記帳本', bg: '#e8f7ec', main: '#58cc7e', gradStart: '#58cc7e', gradEnd: '#a3e4b3' }
        };

        let currentUser = 'lele';
        let transactions = [];

        // 讀取 LocalStorage (手機儲存空間)
        function safeLocalStorageGet(key) {
            try {
                const data = localStorage.getItem(key);
                return data ? JSON.parse(data) : [];
            } catch (e) {
                return [];
            }
        }

        // 寫入 LocalStorage
        function safeLocalStorageSet(key, data) {
            try {
                localStorage.setItem(key, JSON.stringify(data));
            } catch (e) {
                console.error("無法寫入", e);
            }
        }

        function init() {
            // 嘗試從手機讀取最後一次停留的頁面
            const savedUser = localStorage.getItem('last_user');
            if(savedUser && themes[savedUser]){
                currentUser = savedUser;
            }

            setupEventListeners();
            loadData();
            render();
            updateColors();
            
            // 更新按鈕顯示
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelector(`.btn-${currentUser}`).classList.add('active');
        }

        function loadData() {
            transactions = safeLocalStorageGet(`transactions_${currentUser}`);
        }

        function saveData() {
            safeLocalStorageSet(`transactions_${currentUser}`, transactions);
        }

        function setupEventListeners() {
            document.getElementById('btn-lele').onclick = () => switchUser('lele');
            document.getElementById('btn-yueyue').onclick = () => switchUser('yueyue');
            document.getElementById('btn-yaoyao').onclick = () => switchUser('yaoyao');
            formEl.onsubmit = addTransaction;
        }

        function switchUser(user) {
            currentUser = user;
            // 記住最後選擇的孩子
            localStorage.setItem('last_user', user);

            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelector(`.btn-${user}`).classList.add('active');

            loadData();
            render();
            updateColors();
        }

        function updateColors() {
            const theme = themes[currentUser];
            const root = document.documentElement;
            
            titleEl.innerText = theme.title;
            root.style.setProperty('--bg-color', theme.bg);
            root.style.setProperty('--main-color', theme.main);
            root.style.setProperty('--gradient-start', theme.gradStart);
            root.style.setProperty('--gradient-end', theme.gradEnd);
        }

        function addTransaction(e) {
            e.preventDefault();

            if (textEl.value.trim() === '' || amountEl.value.trim() === '') {
                alert('⚠️ 請輸入名稱和金額喔！');
                return;
            }

            const amountValue = Number(amountEl.value);
            const typeValue = typeEl.value;
            let finalAmount = typeValue === 'expense' ? -Math.abs(amountValue) : Math.abs(amountValue);

            const transaction = {
                id: generateID(),
                text: textEl.value,
                amount: finalAmount,
                date: new Date().toLocaleDateString()
            };

            transactions.push(transaction);
            saveData();
            render();

            textEl.value = '';
            amountEl.value = '';
        }

        function generateID() {
            return Math.floor(Math.random() * 100000000);
        }

        function render() {
            listEl.innerHTML = '';
            transactions.forEach(addTransactionDOM);
            updateValues();
        }

        function addTransactionDOM(transaction) {
            const sign = transaction.amount < 0 ? '-' : '+';
            const itemClass = transaction.amount < 0 ? 'minus' : 'plus';
            const absAmount = Math.abs(transaction.amount).toLocaleString();

            const item = document.createElement('li');
            item.classList.add(itemClass);
            item.setAttribute('data-id', transaction.id);

            item.innerHTML = `
                <div style="pointer-events: none;">
                    <span style="font-weight:bold; font-size:1.1rem;">${transaction.text}</span> <br>
                    <span style="font-size:0.85em; color:#999;">${transaction.date}</span>
                </div>
                <div style="display:flex; align-items:center;">
                    <span style="font-weight:bold; margin-right:5px; font-size:1.1rem;">${sign}$${absAmount}</span>
                    <button type="button" class="delete-btn">✕</button>
                </div>
            `;
            listEl.insertBefore(item, listEl.firstChild);
        }

        function removeTransaction(id) {
            if(confirm('確定要刪除這筆紀錄嗎？')){
                transactions = transactions.filter(transaction => transaction.id !== id);
                saveData();
                render();
            }
        }

        listEl.onclick = (e) => {
             if (e.target.classList.contains('delete-btn')) {
                const li = e.target.closest('li');
                const id = Number(li.getAttribute('data-id'));
                removeTransaction(id);
            }
        };

        function updateValues() {
            const amounts = transactions.map(transaction => transaction.amount);
            const total = amounts.reduce((acc, item) => (acc += item), 0);
            const income = amounts.filter(item => item > 0).reduce((acc, item) => (acc += item), 0);
            const expense = (amounts.filter(item => item < 0).reduce((acc, item) => (acc += item), 0) * -1);

            balanceEl.innerText = `$${total.toLocaleString()}`;
            money_plusEl.innerText = `+$${income.toLocaleString()}`;
            money_minusEl.innerText = `-$${expense.toLocaleString()}`;
        }

        init();

    </script>
</body>
</html>
