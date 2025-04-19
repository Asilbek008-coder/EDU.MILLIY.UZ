# EDU.MILLIY.UZ
/project-root
  ├── public/
  │     ├── index.html      # Bosh sahifa
  │     ├── style.css       # Dizayn
  │     ├── script.js       # Interaktivlik uchun JS
  ├── server.js             # Backend logikasi
  ├── package.json          # Node.js konfiguratsiyasi
  <!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EDU.MILLIY.UZ</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>EDU.MILLIY.UZ</h1>
        <nav>
            <ul>
                <li><a href="#home">Bosh Sahifa</a></li>
                <li><a href="#about">Biz Haqimizda</a></li>
                <li><a href="#contact">Aloqa</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section id="home">
            <h2>Ta'lim platformasiga xush kelibsiz!</h2>
            <p>O'qish va o'rganish uchun eng yaxshi joy!</p>
        </section>
        <section id="about">
            <h2>Biz Haqimizda</h2>
            <p>EDU.MILLIY.UZ — bu o‘quvchilar va o‘qituvchilar uchun yaratilgan ta’lim platformasidir. Bu yerda siz turli darslar va testlardan foydalanishingiz mumkin.</p>
        </section>
        <section id="contact">
            <h2>Aloqa</h2>
            <form id="contactForm">
                <label for="name">Ismingiz:</label>
                <input type="text" id="name" name="name" required>
                
                <label for="email">Emailingiz:</label>
                <input type="email" id="email" name="email" required>
                
                <label for="message">Xabaringiz:</label>
                <textarea id="message" name="message" required></textarea>
                
                <button type="submit">Jo'natish</button>
            </form>
        </section>
    </main>
    <footer>
        <p>&copy; 2025 EDU.MILLIY.UZ. Barcha huquqlar himoyalangan.</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
/* Foydalanuvchi ro'yxatdan o'tishi va tizimga kirishi uchun asosiy dizayn */
form {
    max-width: 400px;
    margin: 0 auto;
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

form label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: bold;
}

form input {
    width: 100%;
    padding: 0.5rem;
    margin-bottom: 1rem;
    border: 1px solid #ddd;
    border-radius: 4px;
}

form button {
    width: 100%;
    padding: 0.5rem;
    background-color: #0077cc;
    color: white;
    font-size: 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

form button:hover {
    background-color: #005fa3;
}
document.getElementById("contactForm").addEventListener("submit", async function (event) {
    event.preventDefault();

    const name = document.getElementById("name").value;
    const email = document.getElementById("email").value;
    const message = document.getElementById("message").value;

    try {
        const response = await fetch('/send-message', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ name, email, message })
        });

        if (response.ok) {
            alert("Xabaringiz jo'natildi. Rahmat!");
            this.reset();
        } else {
            alert("Xabarni jo'natishda xatolik yuz berdi!");
        }
    } catch (error) {
        console.error("Error:", error);
        alert("Server bilan bog'lanishda muammo yuz berdi.");
    }
});
const express = require('express');
const bodyParser = require('body-parser');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = 3000;

// Statik fayllarni xizmat qilish
app.use(express.static(path.join(__dirname, 'public')));
app.use(bodyParser.json());

// Aloqa formasi uchun endpoint
app.post('/contact', (req, res) => {
    const { name, email, message } = req.body;

    if (!name || !email || !message) {
        return res.status(400).send("Barcha maydonlarni to'ldiring!");
    }

    const newMessage = { name, email, message, date: new Date().toISOString() };

    // Ma'lumotlarni faylga yozish
    const filePath = path.join(__dirname, 'data', 'messages.json');
    fs.readFile(filePath, (err, data) => {
        const messages = err ? [] : JSON.parse(data);
        messages.push(newMessage);

        fs.writeFile(filePath, JSON.stringify(messages, null, 2), (err) => {
            if (err) {
                return res.status(500).send("Xabarni saqlashda xatolik yuz berdi.");
            }
            res.status(200).send("Xabar muvaffaqiyatli jo'natildi!");
        });
    });
});

// Serverni ishga tushirish
app.listen(PORT, () => {
    console.log(`Server http://localhost:${PORT} manzilida ishlamoqda`);
});
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tizimga kirish | EDU.MILLIY.UZ</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>EDU.MILLIY.UZ</h1>
        <nav>
            <a href="index.html">Bosh Sahifa</a>
            <a href="auth.html">Tizimga Kirish</a>
        </nav>
    </header>
    <main>
        <section>
            <h2>Tizimga Kirish</h2>
            <form id="loginForm">
                <label for="email">Email:</label>
                <input type="email" id="email" name="email" required>
                
                <label for="password">Parol:</label>
                <input type="password" id="password" name="password" required>
                
                <button type="submit">Kirish</button>
            </form>
        </section>
        <section>
            <h2>Ro'yxatdan O'tish</h2>
            <form id="registerForm">
                <label for="name">Ism:</label>
                <input type="text" id="name" name="name" required>
                
                <label for="email">Email:</label>
                <input type="email" id="regEmail" name="email" required>
                
                <label for="password">Parol:</label>
                <input type="password" id="regPassword" name="password" required>
                
                <button type="submit">Ro'yxatdan O'tish</button>
            </form>
        </section>
    </main>
    <footer>
        <p>&copy; 2025 EDU.MILLIY.UZ</p>
    </footer>
    <script src="auth.js"></script>
</body>
</html>
document.getElementById("registerForm").addEventListener("submit", async function (event) {
    event.preventDefault();

    const name = document.getElementById("name").value;
    const email = document.getElementById("regEmail").value;
    const password = document.getElementById("regPassword").value;

    try {
        const response = await fetch('/register', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ name, email, password })
        });

        if (response.ok) {
            alert("Ro'yxatdan o'tish muvaffaqiyatli!");
            this.reset();
        } else {
            alert("Xatolik yuz berdi!");
        }
    } catch (error) {
        console.error("Xatolik:", error);
        alert("Server bilan bog'lanishda muammo yuz berdi.");
    }
});

document.getElementById("loginForm").addEventListener("submit", async function (event) {
    event.preventDefault();

    const email = document.getElementById("email").value;
    const password = document.getElementById("password").value;

    try {
        const response = await fetch('/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password })
        });

        if (response.ok) {
            alert("Tizimga muvaffaqiyatli kirdingiz!");
        } else {
            alert("Email yoki parol noto'g'ri!");
        }
    } catch (error) {
        console.error("Xatolik:", error);
        alert("Server bilan bog'lanishda muammo yuz berdi.");
    }
});
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EDU.MILLIY.UZ</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>EDU.MILLIY.UZ</h1>
        <nav>
            <ul>
                <li><a href="#home">Bosh Sahifa</a></li>
                <li><a href="#about">Biz Haqimizda</a></li>
                <li><a href="#contact">Aloqa</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <section id="home">
            <h2>Ta'lim platformasiga xush kelibsiz!</h2>
            <p>O'qish va o'rganish uchun eng yaxshi joy!</p>
        </section>
        <section id="about">
            <h2>Biz Haqimizda</h2>
            <p>EDU.MILLIY.UZ — bu o‘quvchilar va o‘qituvchilar uchun yaratilgan ta’lim platformasidir. Bu yerda siz turli darslar va testlardan foydalanishingiz mumkin.</p>
        </section>
        <section id="contact">
            <h2>Aloqa</h2>
            <form id="contactForm">
                <label for="name">Ismingiz:</label>
                <input type="text" id="name" name="name" required>
                
                <label for="email">Emailingiz:</label>
                <input type="email" id="email" name="email" required>
                
                <label for="message">Xabaringiz:</label>
                <textarea id="message" name="message" required></textarea>
                
                <button type="submit">Jo'natish</button>
            </form>
        </section>
    </main>
    <footer>
        <p>&copy; 2025 EDU.MILLIY.UZ. Barcha huquqlar himoyalangan.</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
/* Asosiy dizayn */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background-color: #f9f9f9;
    color: #333;
}

header {
    background-color: #0077cc;
    color: white;
    text-align: center;
    padding: 1rem;
}

header nav ul {
    list-style: none;
    padding: 0;
    display: flex;
    justify-content: center;
    gap: 1rem;
}

header nav ul li {
    display: inline;
}

header nav ul li a {
    color: white;
    text-decoration: none;
    font-weight: bold;
}

main {
    padding: 2rem;
    text-align: center;
}

section {
    margin-bottom: 2rem;
}

form {
    max-width: 400px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

form input, form textarea {
    padding: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 4px;
}

form button {
    background-color: #0077cc;
    color: white;
    padding: 0.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

form button:hover {
    background-color: #005fa3;
}

footer {
    background-color: #333;
    color: white;
    text-align: center;
    padding: 1rem;
    margin-top: 2rem;
}
document.getElementById("contactForm").addEventListener("submit", async function (event) {
    event.preventDefault();

    const name = document.getElementById("name").value;
    const email = document.getElementById("email").value;
    const message = document.getElementById("message").value;

    try {
        const response = await fetch('/contact', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ name, email, message }),
        });

        if (response.ok) {
            alert("Xabaringiz muvaffaqiyatli jo'natildi!");
            this.reset();
        } else {
            alert("Xabarni jo'natishda xatolik yuz berdi!");
        }
    } catch (error) {
        console.error("Xatolik:", error);
        alert("Server bilan bog'lanishda muammo yuz berdi.");
    }
});
