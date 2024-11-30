from flask import Flask, render_template_string

app = Flask(__name__)

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hacker Simulator - Mourice Lennox</title>
    <style>
        body {
            background-color: black;
            color: #00ff00;
            font-family: 'Courier New', Courier, monospace;
            overflow: hidden;
        }

        h1, h2 {
            text-align: center;
        }

        .matrix {
            position: fixed;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        #matrix {
            position: absolute;
            width: 100%;
            height: 100%;
            background-color: black;
        }

        .content {
            position: relative;
            top: 20%;
            padding: 20px;
            text-align: center;
        }

        .rolling-text p {
            animation: roll 10s infinite linear;
            color: #0f0;
        }

        @keyframes roll {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }

        .table {
            width: 80%;
            margin: auto;
            border-collapse: collapse;
        }

        .table, .table th, .table td {
            border: 1px solid #00ff00;
            padding: 10px;
        }

        .glow {
            animation: glow 1s infinite alternate;
        }

        @keyframes glow {
            from { text-shadow: 0 0 5px #0f0; }
            to { text-shadow: 0 0 20px #0f0; }
        }
    </style>
</head>
<body>
    <div class="matrix">
        <canvas id="matrix"></canvas>
    </div>
    <div class="content">
        <h1 id="typing-text"></h1>
        <div class="rolling-text">
            <p>Mourice Lennox - Coding Enthusiast, MTL Tech Manager</p>
        </div>
        <h2>Daily Routine</h2>
        <table class="table">
            <tr><th>Time</th><th>Activity</th></tr>
            <tr><td>6:00 AM</td><td>Code and Research AI</td></tr>
            <tr><td>10:00 AM</td><td>Work on MTL Projects</td></tr>
            <tr><td>2:00 PM</td><td>Web Development and Security</td></tr>
            <tr><td>6:00 PM</td><td>AI Circuit Design</td></tr>
        </table>
        <h2>High-Tech Projects</h2>
        <ul>
            <li>MTL Tech Image Generator</li>
            <li>Home Design AI Assistant</li>
            <li>AI Circuit Designer</li>
            <li>Safaricom Airtime Algorithm</li>
        </ul>
    </div>
    <script>
        // Typing animation for introduction text
        const text = "Welcome to the Hacker Simulator: Mourice Lennox - MTL Tech Manager";
        let index = 0;
        function typeText() {
            if (index < text.length) {
                document.getElementById('typing-text').innerHTML += text.charAt(index);
                index++;
                setTimeout(typeText, 100);
            }
        }
        typeText();

        // Matrix Rain Effect
        const canvas = document.getElementById('matrix');
        const context = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const matrixChars = "0123456789ABCDEF".split("");
        const font_size = 16;
        const columns = canvas.width / font_size;
        const drops = Array(Math.floor(columns)).fill(1);

        function drawMatrix() {
            context.fillStyle = "rgba(0, 0, 0, 0.05)";
            context.fillRect(0, 0, canvas.width, canvas.height);

            context.fillStyle = "#0F0";
            context.font = font_size + "px monospace";

            for (let i = 0; i < drops.length; i++) {
                const text = matrixChars[Math.floor(Math.random() * matrixChars.length)];
                context.fillText(text, i * font_size, drops[i] * font_size);

                if (drops[i] * font_size > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }
        setInterval(drawMatrix, 33);
    </script>
</body>
</html>
"""

@app.route('/')
def home():
    return render_template_string(HTML_TEMPLATE)

if __name__ == '__main__':
    app.run(debug=True)
