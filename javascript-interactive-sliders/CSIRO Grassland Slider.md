

```dataviewjs
const container = this.container;

// ---------- preset selector ----------
const presets = {
  average:    { U: 10, C: 50, T: 25, H: 50, m: 12, pasture:"cg" },
  windy:      { U: 40, C: 60, T: 22, H: 40, m: 10, pasture:"cg" },
  hotwind:    { U: 35, C: 70, T: 38, H: 25, m: 8,  pasture:"n" },
  high:       { U: 20, C: 70, T: 30, H: 35, m: 9,  pasture:"n" },       // High danger
  extreme:    { U: 40, C: 80, T: 37, H: 22, m: 7,  pasture:"n" },       // Extreme danger
  catastrophic:{U: 60, C: 95, T: 42, H: 12, m: 5,  pasture:"n" }        // Catastrophic danger
};

const presetSel = makeSelect("Scenario Preset","preset",
  [
    {label:"Custom", value:""},
    {label:"Average Weather", value:"average"},
    {label:"Windy Day", value:"windy"},
    {label:"Hot & Windy Day", value:"hotwind"},
    {label:"High Fire Danger (AUS)", value:"high"},
    {label:"Extreme Fire Danger (AUS)", value:"extreme"},
    {label:"Catastrophic (AUS)", value:"catastrophic"}
  ],"");

// when a preset is chosen, set slider/checkbox/select values
presetSel.addEventListener("input",()=>{
  const key = presetSel.value;
  if(!key) return; // custom
  const p = presets[key];
  U.input.value = p.U; U.val.textContent=p.U;
  C.input.value = p.C; C.val.textContent=p.C;
  T.input.value = p.T; T.val.textContent=p.T;
  H.input.value = p.H; H.val.textContent=p.H;
  mSlider.input.value = p.m; mSlider.val.textContent=`${p.m}`;
  pastureSel.value = p.pasture;
  moistureOverride.checked = true;
  mSlider.input.parentElement.style.display="";
  update();
});


// ---------- helpers ----------
function makeRow(){ 
  const d=document.createElement("div"); 
  d.style.display="grid";
  d.style.gridTemplateColumns="180px 1fr 60px"; // label | slider | value
  d.style.alignItems="center";
  d.style.margin="6px 0"; 
  d.style.columnGap="8px";
  return d; 
}

function makeSlider(label,id,min,max,value,step=1,unit=""){ 
  const row=makeRow(); 
  const lbl=document.createElement("label"); 
  lbl.htmlFor=id; lbl.textContent=`${label}`; 

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
  const dummy=document.createElement("span"); dummy.textContent=""; // placeholder for third column
  row.append(lbl,sel,dummy); 
  container.appendChild(row); 
  return sel; 
}
function makeCheckbox(label,id,checked=false){ 
  const row=document.createElement("div"); row.style.margin="6px 0"; 
  const cb=document.createElement("input"); cb.type="checkbox"; cb.id=id; cb.checked=checked; 
  const lbl=document.createElement("label"); lbl.htmlFor=id; lbl.textContent=" "+label; 
  row.append(cb,lbl); 
  container.appendChild(row); 
  return cb; 
}
function clamp(x,a,b){ return Math.max(a,Math.min(b,x)); }
function fmt(x){ return Number.isFinite(x)? x.toFixed(2): "—"; }

// ---------- controls ----------
const pastureSel = makeSelect("Pasture Type","pasture",[ {label:"Cut/Grazed (cg)", value:"cg"}, {label:"Natural (n = 1.18 × cg)", value:"n"} ],"cg");
const U = makeSlider("Wind Speed (km/h)","U",0,100,10,1,"");
const C = makeSlider("Curing (%)","C",0,100,50,1,"");
const T = makeSlider("Air Temperature (°C)","T",0,50,25,0.5,"");
const H = makeSlider("Relative Humidity (%)","H",0,100,50,1,"");
const moistureOverride = makeCheckbox("Override fuel moisture (m) manually","m_override",false);
const mSlider = makeSlider("Fuel Moisture (%)","m",0,40,12,0.1,"");

// hide until override ticked
mSlider.input.parentElement.style.display="none";
moistureOverride.addEventListener("input",()=>{ 
  mSlider.input.parentElement.style.display = moistureOverride.checked ? "" : "none"; 
});

// ---------- outputs & results ----------
container.appendChild(document.createElement("hr"));
const grid=document.createElement("div"); 
grid.style.display="grid"; 
grid.style.gridTemplateColumns="max-content 1fr"; 
grid.style.rowGap="4px"; grid.style.columnGap="12px"; 
container.appendChild(grid);

function addOut(label){ const k=document.createElement("div"); k.style.fontWeight="600"; k.textContent=label; const v=document.createElement("div"); v.textContent="—"; grid.append(k,v); return v; }
const mOut=addOut("Fuel moisture m (%)"); const extOut=addOut("Extinction moisture (%)"); const phiMOut=addOut("ϕ_m (fuel-moisture coeff)"); const phiCOut=addOut("ϕ_C (curing coeff)"); const fU_cg_Out=addOut("f(U) [cg]"); const R_cg_Out=addOut("R (cut/grazed) km/h"); const R_n_Out=addOut("R (natural) km/h");

const resultBox=document.createElement("div");
resultBox.style.marginTop="12px"; resultBox.style.padding="12px 14px"; resultBox.style.borderTop="2px solid var(--text-muted, #999)"; resultBox.style.background="var(--background-secondary, #1e1e1e10)";
const resultTitle=document.createElement("div"); resultTitle.style.fontSize="0.95em"; resultTitle.style.fontWeight="700"; resultTitle.textContent="Resultant rate of spread R (selected pasture)";
const resultValue=document.createElement("div"); resultValue.setAttribute("aria-live","polite"); resultValue.style.fontFamily="var(--font-monospace)"; resultValue.style.fontWeight="800"; resultValue.style.fontSize="1.6em"; resultValue.style.marginTop="4px"; resultValue.textContent="— km/h";
resultBox.append(resultTitle,resultValue); container.appendChild(resultBox);

const note=document.createElement("div"); note.style.marginTop="8px"; note.style.fontSize="0.92em"; note.style.opacity="0.85"; container.appendChild(note);

// ---------- model pieces ----------
function fuelMoistureFromTH(T,H){ return 9.58 - 0.205*T + 0.138*H; }
function phi_C(C){ return 1.036 / (1 + 103.99 * Math.exp(-0.0996 * (C - 20))); }
function phi_m(m,U){ if(m<=12) return Math.exp(-0.108*m); if(U<=10){ const v=0.684 - 0.0342*m; return Math.max(0,v);} else { const v=0.547 - 0.0228*m; return Math.max(0,v);} }
function extinctionMoisture(U){ return U<=10 ? 20 : 24; }
function fU_cg(U){ return U<=5 ? (0.054 + 0.209*U) : (1.1 + 0.715*Math.pow(U-5,0.844)); }

// ---------- recompute ----------
function update(){
  const u=+U.input.value, cc=+C.input.value, t=+T.input.value, h=+H.input.value;
  U.val.textContent=`${u}`; C.val.textContent=`${cc}`; T.val.textContent=`${t}`; H.val.textContent=`${h}`;

  let mCalc=clamp(fuelMoistureFromTH(t,h),0,100); let m=moistureOverride.checked ? +mSlider.input.value : mCalc; if(moistureOverride.checked) mSlider.val.textContent=`${fmt(m)}`;

  const phiC=phi_C(cc); const phiM=phi_m(m,u); const fUcg=fU_cg(u);
  const R_cg=fUcg*phiM*phiC; const R_n=1.18*R_cg; const R_active=(pastureSel.value==="cg")?R_cg:R_n;

  mOut.textContent=fmt(m); extOut.textContent=`${extinctionMoisture(u)}`; phiMOut.textContent=fmt(phiM); phiCOut.textContent=fmt(phiC); fU_cg_Out.textContent=fmt(fUcg); R_cg_Out.textContent=fmt(R_cg); R_n_Out.textContent=fmt(R_n);

  resultValue.textContent = Number.isFinite(R_active) ? `${fmt(R_active)} km/h` : "— km/h";

  const belowCuring = cc<20; const nearExt = m >= extinctionMoisture(u) - 1e-9;
  const parts=[]; if(belowCuring) parts.push("Curing < 20%: fire spread typically fails."); if(nearExt) parts.push("Fuel moisture at/above extinction: ϕ_m→0, spread unlikely."); parts.push(`Pasture: ${pastureSel.value==="cg"?"Cut/Grazed":"Natural (1.18×cg)"}.`);
  note.textContent = parts.join(" ");
}
[pastureSel,U.input,C.input,T.input,H.input,moistureOverride,mSlider.input].forEach(el=>el.addEventListener("input",update));
update();

```
