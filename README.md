
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Date Night</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    }

    body {
      background-color: #fff0f3;
      color: #4a4a4a;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
      overflow-x: hidden;
    }

    .card {
      background: white;
      border-radius: 24px;
      padding: 28px 20px;
      width: 100%;
      max-width: 360px;
      box-shadow: 0 10px 25px rgba(255, 182, 193, 0.4);
      text-align: center;
      position: relative;
    }

    .step {
      display: none;
      flex-direction: column;
      align-items: center;
      gap: 18px;
    }

    .step.active {
      display: flex;
    }

    .emoji {
      font-size: 3rem;
      animation: pulse 1.5s infinite alternate;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      100% { transform: scale(1.1); }
    }

    h1 {
      color: #ff4d6d;
      font-size: 1.4rem;
      line-height: 1.3;
    }

    p {
      color: #707070;
      font-size: 0.95rem;
    }

    .btn-group {
      display: flex;
      gap: 12px;
      width: 100%;
      justify-content: center;
      margin-top: 10px;
      position: relative;
      min-height: 50px;
    }

    button, .submit-btn {
      padding: 12px 24px;
      border-radius: 12px;
      border: none;
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      transition: transform 0.1s ease;
    }

    .btn-yes {
      background-color: #ff4d6d;
      color: white;
      box-shadow: 0 4px 12px rgba(255, 77, 109, 0.3);
      z-index: 2;
    }

    .btn-yes:active {
      transform: scale(0.95);
    }

    .btn-no {
      background-color: #f1f5f9;
      color: #64748b;
      position: absolute;
      transition: all 0.2s ease-out;
      z-index: 1;
    }

    /* Form Inputs */
    .input-group {
      width: 100%;
      text-align: left;
    }

    label {
      font-size: 0.85rem;
      font-weight: 600;
      color: #ff4d6d;
      display: block;
      margin-bottom: 6px;
    }

    input, textarea {
      width: 100%;
      padding: 12px;
      border: 2px solid #ffe5ec;
      border-radius: 10px;
      font-size: 0.95rem;
      outline: none;
      background-color: #fffafc;
    }

    input:focus, textarea:focus {
      border-color: #ff4d6d;
    }

    textarea {
      resize: none;
      height: 80px;
    }

    .full-width-btn {
      width: 100%;
      background-color: #ff4d6d;
      color: white;
      margin-top: 8px;
    }
  </style>
</head>
<body>

  <div class="card">
    
    <!-- STEP 1: THE QUESTION -->
    <div class="step active" id="step1">
      <div class="emoji"></div>
      <h1>Will you go on a date with me?</h1>
      <p>I have something special planned for us!</p>
      
      <div class="btn-group">
        <button class="btn-yes" id="yesBtn">Yes!</button>
        <button class="btn-no" id="noBtn">No </button>
      </div>
    </div>

    <!-- STEP 2: DATE & TIME -->
    <div class="step" id="step2">
      <div class="emoji">💓</div>
      <h1>Pick a Date & Time</h1>
      <p>When are you free?</p>

      <div class="input-group">
        <label for="date">Select Date</label>
        <input type="date" id="dateInput">
      </div>

      <div class="input-group">
        <label for="time">Select Time</label>
        <input type="time" id="timeInput">
      </div>

      <button class="submit-btn full-width-btn" id="toStep3">Next Step â¨</button>
    </div>

    <!-- STEP 3: FOOD & DRINKS -->
    <div class="step" id="step3">
      <div class="emoji">ð</div>
      <h1>Food & Drinks</h1>
      <p>What are you in the mood for?</p>

      <div class="input-group">
        <label for="food">What would you like to eat?</label>
        <textarea id="foodInput" placeholder="e.g., Italian, Sushi, Tacos, Pizza..."></textarea>
      </div>

      <div class="input-group">
        <label for="drinks">What would you like to drink?</label>
        <textarea id="drinksInput" placeholder="e.g., Wine, Bubble Tea, Coffee, Cocktails..."></textarea>
      </div>

      <button class="submit-btn full-width-btn" id="sendBtn">Send to Me ð²</button>
    </div>

  </div>

  <script>
    // YOUR WHATSAPP NUMBER HERE (Include Country Code, e.g., 12345678901)
    const MY_PHONE_NUMBER = "917872658725";

    const step1 = document.getElementById('step1');
    const step2 = document.getElementById('step2');
    const step3 = document.getElementById('step3');

    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const toStep3 = document.getElementById('toStep3');
    const sendBtn = document.getElementById('sendBtn');

    // 1. RUNAWAY "NO" BUTTON LOGIC
    function moveNoButton() {
      const card = document.querySelector('.card');
      const cardRect = card.getBoundingClientRect();
      const btnRect = noBtn.getBoundingClientRect();

      // Calculate max allowed positions inside the card
      const maxX = cardRect.width - btnRect.width - 20;
      const maxY = cardRect.height - btnRect.height - 20;

      const randomX = Math.floor(Math.random() * maxX) - (cardRect.width / 2 - btnRect.width);
      const randomY = Math.floor(Math.random() * maxY) - (cardRect.height / 2 - btnRect.height);

      noBtn.style.transform = `translate(${randomX}px, ${randomY}px)`;
    }

    // Trigger move on desktop hover or mobile touch
    noBtn.addEventListener('mouseenter', moveNoButton);
    noBtn.addEventListener('touchstart', (e) => {
      e.preventDefault(); // Prevents clicking on touchscreens
      moveNoButton();
    });

    // 2. STEP NAVIGATION
    yesBtn.addEventListener('click', () => {
      step1.classList.remove('active');
      step2.classList.add('active');
    });

    toStep3.addEventListener('click', () => {
      const date = document.getElementById('dateInput').value;
      const time = document.getElementById('timeInput').value;

      if (!date || !time) {
        alert("Please pick both a date and time!");
        return;
      }

      step2.classList.remove('active');
      step3.classList.add('active');
    });

    // 3. WHATSAPP SUBMISSION LOGIC
    sendBtn.addEventListener('click', () => {
      const date = document.getElementById('dateInput').value;
      const time = document.getElementById('timeInput').value;
      const food = document.getElementById('foodInput').value || "Surprise me!";
      const drinks = document.getElementById('drinksInput').value || "Surprise me!";

      // Format text message
      const message = `Hey! I'd love to go on a date with you! ð%0A%0A` +
                      `ð *Date:* ${date}%0A` +
                      `â° *Time:* ${time}%0A` +
                      `ð *Food Craving:* ${food}%0A` +
                      `ð¹ *Drink Craving:* ${drinks}`;

      // Open WhatsApp pre-filled link
      const whatsappUrl = `https://wa.me/${MY_PHONE_NUMBER}?text=${message}`;
      window.open(whatsappUrl, '_blank');
    });
  </script>

</body>
</html>
