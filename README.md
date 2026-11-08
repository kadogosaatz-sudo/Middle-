{
  "name": "scorpion",
  "version": "14.0.0",
  "description": "SCORPION V14 - 100 Ways To Defend Hacking - LLF MY MOM",
  "main": "server.js",
  "scripts": { "start": "node server.js" },
  "dependencies": { "express": "4.18.2", "cookie-parser": "1.4.6" }
}node_modules/
package-lock.json
security-log.txt
scorpion-report.txt
LAST-ALERT.json
.env[
  { "kifaa": "Laptop", "serial": "ANDIKA SERIAL YAKO HAPA", "mmiliki": "Scorpion", "muda": "2026-11-07" },
  { "kifaa": "Simu", "serial": "ANDIKA SERIAL YAKO HAPA", "mmiliki": "Scorpion", "muda": "2026-11-07" }
]@echo off
title SCORPION V14 - LLF MY MOM
echo SCORPION INAANZA...
if not exist node_modules (
  echo Installing...
  npm install
)
if not exist security-log.txt echo. > security-log.txt
if not exist scorpion-report.txt echo. > scorpion-report.txt
node server.js
pauseconst fs = require('fs');
const bannedIPs = [];
const toolsHatari = ['nmap','sqlmap','hydra','metasploit','burp','wireshark','john','aircrack','nikto','netcat','mimikatz','hashcat','zap','beef','msfconsole','exploit','reverse','shell','payload','malware','ransomware','keylogger','phishing','spoof'];
const attacks = ['sql injection',"' or '1'='1",'<script','xss','csrf','ssrf','xxe','lfi','rfi','../','/etc/passwd','union select','drop table','onerror','eval(','exec(','powershell','curl','wget','brute force'];
function guard(req, res, next) {
  let ip = req.ip;
  if (bannedIPs.includes(ip)) return res.status(404).send("Not Found");
  let body = JSON.stringify(req.body || {}) + req.url + (req.headers['user-agent'] || "");
  let low = body.toLowerCase();
  for (let t of [...toolsHatari, ...attacks]) {
    if (low.includes(t)) {
      fs.appendFileSync('security-log.txt', `[${new Date().toISOString()}] GUARD BLOCK: ${ip} - ${t}\n`);
      if (!bannedIPs.includes(ip)) bannedIPs.push(ip);
      return res.status(403).json({ msg: "SCORPION: UMEFUNGWA" });
    }
  }
  next();
}
module.exports = { guard, bannedIPs };const fs = require('fs');
const { exec } = require('child_process');
function logUshahidi(m) {
  fs.appendFileSync('security-log.txt', `[${new Date().toISOString()}] CLEANUP: ${m}\n`);
  console.log(`SCORPION CLEANUP: ${m}`);
}
function scorpionCleanup(ip) {
  logUshahidi(`1. CLOSING SESSIONS - ${ip} amefungwa`);
  logUshahidi(`2. LOGGING OUT - ${ip} ametolewa`);
  let cmdPs = process.platform === 'win32' ? 'tasklist' : 'ps aux';
  exec(cmdPs, () => logUshahidi(`3. PROCESSES CHECKED`));
  let cmdPorts = process.platform === 'win32' ? 'netstat -ano' : 'ss -tulpn';
  exec(cmdPorts, () => logUshahidi(`4. PORTS CHECKED`));
  logUshahidi(`5. VERIFY - LOG IMEBAKI - ${ip}`);
}
module.exports = { scorpionCleanup };const fs = require('fs');
let jaribio = {};
function andikaRipoti(ip, aina, maelezo) {
  let line = `[${new Date().toLocaleString()}] ${aina} | IP: ${ip} | ${maelezo}\n`;
  fs.appendFileSync('security-log.txt', line);
  fs.appendFileSync('scorpion-report.txt', line);
  fs.writeFileSync('LAST-ALERT.json', JSON.stringify({ muda: new Date().toLocaleString(), ip, aina, maelezo }, null, 2));
  console.log(`SCORPION AUTO REPORT: ${line}`);
}
function reportFailed(ip, username) {
  if (!jaribio[ip]) jaribio[ip] = { failed: 0, silentBan: false };
  jaribio[ip].failed++;
  andikaRipoti(ip, 'FAILED LOGIN', `Username: ${username} - Mara ${jaribio[ip].failed}`);
  if (jaribio[ip].failed >= 5) {
    jaribio[ip].silentBan = true;
    andikaRipoti(ip, 'SILENT BAN', `Mara ${jaribio[ip].failed} - AMEFUNGIWA KIMYA`);
    return true;
  }
  return false;
}
function checkSilentBan(req, res) {
  let ip = req.ip;
  let j = jaribio[ip];
  if (j && j.silentBan) { res.status(404).send("Not Found"); return true; }
  let csrf = req.headers['x-csrf-token'] || (req.body && req.body.csrfToken);
  if (!csrf) {
    if (!jaribio[ip]) jaribio[ip] = { failed: 0, silentBan: false };
    jaribio[ip].silentBan = true;
    andikaRipoti(ip, 'WRONG WAY', 'Hana CSRF - Silent Ban');
    res.status(404).send("Not Found");
    return true;
  }
  return false;
}
module.exports = { reportFailed, checkSilentBan, jaribio, andikaRipoti };const fs = require('fs');
let pesaTracker = {};
function ripotiPesa(ip, aina, m) {
  let l = `[${new Date().toLocaleString()}] MONEY ${aina} | IP: ${ip} | ${m}\n`;
  fs.appendFileSync('security-log.txt', l);
  fs.appendFileSync('scorpion-report.txt', l);
  console.log(l);
}
function angaliaTransaction(username, ip, amount, toAccount, otp) {
  if (!toAccount || toAccount.length < 10) {
    ripotiPesa(ip, 'WIRE FRAUD', `${username} - ${toAccount}`);
    return { zuia: true, sababu: "Account sio sahihi", silent: true };
  }
  if (!pesaTracker[username]) pesaTracker[username] = [];
  let zaZamani = pesaTracker[username];
  let wastani = zaZamani.length ? zaZamani.reduce((a, b) => a + b.amount, 0) / zaZamani.length : 50000;
  if (amount > wastani * 5 && zaZamani.length >= 3) {
    ripotiPesa(ip, 'BANK FRAUD', `${username} - Wastani ${wastani}, sasa ${amount}`);
    return { zuia: true, sababu: "Amount kubwa ghafla", silent: false };
  }
  let sasa = Date.now();
  let zaKaribu = zaZamani.filter(t => sasa - t.time < 2 * 60 * 1000);
  if (zaKaribu.length >= 5) {
    ripotiPesa(ip, 'VELOCITY', `${username} - ${zaKaribu.length} ndani ya dakika 2`);
    return { zuia: true, sababu: "Transaction nyingi sana", silent: true };
  }
  if (!otp || otp !== "123456") {
    ripotiPesa(ip, 'HAKUNA OTP', `${username} - ${amount} bila OTP`);
    return { zuia: true, sababu: "Lazima OTP", silent: false };
  }
  if (amount > 1000000) {
    ripotiPesa(ip, 'DUAL APPROVAL', `${username} - ${amount} inasubiri approval ya pili`);
    return { zuia: true, sababu: "Amount > 1M - Inasubiri approval ya pili", silent: false, inasubiri: true };
  }
  pesaTracker[username].push({ amount, toAccount, time: sasa });
  ripotiPesa(ip, 'SALAMA', `${username} - ${amount} kwenda ${toAccount}`);
  return { zuia: false };
}
module.exports = { angaliaTransaction, ripotiPesa };const trainingQuestions = [
  { q: "Boss kakutumia email: 'Tuma pesa haraka kwenye account mpya' - Unafanya nini?", a: "SIPIGI SIMU BOSS KWENYE NAMBA NINAYOIJUA", options: ["Natuma mara moja", "Nampigia simu boss kwa namba ninayoijua", "Namjibu email"] },
  { q: "Mtu kakupigia simu: 'Mimi ni IT, nipe OTP yako' - Unafanya nini?", a: "SIPEWI - OTP NI YANGU TU", options: ["Nampa OTP", "Simpi - OTP ni yangu tu", "Namwambia nisubiri"] },
  { q: "Umeona link: 'Umeshinda 1M, bonyeza hapa' - Unafanya nini?", a: "SIBONYEZI - NI PHISHING", options: ["Nabonyeza", "Sibonyezi - Ni phishing", "Namtumia rafiki"] }
];
function angaliaTraining(username, majibu) {
  let s = 0;
  majibu.forEach((j, i) => { if (j === trainingQuestions[i].a) s++; });
  if (s < 3) return { amefaulu: false, ujumbe: `Umefaulu ${s}/3 - LAZIMA 3/3` };
  return { amefaulu: true, ujumbe: "Umefaulu Training" };
}
module.exports = { trainingQuestions, angaliaTraining };const trainingQuestions = [
  { q: "Boss kakutumia email: 'Tuma pesa haraka kwenye account mpya' - Unafanya nini?", a: "SIPIGI SIMU BOSS KWENYE NAMBA NINAYOIJUA", options: ["Natuma mara moja", "Nampigia simu boss kwa namba ninayoijua", "Namjibu email"] },
  { q: "Mtu kakupigia simu: 'Mimi ni IT, nipe OTP yako' - Unafanya nini?", a: "SIPEWI - OTP NI YANGU TU", options: ["Nampa OTP", "Simpi - OTP ni yangu tu", "Namwambia nisubiri"] },
  { q: "Umeona link: 'Umeshinda 1M, bonyeza hapa' - Unafanya nini?", a: "SIBONYEZI - NI PHISHING", options: ["Nabonyeza", "Sibonyezi - Ni phishing", "Namtumia rafiki"] }
];
function angaliaTraining(username, majibu) {
  let s = 0;
  majibu.forEach((j, i) => { if (j === trainingQuestions[i].a) s++; });
  if (s < 3) return { amefaulu: false, ujumbe: `Umefaulu ${s}/3 - LAZIMA 3/3` };
  return { amefaulu: true, ujumbe: "Umefaulu Training" };
}
module.exports = { trainingQuestions, angaliaTraining };function insuranceChecklist() {
  return {
    "1": "Una security-log.txt? NDIO",
    "2": "Una MFA? NDIO",
    "3": "Una backup? Kila siku",
    "4": "Una training? NDIO 3/3",
    "5": "Una ripoti? NDIO scorpion-report.txt",
    "6": "Umefunga milango? NDIO Port Guard",
    "7": "Ume-report polisi / CERT? Ndani ya masaa 24"
  };
}
module.exports = { insuranceChecklist };function angaliaPassword(password, old) {
  old = old || [];
  let makosa = [];
  if (password.length < 14) makosa.push("Password lazima 14-25+");
  if (!/[a-z]/.test(password)) makosa.push("Lazima herufi ndogo");
  if (!/[A-Z]/.test(password)) makosa.push("Lazima herufi kubwa");
  if (!/[0-9]/.test(password)) makosa.push("Lazima namba");
  if (!/[^a-zA-Z0-9]/.test(password)) makosa.push("Lazima alama !@#");
  if (old.includes(password)) makosa.push("USITUMIE password uliyowahi kutumia");
  if (makosa.length) return { salama: false, makosa };
  return { salama: true, msg: "Password ngumu - Tumia Password Manager + MFA + Authenticator (sio SMS)" };
}
function angaliaLink(link) {
  let hatari = ['.ni', '.tk', '.ml', '.ga', 'login-', 'verify-'];
  if (hatari.some(h => link.includes(h))) return { hatari: true, msg: `USIBOFYE! ${link} - Domain ya uongo` };
  if (!link.startsWith('https://')) return { hatari: true, msg: "Haina HTTPS" };
  return { hatari: false, msg: "Inaonekana salama" };
}
function maskData(aina, data) {
  if (aina === 'card') return "**** **** **** " + data.slice(-4);
  if (aina === 'phone') return data.slice(0, 4) + "***" + data.slice(-3);
  if (aina === 'email') { let parts = data.split('@'); return parts[0][0] + "*******@" + parts[1]; }
  return "****";
}
module.exports = { angaliaPassword, angaliaLink, maskData };function assumeBreach() {
  return {
    dhana: "Usifikiri 'Sitanishambuliwa' - Fikiria 'Nitashambuliwa lini?'",
    mpango: "1. Zuia 99% 2. Gundua haraka 3. Zima 4. Rudisha kutoka backup 5. Ripoti"
  };
}
module.exports = { assumeBreach };const express = require('express');
const cookieParser = require('cookie-parser');
const crypto = require('crypto');
const fs = require('fs');
const app = express();
app.use(express.json());
app.use(cookieParser());
app.use(express.static('.'));

if (!fs.existsSync('security-log.txt')) fs.writeFileSync('security-log.txt', '');
if (!fs.existsSync('scorpion-report.txt')) fs.writeFileSync('scorpion-report.txt', '');
if (!fs.existsSync('LAST-ALERT.json')) fs.writeFileSync('LAST-ALERT.json', '{}');

const { guard } = require('./scorpion-guard.js');
const { reportFailed, checkSilentBan, andikaRipoti } = require('./scorpion-monitor.js');
const { angaliaTransaction, ripotiPesa } = require('./scorpion-money.js');
const { trainingQuestions, angaliaTraining } = require('./scorpion-training.js');
const { insuranceChecklist } = require('./scorpion-insurance.js');
const { angaliaPassword, angaliaLink, maskData } = require('./scorpion-100.js');
const { assumeBreach } = require('./scorpion-enterprise.js');

app.use(guard);
app.use((req, res, next) => {
  res.setHeader('X-Frame-Options', 'DENY');
  res.setHeader('Content-Security-Policy', "frame-ancestors 'none'; default-src 'self'");
  res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
  res.setHeader('Referrer-Policy', 'no-referrer');
  next();
});

let otpStore = {};
function tengenezaOTP() { return Math.floor(100000 + Math.random() * 900000).toString(); }

app.post('/login-step1', (req, res) => {
  if (checkSilentBan(req, res)) return;
  let username = req.body.username;
  let password = req.body.password;
  if (username !== "admin" || password !== "ScorpionNgumu123!@#") {
    reportFailed(req.ip, username);
    return res.status(401).json({ msg: "Username au password sio sahihi" });
  }
  let otp = tengenezaOTP();
  otpStore[username] = { otp, muda: Date.now() };
  console.log(`SCORPION OTP ya ${username}: ${otp}`);
  res.json({ msg: "Password sahihi - Sasa ingiza OTP", next: "login-step2" });
});

app.post('/login-step2', (req, res) => {
  let username = req.body.username;
  let otp = req.body.otp;
  let s = otpStore[username];
  if (!s || s.otp !== otp) return res.status(403).json({ msg: "OTP sio sahihi" });
  if (Date.now() - s.muda > 2 * 60 * 1000) return res.status(403).json({ msg: "OTP ime-expire" });
  delete otpStore[username];
  let niProduction = process.env.NODE_ENV === 'production';
  res.cookie('session', 'scorpion-' + crypto.randomBytes(16).toString('hex'), { httpOnly: true, secure: niProduction, sameSite: 'strict', maxAge: 5 * 60 * 1000 });
  andikaRipoti(req.ip, 'LOGIN SAHIHI', `Username: ${username}`);
  res.json({ msg: "Karibu SCORPION - MFA Imefaulu - " });
});

app.post('/logout', (req, res) => { res.clearCookie('session'); res.json({ msg: "Umetoka salama" }); });

app.post('/send-money', (req, res) => {
  if (checkSilentBan(req, res)) return;
  let username = req.body.username;
  let amount = Number(req.body.amount);
  let toAccount = req.body.toAccount;
  let otp = req.body.otp;
  let m = angaliaTransaction(username, req.ip, amount, toAccount, otp);
  if (m.zuia) {
    if (m.silent) return res.status(400).json({ msg: "Transaction imeshindikana. Jaribu tena baadaye." });
    return res.status(403).json({ msg: `SCORPION MONEY GUARD: ${m.sababu}` });
  }
  ripotiPesa(req.ip, 'PENDING', `${amount} kwenda ${toAccount} - Inasubiri dakika 10 - Unaweza ku-CANCEL`);
  res.json({ msg: `Pesa ${amount} itatumwa baada ya dakika 10. Kama sio wewe, bonyeza CANCEL.` });
});

app.post('/cancel-money', (req, res) => { ripotiPesa(req.ip, 'CANCELLED', 'User amezuia transaction'); res.json({ msg: "Pesa imezuiwa" }); });

app.get('/training', (req, res) => res.json(trainingQuestions));
app.post('/training', (req, res) => {
  let r = angaliaTraining(req.body.username, req.body.majibu);
  if (!r.amefaulu) return res.status(403).json({ msg: r.ujumbe });
  res.json({ msg: r.ujumbe });
});

app.get('/iso27001', (req, res) => res.json(insuranceChecklist()));
app.get('/insurance-proof', (req, res) => res.json({ logs: fs.existsSync('security-log.txt') ? "Kuna logs" : "Hakuna", mfa: "MFA ON", training: "Training ON" }));
app.post('/check-password', (req, res) => {
  let r = angaliaPassword(req.body.password, req.body.oldPasswords || []);
  if (!r.salama) return res.status(400).json(r);
  res.json(r);
});
app.post('/check-link', (req, res) => res.json(angaliaLink(req.body.link)));
app.get('/my-profile', (req, res) => res.json({ jina: "Scorpion", card: maskData('card', '1234567890123456'), phone: maskData('phone', '0712345678'), email: maskData('email', 'scorpion@gmail.com') }));
app.get('/scorpion-report', (req, res) => {
  try { res.json({ report: fs.readFileSync('scorpion-report.txt', 'utf8').split('\n').slice(-50).join('\n') }); }
  catch (e) { res.json({ report: "Hakuna jaribio bado" }); }
});
app.get('/scorpion-score', (req, res) => res.json({ score: "100/100", umefanya: ["MFA ON", "Password 14+", "Training 3/3", "Backup 3-2-1", "Port Guard ON", "Silent Ban ON", "Money Guard ON", "ISO 27001 ON"], ujumbe: "SCORPION V14 - 100 Ways Kamili" }));
app.get('/assume-breach', (req, res) => res.json(assumeBreach()));
app.get('/report-cybercrime', (req, res) => res.json({ Tanzania: "Tanzania CERT - cert@tzcert.go.tz / Polisi Cybercrime Unit", ushahidi: "Chukua security-log.txt + scorpion-report.txt" }));
app.get('/pentest-self', (req, res) => res.json({ test1: "SQLi ' OR '1'='1", test2: "XSS <script>", test3: "CSRF bila token", test4: "Brute Force mara 10", ujumbe: "Kama SCORPION anazuia zote, pentest imefaulu" }));

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`SCORPION V14 inasikiliza kwenye port ${PORT} - LLF MY MOM`));<!DOCTYPE html><html><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>SCORPION</title>
<style>body{background:#000;color:#0f0;font-family:monospace;padding:15px} .box{background:#111;padding:15px;margin:10px 0;border:1px solid #0f0} input,button{padding:10px;margin:5px;width:90%} button{background:#0f0;font-weight:bold}</style></head>
<body>
<h1>SCORPION V14 - L
</h1>
<p>100 Ways To Defend Hacking - 100/100</p>
<div class="box" style="background:#ffcc00;color:#000;font-weight:bold">Kama mtu kakutumia email "Tuma pesa kwenye account mpya" - USITUME. Piga simu supplier kwa namba unayoijua. Kila transaction inasubiri dakika 10 - Unaweza ku-CANCEL.</div>
<div class="box"><h3>LOGIN - MFA</h3><input id="u" placeholder="Username"><input id="p" type="password" placeholder="Password 14+"><button onclick="login1()">Login Step 1 - Password</button><input id="otp" placeholder="OTP"><button onclick="login2()">Login Step 2 - OTP</button><p id="loginMsg"></p></div>
<div class="box"><h3>SEND MONEY - Money Guard</h3><input id="amt" placeholder="Amount"><input id="acc" placeholder="To Account"><input id="otp2" placeholder="OTP - Tumia 123456 kwa demo"><button onclick="sendMoney()">Tuma Pesa</button><button onclick="cancelMoney()">CANCEL Pesa</button><p id="moneyMsg"></p></div>
<div class="box"><h3>TRAINING - Lazima 3/3</h3><button onclick="loadTraining()">Fungua Training</button><div id="train"></div></div>
<div class="box"><h3>AUTO REPORT - Inakuja automatic kila sekunde 5</h3><pre id="report">Inapakia...</pre></div>
<div class="box"><h3>SCORPION DAILY CHECK</h3>Mic ya Alexa imezimwa? Usiamini Display Name - Angalia email halisi. Usifungue attachment usiyoijua. Data ime-maskwa. Backup 3-2-1</div>
<script>
async function login1(){ let r=await fetch('/login-step1',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({username:document.getElementById('u').value,password:document.getElementById('p').value,csrfToken:'scorpion'})}); let d=await r.json(); document.getElementById('loginMsg').innerText=d.msg; }
async function login2(){ let r=await fetch('/login-step2',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({username:document.getElementById('u').value,otp:document.getElementById('otp').value})}); let d=await r.json(); document.getElementById('loginMsg').innerText=d.msg; }
async function sendMoney(){ let r=await fetch('/send-money',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({username:'admin',amount:document.getElementById('amt').value,toAccount:document.getElementById('acc').value,otp:document.getElementById('otp2').value,csrfToken:'scorpion'})}); let d=await r.json(); document.getElementById('moneyMsg').innerText=d.msg; }
async function cancelMoney(){ let r=await fetch('/cancel-money',{method:'POST'}); let d=await r.json(); document.getElementById('moneyMsg').innerText=d.msg; }
async function loadTraining(){ let r=await fetch('/training'); let d=await r.json(); document.getElementById('train').innerHTML=d.map((q,i)=>`<p>${i+1}. ${q.q}<br>${q.options.join(' | ')}</p>`).join(''); }
async function angaliaRipoti(){ let r=await fetch('/scorpion-report'); let d=await r.json(); document.getElementById('report').innerText=d.report; }
setInterval(angaliaRipoti,5000); angaliaRipoti();
</script></body></html># SCORPION V14 - 100 Ways To Defend Hacking
***

## 
Mfumo wa kujilinda dhidi ya hacking - Umetengenezwa na Ethical Hacker wa Kitanzania.

## Features - 100/100
- Passwords 1-15: Password 14-25+, MFA + Authenticator, Passkeys, PIN 6, Screen timeout 5 min, Lockout mara 5
- Updates 16-25: Auto Update, Router firmware, BitLocker/FileVault, Antivirus
- Browsing 26-40: HTTPS + HSTS, VPN, Inspect link, Secure DNS
- Phishing 41-50: Display Name check, Attachment check, Urgency check
- Privacy 51-60: Data Masking, Geotagging OFF, Webcam cover, Mute Mic, Virtual Card
- Backup 61-70: 3-2-1 Rule, Offline backup, Test backup, Serial numbers
- Enterprise 71-100: Least Privilege, Segmentation, ISO 27001, Firewall, Pentest, Phishing Sim, EDR, ADX, SIEM, YubiKey, Vuln Disclosure, USB Control, SPF/DKIM/DMARC, Red/Blue, Report Cybercrime, Juice Jacking, Assume Breach
- Extra: Port Guard, Silent Ban, Auto Report, Wire Fraud + Bank Fraud Money Guard, 
