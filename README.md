<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>nolifecontent — Automated YouTube Shorts</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080808;
    --bg2: #0f0f0f;
    --bg3: #141414;
    --border: #1e1e1e;
    --accent: #00ff88;
    --accent2: #00ccff;
    --text: #f0f0f0;
    --muted: #666;
    --dim: #333;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Mono', monospace;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    width: 12px; height: 12px;
    background: var(--accent);
    border-radius: 50%;
    position: fixed;
    top: 0; left: 0;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.1s ease;
    mix-blend-mode: difference;
  }

  .cursor-ring {
    width: 36px; height: 36px;
    border: 1px solid var(--accent);
    border-radius: 50%;
    position: fixed;
    top: 0; left: 0;
    pointer-events: none;
    z-index: 9998;
    transition: all 0.15s ease;
    opacity: 0.5;
  }

  /* Grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,255,136,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,255,136,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  /* Nav */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    padding: 20px 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid var(--border);
    background: rgba(8,8,8,0.9);
    backdrop-filter: blur(20px);
  }

  .logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.1rem;
    letter-spacing: -0.02em;
  }

  .logo span { color: var(--accent); }

  nav a {
    color: var(--muted);
    text-decoration: none;
    font-size: 0.75rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    transition: color 0.2s;
  }

  nav a:hover { color: var(--accent); }

  /* Hero */
  .hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 120px 40px 80px;
    position: relative;
    z-index: 1;
    max-width: 1200px;
    margin: 0 auto;
  }

  .hero-tag {
    font-size: 0.7rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 32px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .hero-tag::before {
    content: '';
    width: 32px;
    height: 1px;
    background: var(--accent);
  }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(3rem, 8vw, 7rem);
    line-height: 0.95;
    letter-spacing: -0.04em;
    margin-bottom: 40px;
  }

  .hero h1 .line2 {
    color: transparent;
    -webkit-text-stroke: 1px var(--dim);
  }

  .hero h1 .accent { color: var(--accent); }

  .hero-sub {
    font-size: 0.9rem;
    color: var(--muted);
    max-width: 480px;
    line-height: 1.8;
    margin-bottom: 60px;
  }

  /* Waitlist form */
  .waitlist-form {
    display: flex;
    gap: 0;
    max-width: 500px;
    border: 1px solid var(--border);
    background: var(--bg2);
    transition: border-color 0.3s;
  }

  .waitlist-form:focus-within {
    border-color: var(--accent);
  }

  .waitlist-form input {
    flex: 1;
    padding: 16px 20px;
    background: transparent;
    border: none;
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 0.85rem;
    outline: none;
  }

  .waitlist-form input::placeholder { color: var(--muted); }

  .waitlist-form button {
    padding: 16px 24px;
    background: var(--accent);
    color: #000;
    border: none;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    cursor: none;
    transition: background 0.2s;
    white-space: nowrap;
  }

  .waitlist-form button:hover { background: #00ffaa; }

  .form-note {
    margin-top: 12px;
    font-size: 0.7rem;
    color: var(--muted);
  }

  /* Stats bar */
  .stats {
    position: absolute;
    bottom: 40px;
    left: 40px;
    right: 40px;
    display: flex;
    gap: 60px;
    padding-top: 40px;
    border-top: 1px solid var(--border);
  }

  .stat-item {}
  .stat-num {
    font-family: 'Syne', sans-serif;
    font-size: 2rem;
    font-weight: 800;
    color: var(--accent);
  }
  .stat-label {
    font-size: 0.7rem;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-top: 4px;
  }

  /* How it works */
  .section {
    padding: 120px 40px;
    max-width: 1200px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  .section-label {
    font-size: 0.7rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 60px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-label::before {
    content: '';
    width: 32px;
    height: 1px;
    background: var(--accent);
  }

  .steps {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1px;
    background: var(--border);
    border: 1px solid var(--border);
  }

  .step {
    background: var(--bg);
    padding: 40px 32px;
    transition: background 0.3s;
  }

  .step:hover { background: var(--bg3); }

  .step-num {
    font-family: 'Syne', sans-serif;
    font-size: 3rem;
    font-weight: 800;
    color: var(--border);
    line-height: 1;
    margin-bottom: 24px;
    transition: color 0.3s;
  }

  .step:hover .step-num { color: var(--accent); }

  .step h3 {
    font-family: 'Syne', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: 12px;
  }

  .step p {
    font-size: 0.78rem;
    color: var(--muted);
    line-height: 1.7;
  }

  /* Demo section */
  .demo-section {
    padding: 80px 40px 120px;
    max-width: 1200px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  .demo-container {
    border: 1px solid var(--border);
    background: var(--bg2);
    padding: 60px;
    position: relative;
    overflow: hidden;
  }

  .demo-container::before {
    content: 'DEMO OUTPUT';
    position: absolute;
    top: 20px;
    right: 24px;
    font-size: 0.6rem;
    letter-spacing: 0.2em;
    color: var(--muted);
  }

  .terminal {
    background: #000;
    border: 1px solid var(--border);
    padding: 24px;
    font-size: 0.78rem;
    line-height: 2;
    margin-bottom: 40px;
  }

  .terminal .prompt { color: var(--accent); }
  .terminal .output { color: var(--muted); }
  .terminal .success { color: var(--accent2); }
  .terminal .cursor-blink {
    display: inline-block;
    width: 8px;
    height: 14px;
    background: var(--accent);
    animation: blink 1s infinite;
    vertical-align: middle;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .video-mockups {
    display: flex;
    gap: 20px;
    justify-content: center;
  }

  .phone-mockup {
    width: 140px;
    height: 248px;
    background: var(--bg3);
    border: 1px solid var(--border);
    position: relative;
    overflow: hidden;
    flex-shrink: 0;
  }

  .phone-mockup:nth-child(2) { transform: translateY(-20px); }

  .phone-screen {
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, #111 0%, #000 100%);
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    padding: 12px;
  }

  .phone-title {
    font-family: 'Syne', sans-serif;
    font-size: 0.6rem;
    font-weight: 700;
    line-height: 1.3;
    margin-bottom: 8px;
  }

  .phone-bar {
    height: 2px;
    background: var(--accent);
    width: 60%;
    margin-bottom: 4px;
    opacity: 0.6;
  }

  .phone-views {
    font-size: 0.5rem;
    color: var(--muted);
  }

  .phone-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 40%, rgba(0,255,136,0.05) 100%);
  }

  /* Pricing */
  .pricing-section {
    padding: 80px 40px 120px;
    max-width: 1200px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  .pricing-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1px;
    background: var(--border);
    border: 1px solid var(--border);
    margin-top: 60px;
  }

  .pricing-card {
    background: var(--bg);
    padding: 48px 40px;
    position: relative;
    transition: background 0.3s;
  }

  .pricing-card:hover { background: var(--bg3); }

  .pricing-card.featured {
    background: var(--bg2);
    border: 1px solid var(--accent);
    margin: -1px;
    z-index: 1;
  }

  .pricing-tag {
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 24px;
  }

  .pricing-name {
    font-family: 'Syne', sans-serif;
    font-size: 1.4rem;
    font-weight: 800;
    margin-bottom: 8px;
  }

  .pricing-price {
    font-family: 'Syne', sans-serif;
    font-size: 3.5rem;
    font-weight: 800;
    line-height: 1;
    margin-bottom: 4px;
  }

  .pricing-price span {
    font-size: 1rem;
    color: var(--muted);
    font-weight: 400;
  }

  .pricing-desc {
    font-size: 0.75rem;
    color: var(--muted);
    margin-bottom: 40px;
    line-height: 1.6;
  }

  .pricing-features {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 14px;
    margin-bottom: 40px;
  }

  .pricing-features li {
    font-size: 0.78rem;
    color: var(--muted);
    display: flex;
    gap: 12px;
    align-items: flex-start;
  }

  .pricing-features li::before {
    content: '→';
    color: var(--accent);
    flex-shrink: 0;
  }

  .pricing-features li.included { color: var(--text); }

  .btn-primary {
    display: block;
    width: 100%;
    padding: 14px;
    background: var(--accent);
    color: #000;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    text-align: center;
    text-decoration: none;
    border: none;
    cursor: none;
    transition: background 0.2s;
  }

  .btn-primary:hover { background: #00ffaa; }

  .btn-secondary {
    display: block;
    width: 100%;
    padding: 14px;
    background: transparent;
    color: var(--text);
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    text-align: center;
    text-decoration: none;
    border: 1px solid var(--border);
    cursor: none;
    transition: border-color 0.2s;
  }

  .btn-secondary:hover { border-color: var(--text); }

  /* FAQ */
  .faq-section {
    padding: 80px 40px 120px;
    max-width: 800px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  .faq-item {
    border-top: 1px solid var(--border);
    padding: 28px 0;
  }

  .faq-item:last-child { border-bottom: 1px solid var(--border); }

  .faq-q {
    font-family: 'Syne', sans-serif;
    font-size: 0.95rem;
    font-weight: 700;
    margin-bottom: 12px;
    cursor: none;
  }

  .faq-a {
    font-size: 0.8rem;
    color: var(--muted);
    line-height: 1.8;
  }

  /* Footer CTA */
  .footer-cta {
    padding: 120px 40px;
    text-align: center;
    border-top: 1px solid var(--border);
    position: relative;
    z-index: 1;
  }

  .footer-cta h2 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    letter-spacing: -0.03em;
    margin-bottom: 16px;
  }

  .footer-cta p {
    font-size: 0.85rem;
    color: var(--muted);
    margin-bottom: 40px;
  }

  .footer-cta .waitlist-form {
    margin: 0 auto;
  }

  footer {
    padding: 24px 40px;
    border-top: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    z-index: 1;
  }

  footer p {
    font-size: 0.7rem;
    color: var(--muted);
  }

  /* Ticker */
  .ticker {
    overflow: hidden;
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    padding: 16px 0;
    background: var(--bg2);
    position: relative;
    z-index: 1;
  }

  .ticker-inner {
    display: flex;
    gap: 60px;
    animation: ticker 20s linear infinite;
    white-space: nowrap;
  }

  .ticker-item {
    font-size: 0.7rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .ticker-item span { color: var(--accent); }

  @keyframes ticker {
    from { transform: translateX(0); }
    to { transform: translateX(-50%); }
  }

  /* Animations */
  .fade-in {
    opacity: 0;
    transform: translateY(24px);
    animation: fadeIn 0.8s ease forwards;
  }

  .fade-in:nth-child(2) { animation-delay: 0.1s; }
  .fade-in:nth-child(3) { animation-delay: 0.2s; }
  .fade-in:nth-child(4) { animation-delay: 0.3s; }

  @keyframes fadeIn {
    to { opacity: 1; transform: translateY(0); }
  }

  /* Success message */
  .success-msg {
    display: none;
    font-size: 0.8rem;
    color: var(--accent);
    margin-top: 12px;
  }

  @media (max-width: 768px) {
    nav { padding: 16px 20px; }
    .hero { padding: 100px 20px 60px; }
    .stats { flex-wrap: wrap; gap: 30px; position: relative; bottom: auto; left: auto; right: auto; margin-top: 60px; padding-top: 40px; }
    .section, .demo-section, .pricing-section, .faq-section { padding: 60px 20px; }
    .demo-container { padding: 30px 20px; }
    .video-mockups { gap: 12px; }
    footer { flex-direction: column; gap: 12px; text-align: center; }
    body { cursor: auto; }
    .cursor, .cursor-ring { display: none; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<nav>
  <div class="logo">nolife<span>content</span></div>
  <a href="#pricing">Pricing</a>
</nav>

<!-- Ticker -->
<div class="ticker" style="margin-top: 61px;">
  <div class="ticker-inner">
    <div class="ticker-item">Automated Shorts <span>↗</span></div>
    <div class="ticker-item">Any Niche <span>↗</span></div>
    <div class="ticker-item">Daily Uploads <span>↗</span></div>
    <div class="ticker-item">AI Scripts <span>↗</span></div>
    <div class="ticker-item">Auto Subtitles <span>↗</span></div>
    <div class="ticker-item">YouTube Growth <span>↗</span></div>
    <div class="ticker-item">Automated Shorts <span>↗</span></div>
    <div class="ticker-item">Any Niche <span>↗</span></div>
    <div class="ticker-item">Daily Uploads <span>↗</span></div>
    <div class="ticker-item">AI Scripts <span>↗</span></div>
    <div class="ticker-item">Auto Subtitles <span>↗</span></div>
    <div class="ticker-item">YouTube Growth <span>↗</span></div>
  </div>
</div>

<!-- Hero -->
<div class="hero">
  <div class="hero-tag fade-in">YouTube Shorts Automation</div>
  <h1 class="fade-in">
    YOUR CHANNEL.<br>
    <span class="line2">ZERO EFFORT.</span><br>
    <span class="accent">DAILY.</span>
  </h1>
  <p class="hero-sub fade-in">
    We build and upload AI-generated YouTube Shorts to your channel every single day. Any niche. Fully automated. You just collect the views.
  </p>

  <div class="fade-in">
    <form class="waitlist-form" id="heroForm">
      <input type="email" placeholder="your@email.com" required id="heroEmail">
      <button type="submit">Join Waitlist</button>
    </form>
    <p class="form-note">Early access — $49/mo at launch. No card required.</p>
    <p class="success-msg" id="heroSuccess">✓ You're on the list. We'll be in touch.</p>
  </div>

  <div class="stats">
    <div class="stat-item">
      <div class="stat-num">3x</div>
      <div class="stat-label">Videos per day</div>
    </div>
    <div class="stat-item">
      <div class="stat-num">~$0.01</div>
      <div class="stat-label">Cost per video</div>
    </div>
    <div class="stat-item">
      <div class="stat-num">24/7</div>
      <div class="stat-label">Fully automated</div>
    </div>
    <div class="stat-item">
      <div class="stat-num">∞</div>
      <div class="stat-label">Niches supported</div>
    </div>
  </div>
</div>

<!-- How it works -->
<div class="section">
  <div class="section-label">How it works</div>
  <div class="steps">
    <div class="step">
      <div class="step-num">01</div>
      <h3>You connect your channel</h3>
      <p>Sign up, tell us your niche and keywords, connect your YouTube account via OAuth. Takes 5 minutes.</p>
    </div>
    <div class="step">
      <div class="step-num">02</div>
      <h3>We find trending topics</h3>
      <p>Our system scans Reddit, HackerNews, and news feeds daily to find the most viral topics in your niche.</p>
    </div>
    <div class="step">
      <div class="step-num">03</div>
      <h3>AI writes the script</h3>
      <p>Claude generates a punchy, engaging 60-second script with a hook designed to stop the scroll.</p>
    </div>
    <div class="step">
      <div class="step-num">04</div>
      <h3>Video is built & uploaded</h3>
      <p>TTS voiceover, stock footage, burned-in subtitles, title card — fully produced and uploaded automatically.</p>
    </div>
  </div>
</div>

<!-- Demo -->
<div class="demo-section">
  <div class="section-label">Live pipeline output</div>
  <div class="demo-container">
    <div class="terminal">
      <div><span class="prompt">→</span> Scanning trends...</div>
      <div class="output">  Found: "OpenAI releases new reasoning model"</div>
      <div class="output">  Source: r/technology (94% upvoted, 12.4k points)</div>
      <div><span class="prompt">→</span> Generating script...</div>
      <div class="output">  Hook: "Nobody is talking about what this actually means..."</div>
      <div class="output">  Words: 143 | Est. duration: 58s</div>
      <div><span class="prompt">→</span> Rendering video...</div>
      <div class="output">  TTS: 5.2s | Footage: 8.1s | Subtitles: 6.4s</div>
      <div><span class="success">✓ Uploaded to YouTube — youtu.be/xK9mP2vQr</span></div>
      <div><span class="prompt">→</span> <span class="cursor-blink"></span></div>
    </div>

    <div class="video-mockups">
      <div class="phone-mockup">
        <div class="phone-screen">
          <div class="phone-overlay"></div>
          <div class="phone-title">OpenAI Just Changed Everything Nobody Is Talking About</div>
          <div class="phone-bar"></div>
          <div class="phone-views">24.6K views · 3h ago</div>
        </div>
      </div>
      <div class="phone-mockup">
        <div class="phone-screen">
          <div class="phone-overlay"></div>
          <div class="phone-title">This New Law Will Affect Every American Starting Next Month</div>
          <div class="phone-bar"></div>
          <div class="phone-views">891K views · 1d ago</div>
        </div>
      </div>
      <div class="phone-mockup">
        <div class="phone-screen">
          <div class="phone-overlay"></div>
          <div class="phone-title">Scientists Discovered Something Underground That Rewrites History</div>
          <div class="phone-bar"></div>
          <div class="phone-views">142K views · 6h ago</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Pricing -->
<div class="pricing-section" id="pricing">
  <div class="section-label">Pricing</div>
  <div class="pricing-grid">
    <div class="pricing-card">
      <div class="pricing-tag">Starter</div>
      <div class="pricing-name">Basic</div>
      <div class="pricing-price">$29<span>/mo</span></div>
      <div class="pricing-desc">Perfect for testing the waters with one channel.</div>
      <ul class="pricing-features">
        <li class="included">1 YouTube channel</li>
        <li class="included">1 video per day</li>
        <li class="included">AI script generation</li>
        <li class="included">Auto subtitles</li>
        <li>Custom niche keywords</li>
        <li>Priority trend sources</li>
      </ul>
      <a href="#waitlist" class="btn-secondary">Join Waitlist</a>
    </div>

    <div class="pricing-card featured">
      <div class="pricing-tag">Most Popular</div>
      <div class="pricing-name">Growth</div>
      <div class="pricing-price">$49<span>/mo</span></div>
      <div class="pricing-desc">For serious creators ready to scale fast.</div>
      <ul class="pricing-features">
        <li class="included">1 YouTube channel</li>
        <li class="included">3 videos per day</li>
        <li class="included">AI script generation</li>
        <li class="included">Auto subtitles</li>
        <li class="included">Custom niche keywords</li>
        <li class="included">Priority trend sources</li>
      </ul>
      <a href="#waitlist" class="btn-primary">Join Waitlist</a>
    </div>

    <div class="pricing-card">
      <div class="pricing-tag">Agency</div>
      <div class="pricing-name">Scale</div>
      <div class="pricing-price">$149<span>/mo</span></div>
      <div class="pricing-desc">Run multiple channels across different niches.</div>
      <ul class="pricing-features">
        <li class="included">5 YouTube channels</li>
        <li class="included">3 videos per day each</li>
        <li class="included">AI script generation</li>
        <li class="included">Auto subtitles</li>
        <li class="included">Custom niche keywords</li>
        <li class="included">Priority trend sources</li>
      </ul>
      <a href="#waitlist" class="btn-secondary">Join Waitlist</a>
    </div>
  </div>
</div>

<!-- FAQ -->
<div class="faq-section">
  <div class="section-label">FAQ</div>

  <div class="faq-item">
    <div class="faq-q">Does this violate YouTube's terms of service?</div>
    <div class="faq-a">No. We use the official YouTube Data API to upload videos, which is explicitly permitted. The content is original — generated fresh for each upload using AI scripts, unique voiceovers, and licensed stock footage.</div>
  </div>

  <div class="faq-item">
    <div class="faq-q">What niches work best?</div>
    <div class="faq-a">Tech/AI news, finance, science facts, geopolitics, and true crime perform best. Anything with a passionate audience and frequent news cycles. We scan multiple sources to find trending content in your chosen niche daily.</div>
  </div>

  <div class="faq-item">
    <div class="faq-q">How long until I hit YouTube monetization?</div>
    <div class="faq-a">At 3 uploads per day, most channels see significant growth within 60-90 days. YouTube Shorts monetization requires 10M views in 90 days — channels in viral niches have hit this on our system within 4-6 months.</div>
  </div>

  <div class="faq-item">
    <div class="faq-q">Can I review videos before they upload?</div>
    <div class="faq-a">Currently videos are uploaded automatically. A review/approval queue is on our roadmap. For now, videos are set to Public by default but you can request Private upload mode and manually publish.</div>
  </div>

  <div class="faq-item">
    <div class="faq-q">When does this launch?</div>
    <div class="faq-a">We're accepting early access signups now. Waitlist members get priority onboarding and locked-in pricing before public launch.</div>
  </div>
</div>

<!-- Footer CTA -->
<div class="footer-cta" id="waitlist">
  <h2>START GROWING<br>YOUR CHANNEL.</h2>
  <p>Join the waitlist. No card required.</p>
  <form class="waitlist-form" id="footerForm">
    <input type="email" placeholder="your@email.com" required id="footerEmail">
    <button type="submit">Join Waitlist</button>
  </form>
  <p class="success-msg" id="footerSuccess">✓ You're on the list. We'll be in touch.</p>
</div>

<footer>
  <p>© 2026 nolifecontent. All rights reserved.</p>
  <p>nolifecontent.xyz</p>
</footer>

<script>
  // Custom cursor
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx = 0, my = 0, rx = 0, ry = 0;

  document.addEventListener('mousemove', e => {
    mx = e.clientX; my = e.clientY;
    cursor.style.left = mx - 6 + 'px';
    cursor.style.top = my - 6 + 'px';
  });

  function animRing() {
    rx += (mx - rx - 18) * 0.12;
    ry += (my - ry - 18) * 0.12;
    ring.style.left = rx + 'px';
    ring.style.top = ry + 'px';
    requestAnimationFrame(animRing);
  }
  animRing();

  // Waitlist forms
  function handleForm(form, emailId, successId) {
    form.addEventListener('submit', e => {
      e.preventDefault();
      const email = document.getElementById(emailId).value;
      // Store locally for now
      const emails = JSON.parse(localStorage.getItem('waitlist') || '[]');
      if (!emails.includes(email)) emails.push(email);
      localStorage.setItem('waitlist', JSON.stringify(emails));

      form.style.display = 'none';
      document.getElementById(successId).style.display = 'block';
    });
  }

  handleForm(document.getElementById('heroForm'), 'heroEmail', 'heroSuccess');
  handleForm(document.getElementById('footerForm'), 'footerEmail', 'footerSuccess');

  // Smooth scroll for anchor links
  document.querySelectorAll('a[href^="#"]').forEach(a => {
    a.addEventListener('click', e => {
      e.preventDefault();
      document.querySelector(a.getAttribute('href'))?.scrollIntoView({ behavior: 'smooth' });
    });
  });
</script>
</body>
</html>
