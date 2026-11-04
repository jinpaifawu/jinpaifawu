// server.js
const express = require('express');
const app = express();

app.use(express.json());

app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    res.header('Access-Control-Allow-Methods', 'POST, OPTIONS');
    if (req.method === 'OPTIONS') return res.sendStatus(200);
    next();
});

app.post('/api/get-airscript-data', async (req, res) => {
    const targetUrl = 'https://www.kdocs.cn/api/v3/ide/file/473058537400/script/V2-6kKiBCwIM6s011VU7iCIYW/sync_task';
    const token = '"2plcKXpRR6m6HCX9cEfdlS"'; // 令牌藏在后端，不暴露给前端

    try {
        const response = await fetch(targetUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'AirScript-Token':"2plcKXpRR6m6HCX9cEfdlS"
            },
            body: JSON.stringify({ Context: { argv: {} } })
        });
        const data = await response.json();
        res.json(data); // 转发回前端
    } catch (error) {
        res.status(500).json({ error: '请求金山接口失败' });
    }
});

app.listen(3000, () => console.log('代理服务运行在 3000 端口',"http://localhost:3000"));
