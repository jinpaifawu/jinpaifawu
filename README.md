<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AirScript 数据读取（代理版）</title>
    <style>
        body { font-family: system-ui, -apple-system, sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; line-height: 1.6; }
        button { padding: 10px 20px; font-size: 16px; cursor: pointer; background-color: #007bff; color: white; border: none; border-radius: 4px; }
        button:disabled { background-color: #ccc; cursor: not-allowed; }
        #status { margin: 15px 0; color: #666; }
        #result { background: #f5f5f5; padding: 15px; border-radius: 4px; border: 1px solid #ddd; white-space: pre-wrap; word-break: break-all; min-height: 100px; font-family: monospace; font-size: 14px; }
        .error { color: #d9534f; font-weight: bold; }
    </style>
</head>
<body>

    <h2>AirScript 数据读取演示（本地代理版）</h2>
    <p>点击按钮，前端请求本地后端，后端再请求金山接口。</p>
    <!-- 有数据 但不要的 -->
    <button id="fetchBtn">读取数据</button>
    <div id="status"></div>
    <pre id="result">等待操作...</pre>

    <script>
        const btn = document.getElementById('fetchBtn');
        const statusDiv = document.getElementById('status');
        const resultPre = document.getElementById('result');

        btn.addEventListener('click', async () => {
            btn.disabled = true;
            statusDiv.textContent = '请求中，请稍候...';
            resultPre.textContent = '';
            resultPre.classList.remove('error');

            try {
                // 请求本地后端代理，而不是直接请求金山接口
                const response = await fetch('http://localhost:3000/api/get-airscript-data', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({}) // 如需传参，在这里放参数
                });

                if (!response.ok) {
                    throw new Error(`HTTP 错误! 状态码: ${response.status}`);
                }

                const data = await response.json();
                statusDiv.textContent = '请求成功';
                // 展示后端转发回来的完整数据
                resultPre.textContent = JSON.stringify(data, null, 2);

            } catch (error) {
                console.error('调用失败:', error);
                statusDiv.innerHTML = `<span class="error">调用失败</span>`;
                resultPre.textContent = error.message;
                resultPre.classList.add('error');
            } finally {
                btn.disabled = false;
            }
        });
    </script>
</body>
</html>
