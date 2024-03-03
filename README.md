<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    /* Styles remain unchanged */
  </style>
  <title>Colorful Wheel of Fortune</title>
</head>
<body>
  <h1 style="color: #ff6600;">Welcome to the Colorful Wheel of Fortune Game!</h1>
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
        setTimeout(() => checkResult(randomNumber, bidAmount, guessNumber), 1000); // Adjust the duration as needed
      } else {
        document.getElementById('result').innerHTML = `Please enter valid bidding amount and guess within the specified ranges.`;
      }
    }

    function checkResult(randomNumber, bidAmount, guessNumber) {
      if (randomNumber === guessNumber) {
        virtualMoney += bidAmount * 2; // If the wheel stops at the guessed number, double the bidding amount.
        document.getElementById('result').innerHTML = `Congratulations! You won $${bidAmount * 2}.`;
      } else {
        virtualMoney -= bidAmount;
        document.getElementById('result').innerHTML = `Sorry, better luck next time. You lost $${bidAmount}. The correct number was ${randomNumber}.`;

        // Check for bankruptcy and spin the reward wheel
        if (virtualMoney <= 0) {
          spinRewardWheel();
        }
      }

      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }

    function spinRewardWheel() {
      const wheel = document.getElementById('wheel');
      const randomNumber = Math.floor(Math.random() * 4) + 1;

      // Adjust the wheel rotation for each reward outcome
      let rotation;
      switch (randomNumber) {
        case 1:
          rotation = 'rotate(0deg)';
          virtualMoney += 1000;
          document.getElementById('result').innerHTML += `<br>Better Luck Next Time! You received $1,000.`;
          break;
        case 2:
          rotation = 'rotate(90deg)';
          virtualMoney += 10000;
          document.getElementById('result').innerHTML += `<br>Congratulations! You received $10,000.`;
          break;
        case 3:
          rotation = 'rotate(180deg)';
          virtualMoney += 50000;
          document.getElementById('result').innerHTML += `<br>Congratulations! You received $50,000.`;
          break;
        case 4:
          rotation = 'rotate(270deg)';
          virtualMoney += 25000;
          document.getElementById('result').innerHTML += `<br>Congratulations! You received $25,000.`;
          break;
        default:
          rotation = 'rotate(0deg)';
      }

      // Display the result and update virtual money
      wheel.style.transform = rotation;
      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }

    // Initial setup and periodic reward
    updateVirtualMoney();
    setInterval(() => {
      virtualMoney += 1000; // Reward $1000 every hour
      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }, 3600000); // 1 hour in milliseconds
  </script>
</body>
</html>
