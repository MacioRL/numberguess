<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Guess the number!</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f3f3f3;
            max-width: 400px;
            margin: 40px auto;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 12px rgba(0,0,0,0.12);
        }
        input[type="number"] {
            width: 80px;
            font-size: 1.2em;
        }
        button {
            font-size: 1em;
            margin-left: 10px;
        }
        #result {
            margin-top: 15px;
            font-weight: bold;
        }
        #reset {
            margin-top: 20px;
            background: #f44336;
            color: white;
            border: none;
            padding: 7px 15px;
            border-radius: 4px;
            cursor: pointer;
        }
        #reset:hover {
            background: #c62828;
        }
    </style>
</head>
<body>
    <h2>Guess the number!</h2>
    <p>
       The computer has randomly selected a location from <span id="min">1</span> to <span id="max">100</span>. Go guess it!
    </p>
    <input type="number" id="guess" min="1" max="100" autofocus>
    <button onclick="checkGuess()">I'm guessing!</button>
    <div id="result"></div>
    <button id="reset" onclick="resetGame()">New game</button>
    <script>
        // Parametry gry
        const min = 1;
        const max = 100;
        let secretNumber, attempts;

        function losujLiczbe() {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }

        function resetGame() {
            secretNumber = losujLiczbe();
            attempts = 0;
            document.getElementById('result').textContent = '';
            document.getElementById('guess').value = '';
            document.getElementById('guess').disabled = false;
            document.querySelector('button[onclick="checkGuess()"]').disabled = false;
            document.getElementById('guess').focus();
        }

        function checkGuess() {
            const guessInput = document.getElementById('guess');
            const guess = Number(guessInput.value);
            if (isNaN(guess) || guess < min || guess > max) {
                document.getElementById('result').textContent = `Podaj liczbę od ${min} do ${max}!`;
                return;
            }
            attempts++;
            if (guess < secretNumber) {
                document.getElementById('result').textContent = "Too little!";
            } else if (guess > secretNumber) {
                document.getElementById('result').textContent = "Too much!!";
            } else {
                document.getElementById('result').textContent = `Well done! You guessed ${secretNumber} for ${attempts} total!`;
                guessInput.disabled = true;
                document.querySelector('button[onclick="checkGuess()"]').disabled = true;
            }
            guessInput.value = '';
            guessInput.focus();
        }

        // Ustaw min i max w UI
        document.getElementById('min').textContent = min;
        document.getElementById('max').textContent = max;

        // Start gry
        resetGame();

        // Enter = zgaduję
        document.getElementById('guess').addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                checkGuess();
            }
        });
    </script>
</body>
</html>
