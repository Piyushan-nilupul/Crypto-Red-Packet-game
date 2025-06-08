<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Red Packet Grid Game</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: #0d0d0d;
      color: white;
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 110vh;
      overflow-x: hidden;
    }

    .slider-container {
      width: 100%;
      max-width: 400px;
      aspect-ratio: 8 / 5;
      overflow: hidden;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
      margin: 20px auto;
      position: relative;
      margin-bottom: 50px;
    }

    .slider {
      display: flex;
      width: 300%;
      animation: slide 12s infinite ease-in-out;
    }

    .slider-image {
  width: 100%;
  height: auto;
  object-fit: cover;
  position: absolute;
  opacity: 0;
  transition: opacity 1s ease-in-out;
}

    .slider-image.active {
      opacity: 1;
      z-index: 1;
    }

    .sub-header {
      font-size: 20px; 
      color: #ffffff;
      margin-top: 2px;
      margin-bottom: 2px;
      font-weight: bold;
      text-align: center;
      letter-spacing: 2px;
    }

    .sub-header3 {
      font-size: 10px; 
      color: #ffffff;
      margin-top: 2px;
      margin-bottom: 20px;
      font-weight: bold;
      text-align: center;
      letter-spacing: 2px;
    }

    .note {
      color: #ffffff;
      margin-top: 2px;
      margin-bottom: 2px;
      font-size: 16px;
      text-align: left;
      max-width: 400px;
      padding: 0 10px;
    }

    .note strong {
      font-weight: bold;
    }

    .futuristic-border {
      position: relative;
      display: inline-block;
      padding: 12px 30px;
      color: #fff;
      font-size: 18px;
      font-weight: bold;
      text-transform: uppercase;
      background: #111;
      border: 2px solid #00ffe7;
      border-radius: 12px;
      cursor: pointer;
      text-decoration: none;
      overflow: hidden;
      z-index: 1;
      transition: 0.3s;
      box-shadow: 0 0 5px #00ffe7, 0 0 10px #00ffe7, 0 0 20px #00ffe7;
    }

    .futuristic-border::before {
      content: '';
      position: absolute;
      top: -50%;
      left: -50%;
      width: 200%;
      height: 200%;
      background: linear-gradient(45deg, #00ffe7, #ff00ff, #00ffe7, #ff00ff);
      animation: rotate 4s linear infinite;
      z-index: -1;
      filter: blur(20px);
      opacity: 0.6;
    }

    @keyframes rotate {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .futuristic-border:hover {
      background: #000;
      box-shadow: 0 0 10px #00ffe7, 0 0 20px #00ffe7, 0 0 40px #00ffe7;
    }

    .signup-text {
      color: white;
      text-decoration: none;
      z-index: 2;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
      gap: 10px;
      width: 90%;
      max-width: 300px;
    }

    .envelope {
      width: 150px;
      height: 220px;
      background: #f26246;
      border-radius: 10px;
      position: relative;
      box-shadow: 0 6px 15px rgba(0, 0, 0, 0.25);
      cursor: pointer;
      overflow: hidden;
      perspective: 800px;
      transition: opacity 0.3s ease;
    }

    .flap-container {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 70px;
      transform-origin: top center;
      transition: transform 0.6s ease;
      z-index: 3;
    }

    .flap {
      width: 100%;
      height: 80%;
      background: #c62828;
      border-bottom-left-radius: 90px 40px;
      border-bottom-right-radius: 90px 40px;
      position: relative;
    }

    .seal {
      position: absolute;
      top: 45px;
      left: 50%;
      transform: translateX(-50%);
      width: 30px;
      height: 30px;
      background: gold;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      font-size: 16px;
      color: red;
      box-shadow: 0 3px 5px rgba(0,0,0,0.3);
      z-index: 2;
      transition: opacity 0.3s ease;
    }

    .content {
      position: absolute;
      top: 40px;
      width: 100%;
      text-align: center;
      color: white;
      font-size: 11px;
      font-weight: bold;
      opacity: 0;
      transform: translateY(20px);
    }

    .envelope.open .flap-container {
      transform: rotateX(160deg);
    }

    .envelope.open .content {
      animation: popup 0.7s ease-out forwards;
      animation-delay: 0.3s;
    }

    @keyframes popup {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(-20px);
      }
    }

    #messageBox {
      margin-top: 10px;
      font-size: 20px;
      color: #c62828;
      font-weight: bold;
      height: 30px;
      text-align: center;
    }

    .ticker-wrap {
      overflow: hidden;
      background: #111;
      padding: 10px 0;
      white-space: nowrap;
      box-shadow: 0 2px 10px rgba(0,0,0,0.5);
      width: 100%;
    }

    .ticker {
      display: inline-block;
      padding-left: 100%;
      animation: scroll-left 50s linear infinite;
    }

    .ticker-item {
      display: inline-block;
      margin: 0 50px;
      font-size: 18px;
      color: #00ffcc;
    }

    @keyframes scroll-left {
      0% { transform: translateX(0); }
      100% { transform: translateX(-100%); }
    }

    @keyframes glow {
      0% {
        box-shadow: 0 0 5px #ffcc00, 0 0 10px #ffcc00, 0 0 20px #ffcc00;
      }
      50% {
        box-shadow: 0 0 15px #ff9900, 0 0 30px #ff9900, 0 0 45px #ff9900;
      }
      100% {
        box-shadow: 0 0 5px #ffcc00, 0 0 10px #ffcc00, 0 0 20px #ffcc00;
      }
    }

    .glow {
      animation: glow 2s infinite ease-in-out;
    }

    @media (max-width: 400px) {
      .envelope {
        width: 70px;
        height: 100px;
      }

      .grid {
        gap: 10px;
      }

      .header {
        font-size: 24px;
      }

      .sub-header {
        font-size: 16px;
      }

      .note {
        font-size: 14px;
      }
    }
  </style>
</head>
<body>

  <div class="ticker-wrap">
    <div class="ticker" id="cryptoTicker"></div>
  </div>

  <div class="slider-container">
  <img class="slider-image active" src="https://i.ibb.co/Y7b9Y1qj/Copy-of-T-20250514-230746-0000.png" alt="Image 1">
  <img class="slider-image" src="https://i.ibb.co/TDSL6YnD/Copy-of-T-20250514-224231-0000.png" alt="Copy-of-T-20250514-224231-0000" border="0" alt="Image 2">
  <img class="slider-image" src="https://i.ibb.co/tSG9WbX/T-20250514-213538-0000.png" alt="Image 3">
</div>

  <div class="grid" id="grid"></div>
  <div id="messageBox"></div>

  <h2 class="sub-header">▫️How to Claim▫️</h2>
  <div class="note">
    <p>▫️ Among these 9 packets, 1 packet has a lucky word.<br>
      ▫️ Note down the lucky word.<br>
      ▫️ Open the Binance app.<br>
      ▫️ Go to Pay > Red Packet.<br>
      ▫️ Tap "Receive".<br>
      ▫️ Paste the secret code.<br>
      ▫️ Claim your crypto Red Packet.
    </p>
  </div>

  <h3 class="sub-header3">▫️If you don't have Binance Account▫️</h3>
  <div class="futuristic-border">
    <a href="https://accounts.binance.com/register?ref=PZI36S9A" target="_blank" class="signup-text">Sign Up</a>
  </div>

<script>
  async function fetchCryptoPrices() {
    const ids = [
      'bitcoin', 'ethereum', 'binancecoin', 'solana', 'ripple',
      'cardano', 'dogecoin', 'avalanche-2', 'polkadot', 'tron'
    ];
    try {
      const response = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=${ids.join(',')}`);
      const data = await response.json();
      const ticker = document.getElementById('cryptoTicker');
      ticker.innerHTML = '';
      data.forEach(coin => {
        const div = document.createElement('div');
        div.className = 'ticker-item';
        div.innerHTML = `<img src="${coin.image}" alt="${coin.name}" style="width:20px;height:20px;vertical-align:middle;margin-right:5px;">
          ${coin.name} (${coin.symbol.toUpperCase()}): $${coin.current_price.toLocaleString()}`;
        ticker.appendChild(div);
      });
    } catch (error) {
      console.error("Failed to fetch crypto prices:", error);
    }
  }

  fetchCryptoPrices();
  setInterval(fetchCryptoPrices, 60000);

  const codeWords = ["code1", "code2", "code3", "code4","code5","code6", "code7","code8", "code9", "code10","code11","code12", "code13", "code14" , "code15", "U99QZL6W", "YY6ROONY", "code18", "code19", "code20","code21", "code22", "code23", "code24" ]
  const currentHour = new Date().getHours();
  const luckyWord = `congratulation! Your Code ${codeWords[currentHour]}`;
  const secretWords = ['Try again', 'Try again', 'Try again', luckyWord, 'Try again', 'Try again', 'Try again', 'Try again', 'Try again'];
  let openCount = 0;
  const maxOpen = 2;
  const foundWords = [];
  const grid = document.getElementById('grid');
  secretWords.sort(() => Math.random() - 0.5);

  secretWords.forEach((word) => {
    const envelope = document.createElement('div');
    envelope.className = 'envelope glow';
    envelope.innerHTML = `
      <div class="flap-container">
        <div class="flap">
          <div class="seal">福</div>
        </div>
      </div>
      <div class="content">${word}</div>
    `;
    envelope.addEventListener('click', function () {
      if (envelope.classList.contains('open') || openCount >= maxOpen) return;
      envelope.classList.add('open');
      envelope.classList.remove('glow');
      openCount++;
      foundWords.push(word);
      if (openCount === maxOpen) {
        setTimeout(() => {
          document.querySelectorAll('.envelope').forEach(env => {
            if (!env.classList.contains('open')) {
              env.style.pointerEvents = 'none';
              env.style.opacity = '0.5';
            }
          });
          const msg = document.getElementById('messageBox');
          if (foundWords.some(w => w.includes('congratulation'))) {
            msg.textContent = `Congratulation!!`;
          } else {
            msg.textContent = `Please try again later.`;
          }
        }, 800);
      }
    });
    grid.appendChild(envelope);
  });

  // SLIDER LOGIC HERE
  let currentImageIndex = 0;
  const images = document.querySelectorAll(".slider-image");

  function showNextImage() {
    images[currentImageIndex].classList.remove("active");
    currentImageIndex = (currentImageIndex + 1) % images.length;
    images[currentImageIndex].classList.add("active");
  }

  setInterval(showNextImage, 4000); // Slide every 4 seconds
</script>

</body>
</html>
