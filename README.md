<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>CoinzBay - تسجيل الدخول</title>
  <style>
    body {
      font-family: 'Tahoma', sans-serif;
      background: linear-gradient(135deg, #4facfe, #00f2fe);
      text-align: center;
      padding-top: 40px;
      color: #fff;
    }
    input, button {
      padding: 12px;
      margin: 8px;
      width: 260px;
      border-radius: 8px;
      border: none;
      outline: none;
    }
    button {
      background: #ff9800;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover {
      background: #e68900;
    }
    #log {
      background: rgba(255,255,255,0.9);
      color: #000;
      padding: 15px;
      margin-top: 20px;
      width: 340px;
      border-radius: 10px;
      margin-left: auto;
      margin-right: auto;
      text-align: left;
    }
  </style>
</head>
<body>

  <h2>🚀 CoinzBay - تسجيل الدخول</h2>
  <img src="https://via.placeholder.com/600x150?text=إعلان+تجريبي" alt="إعلان تجريبي"><br><br>

  <input type="text" id="name" placeholder="اكتب الاسم"><br>
  <input type="email" id="email" placeholder="اكتب الإيميل"><br>
  <input type="password" id="password" placeholder="كلمة السر"><br>
  <button onclick="login()">دخول</button>

  <div id="message"></div>

  <div id="log">
    <h4>سجل الدخول:</h4>
    <ul id="logList"></ul>
  </div>

  <!-- مكتبة EmailJS -->
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2/dist/email.min.js"></script>
  <script>
    (function(){
      emailjs.init("YOUR_PUBLIC_KEY"); // ضع المفتاح هنا
    })();

    function login() {
      let name = document.getElementById("name").value;
      let email = document.getElementById("email").value;
      let password = document.getElementById("password").value;

      if (name === "" || email === "" || password === "") {
        document.getElementById("message").innerText = "❌ عَمّر جميع الخانات";
        return;
      }

      let time = new Date().toLocaleString();

      // حفظ محلي
      let logins = JSON.parse(localStorage.getItem("logins")) || [];
      logins.push({ name: name, email: email, time: time });
      localStorage.setItem("logins", JSON.stringify(logins));

      // إرسال بريد عبر EmailJS
      emailjs.send("YOUR_SERVICE_ID","YOUR_TEMPLATE_ID",{
        user_name: name,
        user_email: email,
        login_time: time
      }).then(function(response) {
        document.getElementById("message").innerText = "✅ تم تسجيل الدخول وإرسال إشعار لبريدك";
      }, function(error) {
        document.getElementById("message").innerText = "⚠️ خطأ في إرسال البريد";
      });

      showLog();
    }

    function showLog() {
      let logins = JSON.parse(localStorage.getItem("logins")) || [];
      let list = document.getElementById("logList");
      list.innerHTML = "";

      logins.forEach(item => {
        let li = document.createElement("li");
        li.textContent =
          "👤 " + item.name +
          " | 📧 " + item.email +
          " | 🕒 " + item.time;
        list.appendChild(li);
      });
    }

    showLog();
  </script>

</body>
</html>
