---
layout: default
---

![Banner](assets/biscuit.png)


**[Biscuit](http://sblisesivdin.github.io/biscuit)** is a single-page responsive Jekyll theme. This is the simplest and still-good-looking Jekyll theme that you can find. 

## About me

{% raw %}
<style>
.tsne-wrap { position: relative; max-width: 700px; margin: 0 auto; }
.tsne-wrap h1 {
  font-family: monospace; font-weight: 300; font-size: 11px;
  letter-spacing: 0.1em; text-transform: uppercase; color: #bbb; margin-bottom: 1rem;
}
.tsne-tip {
  position: absolute; background: #fff; border: 0.5px solid rgba(0,0,0,0.12);
  border-radius: 6px; padding: 5px 10px; font-family: monospace; font-size: 11px;
  color: #1a1814; pointer-events: none; opacity: 0; transition: opacity 0.1s; white-space: nowrap; z-index: 10;
}
</style>

<div class="tsne-wrap">
  <h1>Research interests · latent space</h1>
  <div style="position:relative">
    <canvas id="tsne-c" style="display:block;width:100%;"></canvas>
    <div class="tsne-tip" id="tsne-tip"></div>
  </div>
</div>

<script>
/* ✏️ Edit your points here */
const POINTS = [
  { label: "Semantic shift",      x: 0.20, y: 0.25, color: "#7F77DD" },
  { label: "Neologisms",          x: 0.72, y: 0.18, color: "#1D9E75" },
  { label: "Polysemy",            x: 0.55, y: 0.60, color: "#D85A30" },
  { label: "Conceptual metaphor", x: 0.18, y: 0.70, color: "#C4527A" },
  { label: "Speech acts",         x: 0.78, y: 0.72, color: "#E5A320" },
];
const NOISE = 120;

const canvas = document.getElementById('tsne-c');
const tip    = document.getElementById('tsne-tip');
let W, H, dpr, hovered = null;

function seededRand(s) { return () => { s = (s*16807)%2147483647; return (s-1)/2147483646; }; }
const rand  = seededRand(42);
const noise = Array.from({length: NOISE}, () => ({x: 0.05+rand()*0.9, y: 0.05+rand()*0.9}));

function xy(p) { const pad=40; return {cx: pad+p.x*(W-pad*2), cy: pad+p.y*(H-pad*2)}; }

function draw() {
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0,0,canvas.width,canvas.height);
  ctx.save(); ctx.scale(dpr,dpr);
  ctx.strokeStyle='rgba(0,0,0,0.05)'; ctx.lineWidth=0.5;
  for(let i=0;i<=4;i++){
    const x=40+(i/4)*(W-80), y=40+(i/4)*(H-80);
    ctx.beginPath(); ctx.moveTo(x,40); ctx.lineTo(x,H-40); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(40,y); ctx.lineTo(W-40,y); ctx.stroke();
  }
  noise.forEach(p=>{
    const {cx,cy}=xy(p);
    ctx.beginPath(); ctx.arc(cx,cy,3,0,Math.PI*2);
    ctx.fillStyle='rgba(0,0,0,0.08)'; ctx.fill();
  });
  POINTS.forEach((p,i)=>{
    const {cx,cy}=xy(p); const isHov=hovered===i;
    if(isHov){ctx.beginPath();ctx.arc(cx,cy,16,0,Math.PI*2);ctx.fillStyle=p.color+'18';ctx.fill();}
    ctx.beginPath(); ctx.arc(cx,cy,isHov?6.5:5.5,0,Math.PI*2); ctx.fillStyle=p.color; ctx.fill();
    const r=cx>W*0.62; ctx.textAlign=r?'right':'left';
    ctx.font=`${isHov?'400':'300'} 11px monospace`; ctx.fillStyle=isHov?p.color:p.color+'cc';
    ctx.fillText(p.label, r?cx-11:cx+11, cy-9);
  });
  ctx.font='300 10px monospace'; ctx.fillStyle='rgba(0,0,0,0.18)'; ctx.textAlign='center';
  ctx.fillText('dimension 1',W/2,H-8);
  ctx.save(); ctx.translate(11,H/2); ctx.rotate(-Math.PI/2); ctx.fillText('dimension 2',0,0); ctx.restore();
  ctx.restore();
}

function resize(){
  W=canvas.parentElement.offsetWidth; H=Math.round(W*0.56);
  dpr=window.devicePixelRatio||1; canvas.width=W*dpr; canvas.height=H*dpr;
  canvas.style.height=H+'px'; draw();
}

canvas.addEventListener('mousemove', e=>{
  const rect=canvas.getBoundingClientRect(), mx=e.clientX-rect.left, my=e.clientY-rect.top;
  let found=null;
  POINTS.forEach((p,i)=>{ const {cx,cy}=xy(p); if(Math.hypot(mx-cx,my-cy)<12) found=i; });
  if(found!==hovered){hovered=found;draw();}
  if(found!==null){
    tip.style.opacity='1'; tip.style.left=(mx+14)+'px'; tip.style.top=(my-10)+'px';
    tip.textContent=POINTS[found].label;
  } else { tip.style.opacity='0'; }
});
canvas.addEventListener('mouseleave',()=>{hovered=null;tip.style.opacity='0';draw();});
window.addEventListener('resize',resize);
resize();
</script>
{% endraw %}
If you prefer to use GitHub Pages, you do not need to download it, upload files to a new repository, etc., just [fork](https://docs.github.com/en/get-starter/quickstart/fork-a-repo) and use it.


### Publications

* M. Sternik, R. Laarman-Quante, A. Drackert. Using k-Shot Prompting with Large k for the Automated Scoring of a German Written Elicited Imitation TEst.
German Written Elicited Imitation Test
* `index.md`               : Website page (for now, this page).
* `_includes/head.html`    : File to add custom code to `<head>` section.
* `_includes/scripts.html` : File to add custom code before `</body>`. You can change footer at here.
* `_sass` folder           : Related scss files can be found at this folder.
* `css/main.csss`          : Main scss file.
* `README.md`              : A simple readme file.

## CV

## Header 1
### Header 2
#### Header 3
**bold**
*italic*

> blockquotes

~~~python
import os,time
print ("Biscuit")
~~~

## Licence and Author Information

Biscuit is derived from the currently deprecated theme [Solo](http://github.com/chibicode/solo). The development of Biscuit is maintained by [Sefer Bora Lisesivdin](https://sblisesivdin.github.io).

Biscuit and the previous code, where Biscuit is derived, are distributed with [MIT license](https://github.com/sblisesivdin/biscuit/blob/gh-pages/LICENSE).
