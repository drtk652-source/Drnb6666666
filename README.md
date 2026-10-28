<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="robots" content="noindex,nofollow">
  <title>Dr</title>
  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      background: #0a0a12;
      color: #e0e0ff;
      font: 16px system-ui, sans-serif;
    }
    main { text-align: center; padding: 24px; }
    button {
      padding: 12px 24px;
      border: 1px solid #00f5ff;
      border-radius: 12px;
      background: #101824;
      color: #00f5ff;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <main>
    <h2>Dr</h2>
    <p id="status">正在连接云端源码……</p>
    <button id="retry" hidden onclick="loadDr()">重试</button>
  </main>
  <script>
    const SOURCE_URL =
      "https://pcxtyauoxrkrvqlpqgir.supabase.co/functions/v1/smart-worker/source";

    async function loadDr() {
      const status = document.getElementById("status");
      const retry = document.getElementById("retry");
      retry.hidden = true;
      status.textContent = "正在连接云端源码……";

      try {
        const response = await fetch(SOURCE_URL, {
          cache: "no-store",
          redirect: "follow"
        });
        if (!response.ok) throw new Error("HTTP " + response.status);

        const html = await response.text();
        if (!html.trim()) throw new Error("云端源码为空");

        const doc = new DOMParser().parseFromString(html, "text/html");
        if (!doc.documentElement || !doc.body) {
          throw new Error("云端源码格式无效");
        }

        document.open();
        document.write(html);
        document.close();
      } catch (e) {
        status.textContent = "云端加载失败，请检查网络后重试。";
        retry.hidden = false;
      }
    }

    loadDr();
  </script>
</body>
</html>
