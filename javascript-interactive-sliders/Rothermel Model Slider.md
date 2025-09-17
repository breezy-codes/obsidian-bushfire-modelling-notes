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
  const input=document.createElement("input"); input.type="range"; input.min=min; input.max=max; 
  input.step=step; input.value=value; input.id=id; input.style.width="100%";
  const val=document.createElement("span"); val.id=id+"_val"; val.style.textAlign="right"; val.textContent=`${value}${unit}`; 
  row.append(lbl,input,val); container.appendChild(row); 
  return {input,val,unit}; 
}
function makeSelect(label,id,options,selected){ 
  const row=makeRow(); 
  const lbl=document.createElement("label"); lbl.htmlFor=id; lbl.textContent=label; 
  const sel=document.createElement("select"); sel.id=id; 
  options.forEach(o=>{ const opt=document.createElement("option"); opt.value=o.value; opt.textContent=o.label; sel.appendChild(opt); }); 
  sel.value=selected; 
  const dummy=document.createElement("span"); dummy.textContent=""; 
  row.append(lbl,sel,dummy); container.appendChild(row); 
  return sel; 
}
function clamp(x,a,b){ return Math.max(a,Math.min(b,x)); }
function fmt(x){ return Number.isFinite(x)? x.toFixed(3): "—"; }

// ---------- fuel models ----------
const fuelModels = {
  "GR1": {w:0.2, rho_b:25, sigma:2200, h:18600}, // Short grass
  "GR2": {w:0.3, rho_b:30, sigma:2000, h:18600}, // Timber grass
  "GS1": {w:0.5, rho_b:35, sigma:1800, h:18600}, // Shrub
  "TL1": {w:1.5, rho_b:40, sigma:1200, h:20000}, // Light timber litter
  "TU1": {w:0.6, rho_b:35, sigma:1600, h:18600}, // Timber understory
};

// ---------- scenario presets ----------
const scenarioPresets = {
  average:     {U:2, theta:0, m:8},   // mild day
  windy:       {U:8, theta:5, m:10},  // strong wind, moderate slope
  hotwind:     {U:10, theta:10, m:7}, // hot windy with slope
  high:        {U:6, theta:5, m:9},   // AUS High FDR
  extreme:     {U:12, theta:10, m:7}, // AUS Extreme FDR
  catastrophic:{U:16, theta:15, m:5}  // AUS Catastrophic FDR
};

// ---------- controls ----------
const presetSel = makeSelect("Fuel Model","fuel_model",
  Object.keys(fuelModels).map(k=>({label:k,value:k})),
  "GR1"
);
const scenarioSel = makeSelect("Scenario Preset","scenario",
  [
    {label:"Custom", value:""},
    {label:"Average Weather", value:"average"},
    {label:"Windy Day", value:"windy"},
    {label:"Hot & Windy", value:"hotwind"},
    {label:"High Fire Danger (AUS)", value:"high"},
    {label:"Extreme Fire Danger (AUS)", value:"extreme"},
    {label:"Catastrophic (AUS)", value:"catastrophic"}
  ], ""
);

const fuelLoad = makeSlider("Fuel load w (kg/m²)","w",0,3,0.5,0.01,"");
const bulkDensity = makeSlider("Bulk density ρ_b (kg/m³)","r_b",0,50,30,0.5,"");
const surfaceAreaRatio = makeSlider("σ (1/m)","sigma",200,6000,3500,50,"");
const heatContent = makeSlider("Heat content h (kJ/kg)","h",5000,25000,18600,100,"");
const windSpeed = makeSlider("Wind speed U (m/s)","U",0,20,2,0.1,"");
const slope = makeSlider("Slope θ (degrees)","theta",0,45,0,1,"");
const moisture = makeSlider("Fuel moisture m (%)","m",0,30,8,0.1,"");

// ---------- outputs ----------
container.appendChild(document.createElement("hr"));
const grid=document.createElement("div"); 
grid.style.display="grid"; 
grid.style.gridTemplateColumns="max-content 1fr"; 
grid.style.rowGap="4px"; grid.style.columnGap="12px"; 
container.appendChild(grid);

function addOut(label){ const k=document.createElement("div"); k.style.fontWeight="600"; k.textContent=label; const v=document.createElement("div"); v.textContent="—"; grid.append(k,v); return v; }
const IRout=addOut("Reaction intensity I_R (kW/m²)");
const xiout=addOut("Propagating flux ratio ξ");
const epsout=addOut("Heat of preignition Q_ig (kJ/kg)");
const R0out=addOut("No-wind/no-slope ROS R₀ (m/min)");
const phiWout=addOut("Wind factor φ_w");
const phiSout=addOut("Slope factor φ_s");
const Rout=addOut("Rate of spread R (m/min)");

// ---------- result box ----------
const resultBox=document.createElement("div");
resultBox.style.marginTop="12px"; resultBox.style.padding="12px 14px";
resultBox.style.borderTop="2px solid var(--text-muted, #999)";
resultBox.style.background="var(--background-secondary, #1e1e1e10)";
const resultTitle=document.createElement("div");
resultTitle.style.fontSize="0.95em"; resultTitle.style.fontWeight="700";
resultTitle.textContent="Resultant Rothermel Rate of Spread";
const resultValue=document.createElement("div");
resultValue.setAttribute("aria-live","polite");
resultValue.style.fontFamily="var(--font-monospace)";
resultValue.style.fontWeight="800"; resultValue.style.fontSize="1.6em";
resultValue.style.marginTop="4px"; resultValue.textContent="— m/min";
resultBox.append(resultTitle,resultValue); container.appendChild(resultBox);

// ---------- model equations ----------
// ---------- model equations ----------
function Qig(m){ return 250 + 1116*m; } // kJ/kg, m fraction
function xi(sigma){ 
  return Math.exp((0.792 + 0.681*Math.sqrt(sigma))*(1+0.1*sigma)/(192+0.2595*sigma)); 
}
function etaM(m, Mx=0.3){
  const r = m/Mx;
  return clamp(1 - 2.59*r + 5.11*r*r - 3.52*r*r*r, 0, 1);
}
function IR(w,h,sigma,m){ 
  const eta_M = etaM(m);
  const eta_S = 0.174*Math.pow(sigma, -0.19); 
  const Gamma = (sigma**1.5)/(495 + 0.0594*sigma**1.5); 
  return w * h * eta_M * eta_S * Gamma / 1000; // kW/m²
}
function phiW(U,sigma){ 
  const C = 0.045, B = 2.0, E = 0.9;
  return C * Math.pow(U,B) * Math.pow(sigma/2000,E);
}
function phiS(theta){ 
  return 5.275 * Math.pow(Math.tan(theta*Math.PI/180),2); 
}

// ---------- recompute ----------
function update(fromFuelPreset=false, fromScenario=false){
  if(fromFuelPreset){
    const p=fuelModels[presetSel.value];
    fuelLoad.input.value=p.w;
    bulkDensity.input.value=p.rho_b;
    surfaceAreaRatio.input.value=p.sigma;
    heatContent.input.value=p.h;
  }
  if(fromScenario){
    const s=scenarioPresets[scenarioSel.value];
    if(s){
      windSpeed.input.value=s.U;
      slope.input.value=s.theta;
      moisture.input.value=s.m;
    }
  }
  const w=+fuelLoad.input.value, rho_b=+bulkDensity.input.value, sigma=+surfaceAreaRatio.input.value, h=+heatContent.input.value, U=+windSpeed.input.value, theta=+slope.input.value, m=+moisture.input.value/100;

  fuelLoad.val.textContent=`${w}`; bulkDensity.val.textContent=`${rho_b}`; surfaceAreaRatio.val.textContent=`${sigma}`; heatContent.val.textContent=`${h}`; windSpeed.val.textContent=`${U}`; slope.val.textContent=`${theta}`; moisture.val.textContent=`${(m*100).toFixed(1)}`;

  const qig=Qig(m);
  const Xi=xi(sigma);
  const IRval=IR(w,h,sigma,m);
  const R0=(IRval*Xi)/(rho_b*qig); // m/s
  const phi_w=phiW(U,sigma);
  const phi_s=phiS(theta);
  const R=R0*(1+phi_w+phi_s)*60; // m/min

  IRout.textContent=fmt(IRval);
  xiout.textContent=fmt(Xi);
  epsout.textContent=fmt(qig);
  R0out.textContent=fmt(R0*60);
  phiWout.textContent=fmt(phi_w);
  phiSout.textContent=fmt(phi_s);
  Rout.textContent=fmt(R);
  resultValue.textContent = Number.isFinite(R) ? `${fmt(R)} m/min` : "— m/min";
}

presetSel.addEventListener("input",()=>update(true,false));
scenarioSel.addEventListener("input",()=>update(false,true));
[fuelLoad.input,bulkDensity.input,surfaceAreaRatio.input,heatContent.input,windSpeed.input,slope.input,moisture.input].forEach(el=>el.addEventListener("input",()=>update(false,false)));
update(true,false);

```
