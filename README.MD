
# Simple WebSocket Chat App

Ini adalah eksperimen sederhana untuk membuktikan efektivitas **WebSocket** dalam komunikasi **real-time**, seperti pada aplikasi chat.

Aplikasi ini dibuat menggunakan:
- **Node.js** sebagai WebSocket server
- **HTML + JavaScript** sebagai client

Pesan yang dikirim dari satu client akan **dibroadcast ke semua client yang terhubung secara real-time**, tanpa perlu refresh halaman.

## 📂 Struktur Project

```
/websocket
├── index.htm      # Halaman client (HTML + JavaScript)
├── server.js       # Kode server WebSocket (Node.js)
├── package.json    # (otomatis dibuat setelah npm init)
```

---

## 🚀 Cara Menjalankan

### 1. Install Node.js
Download dan install Node.js dari [https://nodejs.org/](https://nodejs.org/)  
Pastikan Node.js sudah terinstall dengan mengetik perintah di terminal/cmd:

```bash
node -v
npm -v
```

### 2. Inisialisasi Project
Jalankan perintah berikut di folder project:

```bash
npm init -y
```

### 3. Install WebSocket Module
Install modul WebSocket:

```bash
npm install ws
```

---

## 📝 Kode Server (server.js)

Buat file `server.js` dengan isi berikut:

```javascript
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', ws => {
  ws.on('message', message => {
    wss.clients.forEach(client => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });
})
```

---

## 📝 Kode Client (index.htm)

Buat file `index.htm` dengan isi berikut:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Simple WebSocket Chat</title>
</head>
<body>
  <input id="msg" type="text">
  <button onclick="sendMsg()">Send</button>
  <ul id="chat"></ul>

  <script>
    const socket = new WebSocket('ws://localhost:8080');

    socket.onmessage = event => {
      const reader = new FileReader();
      reader.onload = () => {
        const li = document.createElement('li');
        li.textContent = reader.result;
        document.getElementById('chat').appendChild(li);
      };
      reader.readAsText(event.data);
    };

    function sendMsg() {
      const input = document.getElementById('msg');
      socket.send(input.value);
      input.value = '';
    }
  </script>
</body>
</html>
```

---

## ▶️ Cara Menjalankan Server

Jalankan server Node.js:

```bash
node server.js
```

Server akan berjalan di `ws://localhost:8080`

---

## ▶️ Cara Menjalankan Client

1. Buka file `index.htm` di browser (cukup klik 2x atau drag ke browser)
2. Bisa buka **2 tab atau 2 browser berbeda** dengan file HTML yang sama
3. Coba kirim pesan → pesan akan muncul **di semua tab secara real-time**

![alt text](asset/videos/experimen.mp4)


## 💡 Hasil Eksperimen

✅ Pesan yang dikirim dari satu client akan diterima oleh semua client lain secara **real-time tanpa refresh**  
✅ Waktu jeda penerimaan pesan **sangat kecil (<100ms)**  
✅ WebSocket memungkinkan komunikasi dua arah yang **cepat dan efisien** tanpa polling

---

## 📚 Sumber Inspirasi

Artikel: *Bagaimana WebSocket Mengubah Dunia Aplikasi Chat dan Game Online*

---

🎉 Selamat mencoba dan semoga bermanfaat!
