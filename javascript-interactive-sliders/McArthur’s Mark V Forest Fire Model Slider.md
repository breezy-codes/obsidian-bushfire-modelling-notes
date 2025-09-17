---
aliases:
tags:
---


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
  "Mild": {T:20, H:50, U:10, DF:5},
  "Hot Dry": {T:35, H:20, U:20, DF:8},
  "Severe": {T:40, H:15, U:40, DF:9},
  "Extreme": {T:45, H:10, U:60, DF:10},
};

// ---------- controls ----------
const presetSel = makeSelect("Scenario Preset","preset",
  Object.keys(presets).map(k=>({label:k,value:k})),
  "Mild"
);

const T = makeSlider("Air Temperature (°C)","T",0,50,25,0.5,"");
const H = makeSlider("Relative Humidity (%)","H",0,100,40,1,"");
const U = makeSlider("Wind Speed (km/h)","U",0,150,20,1,"");
const DF = makeSlider("Drought Factor (0–10)","DF",0,10,5,0.1,"");

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

const ffdiOut = addOut("Forest Fire Danger Index (FFDI)");
const rosOut = addOut("Rate of Spread (ROS, m/h)");

// ---------- result box ----------
const resultBox=document.createElement("div");
resultBox.style.marginTop="12px"; resultBox.style.padding="12px 14px";
resultBox.style.borderTop="2px solid var(--text-muted, #999)";
resultBox.style.background="var(--background-secondary, #1e1e1e10)";
const resultTitle=document.createElement("div");
resultTitle.style.fontSize="0.95em"; resultTitle.style.fontWeight="700";
resultTitle.textContent="Resultant McArthur Mark V Forest Fire Model Output";
const resultValue=document.createElement("div");
resultValue.setAttribute("aria-live","polite");
resultValue.style.fontFamily="var(--font-monospace)";
resultValue.style.fontWeight="800"; resultValue.style.fontSize="1.6em";
resultValue.style.marginTop="4px"; resultValue.textContent="— m/h";
resultBox.append(resultTitle,resultValue); container.appendChild(resultBox);

const note=document.createElement("div");
note.style.marginTop="8px"; note.style.fontSize="0.92em"; note.style.opacity="0.85";
container.appendChild(note);

// ---------- model ----------
function computeFFDI(T,H,U,DF){ 
  return 2 * Math.exp(-0.45 + 0.987*Math.log(DF) - 0.0345*H + 0.0338*T + 0.0234*U); 
}
function computeROS(ffdi,U){ 
  return 0.0012 * ffdi * Math.exp(0.069*U); 
}

// ---------- recompute ----------
function update(fromPreset=false){
  if(fromPreset){
    const p=presets[presetSel.value];
    T.input.value=p.T;
    H.input.value=p.H;
    U.input.value=p.U;
    DF.input.value=p.DF;
  }

  const t=+T.input.value, h=+H.input.value, u=+U.input.value, df=+DF.input.value;

  T.val.textContent=`${t}`; 
  H.val.textContent=`${h}`; 
  U.val.textContent=`${u}`; 
  DF.val.textContent=`${df.toFixed(1)}`;

  const ffdi = (df>0) ? computeFFDI(t,h,u,df) : 0;
  const ros = computeROS(ffdi,u);

  ffdiOut.textContent=fmt(ffdi);
  rosOut.textContent=fmt(ros);
  resultValue.textContent = `${fmt(ros)} m/h`;

  // classification of ROS by danger
  let cat="";
  if(ros<100) cat="Low spread";
  else if(ros<500) cat="Moderate spread";
  else if(ros<1500) cat="High spread";
  else cat="Extreme spread";
  note.textContent=`Category: ${cat}`;
}

presetSel.addEventListener("input",()=>update(true));
[T.input,H.input,U.input,DF.input].forEach(el=>el.addEventListener("input",update));
update(true);

```
