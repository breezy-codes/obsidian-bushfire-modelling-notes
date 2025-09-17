
```dataviewjs
const container = this.container;

// ---------- helpers ----------
function makeRow(){ 
  const d=document.createElement("div"); 
  d.style.display="grid";
  d.style.gridTemplateColumns="180px 1fr 60px"; 
  d.style.alignItems="center";
  d.style.margin="6px 0"; 
  d.style.columnGap="8px";
  return d; 
}
function makeSlider(label,id,min,max,value,step=1,unit=""){ 
  const row=makeRow(); 
  const lbl=document.createElement("label"); lbl.htmlFor=id; lbl.textContent=`${label}`; 

  const input=document.createElement("input"); 
  input.type="range"; input.min=min; input.max=max; 
  input.step=step; input.value=value; input.id=id; 
  input.style.width="100%";

  const val=document.createElement("span"); 
  val.id=id+"_val"; val.style.textAlign="right";
  val.textContent=`${value}${unit}`; 

  row.append(lbl,input,val); 
  container.appendChild(row); 
  return {input,val,unit}; 
}
function makeSelect(label,id,options,selected){ 
  const row=makeRow(); 
  const lbl=document.createElement("label"); lbl.htmlFor=id; lbl.textContent=label; 
  const sel=document.createElement("select"); sel.id=id; 
  options.forEach(o=>{ const opt=document.createElement("option"); opt.value=o.value; opt.textContent=o.label; sel.appendChild(opt); }); 
  sel.value=selected; 
  const dummy=document.createElement("span"); dummy.textContent=""; 
  row.append(lbl,sel,dummy); 
  container.appendChild(row); 
  return sel; 
}
function fmt(x){ return Number.isFinite(x)? x.toFixed(2): "—"; }

// ---------- presets ----------
const presets = {
  "Grassfire Mild": {H:18000, w:0.5, R:0.02},     // ROS=0.02 m/s
  "Grassfire Extreme": {H:18000, w:0.8, R:0.10},  // ROS=0.1 m/s
  "Forest Moderate": {H:20000, w:2.0, R:0.01},
  "Forest Severe": {H:20000, w:3.5, R:0.03},
};

// ---------- controls ----------
const presetSel = makeSelect("Scenario Preset","preset",
  Object.keys(presets).map(k=>({label:k,value:k})),
  "Grassfire Mild"
);

const H = makeSlider("Heat of combustion H (kJ/kg)","H",10000,25000,18000,100,"");
const w = makeSlider("Fuel load consumed w (kg/m²)","w",0,5,0.5,0.1,"");
const R = makeSlider("Rate of spread R (m/s)","R",0,0.2,0.02,0.005,"");

// ---------- outputs ----------
container.appendChild(document.createElement("hr"));
const grid=document.createElement("div"); 
grid.style.display="grid"; 
grid.style.gridTemplateColumns="max-content 1fr"; 
grid.style.rowGap="4px"; grid.style.columnGap="12px"; 
container.appendChild(grid);

function addOut(label){ 
  const k=document.createElement("div"); 
  k.style.fontWeight="600"; k.textContent=label; 
  const v=document.createElement("div"); v.textContent="—"; 
  grid.append(k,v); 
  return v; 
}

const Iout = addOut("Byram’s Flame Intensity I (kW/m)");

// ---------- result box ----------
const resultBox=document.createElement("div");
resultBox.style.marginTop="12px"; resultBox.style.padding="12px 14px";
resultBox.style.borderTop="2px solid var(--text-muted, #999)";
resultBox.style.background="var(--background-secondary, #1e1e1e10)";
const resultTitle=document.createElement("div");
resultTitle.style.fontSize="0.95em"; resultTitle.style.fontWeight="700";
resultTitle.textContent="Resultant Byram Flame Intensity";
const resultValue=document.createElement("div");
resultValue.setAttribute("aria-live","polite");
resultValue.style.fontFamily="var(--font-monospace)";
resultValue.style.fontWeight="800"; resultValue.style.fontSize="1.6em";
resultValue.style.marginTop="4px"; resultValue.textContent="— kW/m";
resultBox.append(resultTitle,resultValue); container.appendChild(resultBox);

const note=document.createElement("div");
note.style.marginTop="8px"; note.style.fontSize="0.92em"; note.style.opacity="0.85";
container.appendChild(note);

// ---------- model ----------
function computeByram(H,w,R){ 
  return H * w * R / 1000; // convert kJ/s to kW/m
}

// ---------- recompute ----------
function update(fromPreset=false){
  if(fromPreset){
    const p=presets[presetSel.value];
    H.input.value=p.H;
    w.input.value=p.w;
    R.input.value=p.R;
  }

  const h=+H.input.value, wf=+w.input.value, r=+R.input.value;

  H.val.textContent=`${h}`; 
  w.val.textContent=`${wf}`; 
  R.val.textContent=`${r.toFixed(3)}`;

  const I=computeByram(h,wf,r);

  Iout.textContent=fmt(I);
  resultValue.textContent = `${fmt(I)} kW/m`;

  // rough classification thresholds
  let cat="";
  if(I<500) cat="Low intensity";
  else if(I<2000) cat="Moderate flames";
  else if(I<4000) cat="High intensity";
  else cat="Extreme flames";
  note.textContent=`Category: ${cat}`;
}

presetSel.addEventListener("input",()=>update(true));
[H.input,w.input,R.input].forEach(el=>el.addEventListener("input",update));
update(true);

```
