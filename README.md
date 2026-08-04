<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>vinayak_c :: whoami</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:        #030604;
    --panel:     #060b07;
    --line:      #164027;
    --green:     #3cff7a;
    --green-dim: #1f7a45;
    --green-soft:rgba(60,255,122,0.08);
    --amber:     #ffcf5c;
    --red:       #ff4d4d;
    --red-dim:   #7a2323;
    --red-soft:  rgba(255,77,77,0.09);
    --blue:      #4da8ff;
    --blue-dim:  #23507a;
    --blue-soft: rgba(77,168,255,0.09);
    --text:      #c9f7d6;
    --text-dim:  #6ea87e;
    --text-mute: #3f6650;
    --radius: 2px;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  @media (prefers-reduced-motion: reduce){
    html{scroll-behavior:auto;}
    *{animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important;}
  }
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:'JetBrains Mono', monospace;
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
    position:relative;
    overflow-x:hidden;
  }
  ::selection{ background:var(--green); color:#000; }
  a{ color:inherit; }
  :focus-visible{ outline:2px solid var(--green); outline-offset:3px; }
  .wrap{ max-width:900px; margin:0 auto; padding:0 22px; position:relative; z-index:2; }

  /* ---------- matrix rain canvas ---------- */
  #matrix{ position:fixed; inset:0; z-index:0; opacity:0.22; pointer-events:none; }

  /* ---------- CRT scanline + vignette overlay ---------- */
  .crt{
    position:fixed; inset:0; z-index:3; pointer-events:none;
    background:
      repeating-linear-gradient(rgba(0,0,0,0) 0px, rgba(0,0,0,0) 2px, rgba(0,0,0,0.18) 3px),
      radial-gradient(ellipse at 50% 50%, transparent 55%, rgba(0,0,0,0.55) 100%);
  }
  .flicker{ animation:flicker 6s infinite; }
  @keyframes flicker{
    0%,96%,100%{ opacity:1; }
    97%{ opacity:0.94; }
    98%{ opacity:1; }
    99%{ opacity:0.9; }
  }

  /* ---------- reveal-on-scroll ---------- */
  .reveal{ opacity:0; transform:translateY(14px); transition:opacity .5s ease, transform .5s ease; }
  .reveal.in{ opacity:1; transform:translateY(0); }

  /* ---------- terminal window shell (used for every panel) ---------- */
  .win{
    background:var(--panel);
    border:1px solid var(--line);
    border-radius:var(--radius);
    box-shadow:0 0 0 1px rgba(60,255,122,0.05), 0 20px 50px -30px rgba(0,0,0,0.9);
  }
  .win-bar{
    display:flex; align-items:center; gap:8px;
    padding:9px 14px; border-bottom:1px solid var(--line);
    background:#020402;
    font-size:11.5px; color:var(--text-mute); letter-spacing:0.3px;
  }
  .win-dot{ width:8px; height:8px; border-radius:50%; background:var(--green-dim); }
  .win-title{ margin-left:6px; }
  .win-body{ padding:20px 22px; }

  /* ---------- NAV ---------- */
  header.nav{
    position:sticky; top:0; z-index:50;
    background:rgba(3,6,4,0.88);
    border-bottom:1px solid var(--line);
  }
  .nav-inner{ display:flex; align-items:center; justify-content:space-between; padding:14px 22px; max-width:900px; margin:0 auto; }
  .brand{ font-weight:700; font-size:14px; color:var(--green); }
  .brand .cursor-blk{ display:inline-block; width:8px; height:14px; background:var(--green); vertical-align:-2px; margin-left:2px; animation:blink 1s step-end infinite; }
  nav.links{ display:flex; gap:20px; font-size:12.5px; }
  nav.links a{ text-decoration:none; color:var(--text-dim); }
  nav.links a::before{ content:'./'; color:var(--green-dim); }
  nav.links a:hover{ color:var(--green); }
  @media (max-width:640px){ nav.links{ display:none; } }

  /* ---------- HERO ---------- */
  .hero{ padding:56px 0 36px; }
  .boot-line{ font-size:12.5px; color:var(--text-mute); margin:0 0 4px; }
  .boot-line .ok{ color:var(--green); }
  h1.name{
    font-size:clamp(28px,5vw,44px); margin:18px 0 6px; font-weight:800; color:var(--green);
    text-shadow:0 0 18px rgba(60,255,122,0.35);
    position:relative;
  }
  h1.name .glitch-wrap{ position:relative; display:inline-block; }
  h1.name .glitch-wrap::before, h1.name .glitch-wrap::after{
    content:attr(data-text); position:absolute; top:0; left:0; width:100%; overflow:hidden;
    background:var(--bg); mix-blend-mode:screen;
  }
  h1.name .glitch-wrap::before{ color:var(--red); animation:glitch-r 4.5s infinite; clip-path:inset(0 0 60% 0); }
  h1.name .glitch-wrap::after{ color:var(--blue); animation:glitch-b 4.5s infinite; clip-path:inset(60% 0 0 0); }
  @keyframes glitch-r{
    0%,92%,100%{ transform:translate(0,0); opacity:0; }
    93%{ transform:translate(-3px,-1px); opacity:0.85; }
    94%{ transform:translate(2px,1px); opacity:0.85; }
    95%{ transform:translate(-2px,0); opacity:0; }
  }
  @keyframes glitch-b{
    0%,92%,100%{ transform:translate(0,0); opacity:0; }
    93%{ transform:translate(3px,1px); opacity:0.85; }
    94%{ transform:translate(-2px,-1px); opacity:0.85; }
    95%{ transform:translate(2px,0); opacity:0; }
  }
  .role-line{ font-size:14px; color:var(--text-dim); margin:0 0 18px; }
  .role-line .amber{ color:var(--amber); }
  .hero-meta{ display:flex; flex-wrap:wrap; gap:8px 18px; font-size:12.5px; color:var(--text-mute); margin-bottom:26px; }
  .hero-meta a{ text-decoration:none; color:var(--text-dim); border-bottom:1px dotted var(--text-mute); }
  .hero-meta a:hover{ color:var(--green); border-color:var(--green); }

  .terminal-body{ font-size:13px; min-height:190px; }
  .term-line{ margin:0 0 6px; white-space:pre-wrap; word-break:break-word; opacity:0; }
  .term-line.shown{ opacity:1; }
  .prompt{ color:var(--green); }
  .out{ color:var(--text-dim); }
  .cursor{ display:inline-block; width:7px; height:14px; background:var(--green); vertical-align:-2px; animation:blink 1s step-end infinite; }
  @keyframes blink{ 50%{ opacity:0; } }

  /* ---------- SECTIONS ---------- */
  section{ padding:44px 0; }
  .sec-head{ display:flex; align-items:baseline; gap:10px; margin-bottom:22px; }
  .sec-idx{ color:var(--green-dim); font-size:13px; }
  .sec-head h2{ font-size:16px; margin:0; font-weight:700; color:var(--green); letter-spacing:0.5px; text-transform:uppercase; }
  .sec-head .rule{ flex:1; height:1px; background:var(--line); }

  /* ---------- SUMMARY ---------- */
  .summary p{ color:var(--text-dim); font-size:14.5px; max-width:72ch; }
  .badge{
    display:inline-block; margin-top:12px; margin-right:8px; padding:5px 12px;
    border:1px solid var(--green-dim); border-radius:2px;
    font-size:11.5px; color:var(--green); background:var(--green-soft);
  }
  .badge-red{ border-color:var(--red-dim); color:var(--red); background:var(--red-soft); }
  .badge-blue{ border-color:var(--blue-dim); color:var(--blue); background:var(--blue-soft); }

  /* ---------- SKILLS ---------- */
  .skill-grid{ display:grid; grid-template-columns:repeat(auto-fit,minmax(210px,1fr)); gap:12px; }
  .skill-card{ padding:14px 16px; }
  .skill-card h3{ margin:0 0 10px; font-size:11px; text-transform:uppercase; letter-spacing:1px; color:var(--text-mute); font-weight:600; }
  .skill-card h3::before{ content:'# '; color:var(--green-dim); }
  .skill-card ul{ list-style:none; margin:0; padding:0; display:flex; flex-wrap:wrap; gap:6px; }
  .skill-card li{
    font-size:12px; color:var(--text);
    background:#02110a; border:1px solid var(--line); padding:3px 9px; border-radius:2px;
  }
  .skill-card li::before{ content:'> '; color:var(--green-dim); }

  /* ---------- EXPERIENCE ---------- */
  .timeline{ position:relative; padding-left:22px; border-left:1px dashed var(--line); }
  .tl-item{ position:relative; margin-bottom:30px; }
  .tl-item:last-child{ margin-bottom:0; }
  .tl-item::before{
    content:'●'; position:absolute; left:-27px; top:-2px; color:var(--green); font-size:12px;
    text-shadow:0 0 8px rgba(60,255,122,0.6), 2px 0 4px rgba(255,77,77,0.4), -2px 0 4px rgba(77,168,255,0.4);
  }
  .tl-head{ display:flex; flex-wrap:wrap; justify-content:space-between; gap:8px; align-items:baseline; }
  .tl-head h3{ margin:0; font-size:14.5px; color:var(--text); }
  .tl-head .org{ color:var(--green); }
  .tl-date{ font-size:11.5px; color:var(--text-mute); }
  .tl-item ul{ margin:9px 0 0; padding-left:16px; color:var(--text-dim); font-size:13px; }
  .tl-item li{ margin-bottom:5px; }
  .tl-item li::marker{ color:var(--green-dim); }

  /* ---------- PROJECTS ---------- */
  .case-grid{ display:grid; grid-template-columns:repeat(auto-fit,minmax(270px,1fr)); gap:16px; }
  .case{ padding:18px 20px; }
  .case .win-bar{ padding:7px 12px; }
  .case h3{ margin:0 0 10px; font-size:14px; color:var(--green); }
  .case ul{ margin:0 0 14px; padding-left:16px; color:var(--text-dim); font-size:13px; }
  .case li{ margin-bottom:5px; }
  .case li::marker{ color:var(--green-dim); }
  .tag-row{ display:flex; flex-wrap:wrap; gap:6px; }
  .tag{
    font-size:10.5px; color:var(--green);
    border:1px solid var(--green-dim); padding:2px 8px; border-radius:2px;
  }
  .tag::before{ content:'#'; margin-right:2px; }
  .tag-red{ color:var(--red); border-color:var(--red-dim); }
  .tag-blue{ color:var(--blue); border-color:var(--blue-dim); }

  /* ---------- CERTS + EDU ---------- */
  .twocol{ display:grid; grid-template-columns:1fr 1fr; gap:16px; }
  @media (max-width:700px){ .twocol{ grid-template-columns:1fr; } }
  .col-label{ font-size:11px; color:var(--green-dim); text-transform:uppercase; letter-spacing:1px; margin:0 0 12px; font-weight:600; }
  .list-plain{ list-style:none; margin:0; padding:0; }
  .list-plain li{ padding:11px 0; border-bottom:1px dashed var(--line); }
  .list-plain li:last-child{ border-bottom:none; }
  .list-plain strong{ color:var(--text); display:block; font-size:13.5px; margin-bottom:2px; }
  .list-plain strong::before{ content:'$ '; color:var(--green-dim); }
  .list-plain .meta{ font-size:11.5px; color:var(--text-mute); }

  /* ---------- FOOTER ---------- */
  footer{ padding:50px 0 44px; border-top:1px solid var(--line); text-align:center; }
  footer h2{ font-size:20px; margin:0 0 10px; color:var(--green); }
  footer p{ color:var(--text-dim); margin:0 0 22px; font-size:13.5px; }
  .cta-row{ display:flex; justify-content:center; gap:12px; flex-wrap:wrap; }
  .btn{
    font-size:13px; font-weight:600; text-decoration:none;
    padding:10px 18px; border-radius:2px; border:1px solid var(--green-dim);
    color:var(--green); transition:all .15s; background:var(--green-soft);
  }
  .btn::before{ content:'[ '; } .btn::after{ content:' ]'; }
  .btn.primary{ background:var(--green); border-color:var(--green); color:#000; }
  .btn.primary:hover{ box-shadow:0 0 18px rgba(60,255,122,0.5); }
  .btn:not(.primary):hover{ border-color:var(--green); background:rgba(60,255,122,0.14); }
  .foot-note{ margin-top:36px; font-size:11.5px; color:var(--text-mute); }
</style>
</head>
<body class="flicker">

<canvas id="matrix"></canvas>
<div class="crt"></div>

<header class="nav">
  <div class="nav-inner">
    <div class="brand">VC://cyb3r<span class="cursor-blk"></span></div>
    <nav class="links">
      <a href="#skills">skills</a>
      <a href="#experience">experience</a>
      <a href="#projects">projects</a>
      <a href="#education">education</a>
      <a href="#contact">contact</a>
    </nav>
  </div>
</header>

<div class="wrap hero">
  <p class="boot-line"><span class="ok">[ OK ]</span> loading identity module...</p>
  <p class="boot-line"><span class="ok">[ OK ]</span> access level: intern → target: soc_analyst</p>
  <h1 class="name"><span class="glitch-wrap" data-text="VINAYAK C">VINAYAK C</span></h1>
  <p class="role-line">Cyber Security Intern <span class="amber">//</span> VAPT · Mobile AppSec · turning findings into reports that actually get fixed</p>
  <div class="hero-meta">
    <span>root@kerala:~#</span>
    <a href="tel:+918943614557">+91 89436 14557</a>
    <a href="mailto:vinayakc409@gmail.com">vinayakc409@gmail.com</a>
    <a href="https://www.linkedin.com/in/vinayak-c-0b1828369" target="_blank" rel="noopener">linkedin</a>
    <a href="https://github.com/zero-hunt" target="_blank" rel="noopener">github</a>
  </div>

  <div class="win">
    <div class="win-bar">
      <span class="win-dot"></span><span class="win-dot"></span><span class="win-dot"></span>
      <span class="win-title">vinayak@synnefo:~$ ./run_scan.sh</span>
    </div>
    <div class="win-body terminal-body" id="termBody"></div>
  </div>
</div>

<div class="wrap">

  <section class="summary reveal" id="about">
    <div class="sec-head"><span class="sec-idx">[ 00 ]</span><h2>about</h2><div class="rule"></div></div>
    <p>Computer Science graduate with hands-on experience in vulnerability assessment, ethical hacking, and mobile application security — built through real assessments, not just theory. Comfortable moving from recon to reporting: finding the issue, mapping it to OWASP Top 10 / CWE, and writing it up so a dev team can actually fix it.</p>
    <span class="badge">STATUS: seeking entry-level SOC Analyst role</span>
    <span class="badge badge-red">RED TEAM: offense &amp; VAPT</span>
    <span class="badge badge-blue">BLUE TEAM: detection &amp; reporting</span>
  </section>

  <section id="skills" class="reveal">
    <div class="sec-head"><span class="sec-idx">[ 01 ]</span><h2>skills</h2><div class="rule"></div></div>
    <div class="skill-grid">
      <div class="skill-card win"><div class="win-body">
        <h3>tools</h3>
        <ul><li>Burp Suite</li><li>Nmap</li><li>Nessus</li><li>Metasploitable</li><li>Frida</li><li>MobSF</li></ul>
      </div></div>
      <div class="skill-card win"><div class="win-body">
        <h3>operating systems</h3>
        <ul><li>Kali Linux</li><li>Windows</li></ul>
      </div></div>
      <div class="skill-card win"><div class="win-body">
        <h3>virtualization</h3>
        <ul><li>VirtualBox</li><li>Genymotion</li><li>Android Studio</li></ul>
      </div></div>
      <div class="skill-card win"><div class="win-body">
        <h3>vapt</h3>
        <ul><li>Manual testing</li><li>Scripting & automation</li></ul>
      </div></div>
      <div class="skill-card win"><div class="win-body">
        <h3>soft skills</h3>
        <ul><li>Analytical thinking</li><li>Communication</li><li>Leadership</li><li>Adaptability</li></ul>
      </div></div>
    </div>
  </section>

  <section id="experience" class="reveal">
    <div class="sec-head"><span class="sec-idx">[ 02 ]</span><h2>work_experience</h2><div class="rule"></div></div>
    <div class="timeline">
      <div class="tl-item">
        <div class="tl-head">
          <h3>Cyber Security Intern — <span class="org">Synnefo Solutions</span></h3>
          <span class="tl-date">March 2026</span>
        </div>
        <ul>
          <li>Assisted in vulnerability assessment and penetration testing of web applications and networks.</li>
          <li>Performed security assessments using Burp Suite, Nmap, and Wireshark.</li>
          <li>Identified and documented security vulnerabilities across engagements.</li>
          <li>Applied ethical hacking methodologies and cybersecurity best practices hands-on.</li>
          <li>Prepared security reports and recommendations under senior analyst guidance.</li>
        </ul>
      </div>
    </div>
  </section>

  <section id="projects" class="reveal">
    <div class="sec-head"><span class="sec-idx">[ 03 ]</span><h2>projects</h2><div class="rule"></div></div>
    <div class="case-grid">
      <div class="case win">
        <div class="win-bar"><span class="win-dot" style="background:var(--red)"></span><span class="win-title">project_01.log</span></div>
        <div class="win-body">
          <h3>Web Application VAPT — Synnefo Academy</h3>
          <ul>
            <li>Conducted a full VAPT engagement on a production web application.</li>
            <li>Identified Stored XSS, Clickjacking, and security misconfigurations.</li>
            <li>Followed OWASP Top 10 methodology throughout testing.</li>
          </ul>
          <div class="tag-row">
            <span class="tag tag-red">burpsuite</span><span class="tag tag-red">nmap</span><span class="tag tag-blue">gobuster</span><span class="tag">owasp_top10</span>
          </div>
        </div>
      </div>
      <div class="case win">
        <div class="win-bar"><span class="win-dot" style="background:var(--blue)"></span><span class="win-title">project_02.log</span></div>
        <div class="win-body">
          <h3>Simple Subdomain Enumeration</h3>
          <ul>
            <li>Built a Bash script for automated subdomain discovery.</li>
            <li>Integrated Assetfinder with live HTTP probing.</li>
            <li>Exported live subdomains for reconnaissance workflows.</li>
          </ul>
          <div class="tag-row">
            <span class="tag tag-blue">bash</span><span class="tag tag-blue">assetfinder</span><span class="tag">recon</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section id="education" class="reveal">
    <div class="sec-head"><span class="sec-idx">[ 04 ]</span><h2>certifications_&_education</h2><div class="rule"></div></div>
    <div class="twocol">
      <div>
        <p class="col-label">certifications</p>
        <ul class="list-plain">
          <li><strong>Introduction to the Threat Landscape 3.0</strong><span class="meta">Fortinet Training Institute</span></li>
          <li><strong>Getting Started in Cybersecurity 3.0</strong><span class="meta">Fortinet Training Institute</span></li>
        </ul>
      </div>
      <div>
        <p class="col-label">education</p>
        <ul class="list-plain">
          <li><strong>Advanced Diploma in Cyber Security</strong><span class="meta">Synnefo Solutions Pvt. Ltd., Kochi, Kerala · Jul 2025 – Mar 2026</span></li>
          <li><strong>B.Sc. Computer Science</strong><span class="meta">Assabah Arts and Science College, Valayamkulam · 2022 – 2025</span></li>
        </ul>
      </div>
    </div>
  </section>

</div>

<footer id="contact">
  <div class="wrap">
    <h2>$ nc -lvp connect</h2>
    <p>Open to SOC Analyst and cybersecurity opportunities — happy to walk through any finding on this page in detail.</p>
    <div class="cta-row">
      <a class="btn primary" href="mailto:vinayakc409@gmail.com">EMAIL ME</a>
      <a class="btn" href="tel:+918943614557">CALL</a>
      <a class="btn" href="https://www.linkedin.com/in/vinayak-c-0b1828369" target="_blank" rel="noopener">LINKEDIN</a>
      <a class="btn" href="https://github.com/zero-hunt" target="_blank" rel="noopener">GITHUB</a>
    </div>
    <p class="foot-note">root@kerala:~# built by VC://cyb3r · session terminated on scroll end</p>
  </div>
</footer>

<script>
  // Matrix rain background
  const canvas = document.getElementById('matrix');
  const ctx = canvas.getContext('2d');
  let w, h, cols, drops;
  const chars = 'アイウエオカキクケコサシスセソ01ABCDEFGHIJKLMNOPQRSTUVWXYZ$#@%&';
  function resize(){
    w = canvas.width = window.innerWidth;
    h = canvas.height = window.innerHeight;
    cols = Math.floor(w / 16);
    drops = new Array(cols).fill(0).map(() => Math.random() * -50);
  }
  resize();
  window.addEventListener('resize', resize);
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  function drawMatrix(){
    ctx.fillStyle = 'rgba(3,6,4,0.08)';
    ctx.fillRect(0,0,w,h);
    ctx.fillStyle = '#3cff7a';
    ctx.font = '14px JetBrains Mono, monospace';
    for(let i=0;i<cols;i++){
      const text = chars[Math.floor(Math.random()*chars.length)];
      ctx.fillText(text, i*16, drops[i]*16);
      if(drops[i]*16 > h && Math.random() > 0.975) drops[i] = 0;
      drops[i]++;
    }
  }
  if(!reduceMotion){
    setInterval(drawMatrix, 55);
  }

  // Terminal typewriter
  const lines = [
    { text: '$ whoami', cls: 'prompt' },
    { text: 'vinayak_c — cyber security intern, aspiring soc analyst', cls: 'out' },
    { text: '$ nmap -sV --top-ports 3 vinayakc.dev', cls: 'prompt' },
    { text: 'PORT      STATE  SERVICE', cls: 'out' },
    { text: '443/tcp   open   vapt-experience', cls: 'out' },
    { text: '8080/tcp  open   mobile-app-security', cls: 'out' },
    { text: '9000/tcp  open   soc-analyst-track', cls: 'out' }
  ];
  const termBody = document.getElementById('termBody');

  function renderLine(line){
    const p = document.createElement('p');
    p.className = 'term-line';
    if(line.cls === 'prompt'){
      const cmd = line.text.replace('$ ', '');
      p.innerHTML = '<span class="prompt">$</span> ' + cmd;
    } else {
      p.innerHTML = '<span class="out">' + line.text + '</span>';
    }
    return p;
  }
  function showCursorLine(){
    const p = document.createElement('p');
    p.className = 'term-line shown';
    p.innerHTML = '<span class="prompt">$</span> <span class="cursor"></span>';
    termBody.appendChild(p);
  }
  if(reduceMotion){
    lines.forEach(l => { const el = renderLine(l); el.classList.add('shown'); termBody.appendChild(el); });
    showCursorLine();
  } else {
    let i = 0;
    function step(){
      if(i < lines.length){
        const el = renderLine(lines[i]);
        termBody.appendChild(el);
        requestAnimationFrame(() => requestAnimationFrame(() => el.classList.add('shown')));
        i++;
        setTimeout(step, 400);
      } else {
        showCursorLine();
      }
    }
    setTimeout(step, 300);
  }

  // Scroll reveal
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if(entry.isIntersecting){
        entry.target.classList.add('in');
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });
  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
</script>

</body>
</html>
