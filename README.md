# luzhiyanghu
can kook dates
[养护台账管理系统.html](https://github.com/user-attachments/files/34067927/default.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>土壤传感器监测 & 养护台账管理系统</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft Yahei", system-ui, sans-serif;
        }
        body {
            background: linear-gradient(135deg, #f0f7f0, #e4efe2);
            padding: 20px;
        }
        .container {
            max-width: 1100px;
            margin: 0 auto;
        }
        header {
            text-align: center;
            padding: 20px;
            color: #2b7a2f;
        }
        header h1::before {
            content: "📡 ";
        }
        header p {
            color: #666;
            font-size: 15px;
            margin-top: 6px;
        }
        .card {
            background: #fff;
            border-radius: 18px;
            padding: 24px;
            box-shadow: 0 6px 18px rgba(44, 122, 47, 0.1);
            margin-bottom: 22px;
        }
        .card-title {
            font-size: 19px;
            color: #2b7a2f;
            margin-bottom: 18px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .grid-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
            gap: 16px;
        }
        .sensor-card {
            background: linear-gradient(145deg, #f2faf1, #f8fcf7);
            padding: 18px 12px;
            border-radius: 14px;
            text-align: center;
            border: 1px solid #d9ead3;
            transition: 0.25s;
        }
        .sensor-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
        }
        .sensor-name {
            font-size: 14px;
            color: #555;
        }
        .sensor-val {
            font-size: 28px;
            font-weight: bold;
            color: #2b7a2f;
            margin: 8px 0;
        }
        .progress-outer {
            width: 100%;
            height: 9px;
            background: #ddecdb;
            border-radius: 10px;
            overflow: hidden;
        }
        .progress-inner {
            height: 100%;
            background: #4caf50;
            border-radius: 10px;
            transition: 0.7s ease;
            width: 0;
        }
        .advice-box {
            margin-top: 20px;
            padding: 16px 20px;
            border-radius: 12px;
            border-left: 7px solid;
            line-height: 1.7;
        }
        .advice-normal {
            background: #eaf7eb;
            border-left-color: #388e3c;
        }
        .advice-warn {
            background: #fff8e1;
            border-left-color: #f57c00;
        }
        .advice-danger {
            background: #ffebee;
            border-left-color: #e53935;
        }
        .info-text {
            margin: 12px 0;
            color: #777;
            font-size: 14px;
            line-height: 1.8;
        }
        .btn-group {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 16px 0;
        }
        button {
            padding: 10px 18px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 14px;
            transition: 0.2s;
        }
        .btn-primary {
            background: #388e3c;
            color: white;
        }
        .btn-primary:hover {
            background: #2b7a2f;
        }
        .btn-secondary {
            background: #86b889;
            color: white;
        }
        .btn-secondary:hover {
            background: #6aa36e;
        }
        .btn-warning {
            background: #f57c00;
            color: white;
        }
        .btn-warning:hover {
            background: #e65100;
        }
        .btn-gray {
            background: #aaa;
            color: white;
        }
        .btn-gray:hover {
            background: #888;
        }
        .btn-mini {
            padding: 5px 12px;
            font-size: 12.5px;
            border-radius: 8px;
        }
        .search-bar {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        .search-bar input,
        .search-bar select {
            padding: 11px 14px;
            border: 1px solid #b9d7b3;
            border-radius: 10px;
            outline: none;
            font-size: 14px;
            background: #fff;
        }
        .search-bar input {
            flex: 1;
            min-width: 160px;
        }
        .search-bar input:focus,
        .search-bar select:focus {
            border-color: #388e3c;
            box-shadow: 0 0 0 3px rgba(56, 142, 60, 0.12);
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            padding: 12px 6px;
            text-align: center;
            font-size: 13.5px;
            border-bottom: 1px solid #eee;
        }
        th {
            background: #e8f5e9;
            color: #2b7a2f;
        }
        .tag {
            padding: 4px 9px;
            border-radius: 20px;
            color: white;
            font-size: 12.5px;
        }
        .tag-n { background: #4caf50; }
        .tag-w { background: #f57c00; }
        .tag-d { background: #e53935; }
        .empty-row td {
            color: #999;
            padding: 24px 6px;
        }
        #chart {
            width: 100%;
            height: 270px;
        }
        .chart-fallback {
            height: 270px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #999;
            font-size: 14px;
            background: #f7fbf6;
            border-radius: 12px;
        }
        /*绿植弹窗*/
        .mask {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.4);
            display: none;
            z-index: 99;
        }
        .popup {
            width: 420px;
            max-width: 92vw;
            background: #fff;
            border-radius: 16px;
            padding: 24px;
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
        }
        .popup h3 {
            margin-bottom: 16px;
            color: #2b7a2f;
        }
        .popup input {
            width: 100%;
            margin: 7px 0;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 8px;
        }
        .popup .btn-group {
            margin-bottom: 0;
        }
        /*打印样式*/
        @media print {
            .btn-group, button, .mask, #chart, .search-bar {
                display: none !important;
            }
            body { background: white; }
            .card { box-shadow: none; border: 1px solid #ccc; page-break-inside: avoid; }
        }
    </style>
</head>
<body>
<div class="container">
    <header>
        <h1>智能土壤传感器监测 & 养护台账管理系统</h1>
        <p>数据底层与持久化台账模块｜负责人：杜萱｜模拟传感器采集+本地持久化存储</p>
    </header>

    <!-- 实时传感器 -->
    <div class="card">
        <div class="card-title">📊 实时土壤采集数据（模拟传感器信号）</div>
        <div class="grid-row">
            <div class="sensor-card">
                <div class="sensor-name">土壤含水量(%)</div>
                <div class="sensor-val" id="val-water">--</div>
                <div class="progress-outer"><div class="progress-inner" id="p-water"></div></div>
            </div>
            <div class="sensor-card">
                <div class="sensor-name">氮 N (mg/kg)</div>
                <div class="sensor-val" id="val-n">--</div>
                <div class="progress-outer"><div class="progress-inner" id="p-n"></div></div>
            </div>
            <div class="sensor-card">
                <div class="sensor-name">磷 P (mg/kg)</div>
                <div class="sensor-val" id="val-p">--</div>
                <div class="progress-outer"><div class="progress-inner" id="p-p"></div></div>
            </div>
            <div class="sensor-card">
                <div class="sensor-name">钾 K (mg/kg)</div>
                <div class="sensor-val" id="val-k">--</div>
                <div class="progress-outer"><div class="progress-inner" id="p-k"></div></div>
            </div>
        </div>
        <div class="advice-box advice-normal" id="advice">
            <strong>💡 养护建议：</strong>正在读取数据…
        </div>
        <div class="info-text">
            上次更新时间：<span id="lastTime">--</span><br>
            自动更新周期：<span id="intervalText">--</span>，下次自动更新：<span id="nextUpdate">--</span>
        </div>
        <div class="btn-group">
            <button class="btn-primary" onclick="sampleNow()">🔄 手动刷新模拟数据</button>
            <button class="btn-secondary" onclick="adoptAdvice()">✅ 采纳建议并记养护事件</button>
        </div>
    </div>

    <!-- 绿植信息管理（CRUD） -->
    <div class="card">
        <div class="card-title">🪴 绿植信息管理（增删改查）</div>
        <div class="btn-group">
            <button class="btn-primary" onclick="openPlantPopup(null)">＋ 添加绿植</button>
        </div>
        <table>
            <thead>
                <tr>
                    <th>名称</th>
                    <th>品种</th>
                    <th>摆放位置</th>
                    <th>备注</th>
                    <th>录入时间</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody id="plantTable"></tbody>
        </table>
    </div>

    <!-- 养护事件记录 -->
    <div class="card">
        <div class="card-title">🧴 养护事件记录（浇水/施肥/修剪…）</div>
        <div class="search-bar">
            <select id="eventType">
                <option>浇水</option>
                <option>施肥</option>
                <option>修剪</option>
                <option>换盆</option>
                <option>病虫害处理</option>
                <option>其他</option>
            </select>
            <select id="eventPlant">
                <option value="">（通用，不指定绿植）</option>
            </select>
            <input id="eventNote" placeholder="备注，例如：浇透一次 / 施稀释液肥">
            <button class="btn-primary" onclick="addEvent()">📝 记录事件</button>
        </div>
        <table>
            <thead>
                <tr>
                    <th>时间</th>
                    <th>类型</th>
                    <th>关联绿植</th>
                    <th>备注</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody id="eventTable"></tbody>
        </table>
    </div>

    <!-- 历史台账 + 查询 + 导出备份恢复 -->
    <div class="card">
        <div class="card-title">📋 养护历史台账（查询 / 导出 / 备份 / 恢复）</div>
        <div class="search-bar">
            <input id="logSearch" placeholder="🔍 按时间或状态查询，例如：缺水 / 2026" oninput="renderLog()">
            <select id="logLevel" onchange="renderLog()">
                <option value="">全部状态</option>
                <option value="normal">正常</option>
                <option value="warn">预警（缺肥）</option>
                <option value="danger">异常（缺水/积水）</option>
            </select>
        </div>
        <table>
            <thead>
                <tr>
                    <th>时间</th>
                    <th>含水量%</th>
                    <th>氮</th>
                    <th>磷</th>
                    <th>钾</th>
                    <th>状态判断</th>
                </tr>
            </thead>
            <tbody id="logTable"></tbody>
        </table>
        <div class="btn-group">
            <button class="btn-secondary" onclick="exportCSV()">⬇️ 导出台账 CSV</button>
            <button class="btn-secondary" onclick="backupAll()">💾 备份全部数据 JSON</button>
            <button class="btn-warning" onclick="document.getElementById('restoreFile').click()">📂 恢复备份</button>
            <button class="btn-gray" onclick="window.print()">🖨 打印台账</button>
            <button class="btn-warning" onclick="clearLog()">🗑 清空台账</button>
        </div>
        <input type="file" id="restoreFile" accept="application/json,.json" style="display:none" onchange="restoreBackup(this)">
    </div>

    <!-- 趋势图 -->
    <div class="card">
        <div class="card-title">📈 近期数据趋势（最近12条采样）</div>
        <div id="chart"></div>
    </div>
</div>

<!-- 绿植添加/编辑弹窗 -->
<div class="mask" id="mask" onclick="if(event.target===this)closePopup()">
    <div class="popup">
        <h3 id="popupTitle">添加绿植</h3>
        <input id="f-name" placeholder="名称（必填）">
        <input id="f-species" placeholder="品种，例如：绿萝 / 多肉">
        <input id="f-location" placeholder="摆放位置，例如：客厅窗台">
        <input id="f-note" placeholder="备注（可选）">
        <div class="btn-group">
            <button class="btn-primary" onclick="savePlant()">💾 保存</button>
            <button class="btn-gray" onclick="closePopup()">取消</button>
        </div>
    </div>
</div>

<script>
/* ==================== 配置区（要改就改这里） ==================== */
// 自动更新间隔：调试演示 30 秒；正式上线改成 1 小时 = 60 * 60 * 1000
const AUTO_REFRESH_MS = 30 * 1000;
const MAX_LOG = 30;      // 台账最多保留条数
const MAX_EVENTS = 100;  // 养护事件最多保留条数

/* ==================== 数据底层：持久化封装 ====================
 * 所有 localStorage 读写都走 store，带 try/catch，数据坏掉也不白屏
 *
 * 数据结构设计（本地存储 JSON）：
 *   绿植 plants : {id, name, species, location, note, createdAt}
 *   台账 log    : {id, ts, time, water, n, p, k, status, level, advice}
 *   事件 events : {id, ts, time, type, plantId, plantName, note}
 *   当前读数 last: 同 log 单条（刷新页面直接显示，不重复生成）
 * ============================================================ */
const store = {
    get(key, fallback) {
        try {
            const raw = localStorage.getItem(key);
            if (raw == null) return fallback;
            const v = JSON.parse(raw);
            return v == null ? fallback : v;
        } catch (e) {
            return fallback;
        }
    },
    set(key, val) {
        try {
            localStorage.setItem(key, JSON.stringify(val));
            return true;
        } catch (e) {
            alert("本地存储写入失败：" + e.message);
            return false;
        }
    },
    del(key) {
        localStorage.removeItem(key);
    }
};

const DB_KEYS = {
    plants: "soilPlants",
    log: "soilLog",
    events: "soilEvents",
    last: "soilLast"
};

function uid() {
    return Date.now().toString(36) + Math.random().toString(36).slice(2, 7);
}

/* ==================== 读取内存数据 ==================== */
let plantList = store.get(DB_KEYS.plants, []);
let logList = store.get(DB_KEYS.log, []);
let eventList = store.get(DB_KEYS.events, []);

/* ==================== 模拟传感器 ==================== */
function getSensorData() {
    return {
        water: parseFloat((Math.random() * 60 + 10).toFixed(1)), // 10~70%
        n: Math.floor(Math.random() * 120 + 20),                 // 20~140
        p: Math.floor(Math.random() * 100 + 15),                 // 15~115
        k: Math.floor(Math.random() * 110 + 25),                 // 25~135
        time: new Date().toLocaleString()
    };
}

/* ==================== 判断 & 养护建议 ==================== */
function checkStatus(d) {
    const water = d.water;
    let text = "";
    let tags = [];
    let level = "normal"; // normal | warn | danger

    if (water < 25) {
        text = "⚠️ 土壤缺水！请及时给绿植浇水，一次浇透，不要少量多次洒水。";
        tags.push("缺水");
        level = "danger";
    } else if (water > 55) {
        text = "💧 水分偏多！停止浇水，保持通风，防止烂根。";
        tags.push("偏湿");
        level = "danger";
    } else {
        text = "✅ 水分适宜；";
    }

    if (d.n < 40) {
        text += " 氮肥不足，可施加少量腐熟有机肥；";
        tags.push("缺氮");
        if (level === "normal") level = "warn";
    }
    if (d.p < 30) {
        text += " 磷含量偏低，适当补磷肥促进开花；";
        tags.push("缺磷");
        if (level === "normal") level = "warn";
    }
    if (d.k < 40) {
        text += " 钾不足，补钾肥增强植株抗性；";
        tags.push("缺钾");
        if (level === "normal") level = "warn";
    }
    if (tags.length === 0) {
        text += " 土壤养分均衡，正常养护即可。";
        tags.push("正常");
    }

    return {
        text: text,
        short: tags.join("·"),
        level: level,
        adviceClass: level === "danger" ? "advice-danger" : (level === "warn" ? "advice-warn" : "advice-normal"),
        tagClass: level === "danger" ? "tag-d" : (level === "warn" ? "tag-w" : "tag-n")
    };
}

/* ==================== 传感器卡片渲染 ==================== */
function renderSensors(d) {
    document.getElementById("val-water").innerText = d.water;
    document.getElementById("val-n").innerText = d.n;
    document.getElementById("val-p").innerText = d.p;
    document.getElementById("val-k").innerText = d.k;
    if (d.time) document.getElementById("lastTime").innerText = d.time;

    // 进度条：水量按百分比直接显示；氮磷钾按 150 满刻度换算
    document.getElementById("p-water").style.width = Math.min(100, d.water) + "%";
    document.getElementById("p-n").style.width = Math.min(100, d.n / 150 * 100) + "%";
    document.getElementById("p-p").style.width = Math.min(100, d.p / 150 * 100) + "%";
    document.getElementById("p-k").style.width = Math.min(100, d.k / 150 * 100) + "%";

    const res = checkStatus(d);
    const box = document.getElementById("advice");
    box.className = "advice-box " + res.adviceClass;
    box.innerHTML = "<strong>💡 养护建议：</strong>" + res.text;
}

/* ==================== 台账：写入 + 查询渲染 ==================== */
function addLog(d, res) {
    const now = new Date();
    logList.unshift({
        id: uid(),
        ts: now.getTime(),
        time: d.time || now.toLocaleString(),
        water: d.water,
        n: d.n,
        p: d.p,
        k: d.k,
        status: res.short,
        level: res.level,
        advice: res.text
    });
    while (logList.length > MAX_LOG) logList.pop();
    store.set(DB_KEYS.log, logList);
    // 当前读数单独存一份，刷新页面时直接恢复显示
    store.set(DB_KEYS.last, logList[0]);
}

function renderLog() {
    const kw = (document.getElementById("logSearch").value || "").trim().toLowerCase();
    const lv = document.getElementById("logLevel").value;
    const filtered = logList.filter(item => {
        const hitKw = !kw ||
            (item.time || "").toLowerCase().includes(kw) ||
            (item.status || "").toLowerCase().includes(kw) ||
            (item.advice || "").toLowerCase().includes(kw);
        const hitLv = !lv || item.level === lv;
        return hitKw && hitLv;
    });

    const tb = document.getElementById("logTable");
    tb.innerHTML = "";
    if (filtered.length === 0) {
        tb.innerHTML = '<tr class="empty-row"><td colspan="6">' +
            (logList.length === 0 ? "暂无台账记录，等待第一次自动采样…" : "没有符合查询条件的记录") +
            "</td></tr>";
        return;
    }
    const tagCls = { normal: "tag-n", warn: "tag-w", danger: "tag-d" };
    filtered.forEach(item => {
        const tr = document.createElement("tr");
        tr.innerHTML =
            "<td>" + item.time + "</td>" +
            "<td>" + item.water + "</td>" +
            "<td>" + item.n + "</td>" +
            "<td>" + item.p + "</td>" +
            "<td>" + item.k + "</td>" +
            '<td><span class="tag ' + (tagCls[item.level] || "tag-n") + '" title="' + item.advice + '">' + item.status + "</span></td>";
        tb.appendChild(tr);
    });
}

/* ==================== 绿植信息 CRUD ==================== */
let editingPlantId = null;

function renderPlants() {
    const tb = document.getElementById("plantTable");
    tb.innerHTML = "";
    if (plantList.length === 0) {
        tb.innerHTML = '<tr class="empty-row"><td colspan="6">还没有绿植档案，点上方「添加绿植」录入</td></tr>';
    }
    plantList.forEach(p => {
        const tr = document.createElement("tr");
        tr.innerHTML =
            "<td>" + p.name + "</td>" +
            "<td>" + (p.species || "—") + "</td>" +
            "<td>" + (p.location || "—") + "</td>" +
            "<td>" + (p.note || "—") + "</td>" +
            "<td>" + (p.createdAt || "—") + "</td>" +
            '<td><button class="btn-secondary btn-mini" onclick="openPlantPopup(\'' + p.id + '\')">编辑</button> ' +
            '<button class="btn-warning btn-mini" onclick="deletePlant(\'' + p.id + '\')">删除</button></td>';
        tb.appendChild(tr);
    });
    renderPlantOptions();
}

function renderPlantOptions() {
    const sel = document.getElementById("eventPlant");
    const cur = sel.value;
    sel.innerHTML = '<option value="">（通用，不指定绿植）</option>';
    plantList.forEach(p => {
        const opt = document.createElement("option");
        opt.value = p.id;
        opt.innerText = p.name;
        sel.appendChild(opt);
    });
    sel.value = cur;
}

function openPlantPopup(id) {
    editingPlantId = id || null;
    const p = id ? plantList.find(x => x.id === id) : null;
    document.getElementById("popupTitle").innerText = p ? "编辑绿植" : "添加绿植";
    document.getElementById("f-name").value = p ? p.name : "";
    document.getElementById("f-species").value = p ? (p.species || "") : "";
    document.getElementById("f-location").value = p ? (p.location || "") : "";
    document.getElementById("f-note").value = p ? (p.note || "") : "";
    document.getElementById("mask").style.display = "block";
}

function closePopup() {
    document.getElementById("mask").style.display = "none";
}

function savePlant() {
    const name = document.getElementById("f-name").value.trim();
    if (!name) {
        alert("名称必填");
        return;
    }
    const data = {
        name: name,
        species: document.getElementById("f-species").value.trim(),
        location: document.getElementById("f-location").value.trim(),
        note: document.getElementById("f-note").value.trim()
    };
    if (editingPlantId) {
        const p = plantList.find(x => x.id === editingPlantId);
        if (p) Object.assign(p, data);
    } else {
        plantList.unshift(Object.assign({ id: uid(), createdAt: new Date().toLocaleString() }, data));
    }
    store.set(DB_KEYS.plants, plantList);
    closePopup();
    renderPlants();
}

function deletePlant(id) {
    const p = plantList.find(x => x.id === id);
    if (!p) return;
    if (!confirm("确定删除绿植「" + p.name + "」吗？")) return;
    plantList = plantList.filter(x => x.id !== id);
    store.set(DB_KEYS.plants, plantList);
    renderPlants();
}

/* ==================== 养护事件记录 ==================== */
function addEvent(type, plantId, note) {
    // 允许表单调用，也允许其他功能（如“采纳建议”）直接传参调用
    if (typeof type !== "string") {
        type = document.getElementById("eventType").value;
        plantId = document.getElementById("eventPlant").value;
        note = document.getElementById("eventNote").value.trim();
        document.getElementById("eventNote").value = "";
    }
    const now = new Date();
    const plant = plantList.find(x => x.id === plantId);
    eventList.unshift({
        id: uid(),
        ts: now.getTime(),
        time: now.toLocaleString(),
        type: type,
        plantId: plantId || "",
        plantName: plant ? plant.name : "通用",
        note: note || "—"
    });
    while (eventList.length > MAX_EVENTS) eventList.pop();
    store.set(DB_KEYS.events, eventList);
    renderEvents();
}

function deleteEvent(id) {
    eventList = eventList.filter(x => x.id !== id);
    store.set(DB_KEYS.events, eventList);
    renderEvents();
}

function renderEvents() {
    const tb = document.getElementById("eventTable");
    tb.innerHTML = "";
    if (eventList.length === 0) {
        tb.innerHTML = '<tr class="empty-row"><td colspan="5">暂无养护事件，浇水施肥后记一笔吧</td></tr>';
        return;
    }
    eventList.slice(0, 20).forEach(ev => {
        const tr = document.createElement("tr");
        tr.innerHTML =
            "<td>" + ev.time + "</td>" +
            "<td>" + ev.type + "</td>" +
            "<td>" + ev.plantName + "</td>" +
            "<td>" + ev.note + "</td>" +
            '<td><button class="btn-warning btn-mini" onclick="deleteEvent(\'' + ev.id + '\')">删除</button></td>';
        tb.appendChild(tr);
    });
}

// 把当前养护建议直接记成一条“其他”事件（类型=建议采纳）
function adoptAdvice() {
    const last = logList[0];
    if (!last) {
        alert("还没有采样数据");
        return;
    }
    addEvent("建议采纳", "", last.advice);
    alert("已把当前养护建议记入事件台账 ✅");
}

/* ==================== 导出 / 备份 / 恢复 ==================== */
function downloadBlob(content, filename, mime) {
    const blob = new Blob([content], { type: mime + ";charset=utf-8" });
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    setTimeout(function () {
        URL.revokeObjectURL(a.href);
        a.remove();
    }, 500);
}

// 导出 CSV（带 BOM，Excel 打开中文不乱码）
function exportCSV() {
    if (logList.length === 0) {
        alert("台账为空，没有可导出的记录");
        return;
    }
    const rows = [["时间", "含水量(%)", "氮N", "磷P", "钾K", "状态判断", "养护建议"]];
    logList.forEach(item => {
        rows.push([item.time, item.water, item.n, item.p, item.k, item.status, item.advice]);
    });
    const csv = "﻿" + rows.map(r =>
        r.map(cell => '"' + String(cell).replace(/"/g, '""') + '"').join(",")
    ).join("\n");
    downloadBlob(csv, "养护台账_" + Date.now() + ".csv", "text/csv");
}

// 备份：把三个数据表打成一个 JSON 文件下载
function backupAll() {
    const data = {
        app: "soil-ledger",
        version: 1,
        exportedAt: new Date().toLocaleString(),
        plants: plantList,
        log: logList,
        events: eventList
    };
    downloadBlob(JSON.stringify(data, null, 2), "绿植台账备份_" + Date.now() + ".json", "application/json");
}

// 恢复：读 JSON 文件 → 校验 → 覆盖写入 → 重渲染
function restoreBackup(input) {
    const file = input.files && input.files[0];
    input.value = ""; // 允许重复选同一个文件
    if (!file) return;
    const reader = new FileReader();
    reader.onload = function () {
        let data = null;
        try {
            data = JSON.parse(reader.result);
        } catch (e) {
            alert("文件不是有效的 JSON 备份");
            return;
        }
        if (!data || data.app !== "soil-ledger" || !Array.isArray(data.log)) {
            alert("备份文件格式不对（缺少 app/version/log 标识）");
            return;
        }
        if (!confirm("恢复将覆盖当前 " + plantList.length + " 条绿植、" + logList.length +
            " 条台账、" + eventList.length + " 条事件，确定继续吗？")) return;

        plantList = Array.isArray(data.plants) ? data.plants : [];
        logList = Array.isArray(data.log) ? data.log : [];
        eventList = Array.isArray(data.events) ? data.events : [];
        store.set(DB_KEYS.plants, plantList);
        store.set(DB_KEYS.log, logList);
        store.set(DB_KEYS.events, eventList);
        if (logList[0]) store.set(DB_KEYS.last, logList[0]);

        renderPlants();
        renderEvents();
        renderLog();
        renderChart();
        if (logList[0]) renderSensors(logList[0]);
        alert("恢复完成 ✅");
    };
    reader.readAsText(file, "utf-8");
}

/* ==================== 趋势图（ECharts，可离线降级） ==================== */
let chartInstance = null;

function renderChart() {
    const box = document.getElementById("chart");
    if (!window.echarts) {
        box.outerHTML = '<div id="chart" class="chart-fallback">📈 图表库加载失败（需要联网才能加载 ECharts CDN），不影响台账其他功能</div>';
        return;
    }
    if (!chartInstance) {
        chartInstance = echarts.init(box);
    }
    const rows = logList.slice(0, 12).slice().reverse(); // 最近12条，按时间正序
    chartInstance.setOption({
        tooltip: { trigger: "axis" },
        legend: { data: ["含水量", "氮N", "磷P", "钾K"], top: 0 },
        grid: { left: 40, right: 20, top: 36, bottom: 40 },
        xAxis: {
            type: "category",
            data: rows.map(x => (x.time || "").split(" ").pop() || x.time),
            axisLabel: { rotate: 30, fontSize: 10 }
        },
        yAxis: { type: "value" },
        series: [
            { name: "含水量", type: "line", smooth: true, data: rows.map(x => x.water), color: "#2b7a2f" },
            { name: "氮N", type: "line", smooth: true, data: rows.map(x => x.n), color: "#4caf50" },
            { name: "磷P", type: "line", smooth: true, data: rows.map(x => x.p), color: "#f57c00" },
            { name: "钾K", type: "line", smooth: true, data: rows.map(x => x.k), color: "#8d6e63" }
        ]
    });
}

/* ==================== 定时自动更新 ==================== */
let timerId = null;
let nextAt = 0;

function humanInterval(ms) {
    const s = Math.round(ms / 1000);
    if (s % 3600 === 0) return "每" + (s / 3600) + "小时";
    if (s % 60 === 0) return "每" + (s / 60) + "分钟";
    return "每" + s + "秒";
}

function tickCountdown() {
    const left = Math.max(0, Math.ceil((nextAt - Date.now()) / 1000));
    const m = Math.floor(left / 60);
    const s = left % 60;
    document.getElementById("nextUpdate").innerText =
        left <= 0 ? "即将刷新…" : (m > 0 ? m + "分" + s + "秒后" : s + "秒后");
}

function restartTimer() {
    if (timerId) clearInterval(timerId);
    timerId = setInterval(sampleNow, AUTO_REFRESH_MS);
    nextAt = Date.now() + AUTO_REFRESH_MS;
}

/* ==================== 主流程 ==================== */
function sampleNow() {
    const data = getSensorData();
    renderSensors(data);
    const res = checkStatus(data);
    addLog(data, res);
    renderLog();
    renderChart();
    restartTimer();
}

function clearLog() {
    if (!confirm("确定清空全部台账记录吗？（绿植档案和养护事件不受影响）")) return;
    logList = [];
    store.del(DB_KEYS.log);
    store.del(DB_KEYS.last);
    renderLog();
    renderChart();
}

/* ==================== 初始化 ==================== */
(function init() {
    document.getElementById("intervalText").innerText = humanInterval(AUTO_REFRESH_MS);

    renderPlants();
    renderEvents();

    // 打开页面：优先恢复上次读数（不重复记台账），没有才首次采样
    const last = store.get(DB_KEYS.last, null);
    if (last && last.water != null) {
        renderSensors(last);
        renderLog();
        renderChart();
        restartTimer();
    } else {
        sampleNow(); // 首次访问：采样并记第一条台账
    }

    setInterval(tickCountdown, 1000);
    tickCountdown();
})();
</script>
</body>
</html>
