Pihuu

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>A Special Message for You</title>
  <style>
    :root{
      --bg1:#e6f7ff;
      --bg2:#fff7ff;
      --card:#ffffff;
      --accent:#007bff;
      --accent-dark:#0056b3;
      --muted:#666;
      --radius:16px;
      --shadow: 0 8px 30px rgba(16,24,40,0.08);
      --maxw:720px;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      color:#222;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
    }

    .card{
      width:100%;
      max-width:var(--maxw);
      background:var(--card);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
      padding:28px;
      text-align:center;
      position:relative;
      overflow:hidden;
    }

    header h1{
      margin:0 0 8px;
      color:var(--accent);
      font-size:1.6rem;
      line-height:1.1;
    }
    header p.lead{
      margin:0;
      color:var(--muted);
      font-size:1rem;
    }

    .controls{
      margin-top:20px;
      display:flex;
      gap:12px;
      justify-content:center;
      flex-wrap:wrap;
    }

    .btn{
      background:var(--accent);
      color:white;
      border:0;
      padding:10px 16px;
      font-size:1rem;
      border-radius:10px;
      cursor:pointer;
      transition:transform .12s ease, background-color .12s ease;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
    }
    .btn:active{transform:translateY(1px)}
    .btn.secondary{
      background:transparent;
      color:var(--accent);
      border:1.5px solid rgba(0,123,255,0.12);
    }
    .btn:focus{
      outline:3px solid rgba(0,123,255,0.12);
      outline-offset:2px;
    }

    .message{
      margin-top:22px;
      font-size:1.05rem;
      color:#1f2937;
      line-height:1.6;
      text-align:left;
      padding:18px;
      border-radius:12px;
      background: linear-gradient(180deg, rgba(0,123,255,0.03), rgba(0,0,0,0.01));
      display:none;
      transform-origin:center top;
    }

    .message.show{
      display:block;
      animation:pop .45s cubic-bezier(.2,.9,.2,1);
    }
    @keyframes pop{
      from{opacity:0; transform:translateY(8px) scale(.99)}
      to{opacity:1; transform:translateY(0) scale(1)}
    }

    .byline{
      margin-top:18px;
      color:var(--muted);
      font-size:.95rem;
    }

    .stickers{
      margin-top:14px;
      font-size:1.6rem;
      user-select:none;
    }

    /* simple confetti pieces */
    #confetti{
      position:absolute;
      inset:0;
      pointer-events:none;
      overflow:visible;
    }
    .confetti-piece{
      position:absolute;
      width:10px;
      height:14px;
      opacity:0;
      transform-origin:center;
      border-radius:2px;
      animation:confetti-fall 1400ms cubic-bezier(.18,.9,.32,1) forwards;
    }
    @keyframes confetti-fall{
      0%{opacity:1; transform:translateY(-10px) rotate(0deg) scale(1)}
      100%{opacity:0; transform:translateY(140vh) rotate(540deg) scale(.7)}
    }

    /* Respect users who prefer reduced motion */
    @media (prefers-reduced-motion: reduce){
      .message.show{animation:none}
      .confetti-piece{animation:none; display:none}
    }

    /* Responsive tweaks */
    @media (max-width:480px){
      .card{padding:18px}
      header h1{font-size:1.25rem}
      .message{font-size:1rem}
    }
  </style>
</head>
<body>
  <main class="card" aria-labelledby="title">
    <div id="confetti" aria-hidden="true"></div>

    <header>
      <h1 id="title">Hey Pihuudaa — You're Not Alone</h1>
      <p class="lead">I know sometimes you want space. That's completely okay. I made this because I care about you.</p>
    </header>

    <div class="controls" role="group" aria-label="Message controls">
      <button id="openBtn" class="btn" aria-expanded="false" aria-controls="specialMessage">Open My Special Message</button>
      <button id="closeBtn" class="btn secondary" style="display:none">Hide Message</button>
      <button id="copyBtn" class="btn secondary" title="Copy the message to clipboard">Copy Message</button>
    </div>

    <section id="specialMessage" class="message" role="region" aria-live="polite" aria-hidden="true">
      <p>You mean the world to me. Your smile lights up my day, and your friendship has been a constant source of joy in my life. Even if you're going through tough times, remember you're loved and valued.</p>
      <p>I'm always just a message away. No pressure — talk whenever you feel ready. You're not alone in this.</p>
      <p style="margin-top:8px; font-weight:600;">Your loved once Havan 💙</p>
      <div class="stickers" aria-hidden="true">🌟 💖 🌸 ✨ 😊 🌈</div>
    </section>

    <p class="byline" aria-hidden="true">Small things can help: a walk, a call, or even a shared playlist. I'm here.</p>
  </main>

  <script>
    (function(){
      const openBtn = document.getElementById('openBtn');
      const closeBtn = document.getElementById('closeBtn');
      const copyBtn = document.getElementById('copyBtn');
      const msg = document.getElementById('specialMessage');
      const confettiContainer = document.getElementById('confetti');

      function setExpanded(expanded){
        openBtn.setAttribute('aria-expanded', String(expanded));
        msg.setAttribute('aria-hidden', String(!expanded));
        if(expanded){
          msg.classList.add('show');
          openBtn.style.display = 'none';
          closeBtn.style.display = '';
        } else {
          msg.classList.remove('show');
          openBtn.style.display = '';
          closeBtn.style.display = 'none';
        }
      }

      function createConfetti(){
        if (window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
        const colors = ['#ffd166','#ef476f','#06d6a0','#118ab2','#06b6d4'];
        const count = 26;
        for(let i=0;i<count;i++){
          const el = document.createElement('span');
          el.className = 'confetti-piece';
          el.style.background = colors[Math.floor(Math.random()*colors.length)];
          el.style.left = (Math.random()*100) + '%';
          el.style.top = (-Math.random()*20) + 'px';
          el.style.transform = `rotate(${Math.random()*360}deg)`;
          el.style.animationDelay = (Math.random()*300) + 'ms';
          el.style.width = (8 + Math.random()*8) + 'px';
          el.style.height = (10 + Math.random()*12) + 'px';
          confettiContainer.appendChild(el);
          // cleanup after animation
          el.addEventListener('animationend', ()=> el.remove());
        }
      }

      openBtn.addEventListener('click', ()=>{
        setExpanded(true);
        createConfetti();
        // for screen readers, move focus into the message
        msg.querySelector('p')?.focus?.();
      });

      closeBtn.addEventListener('click', ()=> setExpanded(false));

      copyBtn.addEventListener('click', async ()=>{
        const text = Array.from(msg.querySelectorAll('p')).map(p=>p.textContent.trim()).join('\n\n');
        try{
          await navigator.clipboard.writeText(text);
          copyBtn.textContent = 'Copied ✓';
          setTimeout(()=> copyBtn.textContent = 'Copy Message', 2000);
        }catch(e){
          // fallback: select and prompt
          const textarea = document.createElement('textarea');
          textarea.value = text;
          document.body.appendChild(textarea);
          textarea.select();
          try{ document.execCommand('copy'); copyBtn.textContent = 'Copied ✓'; }
          catch(e){ alert('Could not copy. You can select and copy the message manually.'); }
          textarea.remove();
          setTimeout(()=> copyBtn.textContent = 'Copy Message', 2000);
        }
      });

      // keyboard accessibility: Enter/Space on focused openBtn
      openBtn.addEventListener('keyup', (e)=>{
        if(e.key === 'Enter' || e.key === ' '){ openBtn.click(); }
      });
    })();
  </script>
</body>
</html>
