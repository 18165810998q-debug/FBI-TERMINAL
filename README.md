# FBI-TERMINAL<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>FBI - RESTRICTED ACCESS</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/echarts@4.9.0/map/js/china.js"></script>
    <style>
        body { background-color: #000; color: #00ff00; font-family: 'Courier New', monospace; margin: 0; overflow: hidden; }
        #login-screen, #main-screen { height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; }
        .hidden { display: none !important; }
        .fbi-header { position: absolute; top: 20px; left: 40px; display: flex; align-items: center; }
        .fbi-logo { width: 60px; height: 60px; margin-right: 20px; }
        .terminal-box { border: 1px solid #00ff00; padding: 30px; background: rgba(0,20,0,0.8); box-shadow: 0 0 20px #004400; width: 350px; text-align: center; }
        input { background: #000; border: 1px solid #008800; color: #00ff00; padding: 10px; margin: 10px 0; width: 80%; text-align: center; outline: none; }
        button { background: #004400; color: #00ff00; border: 1px solid #00ff00; padding: 10px 20px; cursor: pointer; width: 85%; letter-spacing: 2px; }
        button:hover { background: #00ff00; color: #000; }
        
        /* 地图容器 */
        #map-container { width: 800px; height: 500px; margin-top: 20px; border: 1px solid #004400; }
        .info-panel { position: absolute; right: 40px; top: 100px; width: 300px; font-size: 14px; line-height: 1.6; background: rgba(0,10,0,0.9); padding: 15px; border-left: 3px solid #00ff00; }
        .blink { animation: blink 1s infinite; }
        @keyframes blink { 50% { opacity: 0; } }
    </style>
</head>
<body>

<div id="login-screen">
    <div class="fbi-header">
        < img src="https://upload.wikimedia.org/wikipedia/commons/d/d1/Seal_of_the_Federal_Bureau_of_Investigation.svg" class="fbi-logo">
        <div>
            <div style="font-size: 20px; font-weight: bold;">FEDERAL BUREAU OF INVESTIGATION</div>
            <div style="font-size: 12px; letter-spacing: 4px;">SECURE LOGIN TERMINAL</div>
        </div>
    </div>
    <div class="terminal-box">
        <div style="color: #ff0000; margin-bottom: 15px;">[ SYSTEM RESTRICTED ]</div>
        <input type="text" id="user" placeholder="AGENT_ID">
        <input type="password" id="pass" placeholder="ACCESS_CODE">
        <button onclick="login()">AUTHENTICATE</button>
        <p id="err" style="color: red; font-size: 12px;"></p >
    </div>
</div>

<div id="main-screen" class="hidden">
    <div class="fbi-header">
        < img src="https://upload.wikimedia.org/wikipedia/commons/d/d1/Seal_of_the_Federal_Bureau_of_Investigation.svg" class="fbi-logo">
        <h2 style="text-shadow: 0 0 10px #00ff00;">INTELLIGENCE DASHBOARD</h2>
    </div>

    <div id="map-container"></div>

    <div class="info-panel" id="intel">
        <div style="color: #00ff00; border-bottom: 1px solid #00ff00; padding-bottom: 5px;">> DECRYPTING DATA...</div>
        <p>> TARGET: CHENG_OFFICE</p >
        <p>> 1000M BEST: 3'35"</p >
        <p>> VPS STATUS: <span style="color: gold;">AWAITING MAY DEPLOYMENT</span></p >
        <p>> NODE: SAN JOSE (CA, US)</p >
        <p>> LATENCY: 152MS (OPTIMAL)</p >
        <p id="ip-display">> REMOTE_IP: LOADING...</p >
        <div class="blink">_</div>
    </div>
</div>

<script>
    // 预设账号密码
    const MY_ID = "CHENG_0416";
    const MY_PASS = "335";

    function login() {
        const u = document.getElementById('user').value;
        const p = document.getElementById('pass').value;
        if(u === MY_ID && p === MY_PASS) {
            document.getElementById('login-screen').classList.add('hidden');
            document.getElementById('main-screen').classList.remove('hidden');
            initMap();
            fetchIP();
        } else {
            document.getElementById('err').innerText = "ACCESS DENIED. UNAUTHORIZED ATTEMPT LOGGED.";
        }
    }

    function fetchIP() {
        fetch('https://api.ipify.org?format=json')
            .then(res => res.json())
            .then(data => {
                document.getElementById('ip-display').innerText = "> REMOTE_IP: " + data.ip;
            });
    }

    function initMap() {
        var myChart = echarts.init(document.getElementById('map-container'));
        var option = {
            backgroundColor: 'transparent',
            title: { text: 'GLOBAL LATENCY MONITORING', left: 'center', textStyle: { color: '#00ff00', fontSize: 16 } },
            geo: {
                map: 'china',
                roam: false,
                emphasis: { itemStyle: { areaColor: '#002200' } },
                itemStyle: { areaColor: '#001100', borderColor: '#00ff00' }
            },
            series: [{
                type: 'lines',
                effect: { show: true, period: 4, trailLength: 0.5, color: '#00ff00', symbolSize: 3 },
                lineStyle: { color: '#00ff00', width: 1, opacity: 0.2, curveness: 0.3 },
                data: [
                    // 模拟从圣何塞(右侧)发射到中国各地的信号
                    { fromName: 'SJ', toName: 'GZ', coords: [[121, 23], [113, 23]] },
                    { fromName: 'SJ', toName: 'BJ', coords: [[121, 23], [116, 40]] },
                    { fromName: 'SJ', toName: 'SH', coords: [[121, 23], [121, 31]] }
                ]
            }]
        };
        myChart.setOption(option);
    }
</script>

</body>
</html>
