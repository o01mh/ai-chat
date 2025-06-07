<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>محادثة ذكاء اصطناعي</title>
  <script defer src="script.js"></script>
</head>
<body>
  <h2>اختر شخصية</h2>
  <select id="characterSelect"></select>

  <h3>أدخل رسالتك:</h3>
  <textarea id="userInput"></textarea><br>
  <button onclick="sendMessage()">إرسال</button>

  <h3>رد الذكاء الاصطناعي:</h3>
  <div id="responseArea"></div>
</body>
</html>
const characters = {
  scientist: "أنت عالم عبقري تشرح الأمور بذكاء ومنطق.",
  comedian: "أنت كوميدي مرح تحب المزاح أثناء الإجابة."
};

window.onload = () => {
  const select = document.getElementById("characterSelect");
  for (const key in characters) {
    const option = document.createElement("option");
    option.value = key;
    option.textContent = key;
    select.appendChild(option);
  }
};

async function sendMessage() {
  const character = document.getElementById("characterSelect").value;
  const userInput = document.getElementById("userInput").value;

  const prompt = characters[character] + "\n\nالمستخدم: " + userInput;

  const responseArea = document.getElementById("responseArea");
  responseArea.textContent = "جاري التفكير...";

  const res = await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Bearer YOUR_OPENAI_API_KEY"
    },
    body: JSON.stringify({
      model: "gpt-3.5-turbo",
      messages: [
        { role: "system", content: characters[character] },
        { role: "user", content: userInput }
      ]
    })
  });

  const data = await res.json();
  responseArea.textContent = data.choices?.[0]?.message?.content || "حدث خطأ.";
}
# AI Chat Multi-Character

واجهة بسيطة لمحادثة ذكاء اصطناعي بعدة شخصيات (مثل عالم أو كوميدي) باستخدام OpenAI GPT API.

## طريقة الاستخدام

- اختر شخصية من القائمة.
- اكتب رسالة في مربع النص.
- اضغط على "إرسال" لتلقي الرد.

> ملاحظة: تحتاج إلى إدخال مفتاح API الخاص بك في الملف `script.js` بدلًا من `YOUR_OPENAI_API_KEY`.

## الشخصيات المتاحة

- 👨‍🔬 العالم
- 🎭 الكوميدي
- 🦁رجل مافيا
