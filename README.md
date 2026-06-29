<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🔮 Soul Reveal</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;900&display=swap');

:root {
  --bg: #07070f;
  --card: #0f0f1a;
  --border: #1a1a2e;
  --purple: #a855f7;
  --pink: #f72585;
  --cyan: #00f5d4;
  --gold: #fbbf24;
  --red: #ff4d6d;
  --text: #f0f0ff;
  --muted: #7070a0;
}

*{margin:0;padding:0;box-sizing:border-box;}

body{
  font-family:'Poppins',sans-serif;
  background:var(--bg);
  color:var(--text);
  min-height:100vh;
  overflow-x:hidden;
}

/* ── BACKGROUND ── */
.bg-orbs{position:fixed;inset:0;pointer-events:none;z-index:0;overflow:hidden;}
.orb{
  position:absolute;border-radius:50%;filter:blur(80px);
  animation:orbFloat 8s ease-in-out infinite alternate;
}
.orb1{width:300px;height:300px;background:rgba(168,85,247,0.12);top:-100px;left:-100px;animation-delay:0s;}
.orb2{width:250px;height:250px;background:rgba(247,37,133,0.10);bottom:-80px;right:-80px;animation-delay:3s;}
.orb3{width:200px;height:200px;background:rgba(0,245,212,0.08);top:40%;left:50%;animation-delay:1.5s;}
@keyframes orbFloat{0%{transform:translate(0,0) scale(1);}100%{transform:translate(30px,20px) scale(1.1);}}

.stars{position:fixed;inset:0;pointer-events:none;z-index:0;}
.star{position:absolute;width:2px;height:2px;background:#fff;border-radius:50%;
  animation:twinkle var(--d) ease-in-out infinite;opacity:0;animation-delay:var(--dl);}
@keyframes twinkle{0%,100%{opacity:0;}50%{opacity:var(--op);}}

/* ── CONTAINER ── */
.container{max-width:460px;margin:0 auto;padding:20px 16px 80px;position:relative;z-index:1;}

/* ── SCREENS ── */
.screen{display:none;}
.screen.active{display:block;}
#s-input{display:block;}

@keyframes cardUp{from{opacity:0;transform:translateY(22px) scale(0.98);}to{opacity:1;transform:translateY(0) scale(1);}}
.screen.active > *{opacity:0;animation:cardUp 0.65s cubic-bezier(0.16,1,0.3,1) forwards;}
.screen.active > *:nth-child(1){animation-delay:.04s}
.screen.active > *:nth-child(2){animation-delay:.11s}
.screen.active > *:nth-child(3){animation-delay:.18s}
.screen.active > *:nth-child(4){animation-delay:.25s}
.screen.active > *:nth-child(5){animation-delay:.32s}
.screen.active > *:nth-child(6){animation-delay:.39s}
.screen.active > *:nth-child(7){animation-delay:.46s}

/* ── GLITCH OVERLAY ── */
#glitch-overlay{
  position:fixed;inset:0;z-index:9000;
  background:#07070f;
  display:flex;flex-direction:column;align-items:center;justify-content:center;gap:24px;
  opacity:0;pointer-events:none;transition:opacity 0.15s;
}
#glitch-overlay.on{opacity:1;pointer-events:all;}

.glitch-scanline{
  position:absolute;inset:0;pointer-events:none;
  background:repeating-linear-gradient(0deg,rgba(0,245,212,0.03) 0px,rgba(0,245,212,0.03) 1px,transparent 1px,transparent 4px);
}

.glitch-word{
  font-size:20px;font-weight:900;letter-spacing:6px;color:#fff;
  font-family:monospace;text-transform:uppercase;position:relative;
}
.glitch-word::before{content:attr(data-t);position:absolute;left:0;top:0;color:#f72585;
  clip-path:polygon(0 30%,100% 30%,100% 50%,0 50%);animation:gc1 0.2s infinite;}
.glitch-word::after{content:attr(data-t);position:absolute;left:0;top:0;color:#00f5d4;
  clip-path:polygon(0 60%,100% 60%,100% 75%,0 75%);animation:gc2 0.2s infinite;}
@keyframes gc1{0%{transform:translate(-4px,0);}50%{transform:translate(4px,0);}100%{transform:translate(-2px,0);}}
@keyframes gc2{0%{transform:translate(4px,0);}50%{transform:translate(-4px,0);}100%{transform:translate(2px,0);}}

.glitch-bar-line{
  width:200px;height:2px;
  background:linear-gradient(90deg,transparent,var(--cyan),transparent);
  animation:barScan 0.8s ease-in-out infinite;
}
@keyframes barScan{0%{opacity:0;transform:scaleX(0);}50%{opacity:1;transform:scaleX(1);}100%{opacity:0;transform:scaleX(0);}}

/* ── HEADER ── */
.header{text-align:center;padding:36px 0 28px;}
.h-icon{font-size:68px;display:block;animation:float 3s ease-in-out infinite;
  filter:drop-shadow(0 0 24px rgba(168,85,247,0.7));}
@keyframes float{0%,100%{transform:translateY(0) rotate(-3deg);}50%{transform:translateY(-14px) rotate(3deg);}}
.h-title{font-size:30px;font-weight:900;margin:10px 0 6px;
  background:linear-gradient(135deg,var(--purple),var(--pink),var(--cyan));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.h-sub{color:var(--muted);font-size:13px;line-height:1.6;}

/* ── WARNING BAR ── */
.warn-bar{
  background:linear-gradient(135deg,rgba(255,77,109,0.12),rgba(168,85,247,0.12));
  border:1px solid rgba(255,77,109,0.25);
  border-radius:12px;padding:12px 16px;
  font-size:12px;color:#ff8fa3;text-align:center;
  margin-bottom:20px;line-height:1.5;
}

/* ── INPUT CARD ── */
.input-card{
  background:linear-gradient(160deg,rgba(255,255,255,0.04),var(--card) 40%);
  border:1px solid var(--border);backdrop-filter:blur(16px);
  border-radius:24px;padding:28px 22px;margin-bottom:16px;
  box-shadow:0 8px 32px rgba(0,0,0,0.4),0 0 0 1px rgba(168,85,247,0.06) inset;
}
.lbl{font-size:11px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:1.5px;margin-bottom:10px;}

input[type=text],select{
  width:100%;background:rgba(255,255,255,0.05);
  border:1px solid var(--border);border-radius:14px;
  padding:16px 18px;color:var(--text);font-size:16px;
  font-family:'Poppins',sans-serif;outline:none;
  transition:all 0.3s;margin-bottom:18px;appearance:none;
}
input[type=text]:focus,select:focus{
  border-color:var(--purple);background:rgba(168,85,247,0.08);
  box-shadow:0 0 0 3px rgba(168,85,247,0.15);
}
input::placeholder{color:var(--muted);}
select option{background:#12121f;color:var(--text);}

.reveal-btn{
  width:100%;padding:18px;border:none;border-radius:16px;
  background:linear-gradient(135deg,var(--purple),var(--pink));
  background-size:160% 160%;
  color:#fff;font-family:'Poppins',sans-serif;font-weight:800;
  font-size:16px;cursor:pointer;letter-spacing:0.5px;
  box-shadow:0 8px 30px rgba(168,85,247,0.4);
  transition:all 0.35s cubic-bezier(0.16,1,0.3,1);position:relative;overflow:hidden;
  animation:btnGlow 3s ease-in-out infinite;
}
@keyframes btnGlow{
  0%,100%{box-shadow:0 8px 30px rgba(168,85,247,0.4);background-position:0% 50%;}
  50%{box-shadow:0 10px 42px rgba(247,37,133,0.55);background-position:100% 50%;}
}
.reveal-btn::after{
  content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(255,255,255,0.15),transparent);
  opacity:0;transition:opacity 0.3s;
}
.reveal-btn:hover{transform:translateY(-3px);box-shadow:0 14px 40px rgba(168,85,247,0.6);}
.reveal-btn:hover::after{opacity:1;}
.reveal-btn:active{transform:translateY(0);}

/* ── LOADING SCREEN ── */
#s-loading{text-align:center;padding:70px 20px 40px;}
.load-icon{font-size:76px;display:block;margin:0 auto 20px;
  animation:spinPulse 2s ease-in-out infinite;}
@keyframes spinPulse{
  0%{transform:rotate(0) scale(1);filter:hue-rotate(0deg);}
  50%{transform:rotate(180deg) scale(1.15);filter:hue-rotate(180deg);}
  100%{transform:rotate(360deg) scale(1);filter:hue-rotate(360deg);}
}
.load-name{font-size:20px;font-weight:800;margin-bottom:6px;
  background:linear-gradient(135deg,var(--purple),var(--pink));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.load-sub{color:var(--muted);font-size:13px;margin-bottom:32px;}
.steps{list-style:none;text-align:left;max-width:280px;margin:0 auto;}
.step{
  display:flex;align-items:center;gap:12px;
  padding:10px 0;border-bottom:1px solid var(--border);
  color:var(--muted);font-size:13px;
  opacity:0;transform:translateX(-16px);transition:all 0.5s;
}
.step.done{opacity:1;transform:translateX(0);color:var(--cyan);}
.step-ic{font-size:18px;}

/* ── SCREEN HEADER ── */
.sc-header{text-align:center;padding:24px 0 18px;}
.sc-tag{font-size:10px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:2px;margin-bottom:8px;}
.sc-name{font-size:30px;font-weight:900;
  background:linear-gradient(135deg,var(--purple),var(--pink));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}

/* ── FACE CARD ── */
.face-card{
  background:linear-gradient(160deg,rgba(255,255,255,0.04),var(--card) 40%);
  border:1px solid var(--border);backdrop-filter:blur(16px);
  border-radius:24px;padding:28px 20px;margin-bottom:14px;
  box-shadow:0 8px 32px rgba(0,0,0,0.4);
  position:relative;overflow:hidden;
}
.face-card::before{
  content:'';position:absolute;top:0;left:0;right:0;height:2px;
  background:linear-gradient(90deg,var(--purple),var(--pink),var(--cyan));
  background-size:200% 100%;animation:barShift 4s linear infinite;
}
@keyframes barShift{0%{background-position:0% 0%;}100%{background-position:200% 0%;}}

/* ── DOMINANT EMOTION ── */
.dom-wrap{text-align:center;padding:8px 0 20px;}
.dom-eyebrow{
  display:inline-block;font-size:10px;font-weight:700;
  color:var(--muted);text-transform:uppercase;letter-spacing:2px;
  padding:4px 14px;border:1px solid var(--border);border-radius:99px;margin-bottom:20px;
}
.dom-emoji{
  font-size:100px;display:block;margin-bottom:4px;
  animation:domPulse 2.5s ease-in-out infinite;
  filter:drop-shadow(0 0 28px rgba(247,37,133,0.5));
}
@keyframes domPulse{
  0%,100%{transform:scale(1);filter:drop-shadow(0 0 20px rgba(247,37,133,0.4));}
  50%{transform:scale(1.08);filter:drop-shadow(0 0 50px rgba(247,37,133,1));}
}
.dom-name{
  font-size:36px;font-weight:900;text-transform:uppercase;letter-spacing:3px;
  background:linear-gradient(135deg,#fff 40%,var(--pink));
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
  margin-bottom:4px;
}
.dom-pct{
  font-size:56px;font-weight:900;color:var(--pink);line-height:1;
  text-shadow:0 0 30px rgba(247,37,133,0.8);margin-bottom:4px;
  animation:pctGlow 2s ease-in-out infinite alternate;
}
@keyframes pctGlow{
  0%{text-shadow:0 0 20px rgba(247,37,133,0.5);}
  100%{text-shadow:0 0 50px rgba(247,37,133,1),0 0 80px rgba(247,37,133,0.4);}
}
.dom-sub{font-size:11px;color:var(--muted);margin-bottom:24px;}

.others-divider{
  display:flex;align-items:center;gap:10px;margin-bottom:14px;
}
.others-divider::before,.others-divider::after{content:'';flex:1;height:1px;background:var(--border);}
.others-divider span{font-size:10px;color:var(--muted);font-weight:700;
  text-transform:uppercase;letter-spacing:1.5px;white-space:nowrap;}

.others-row{display:flex;gap:8px;}
.ochip{
  flex:1;background:rgba(255,255,255,0.03);border:1px solid var(--border);
  border-radius:14px;padding:12px 6px;text-align:center;
  opacity:0;transform:translateY(12px);transition:all 0.4s;
}
.ochip.show{opacity:1;transform:translateY(0);}
.ochip-em{font-size:22px;display:block;margin-bottom:4px;}
.ochip-nm{font-size:10px;color:var(--muted);font-weight:600;margin-bottom:3px;}
.ochip-pt{font-size:15px;font-weight:800;}
.pubface-total{font-size:10px;color:var(--cyan);text-align:center;margin-top:12px;
  font-weight:700;letter-spacing:0.5px;opacity:0;animation:fadeIn 0.6s 1.2s forwards;}
@keyframes fadeIn{to{opacity:0.85;}}

/* ── SHOCK BOX ── */
.shock-box{
  background:linear-gradient(160deg,rgba(255,255,255,0.04),var(--card) 40%);
  border:1px solid var(--border);backdrop-filter:blur(16px);
  border-radius:20px;padding:20px;margin-bottom:14px;
  position:relative;overflow:hidden;
  box-shadow:0 8px 32px rgba(0,0,0,0.4);
}
.shock-box::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;
  background:linear-gradient(90deg,var(--cyan),var(--purple),var(--pink));}
.shock-label{font-size:10px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:2px;text-align:center;margin-bottom:14px;}
.shock-nums{display:flex;border-radius:14px;overflow:hidden;border:1px solid var(--border);margin-bottom:14px;}
.shock-side{flex:1;padding:16px 10px;text-align:center;}
.shock-side.left{background:rgba(0,245,212,0.06);border-right:1px solid var(--border);}
.shock-side.right{background:rgba(247,37,133,0.1);position:relative;}
.shock-side.right::after{
  content:'';position:absolute;inset:0;
  background:rgba(247,37,133,0.06);
  animation:rGlow 2s ease-in-out infinite alternate;
}
@keyframes rGlow{0%{opacity:0.3;}100%{opacity:1;}}
.shock-n{font-size:48px;font-weight:900;line-height:1;position:relative;z-index:1;}
.shock-n.pub{color:var(--cyan);text-shadow:0 0 20px rgba(0,245,212,0.4);}
.shock-n.real{
  color:var(--pink);
  animation:realPulse 1.8s ease-in-out infinite;
}
@keyframes realPulse{
  0%,100%{text-shadow:0 0 20px rgba(247,37,133,0.5);}
  50%{text-shadow:0 0 50px rgba(247,37,133,1),0 0 80px rgba(247,37,133,0.3);}
}
.shock-side-lbl{font-size:10px;color:var(--muted);margin-top:5px;
  text-transform:uppercase;letter-spacing:1px;line-height:1.4;position:relative;z-index:1;}
.shock-side.right .shock-side-lbl{color:#ff8fa3;font-weight:700;}
.shock-badge{
  display:inline-block;background:var(--pink);color:#fff;
  font-size:9px;font-weight:800;padding:2px 8px;border-radius:99px;
  letter-spacing:1px;margin-top:5px;position:relative;z-index:1;
}
.shock-line{
  font-size:13px;color:var(--text);line-height:1.7;font-weight:500;
  padding-top:14px;border-top:1px solid var(--border);text-align:center;
}
.shock-line b{
  color:var(--pink);font-weight:800;
  background:rgba(247,37,133,0.1);padding:1px 6px;border-radius:6px;
}

/* ── NEXT BTN ── */
.next-btn{
  width:100%;padding:17px;border:1px solid var(--purple);border-radius:16px;
  background:transparent;color:var(--purple);font-family:'Poppins',sans-serif;
  font-weight:800;font-size:15px;cursor:pointer;
  transition:all 0.35s;position:relative;overflow:hidden;margin-bottom:10px;
}
.next-btn::before{
  content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,var(--purple),var(--pink));
  transform:translateX(-100%);transition:transform 0.35s;z-index:0;
}
.next-btn:hover::before{transform:translateX(0);}
.next-btn:hover{color:#fff;border-color:transparent;}
.next-btn span{position:relative;z-index:1;}

/* ── REVEAL CARD ── */
.reveal-card{
  background:linear-gradient(160deg,rgba(255,255,255,0.04),var(--card) 40%);
  border:1px solid var(--border);backdrop-filter:blur(16px);
  border-radius:24px;padding:28px 20px;margin-bottom:14px;
  position:relative;overflow:hidden;
  box-shadow:0 8px 32px rgba(0,0,0,0.4);
}
.reveal-card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;
  background:linear-gradient(90deg,var(--purple),var(--pink),var(--cyan));
  background-size:200% 100%;animation:barShift 4s linear infinite;}

.hidden-wrap{text-align:center;padding:12px 0 20px;}
.hidden-eyebrow{font-size:10px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:2px;margin-bottom:16px;}
.hidden-em{
  font-size:80px;display:block;margin-bottom:10px;
  animation:hiddenPulse 2.2s ease-in-out infinite;
}
@keyframes hiddenPulse{
  0%,100%{filter:drop-shadow(0 0 16px rgba(247,37,133,0.4));}
  50%{filter:drop-shadow(0 0 40px rgba(247,37,133,1));}
}
.hidden-name{font-size:26px;font-weight:900;color:var(--pink);margin-bottom:6px;}
.hidden-sub{font-size:12px;color:var(--muted);line-height:1.6;}

/* ── QUADRANT GRID ── */
.quad-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px;}
.quad{
  border-radius:18px;padding:18px 14px;text-align:center;border:1px solid transparent;
  opacity:0;transform:scale(0.88);transition:all 0.5s cubic-bezier(0.16,1,0.3,1);
}
.quad.show{opacity:1;transform:scale(1);}
.quad.show:active{transform:scale(0.95);}
.quad.pub{background:rgba(0,245,212,0.07);border-color:rgba(0,245,212,0.2);}
.quad.real{background:rgba(247,37,133,0.08);border-color:rgba(247,37,133,0.25);}
.quad.dark{background:rgba(255,77,109,0.07);border-color:rgba(255,77,109,0.2);}
.quad.super{background:rgba(168,85,247,0.07);border-color:rgba(168,85,247,0.2);}
.q-lbl{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1.5px;margin-bottom:8px;opacity:0.7;}
.quad.pub .q-lbl{color:var(--cyan);}
.quad.real .q-lbl{color:var(--pink);}
.quad.dark .q-lbl{color:var(--red);}
.quad.super .q-lbl{color:var(--purple);}
.q-em{font-size:32px;display:block;margin-bottom:6px;}
.q-nm{font-size:13px;font-weight:700;margin-bottom:4px;}
.q-pt{font-size:24px;font-weight:900;}
.quad.pub .q-pt{color:var(--cyan);}
.quad.real .q-pt{color:var(--pink);}
.quad.dark .q-pt{color:var(--red);}
.quad.super .q-pt{color:var(--purple);}

/* ── VIRAL QUOTE ── */
.viral-quote{
  background:linear-gradient(135deg,rgba(168,85,247,0.08),rgba(247,37,133,0.08));
  border-left:3px solid var(--pink);border-radius:0 16px 16px 0;
  padding:18px 18px;margin-bottom:14px;
}
.viral-quote p{font-size:14px;line-height:1.75;color:var(--text);font-weight:500;}
.viral-quote p b{color:var(--pink);font-weight:800;}
.viral-footer{margin-top:10px;font-size:11px;color:var(--muted);font-style:italic;}

/* ── WARN BOX ── */
.warn-box{
  background:rgba(255,77,109,0.07);border:1px solid rgba(255,77,109,0.2);
  border-radius:16px;padding:16px;margin-bottom:14px;text-align:center;
}
.warn-box-title{font-size:11px;font-weight:700;color:var(--red);
  text-transform:uppercase;letter-spacing:1.5px;margin-bottom:8px;}
.warn-box-text{font-size:13px;color:var(--text);line-height:1.65;}
.warn-box-text b{color:var(--gold);font-weight:800;}

/* ── SHARE SECTION ── */
.share-section{margin-top:6px;}
.share-title{
  font-size:10px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:2px;text-align:center;margin-bottom:10px;
}
.ss-status{
  display:flex;align-items:center;justify-content:center;gap:6px;
  font-size:11px;color:var(--muted);text-align:center;margin-bottom:12px;
  padding:7px 10px;border-radius:99px;background:rgba(255,255,255,0.04);
  border:1px solid var(--border);transition:all 0.4s;
}
.ss-status .dot{width:6px;height:6px;border-radius:50%;background:var(--gold);
  animation:dotPulse 1s ease-in-out infinite;}
@keyframes dotPulse{0%,100%{opacity:0.4;}50%{opacity:1;}}
.ss-status.ready{color:var(--cyan);border-color:rgba(0,245,212,0.3);background:rgba(0,245,212,0.06);}
.ss-status.ready .dot{background:var(--cyan);animation:none;opacity:1;}

.share-main-btn{
  width:100%;padding:17px;border:none;border-radius:16px;margin-bottom:12px;
  background:linear-gradient(135deg,var(--cyan),var(--purple),var(--pink));
  background-size:200% 200%;animation:btnGlow2 3.5s ease-in-out infinite;
  color:#07070f;font-family:'Poppins',sans-serif;font-weight:800;
  font-size:15px;cursor:pointer;letter-spacing:0.3px;
  box-shadow:0 8px 28px rgba(0,245,212,0.3);
  transition:all 0.3s cubic-bezier(0.16,1,0.3,1);
}
.share-main-btn:hover{transform:translateY(-2px);box-shadow:0 12px 36px rgba(0,245,212,0.5);}
.share-main-btn:active{transform:translateY(0) scale(0.98);}
@keyframes btnGlow2{0%,100%{background-position:0% 50%;}50%{background-position:100% 50%;}}

/* Screenshot card — hidden, only for capture */
#screenshot-target{
  position:fixed;top:-9999px;left:-9999px;
  width:380px;background:#07070f;padding:28px 22px;
  font-family:'Poppins',sans-serif;color:#f0f0ff;
  border-radius:24px;border:1px solid #1a1a2e;
}

.ss-header{text-align:center;margin-bottom:20px;}
.ss-icon{font-size:40px;display:block;}
.ss-title{font-size:18px;font-weight:900;
  background:linear-gradient(135deg,#a855f7,#f72585);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.ss-name{font-size:24px;font-weight:900;color:#a855f7;margin-top:4px;}

.ss-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin:16px 0;}
.ss-quad{border-radius:14px;padding:14px;text-align:center;border:1px solid #1a1a2e;}
.ss-quad.pub{background:rgba(0,245,212,0.1);}
.ss-quad.real{background:rgba(247,37,133,0.1);}
.ss-quad.dark{background:rgba(255,77,109,0.1);}
.ss-quad.super{background:rgba(168,85,247,0