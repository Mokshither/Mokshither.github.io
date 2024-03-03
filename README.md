<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    /* Styles remain unchanged */
  </style>
  <title>Will You Be Rich?</title>
</head>
<body>
  <h1 style="color: #ff6600;">Will You Be Rich></h1>
  <p style="color: #666666;">You start with $1,000,000 virtual money.</p>
  <label for="bidInput" style="color: #3366cc;">Enter your bidding amount:</label>
  <input type="number" id="bidInput" min="1" placeholder="Enter amount">
  <label for="guessInput" style="color: #3366cc;">Guess the number (1-10):</label>
  <input type="number" id="guessInput" min="1" max="10" placeholder="Enter guess">
  <div id="wheel" onclick="spinWheel()"></div>
  <p id="result" style="color: #cc3300;"></p>
  <p id="virtualMoney" style="color: #3366cc;"></p>
  <button onclick="spinWheel()" style="background-color: #ff6600;">Spin the Wheel</button>

  <script>
    let virtualMoney = parseInt(localStorage.getItem('virtualMoney')) || 1000000;
    let bankruptcyBonusGiven = localStorage.getItem('bankruptcyBonusGiven') === 'true' || false;

    function updateVirtualMoney() {
      document.getElementById('virtualMoney').innerHTML = `Virtual Money: $${virtualMoney}`;
    }

    function spinWheel() {
      const bidAmount = parseInt(document.getElementById('bidInput').value);
      const guessNumber = parseInt(document.getElementById('guessInput').value);
      const wheel = document.getElementById('wheel');
      const randomNumber = Math.floor(Math.random() * 10) + 1;

      if (bidAmount >= 1 && bidAmount <= virtualMoney && guessNumber >= 1 && guessNumber <= 10) {
        wheel.style.transform = `rotate(${360 * (randomNumber / 10)}deg)`;
        setTimeout(() => checkResult(randomNumber, bidAmount, guessNumber), 1000);
      } else {
        document.getElementById('result').innerHTML = `Please enter valid bidding amount and guess within the specified ranges.`;
      }
    }

    function checkResult(randomNumber, bidAmount, guessNumber) {
      if (randomNumber === guessNumber) {
        virtualMoney += bidAmount * 2;
        document.getElementById('result').innerHTML = `Congratulations! You won $${bidAmount * 2}.`;
      } else {
        virtualMoney -= bidAmount;
        document.getElementById('result').innerHTML = `Sorry, better luck next time. You lost $${bidAmount}. The correct number was ${randomNumber}.`;

        // Check for bankruptcy and give bonus if not already given
        if (virtualMoney === 0 && !bankruptcyBonusGiven) {
          virtualMoney += 10000;
          bankruptcyBonusGiven = true;
          localStorage.setItem('bankruptcyBonusGiven', true);
          document.getElementById('result').innerHTML += `<br>Bankruptcy bonus! You received $10,000.`;
        }
      }

      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }

    // Initial setup and periodic reward
    updateVirtualMoney();
    setInterval(() => {
      virtualMoney += 1000;
      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }, 3600000);
  </script>
</body>
</html>
