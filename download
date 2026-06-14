import React, { useState, useEffect, useRef } from "react";
const fmt = (n) => new Intl.NumberFormat("es-UY", { minimumFractionDigits: 0, maximumFractionDigits: 0 }).format(n);
const timeNow = () => new Date().toLocaleDateString("es-UY", { day: "2-digit", month: "2-digit" });
const uid = () => "id_" + Date.now() + "_" + Math.random().toString(36).slice(2, 7);
const C = {
  bg: "#f0f2f8", surface: "#ffffff", surface2: "#f7f8fc",
  primary: "#4f46e5", primary2: "#6366f1", primaryBg: "#eef2ff",
  success: "#059669", successBg: "#ecfdf5",
  danger: "#dc2626", dangerBg: "#fef2f2",
  warning: "#d97706", warningBg: "#fffbeb",
  purple: "#7c3aed", purpleBg: "#f5f3ff",
  text: "#111827", text2: "#6b7280", text3: "#9ca3af",
  border: "#e5e7eb", border2: "#d1d5db",
  shadow:  "0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04)",
  shadow2: "0 4px 16px rgba(79,70,229,0.10), 0 1px 4px rgba(0,0,0,0.06)",
  shadow3: "0 8px 32px rgba(79,70,229,0.14), 0 2px 8px rgba(0,0,0,0.08)",
};
const PLAN_DEFAULT = [
  { id: "comida", fecha: "Todo el mes", concepto: "Comida",               monto: 60000, tipo: "variable", expandible: true },
  { id: "transp", fecha: "Mensual",     concepto: "Combustible",          monto: 12000, tipo: "variable", expandible: true },
  { id: "entret", fecha: "Todo el mes", concepto: "Entretenimiento",      monto: 10000, tipo: "variable", expandible: true },
  { id: "varios", fecha: "Todo el mes", concepto: "Gastos varios",        monto: 10000, tipo: "variable", expandible: true },
  { id: "tizi",   fecha: "Todo el mes", concepto: "Entrenamiento Tizi",   monto: 4600,  tipo: "variable", expandible: true },
  { id: "cel",       fecha: "Vto: 01/04", vto: 1,  concepto: "Telefono",            monto: 800,   tipo: "fijo" },
  { id: "clubjuana", fecha: "Vto: 05/04", vto: 5,  concepto: "Club Juana",          monto: 3500,  tipo: "fijo" },
  { id: "pension",   fecha: "Vto: 05/04", vto: 5,  concepto: "Pension alimenticia", monto: 10000, tipo: "fijo", urgente: true },
  { id: "oca",       fecha: "Vto: 16/04", vto: 16, concepto: "Tarjeta OCA",         monto: 42830, tipo: "tarjeta", urgente: true, usd: 30.48 },
  { id: "prestamo1", fecha: "Vto: 11/04", vto: 11, concepto: "Prestamo OCA Light",  monto: 3218,  tipo: "fijo", urgente: true },
  { id: "patente",   fecha: "Mensual",    vto: 99, concepto: "Patente auto",        monto: 5900,  tipo: "fijo" },
];
const GASTOS_FIJOS = [
  { concepto: "Comida",               monto: 60000, vto: "Todo el mes" },
  { concepto: "Combustible",          monto: 12000, vto: "Mensual" },
  { concepto: "Entretenimiento",      monto: 10000, vto: "Todo el mes" },
  { concepto: "Gastos varios",        monto: 10000, vto: "Todo el mes" },
  { concepto: "Entrenamiento Tizi",   monto: 4600,  vto: "Todo el mes" },
  { concepto: "Telefono",             monto: 800,   vto: "Vto: 01/04" },
  { concepto: "Club Juana",           monto: 3500,  vto: "Vto: 05/04" },
  { concepto: "Pension alimenticia",  monto: 10000, vto: "Vto: 05/04" },
  { concepto: "Patente auto",         monto: 5900,  vto: "Mensual" },
];
// ── TARJETAS ─────────────────────────────────────────────────────────────────
// Dejá los montos en 0 — los cargás cuando tengas los estados de cuenta
const TARJETAS_DEFAULT = [
  { id: "oca", nombre: "OCA", color: "#CC0000", bg: "#fdf2f2", monto: 42830, usd: 30.48, vto: "16/04", nota: "Próx. cierre 06/04", urgente: true },
];
// ── CUOTAS ───────────────────────────────────────────────────────────────────
// Agrega acá tus cuotas comprometidas mes a mes
const CUOTAS_DEFAULT = [
  // Abril = estado actual OCA
  { mes: "Abril",      total: 42830, usd: 30.48,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 42830, usd: 30.48, vto: "16/04" },
    ]
  },
  // Mayo en adelante = cuotas comprometidas OCA
  { mes: "Mayo",       total: 33109, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 33109, usd: 0, vto: "~16/05" },
    ]
  },
  { mes: "Junio",      total: 27531, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 27531, usd: 0, vto: "~16/06" },
    ]
  },
  { mes: "Julio",      total: 21531, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 21531, usd: 0, vto: "~16/07" },
    ]
  },
  { mes: "Agosto",     total: 18494, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 18494, usd: 0, vto: "~16/08" },
    ]
  },
  { mes: "Septiembre", total: 10829, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 10829, usd: 0, vto: "~16/09" },
    ]
  },
  { mes: "Octubre",    total: 10430, usd: 0,
    detalle: [
      { tarjeta: "OCA",  color: "#CC0000", monto: 10430, usd: 0, vto: "~16/10" },
    ]
  },
];
const TC = 41;
const PROPIEDADES_DEFAULT = [
  { id: "pocitos",    nombre: "Apto Pocitos",          color: "#4f46e5", emoji: "🏢" },
  { id: "laspiedras", nombre: "Galón Las Piedras",    color: "#059669", emoji: "🏗️" },
  { id: "shangrila",  nombre: "Shangrila del Remanso", color: "#7c3aed", emoji: "🏡" },
  { id: "cascagno",   nombre: "Casas Cascagno",        color: "#d97706", emoji: "🏘️" },
  { id: "puntaeste",  nombre: "Apto Punta del Este",   color: "#0891b2", emoji: "🏖️" },
  { id: "emir3d",     nombre: "El Emir 3 Dormitorios", color: "#be185d", emoji: "🏠" },
];
const ITEMS_PROP_DEFAULT = [
  // Apto Pocitos - Scoseria 2870 apt 102
  { id: "gc_poc",   propId: "pocitos", tipo: "gasto", concepto: "Gastos comunes", monto: 21000, fecha: "Mensual" },
  { id: "wifi_poc", propId: "pocitos", tipo: "gasto", concepto: "WiFi",           monto: 1900,  fecha: "Mensual" },
  { id: "luz_poc",  propId: "pocitos", tipo: "gasto", concepto: "UTE (luz)",      monto: 4000,  fecha: "Mensual", variable: true },
  { id: "imp_poc",  propId: "pocitos", tipo: "gasto", concepto: "Impuesto",       monto: 1800,  fecha: "Mensual" },
  { id: "cont_poc", propId: "pocitos", tipo: "gasto", concepto: "Contribucion",   monto: 5417,  fecha: "Mensual" },
  { id: "prim_poc", propId: "pocitos", tipo: "gasto", concepto: "Primaria",       monto: 0,     fecha: "Mensual" },
  { id: "mant_poc", propId: "pocitos", tipo: "gasto", concepto: "Mantenimiento",  monto: 0,     fecha: "Mensual", variable: true },
  // Galon Las Piedras - Av. Julio Sosa 1621
  { id: "alq_lp",   propId: "laspiedras", tipo: "ingreso", concepto: "Alquiler",     monto: 93000, fecha: "Mensual" },
  { id: "irpf_lp",  propId: "laspiedras", tipo: "gasto",   concepto: "IRPF (10.5%)", monto: 9765,  fecha: "Mensual" },
  { id: "cont_lp",  propId: "laspiedras", tipo: "gasto",   concepto: "Contribucion", monto: 6000,  fecha: "Mensual" },
  { id: "prim_lp",  propId: "laspiedras", tipo: "gasto",   concepto: "Primaria",     monto: 0,     fecha: "Mensual" },
  { id: "mant_lp",  propId: "laspiedras", tipo: "gasto",   concepto: "Mantenimiento",monto: 0,     fecha: "Mensual", variable: true },
  // Shangrila del Remanso - Remanso M54 S11 U3
  { id: "alq_sh",   propId: "shangrila", tipo: "ingreso", concepto: "Alquiler",     monto: 40500, fecha: "Mensual" },
  { id: "irpf_sh",  propId: "shangrila", tipo: "gasto",   concepto: "IRPF (10.5%)", monto: 4252,  fecha: "Mensual" },
  { id: "cont_sh",  propId: "shangrila", tipo: "gasto",   concepto: "Contribucion", monto: 3022,  fecha: "Mensual" },
  { id: "prim_sh",  propId: "shangrila", tipo: "gasto",   concepto: "Primaria",     monto: 283,   fecha: "Mensual" },
  { id: "mant_sh",  propId: "shangrila", tipo: "gasto",   concepto: "Mantenimiento",monto: 0,     fecha: "Mensual", variable: true },
  // Casas Cascagno - Calcagno S18 M56
  { id: "alq_cc",   propId: "cascagno", tipo: "ingreso", concepto: "Alquiler",             monto: 140000, fecha: "Mensual" },
  { id: "irpf_cc",  propId: "cascagno", tipo: "gasto",   concepto: "IRPF (10.5%)",         monto: 14700,  fecha: "Mensual" },
  { id: "adm_cc",   propId: "cascagno", tipo: "gasto",   concepto: "Administracion (10%)", monto: 14000,  fecha: "Mensual" },
  { id: "luz_cc",   propId: "cascagno", tipo: "gasto",   concepto: "UTE (luz)",            monto: 0,      fecha: "Mensual", variable: true },
  { id: "cont_cc",  propId: "cascagno", tipo: "gasto",   concepto: "Contribucion",         monto: 11792,  fecha: "Mensual" },
  { id: "prim_cc",  propId: "cascagno", tipo: "gasto",   concepto: "Primaria",             monto: 1542,   fecha: "Mensual" },
  { id: "mant_cc",  propId: "cascagno", tipo: "gasto",   concepto: "Mantenimiento",        monto: 0,      fecha: "Mensual", variable: true },
  // Apto Punta del Este - El Emir 305 apt 205 (2 dormitorios)
  { id: "alq_pe",   propId: "puntaeste", tipo: "ingreso", concepto: "Alquiler",     monto: 0,    fecha: "Mensual" },
  { id: "irpf_pe",  propId: "puntaeste", tipo: "gasto",   concepto: "IRPF (10.5%)", monto: 0,    fecha: "Mensual" },
  { id: "luz_pe",   propId: "puntaeste", tipo: "gasto",   concepto: "UTE (luz)",    monto: 0,    fecha: "Mensual", variable: true },
  { id: "gc_pe",    propId: "puntaeste", tipo: "gasto",   concepto: "Gastos comunes",monto: 14500,fecha: "Mensual" },
  { id: "cont_pe",  propId: "puntaeste", tipo: "gasto",   concepto: "Contribucion", monto: 5182, fecha: "Mensual" },
  { id: "prim_pe",  propId: "puntaeste", tipo: "gasto",   concepto: "Primaria",     monto: 708,  fecha: "Mensual" },
  { id: "mant_pe",  propId: "puntaeste", tipo: "gasto",   concepto: "Mantenimiento",monto: 0,    fecha: "Mensual", variable: true },
  // El Emir 3 Dormitorios - El Emir 302 apt 202 (3 dormitorios)
  { id: "alq_em",   propId: "emir3d", tipo: "ingreso", concepto: "Alquiler",      monto: 0,    fecha: "Mensual" },
  { id: "irpf_em",  propId: "emir3d", tipo: "gasto",   concepto: "IRPF (10.5%)", monto: 0,    fecha: "Mensual" },
  { id: "luz_em",   propId: "emir3d", tipo: "gasto",   concepto: "UTE (luz)",    monto: 1500, fecha: "Mensual", variable: true },
  { id: "wifi_em",  propId: "emir3d", tipo: "gasto",   concepto: "WiFi",         monto: 0,    fecha: "Mensual" },
  { id: "gc_em",    propId: "emir3d", tipo: "gasto",   concepto: "Gastos comunes",monto: 28500,fecha: "Mensual" },
  { id: "cont_em",  propId: "emir3d", tipo: "gasto",   concepto: "Contribucion", monto: 10000,fecha: "Mensual" },
  { id: "prim_em",  propId: "emir3d", tipo: "gasto",   concepto: "Primaria",     monto: 1696, fecha: "Mensual" },
  { id: "mant_em",  propId: "emir3d", tipo: "gasto",   concepto: "Mantenimiento",monto: 0,    fecha: "Mensual", variable: true },
];
const PRESTAMOS_DEFAULT = [
  {
    id: "prestamo1",
    nombre: "Prestamo Light OCA",
    numero: "#7211345",
    cuotaActual: 8,
    totalCuotas: 12,
    montoCuota: 3218,
    vtoProximo: "11/04/2026",
    color: "#CC0000",
  },
];

const SUSCRIPCIONES_OCA = [
  { concepto: "SUAT",                 monto: 1288, monto2: 1025, usd: 0,     icono: "🏥", nota: "Dos cobros mensuales" },
  { concepto: "La Española",          monto: 2024, monto2: 1138, usd: 0,     icono: "🏥", nota: "Dos cobros mensuales" },
  { concepto: "PlayStation",          monto: 0,    monto2: 0,    usd: 9.75,  icono: "🎮", nota: "U$S 9.75/mes" },
  { concepto: "ChatGPT",              monto: 0,    monto2: 0,    usd: 20.00, icono: "🤖", nota: "U$S 20.00/mes" },
];
const TC_SUSCR = 40.40;

const PROP_EMOJIS = ["🏢","🏠","🏡","🏬","🏗️","🏘️","🏖️","🏰"];
const PROP_COLORS = ["#4f46e5","#059669","#d97706","#dc2626","#7c3aed","#0891b2","#be185d","#65a30d"];
function Badge({ label, color, bg }) {
  return <span style={{ fontSize: 9, fontWeight: 600, color, background: bg, borderRadius: 99, padding: "2px 8px", letterSpacing: 0.3, textTransform: "uppercase" }}>{label}</span>;
}
function DonutChart({ pct, disponible, porPagar }) {
  const r = 52, cx = 70, cy = 70, stroke = 10;
  const circ = 2 * Math.PI * r;
  const dash = (circ * Math.min(100, pct)) / 100;
  const tot = Math.max(1, disponible + porPagar);
  return (
    <div style={{ display: "flex", alignItems: "center", gap: 20 }}>
      <svg width="140" height="140" viewBox="0 0 140 140">
        <circle cx={cx} cy={cy} r={r} fill="none" stroke={C.border} strokeWidth={stroke} />
        <circle cx={cx} cy={cy} r={r} fill="none" stroke={C.primary} strokeWidth={stroke}
          strokeDasharray={dash + " " + circ} strokeLinecap="round"
          transform={"rotate(-90 " + cx + " " + cy + ")"} style={{ transition: "stroke-dasharray 1s ease" }} />
        <text x={cx} y={cy - 8}  textAnchor="middle" fill={C.text}  fontSize="20" fontWeight="700" fontFamily="Inter, sans-serif">{pct}%</text>
        <text x={cx} y={cy + 10} textAnchor="middle" fill={C.text2} fontSize="10" fontFamily="Inter, sans-serif">pagado</text>
      </svg>
      <div style={{ flex: 1 }}>
        <div style={{ marginBottom: 12 }}>
          <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
            <span style={{ fontSize: 11, color: C.text2 }}>Disponible</span>
            <span style={{ fontSize: 18, fontWeight: 700, color: disponible < 0 ? C.danger : C.success }}>
              {disponible < 0 ? "-$" + fmt(Math.abs(disponible)) : "$" + fmt(disponible)}
            </span>
          </div>
          <div style={{ height: 4, background: C.border, borderRadius: 99 }}>
            <div style={{ height: "100%", width: Math.min(100, Math.max(0, disponible / tot * 100)) + "%", background: C.success, borderRadius: 99, transition: "width 0.8s ease" }} />
          </div>
        </div>
        <div>
          <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
            <span style={{ fontSize: 11, color: C.text2 }}>Por pagar</span>
            <span style={{ fontSize: 18, fontWeight: 700, color: C.danger }}>${fmt(porPagar)}</span>
          </div>
          <div style={{ height: 4, background: C.border, borderRadius: 99 }}>
            <div style={{ height: "100%", width: Math.min(100, porPagar / tot * 100) + "%", background: "#fca5a5", borderRadius: 99, transition: "width 0.8s ease" }} />
          </div>
        </div>
      </div>
    </div>
  );
}
function TabPropiedades({ propiedades, setPropiedades, itemsProp, setItemsProp, pagadosProp, setPagadosProp }) {
  const [collapsed, setCollapsed]         = useState({
    pocitos: true, laspiedras: true, shangrila: true,
    cascagno: true, puntaeste: true, emir3d: true,
  });
  const [uteVals, setUteVals]             = useState({});
  const [confirmDelete, setConfirmDelete] = useState(null);
  const [showNuevaProp, setShowNuevaProp] = useState(false);
  const [showNuevoItem, setShowNuevoItem] = useState(null);
  const [niTipo, setNiTipo]               = useState("ingreso");
  const [niConcepto, setNiConcepto]       = useState("");
  const [niMonto, setNiMonto]             = useState("");
  const [niFecha, setNiFecha]             = useState("");
  const [niVariable, setNiVariable]       = useState(false);
  const [npNombre, setNpNombre]           = useState("");
  const [npEmoji, setNpEmoji]             = useState("🏠");
  const [npColor, setNpColor]             = useState("#4f46e5");
  const marcarItem   = (id) => setPagadosProp(prev => prev.includes(id) ? prev : [...prev, id]);
  const desmarcarItem = (id) => setPagadosProp(prev => prev.filter(x => x !== id));
  const toggleItem = marcarItem;
  const deleteItem = (id) => { setItemsProp(prev => prev.filter(i => i.id !== id)); setPagadosProp(prev => prev.filter(x => x !== id)); setConfirmDelete(null); };
  const deleteProp = (propId) => { setPropiedades(prev => prev.filter(p => p.id !== propId)); setItemsProp(prev => prev.filter(i => i.propId !== propId)); };
  const updateItemMonto = (id, val) => { if (!isNaN(val) && val >= 0) setItemsProp(prev => prev.map(i => i.id === id ? { ...i, monto: val } : i)); };
  const agregarItem = () => {
    const val = parseFloat(niMonto);
    if (!niConcepto.trim() || isNaN(val) || val < 0) return;
    setItemsProp(prev => [...prev, { id: uid(), propId: showNuevoItem, tipo: niTipo, concepto: niConcepto.trim(), monto: val, fecha: niFecha || "Mensual", variable: niVariable }]);
    setNiConcepto(""); setNiMonto(""); setNiFecha(""); setNiVariable(false); setNiTipo("ingreso"); setShowNuevoItem(null);
  };
  const agregarProp = () => {
    if (!npNombre.trim()) return;
    setPropiedades(prev => [...prev, { id: uid(), nombre: npNombre.trim(), emoji: npEmoji, color: npColor }]);
    setNpNombre(""); setNpEmoji("🏠"); setNpColor("#4f46e5"); setShowNuevaProp(false);
  };
  const iSt = (extra) => ({ background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 10, color: C.text, padding: "9px 12px", fontSize: 13, fontFamily: "Inter, sans-serif", outline: "none", width: "100%", ...(extra || {}) });
  const ICON_MAP = {
    "alquiler": "🏠",
    "contribución": "🏛️",
    "gastos comunes": "🏢",
    "mantenimiento": "🔧",
    "wifi": "📶",
    "ute (luz)": "💡",
    "irpf": "🧾",
    "primaria": "🏛️",
    "impuesto": "🧾",
    "administración": "📋",
  };
  const totalIng  = itemsProp.filter(i => i.tipo === "ingreso").reduce((a, b) => a + b.monto, 0);
  const totalGst  = itemsProp.filter(i => i.tipo === "gasto").reduce((a, b) => a + b.monto, 0);
  const cobrado   = itemsProp.filter(i => i.tipo === "ingreso" && pagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  const pagadoGst = itemsProp.filter(i => i.tipo === "gasto"   && pagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  return (
    <div style={{ animation: "fadeUp 0.3s ease" }}>
      <div style={{ background: C.surface, borderRadius: 16, padding: 16, boxShadow: C.shadow2, marginBottom: 14, border: "1.5px solid " + C.primaryBg }}>
        <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>Resumen propiedades</div>
        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr 1fr", gap: 8 }}>
          {[
            { label: "Ingresos", val: totalIng,           color: C.success },
            { label: "Gastos",   val: totalGst,           color: C.danger  },
            { label: "Neto",     val: totalIng - totalGst, color: (totalIng - totalGst) >= 0 ? C.success : C.danger },
          ].map(k => (
            <div key={k.label} style={{ background: C.surface2, borderRadius: 10, padding: "10px 8px", textAlign: "center" }}>
              <div style={{ fontSize: 9, color: C.text3, fontWeight: 500, marginBottom: 3, textTransform: "uppercase" }}>{k.label}</div>
              <div style={{ fontSize: 13, fontWeight: 700, color: k.color }}>{k.val < 0 ? "-" : ""}${fmt(Math.abs(k.val))}</div>
            </div>
          ))}
        </div>
        <div style={{ display: "flex", justifyContent: "space-between", marginTop: 10, fontSize: 11, color: C.text2 }}>
          <span>Cobrado: <strong style={{ color: C.success }}>${fmt(cobrado)}</strong></span>
          <span>Gastos pag.: <strong style={{ color: C.danger }}>${fmt(pagadoGst)}</strong></span>
        </div>
      </div>
      <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>
        Toca para marcar cobrado/pagado
      </div>
      {propiedades.map(prop => {
        const items    = itemsProp.filter(i => i.propId === prop.id);
        const ingresos       = items.filter(i => i.tipo === "ingreso").reduce((a, b) => a + b.monto, 0);
        const gastos         = items.filter(i => i.tipo === "gasto").reduce((a, b) => a + b.monto, 0);
        const neto           = ingresos - gastos;
        // Neto REAL = solo lo que entró cobrado vs gastos pagados
        const ingresoCobrado = items.filter(i => i.tipo === "ingreso" && pagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
        const gastosPagados  = items.filter(i => i.tipo === "gasto"   && pagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
        const netoReal       = ingresoCobrado - gastosPagados;
        const isOpen         = !collapsed[prop.id];
        return (
          <div key={prop.id} style={{ marginBottom: 16 }}>
            <div onClick={() => setCollapsed(p => ({ ...p, [prop.id]: !p[prop.id] }))}
              style={{ background: C.surface, borderRadius: isOpen ? "16px 16px 0 0" : 16, padding: "14px 16px", boxShadow: C.shadow2, borderLeft: "4px solid " + prop.color, cursor: "pointer", transition: "border-radius 0.2s" }}>
              <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
                <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                  <span style={{ fontSize: 22 }}>{prop.emoji}</span>
                  <div>
                    <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{prop.nombre}</div>
                    <div style={{ display: "flex", gap: 10, marginTop: 2 }}>
                      <span style={{ fontSize: 10, color: C.success }}>+${fmt(ingresos)}</span>
                      <span style={{ fontSize: 10, color: C.danger }}>-${fmt(gastos)}</span>
                    </div>
                  </div>
                </div>
                <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                  <div style={{ textAlign: "right" }}>
                    <div style={{ fontSize: 9, color: C.text3 }}>Neto estimado</div>
                    <div style={{ fontSize: 14, fontWeight: 700, color: neto >= 0 ? C.success : C.danger }}>{neto >= 0 ? "+" : "-"}${fmt(Math.abs(neto))}</div>
                    {ingresoCobrado > 0 && (
                      <div style={{ fontSize: 11, fontWeight: 700, color: netoReal >= 0 ? C.success : C.danger, marginTop: 2 }}>
                        Neto real: {netoReal >= 0 ? "+" : "-"}${fmt(Math.abs(netoReal))}
                      </div>
                    )}
                  </div>
                  <span style={{ fontSize: 11, color: C.text3 }}>{isOpen ? "▲" : "▼"}</span>
                </div>
              </div>
            </div>
            {isOpen && (
              <div style={{ background: C.surface2, border: "1.5px solid " + C.border, borderTop: "none", borderRadius: "0 0 16px 16px", padding: "10px 10px 12px" }}>
                {items.filter(i => i.tipo === "ingreso").length > 0 && (
                  <div style={{ fontSize: 9, color: C.success, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", margin: "4px 4px 6px" }}>Ingresos</div>
                )}
                {items.filter(i => i.tipo === "ingreso").map(item => {
                  const pagado = pagadosProp.includes(item.id);
                  return (
                    <div key={item.id}
                      onClick={() => { if (!pagado) marcarItem(item.id); }}
                      onDoubleClick={() => { if (pagado) desmarcarItem(item.id); }}
                      style={{ background: pagado ? "#f0fdf4" : C.surface, borderRadius: 14, padding: "13px 14px", marginBottom: 6, boxShadow: pagado ? "0 2px 12px rgba(5,150,105,0.15)" : C.shadow, border: "1.5px solid " + (pagado ? "#059669" : prop.color + "55"), display: "flex", alignItems: "center", gap: 12, cursor: "pointer", transition: "all 0.15s" }}>
                      <div style={{ width: 40, height: 40, borderRadius: 12, flexShrink: 0, background: pagado ? C.success : prop.color + "22", display: "flex", alignItems: "center", justifyContent: "center", fontSize: pagado ? 18 : 20, color: pagado ? "#fff" : prop.color, fontWeight: 700, boxShadow: pagado ? "0 2px 8px rgba(5,150,105,0.35)" : "none" }}>
                        {pagado ? "✓" : "💰"}
                      </div>
                      <div style={{ flex: 1 }}>
                        <div style={{ display: "flex", alignItems: "center", gap: 6 }}>
                          <div style={{ fontSize: 13, fontWeight: 600, color: pagado ? C.success : C.text }}>{item.concepto}</div>
                          {pagado && <span style={{ fontSize: 8, fontWeight: 700, color: "#fff", background: C.success, borderRadius: 99, padding: "2px 7px", letterSpacing: 0.5 }}>COBRADO</span>}
                        </div>
                        <div style={{ fontSize: 11, color: C.text3, marginTop: 2 }}>
                          {pagado ? "Ingresado · " + item.fecha : "Pendiente · " + item.fecha}
                        </div>
                      </div>
                      <div style={{ textAlign: "right" }}>
                        <div style={{ fontSize: 15, fontWeight: 700, color: pagado ? C.success : prop.color }}>+${fmt(item.monto)}</div>
                        {!pagado && <div style={{ fontSize: 9, color: C.text3 }}>Toca para cobrar</div>}
                      </div>
                      <div onClick={e => { e.stopPropagation(); setConfirmDelete(item.id); }} style={{ color: C.text3, fontSize: 12, padding: "4px 6px", cursor: "pointer" }}>&times;</div>
                    </div>
                  );
                })}
                {items.filter(i => i.tipo === "gasto").length > 0 && (
                  <div style={{ fontSize: 9, color: C.danger, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", margin: "10px 4px 6px" }}>Gastos</div>
                )}
                {items.filter(i => i.tipo === "gasto").map(item => {
                  const pagado    = pagadosProp.includes(item.id);
                  const editando  = uteVals[item.id] !== undefined && uteVals[item.id] !== "" && item.variable && !pagado;
                  return (
                    <div key={item.id} style={{ marginBottom: 6 }}>
                      <div onClick={() => { if (!pagado) marcarItem(item.id); }}
                        onDoubleClick={() => { if (pagado) desmarcarItem(item.id); }}
                        style={{ background: pagado ? "#f0fdf4" : C.surface, borderRadius: 14, padding: "13px 14px", boxShadow: C.shadow, border: "1.5px solid " + (pagado ? "#a7f3d0" : C.border), display: "flex", alignItems: "center", gap: 12, cursor: "pointer", transition: "all 0.15s" }}>
                        <div style={{ width: 36, height: 36, borderRadius: 12, flexShrink: 0, background: pagado ? C.success : C.surface2, display: "flex", alignItems: "center", justifyContent: "center", fontSize: pagado ? 16 : 18, color: pagado ? "#fff" : C.text2, fontWeight: 700, boxShadow: pagado ? "0 2px 8px rgba(5,150,105,0.35)" : "none" }}>
                          {pagado ? "✓" : (ICON_MAP[item.concepto.toLowerCase()] || "📌")}
                        </div>
                        <div style={{ flex: 1 }}>
                          <div style={{ fontSize: 13, fontWeight: 600, color: pagado ? C.text3 : C.text, textDecoration: pagado ? "line-through" : "none" }}>{item.concepto}</div>
                          <div style={{ fontSize: 11, color: C.text3, marginTop: 2 }}>{item.fecha}{item.variable ? " · variable" : ""}</div>
                        </div>
                        <div style={{ textAlign: "right" }}
                          onTouchStart={e => { if (item.variable && !pagado) { const t = setTimeout(() => { e.stopPropagation(); setUteVals(p => ({...p, [item.id]: String(item.monto)})); }, 500); e.currentTarget._lt = t; } }}
                          onTouchEnd={e => { clearTimeout(e.currentTarget._lt); }}
                          onMouseDown={e => { if (item.variable && !pagado) { const t = setTimeout(() => { e.stopPropagation(); setUteVals(p => ({...p, [item.id]: String(item.monto)})); }, 500); e.currentTarget._lt = t; } }}
                          onMouseUp={e => { clearTimeout(e.currentTarget._lt); }}
                        >
                          {item.variable && !pagado && uteVals[item.id] !== undefined ? (
                            <input
                              type="number"
                              autoFocus
                              defaultValue={item.monto}
                              onBlur={e => { const v = parseFloat(e.target.value); if (!isNaN(v) && v >= 0) updateItemMonto(item.id, v); setUteVals(p => { const n = {...p}; delete n[item.id]; return n; }); }}
                              onKeyDown={e => { if (e.key === "Enter") e.target.blur(); if (e.key === "Escape") setUteVals(p => { const n = {...p}; delete n[item.id]; return n; }); }}
                              onClick={e => e.stopPropagation()}
                              style={{ width: 80, textAlign: "right", fontSize: 14, fontWeight: 700, color: C.text2, background: C.primaryBg, border: "1.5px solid " + C.primary, borderRadius: 8, padding: "3px 6px", outline: "none", fontFamily: "Inter, sans-serif" }}
                            />
                          ) : (
                            <div style={{ fontSize: 14, fontWeight: 700, color: pagado ? C.success : C.text2, borderBottom: item.variable && !pagado ? "1.5px dashed " + C.border2 : "none", cursor: item.variable && !pagado ? "text" : "default" }}>
                              {item.monto > 0 ? "-$" + fmt(item.monto) : <span style={{ color: C.text3 }}>$0</span>}
                            </div>
                          )}
                        </div>
                        <div onClick={e => { e.stopPropagation(); setConfirmDelete(item.id); }} style={{ color: C.text3, fontSize: 12, padding: "4px 6px", cursor: "pointer" }}>&times;</div>
                      </div>
                    </div>
                  );
                })}
                <div style={{ display: "flex", gap: 8, marginTop: 8 }}>
                  <button onClick={() => setShowNuevoItem(prop.id)} style={{ flex: 1, background: C.surface, border: "1.5px dashed " + prop.color + "55", padding: "10px", color: prop.color, fontSize: 12, fontWeight: 500, borderRadius: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>
                    + Agregar item
                  </button>
                  <button onClick={() => deleteProp(prop.id)} style={{ background: C.surface, border: "1.5px solid " + C.border, padding: "10px 14px", color: C.text3, fontSize: 12, borderRadius: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>🗑️</button>
                </div>
                {showNuevoItem === prop.id && (
                  <div style={{ background: C.surface, borderRadius: 12, padding: 14, marginTop: 8, border: "1.5px solid " + prop.color + "44", boxShadow: C.shadow }}>
                    <div style={{ fontSize: 11, color: C.text2, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1, marginBottom: 10 }}>Nuevo item</div>
                    <div style={{ display: "flex", gap: 6, marginBottom: 10 }}>
                      {["ingreso","gasto"].map(t => (
                        <button key={t} onClick={() => setNiTipo(t)} style={{ flex: 1, padding: "8px", borderRadius: 8, border: "1.5px solid " + (niTipo === t ? (t === "ingreso" ? C.success : C.danger) : C.border), background: niTipo === t ? (t === "ingreso" ? C.successBg : C.dangerBg) : C.surface2, color: niTipo === t ? (t === "ingreso" ? C.success : C.danger) : C.text2, fontWeight: 600, fontSize: 11, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>
                          {t === "ingreso" ? "Ingreso" : "Gasto"}
                        </button>
                      ))}
                    </div>
                    <input type="text" value={niConcepto} onChange={e => setNiConcepto(e.target.value)} placeholder="Concepto" style={iSt({ marginBottom: 8 })} />
                    <div style={{ display: "flex", gap: 8, marginBottom: 8 }}>
                      <input type="number" value={niMonto} onChange={e => setNiMonto(e.target.value)} placeholder="Monto $" style={iSt({ flex: 1, width: "auto" })} />
                      <input type="text" value={niFecha} onChange={e => setNiFecha(e.target.value)} placeholder="Fecha" style={iSt({ flex: 1, width: "auto" })} />
                    </div>
                    <div style={{ display: "flex", gap: 8 }}>
                      <button onClick={() => { setShowNuevoItem(null); setNiConcepto(""); setNiMonto(""); setNiFecha(""); }} style={{ flex: 1, background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 8, padding: "10px", color: C.text2, fontSize: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Cancelar</button>
                      <button onClick={agregarItem} style={{ flex: 2, background: prop.color, border: "none", borderRadius: 8, padding: "10px", color: "#fff", fontWeight: 600, fontSize: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Agregar</button>
                    </div>
                  </div>
                )}
              </div>
            )}
          </div>
        );
      })}
      {/* Modal confirmación borrado */}
      {confirmDelete && (
        <div style={{ position: "fixed", top: 0, left: 0, right: 0, bottom: 0, background: "rgba(0,0,0,0.5)", display: "flex", alignItems: "center", justifyContent: "center", zIndex: 999, padding: 20 }}>
          <div style={{ background: "#fff", borderRadius: 20, padding: 24, maxWidth: 320, width: "100%", boxShadow: "0 8px 32px rgba(0,0,0,0.25)" }}>
            <div style={{ fontSize: 32, textAlign: "center", marginBottom: 12 }}>🗑️</div>
            <div style={{ fontSize: 15, fontWeight: 700, color: C.text, textAlign: "center", marginBottom: 8 }}>¿Eliminar este ítem?</div>
            <div style={{ fontSize: 12, color: C.text2, textAlign: "center", marginBottom: 20 }}>Esta acción no se puede deshacer</div>
            <div style={{ display: "flex", gap: 10 }}>
              <button onClick={() => setConfirmDelete(null)} style={{ flex: 1, padding: "12px", borderRadius: 12, border: "1.5px solid " + C.border, background: C.surface2, color: C.text2, fontSize: 13, fontWeight: 600, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Cancelar</button>
              <button onClick={() => deleteItem(confirmDelete)} style={{ flex: 1, padding: "12px", borderRadius: 12, border: "none", background: C.danger, color: "#fff", fontSize: 13, fontWeight: 700, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Sí, eliminar</button>
            </div>
          </div>
        </div>
      )}

      {!showNuevaProp ? (
        <button onClick={() => setShowNuevaProp(true)} style={{ width: "100%", background: C.surface, border: "1.5px dashed " + C.border2, padding: "13px", color: C.text2, fontSize: 12, fontWeight: 500, boxShadow: C.shadow, textAlign: "center", borderRadius: 14, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>
          + Agregar propiedad
        </button>
      ) : (
        <div style={{ background: C.surface, borderRadius: 14, padding: 16, boxShadow: C.shadow2, border: "1.5px solid " + C.border }}>
          <div style={{ fontSize: 11, color: C.text2, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1, marginBottom: 12 }}>Nueva propiedad</div>
          <div style={{ display: "flex", gap: 6, marginBottom: 10, flexWrap: "wrap" }}>
            {PROP_EMOJIS.map(e => <button key={e} onClick={() => setNpEmoji(e)} style={{ fontSize: 20, background: npEmoji === e ? C.primaryBg : C.surface2, border: "2px solid " + (npEmoji === e ? C.primary : C.border), borderRadius: 8, padding: "5px 8px", cursor: "pointer" }}>{e}</button>)}
          </div>
          <div style={{ display: "flex", gap: 6, marginBottom: 10, flexWrap: "wrap" }}>
            {PROP_COLORS.map(c => <button key={c} onClick={() => setNpColor(c)} style={{ width: 26, height: 26, background: c, border: "3px solid " + (npColor === c ? C.text : "transparent"), borderRadius: 99, cursor: "pointer" }} />)}
          </div>
          <input value={npNombre} onChange={e => setNpNombre(e.target.value)} placeholder="Nombre de la propiedad" style={iSt({ marginBottom: 10 })} />
          <div style={{ display: "flex", gap: 8 }}>
            <button onClick={() => { setShowNuevaProp(false); setNpNombre(""); }} style={{ flex: 1, background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 10, padding: "10px", color: C.text2, fontSize: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Cancelar</button>
            <button onClick={agregarProp} style={{ flex: 2, background: C.primary, border: "none", borderRadius: 10, padding: "10px", color: "#fff", fontWeight: 600, fontSize: 12, cursor: "pointer", fontFamily: "Inter, sans-serif" }}>Agregar</button>
          </div>
        </div>
      )}
    </div>
  );
}
export default function Dashboard() {
  const [ready, setReady]                     = useState(false);
  const [history, setHistory]                 = useState([]);
  const [dragItem, setDragItem]               = useState(null);
  const [dragOver, setDragOver]               = useState(null);
  const [reorderMode, setReorderMode]         = useState(false);
  const dragY = useRef(0);
  const dragStartIdx = useRef(null);
  const [tab, setTab]                         = useState("personal");
  const [plan, setPlan]                       = useState(null);
  const [pagados, setPagados]                 = useState(null);
  const [expandido, setExpandido]             = useState(null);
  const [subGastos, setSubGastos]             = useState(null);
  const [ingresos, setIngresos]               = useState(null);
  const [inputSub, setInputSub]               = useState({ desc: "", monto: "" });
  const [inputFactura, setInputFactura]       = useState("");
  const [inputLabel, setInputLabel]           = useState("");
  const [showFacturaInput, setShowFacturaInput] = useState(false);
  const [showHistorial, setShowHistorial]     = useState(false);
  const [editandoIngreso, setEditandoIngreso] = useState(null);
  const [uteInput, setUteInput]               = useState("");
  const [editandoItem, setEditandoItem]       = useState(null);
  const [showNuevoItem, setShowNuevoItem]     = useState(false);
  const [nuevoItem, setNuevoItem]             = useState({ concepto: "", monto: "", fecha: "" });
  const [savedMsg, setSavedMsg]               = useState(false);
  const [unsaved, setUnsaved]                 = useState(false);
  const [editMonto, setEditMonto]             = useState("");
  const [showExport, setShowExport]           = useState(false);
  const [showImport, setShowImport]           = useState(false);
  const [importText, setImportText]           = useState("");
  const [importMsg, setImportMsg]             = useState("");
  const [propiedades, setPropiedades]         = useState(null);
  const [itemsProp, setItemsProp]             = useState(null);
  const [pagadosProp, setPagadosProp]         = useState(null);
  const [tarjetas, setTarjetas]               = useState(null);
  const [cuotas, setCuotas]                   = useState(null);
  const [expandedMes, setExpandedMes]         = useState(null);
  const [pagadosTarj, setPagadosTarj]         = useState([]);
  useEffect(() => {
    (async () => {
      try {
        let data = null;
        try { const l = localStorage.getItem("finanzas_v2_oca19"); if (l) data = JSON.parse(l); } catch(e) {}
        if (data) {
          if (data.plan) {
            const merged = PLAN_DEFAULT.map(def => { const s = data.plan.find(i => i.id === def.id); return s ? { ...def, monto: s.monto } : def; });
            const extras = data.plan.filter(i => !PLAN_DEFAULT.find(d => d.id === i.id));
            setPlan([...merged, ...extras]);
          } else { setPlan(PLAN_DEFAULT); }
          setPagados(data.pagados     || []);
          setSubGastos(data.subGastos || {});
          setIngresos(data.ingresos   || []);
          setPropiedades(data.propiedades || PROPIEDADES_DEFAULT);
          setItemsProp(data.itemsProp     || ITEMS_PROP_DEFAULT);
          setPagadosProp(data.pagadosProp || []);
          setTarjetas(data.tarjetas       || TARJETAS_DEFAULT);
          setCuotas(data.cuotas           || CUOTAS_DEFAULT);
          setPagadosTarj(data.pagadosTarj || []);
        } else {
          setPlan(PLAN_DEFAULT); setPagados([]); setSubGastos({}); setIngresos([]);
          setPropiedades(PROPIEDADES_DEFAULT); setItemsProp(ITEMS_PROP_DEFAULT); setPagadosProp([]);
          setTarjetas(TARJETAS_DEFAULT); setCuotas(CUOTAS_DEFAULT); setPagadosTarj([]);
        }
      } catch(e) {
        setPlan(PLAN_DEFAULT); setPagados([]); setSubGastos({}); setIngresos([]);
        setPropiedades(PROPIEDADES_DEFAULT); setItemsProp(ITEMS_PROP_DEFAULT); setPagadosProp([]);
        setTarjetas(TARJETAS_DEFAULT); setCuotas(CUOTAS_DEFAULT); setPagadosTarj([]);
      }
      setReady(true);
    })();
  }, []);
  useEffect(() => {
    if (!ready) return;
    setUnsaved(true);
    const timer = setTimeout(async () => {
      const data = JSON.stringify({ plan: safePlan, pagados: safePagados, subGastos: safeSubGastos, ingresos: safeIngresos, propiedades: safePropiedades, itemsProp: safeItemsProp, pagadosProp: safePagadosProp, tarjetas: safeTarjetas, cuotas: safeCuotas, pagadosTarj: safePagadosTarj });
      try { localStorage.setItem("finanzas_v2_oca19", data); } catch(e) {}
      setUnsaved(false);
    }, 800);
    return () => clearTimeout(timer);
  }, [plan, pagados, subGastos, ingresos, propiedades, itemsProp, pagadosProp, tarjetas, cuotas, pagadosTarj]);
  const guardarTodo = async () => {
    const data = JSON.stringify({ plan: safePlan, pagados: safePagados, subGastos: safeSubGastos, ingresos: safeIngresos, propiedades: safePropiedades, itemsProp: safeItemsProp, pagadosProp: safePagadosProp });
    setSavedMsg("saving");
    try {
      try { localStorage.setItem("finanzas_v2_oca19", data); } catch(e) {}
      setSavedMsg("ok"); setUnsaved(false); setTimeout(() => setSavedMsg(false), 2000);
    } catch(e) { setSavedMsg("error"); setTimeout(() => setSavedMsg(false), 3000); }
  };
  const getBackupText = () => JSON.stringify({ version: 2, fecha: new Date().toLocaleDateString("es-UY"), plan: safePlan, pagados: safePagados, subGastos: safeSubGastos, ingresos: safeIngresos, propiedades: safePropiedades, itemsProp: safeItemsProp, pagadosProp: safePagadosProp, tarjetas: safeTarjetas, cuotas: safeCuotas, pagadosTarj: safePagadosTarj });
  const importarDatos = () => {
    try {
      const d = JSON.parse(importText);
      if (d.plan)        setPlan(d.plan);
      if (d.pagados)     setPagados(d.pagados);
      if (d.subGastos)   setSubGastos(d.subGastos);
      if (d.ingresos)    setIngresos(d.ingresos);
      if (d.propiedades) setPropiedades(d.propiedades);
      if (d.itemsProp)   setItemsProp(d.itemsProp);
      if (d.pagadosProp) setPagadosProp(d.pagadosProp);
      if (d.tarjetas)    setTarjetas(d.tarjetas);
      if (d.cuotas)      setCuotas(d.cuotas);
      if (d.pagadosTarj) setPagadosTarj(d.pagadosTarj);
      setImportMsg("✓ Datos restaurados correctamente"); setImportText("");
      setTimeout(() => { setImportMsg(""); setShowImport(false); }, 2000);
    } catch(e) { setImportMsg("\u2717 Texto inv\u00e1lido"); }
  };
  const marcarPago = (id) => {
    const item = (plan || PLAN_DEFAULT).find(i => i.id === id);
    if (item && item.expandible) return;
    setPagados(prev => (prev || []).includes(id) ? prev : [...(prev || []), id]);
  };
  const desmarcarPago = (id) => {
    const item = (plan || PLAN_DEFAULT).find(i => i.id === id);
    if (item && item.expandible) return;
    setPagados(prev => (prev || []).filter(c => c !== id));
  };
  const reorderPlan = (fromId, toId) => {
    if (fromId === toId) return;
    saveHistory();
    setPlan(prev => {
      const arr = [...(prev || PLAN_DEFAULT)];
      const fromIdx = arr.findIndex(i => i.id === fromId);
      const toIdx   = arr.findIndex(i => i.id === toId);
      if (fromIdx === -1 || toIdx === -1) return arr;
      const [moved] = arr.splice(fromIdx, 1);
      arr.splice(toIdx, 0, moved);
      return arr;
    });
  };

  const saveHistory = () => {
    setHistory(prev => {
      const snap = { plan: safePlan, pagados: safePagados, subGastos: safeSubGastos, ingresos: safeIngresos, propiedades: safePropiedades, itemsProp: safeItemsProp, pagadosProp: safePagadosProp, pagadosTarj: safePagadosTarj };
      return [...prev.slice(-19), snap]; // max 20 pasos
    });
  };

  const undoLast = () => {
    setHistory(prev => {
      if (prev.length === 0) return prev;
      const last = prev[prev.length - 1];
      setPlan(last.plan);
      setPagados(last.pagados);
      setSubGastos(last.subGastos);
      setIngresos(last.ingresos);
      setPropiedades(last.propiedades);
      setItemsProp(last.itemsProp);
      setPagadosProp(last.pagadosProp);
      setPagadosTarj(last.pagadosTarj);
      return prev.slice(0, -1);
    });
  };

  const togglePago = marcarPago;
  const agregarSubGasto = (itemId) => {
    const val = parseFloat(inputSub.monto);
    if (isNaN(val) || val <= 0) return;
    saveHistory();
    setSubGastos(prev => ({ ...(prev || {}), [itemId]: [...((prev || {})[itemId] || []), { id: Date.now(), desc: inputSub.desc || "Gasto", monto: val, fecha: timeNow() }] }));
    setInputSub({ desc: "", monto: "" });
  };
  const borrarSubGasto = (itemId, subId) => { saveHistory(); setSubGastos(prev => ({ ...(prev || {}), [itemId]: ((prev || {})[itemId] || []).filter(s => s.id !== subId) })); };
  const agregarIngreso = () => {
    const val = parseFloat(inputFactura);
    if (isNaN(val) || val <= 0) return;
    saveHistory();
    setIngresos(prev => [...(prev || []), { id: Date.now(), monto: val, label: inputLabel || "Facturado", fecha: timeNow() }]);
    setInputFactura(""); setInputLabel(""); setShowFacturaInput(false);
  };
  const borrarIngreso = (id) => { saveHistory(); setIngresos(prev => (prev || []).filter(i => i.id !== id)); };
  const guardarEdicionIngreso = (id) => {
    const val = parseFloat(editMonto);
    if (!isNaN(val) && val > 0) setIngresos(prev => (prev || []).map(i => i.id === id ? { ...i, monto: val } : i));
    setEditandoIngreso(null); setEditMonto("");
  };
  const agregarItem = () => {
    const val = parseFloat(nuevoItem.monto);
    if (!nuevoItem.concepto || isNaN(val) || val <= 0) return;
    const newId = "extra_" + Date.now();
    setPlan(prev => [...(prev || PLAN_DEFAULT), { id: newId, fecha: nuevoItem.fecha || "Mensual", concepto: nuevoItem.concepto, monto: val, tipo: "extra" }]);
    setNuevoItem({ concepto: "", monto: "", fecha: "" }); setShowNuevoItem(false);
  };
  const borrarItem = (id) => { saveHistory(); setPlan(prev => (prev || PLAN_DEFAULT).filter(i => i.id !== id)); setPagados(prev => (prev || []).filter(p => p !== id)); };
  const getMontoEfectivo = (item) => {
    if (!item.expandible) return item.monto;
    return Math.max(0, item.monto - ((safeSubGastos)[item.id] || []).reduce((a, b) => a + b.monto, 0));
  };
  const safeTarjetas    = tarjetas    || TARJETAS_DEFAULT;
  const safeCuotas      = cuotas      || CUOTAS_DEFAULT;
  const safePagadosTarj = pagadosTarj || [];
  const safePlan        = plan        || PLAN_DEFAULT;
  const safePagados     = pagados     || [];
  const safeSubGastos   = subGastos   || {};
  const safeIngresos    = ingresos    || [];
  const safePropiedades = propiedades || PROPIEDADES_DEFAULT;
  const safeItemsProp   = itemsProp   || ITEMS_PROP_DEFAULT;
  const safePagadosProp = pagadosProp || [];
  const facturadoTotal  = safeIngresos.reduce((a, b) => a + b.monto, 0);
  const totalSubGastos  = Object.values(safeSubGastos).flat().reduce((a, b) => a + b.monto, 0);
  const pagadosNuevos   = safePlan.filter(i => safePagados.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  const porPagar        = safePlan.filter(i => !safePagados.includes(i.id)).reduce((a, b) => a + getMontoEfectivo(b), 0);
  const ingPropCobrados = safeItemsProp.filter(i => i.tipo === "ingreso" && safePagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  const gstPropPagados  = safeItemsProp.filter(i => i.tipo === "gasto"   && safePagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  const disponible      = (ingPropCobrados - gstPropPagados) + facturadoTotal - pagadosNuevos - totalSubGastos;
  const necesitasGanar  = Math.max(0, porPagar - disponible);
  const totalPagado     = safePlan.filter(i => safePagados.includes(i.id)).reduce((a, b) => a + b.monto, 0);
  const totalMes        = safePlan.reduce((a, b) => a + b.monto, 0);
  const pct             = totalMes > 0 ? Math.min(100, Math.round((totalPagado + totalSubGastos) / totalMes * 100)) : 0;
  const iStyle = (extra) => ({ background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 10, color: C.text, padding: "9px 12px", fontSize: 13, fontFamily: "Inter, sans-serif", outline: "none", width: "100%", ...(extra || {}) });
  const Btn = ({ children, onClick, style }) => (
    <button onClick={onClick} style={{ border: "none", cursor: "pointer", fontFamily: "Inter, sans-serif", borderRadius: 10, ...style }}>{children}</button>
  );
  const itemAccent = (item, esPagado) => {
    if (esPagado)              return { color: C.success, bg: C.successBg };
    if (item.urgente)          return { color: C.danger,  bg: C.dangerBg  };
    if (item.expandible)       return { color: C.primary, bg: C.primaryBg };
    if (item.tipo === "extra") return { color: C.warning, bg: C.warningBg };
    return { color: C.text2, bg: C.surface2 };
  };
  const ITEM_ICONS = { cel: "📱", comida: "🍔", transp: "🚗", entret: "🎭", varios: "📦", oca: "💳", clubjuana: "⚽", tizi: "🏃", pension: "👨‍👧", patente: "🚗" };
  if (!ready) return (
    <div style={{ minHeight: "100vh", background: C.bg, display: "flex", alignItems: "center", justifyContent: "center" }}>
      <div style={{ color: C.primary, fontSize: 13, fontFamily: "Inter, sans-serif" }}>Cargando…</div>
    </div>
  );
  const TABS = [
    { id: "personal",    label: "Personal" },
    { id: "propiedades", label: "Prop." },
    { id: "tarjetas",    label: "Tarjetas" },
    { id: "fijos",       label: "Fijos" },
  ];
  return (
    <div style={{ minHeight: "100vh", background: C.bg, color: C.text, fontFamily: "Inter, sans-serif", maxWidth: 430, margin: "0 auto", paddingBottom: 30 }}>
      {/* Botón deshacer flotante */}
      {history.length > 0 && (
        <button onClick={undoLast} style={{ position: "fixed", top: 16, left: 16, zIndex: 9999, background: "rgba(79,70,229,0.92)", backdropFilter: "blur(8px)", border: "none", borderRadius: 12, padding: "8px 14px", color: "#fff", fontSize: 12, fontWeight: 700, cursor: "pointer", display: "flex", alignItems: "center", gap: 6, boxShadow: "0 4px 16px rgba(79,70,229,0.35)", fontFamily: "Inter, sans-serif" }}>
          ↩ Deshacer
          <span style={{ background: "rgba(255,255,255,0.25)", borderRadius: 99, padding: "1px 6px", fontSize: 10 }}>{history.length}</span>
        </button>
      )}
      <style>{"* { box-sizing: border-box; margin: 0; padding: 0; } @keyframes fadeUp { from { opacity:0; transform:translateY(8px); } to { opacity:1; transform:translateY(0); } } input::placeholder { color: #9ca3af; } input:focus { border-color: #4f46e5 !important; outline: none; box-shadow: 0 0 0 3px rgba(79,70,229,0.09); }"}</style>
      <div style={{ background: "linear-gradient(135deg, #4f46e5 0%, #7c3aed 100%)", padding: "32px 20px 24px", borderRadius: "0 0 28px 28px" }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 24 }}>
          <div>
            <div style={{ fontSize: 11, color: "rgba(255,255,255,0.65)", letterSpacing: 2, textTransform: "uppercase", marginBottom: 4 }}>Abril 2026</div>
            <div style={{ fontSize: 26, fontWeight: 700, color: "#fff" }}>Mis Finanzas</div>
          </div>
          <div style={{ background: "rgba(255,255,255,0.15)", borderRadius: 12, padding: "6px 12px", fontSize: 11, color: "rgba(255,255,255,0.85)", fontWeight: 500 }}>
            {safePropiedades.length} prop.
          </div>
        </div>
        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10 }}>
          <div style={{ background: "rgba(255,255,255,0.12)", borderRadius: 16, padding: 14, backdropFilter: "blur(10px)" }}>
            <div style={{ fontSize: 10, color: "rgba(255,255,255,0.65)", marginBottom: 4, fontWeight: 500 }}>DISPONIBLE</div>
            <div style={{ fontSize: 20, fontWeight: 700, color: disponible < 0 ? "#fca5a5" : "#fff" }}>
              {disponible < 0 ? "-$" + fmt(Math.abs(disponible)) : "$" + fmt(disponible)}
            </div>
            <div style={{ fontSize: 9, color: "rgba(255,255,255,0.5)", marginTop: 2 }}>
              {"prop: +" + fmt(ingPropCobrados) + " / -" + fmt(gstPropPagados)}
            </div>
          </div>
          <div style={{ background: "rgba(255,255,255,0.12)", borderRadius: 16, padding: 14, backdropFilter: "blur(10px)" }}>
            <div style={{ fontSize: 10, color: "rgba(255,255,255,0.65)", marginBottom: 4, fontWeight: 500 }}>POR PAGAR</div>
            <div style={{ fontSize: 20, fontWeight: 700, color: porPagar === 0 ? "#6ee7b7" : "#fca5a5" }}>
              {porPagar === 0 ? "✓ OK" : "$" + fmt(porPagar)}
            </div>
            <div style={{ fontSize: 9, color: necesitasGanar > 0 ? "#fca5a5" : "#6ee7b7", marginTop: 2 }}>
              {necesitasGanar > 0 ? "faltan $" + fmt(necesitasGanar) : porPagar > 0 ? "cubierto ✓" : "todo pago ✓"}
            </div>
          </div>
        </div>
      </div>
      <div style={{ padding: "20px 16px 0" }}>
        <div style={{ background: C.surface, borderRadius: 20, padding: 20, boxShadow: C.shadow2, marginBottom: 14 }}>
          <div style={{ fontSize: 12, fontWeight: 600, color: C.text, marginBottom: 16 }}>Resumen del mes</div>
          <DonutChart pct={pct} disponible={disponible} porPagar={porPagar} />
        </div>
        <div style={{ background: C.surface, borderRadius: 20, padding: 18, boxShadow: C.shadow2, marginBottom: 14 }}>
          <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: showFacturaInput || showHistorial ? 14 : 0 }}>
            <div>
              <div style={{ fontSize: 10, color: C.text2, fontWeight: 500, marginBottom: 2, textTransform: "uppercase", letterSpacing: 1 }}>Ingresos extra</div>
              <div style={{ fontSize: 22, fontWeight: 700, color: C.primary }}>${fmt(facturadoTotal)}</div>
            </div>
            <div style={{ display: "flex", gap: 8 }}>
              <Btn onClick={() => setShowHistorial(v => !v)} style={{ background: showHistorial ? C.primaryBg : C.surface2, width: 38, height: 38, color: showHistorial ? C.primary : C.text2, fontSize: 14, border: "1.5px solid " + (showHistorial ? C.primary : C.border) }}>{showHistorial ? "▲" : "▼"}</Btn>
              <Btn onClick={() => setShowFacturaInput(v => !v)} style={{ background: showFacturaInput ? C.surface2 : C.primary, width: 38, height: 38, color: showFacturaInput ? C.text2 : "#fff", fontSize: 22, fontWeight: 700, border: "none", boxShadow: showFacturaInput ? "none" : C.shadow2 }}>
                {showFacturaInput ? "×" : "+"}
              </Btn>
            </div>
          </div>
          {showFacturaInput && (
            <div>
              <input type="text" value={inputLabel} onChange={e => setInputLabel(e.target.value)} placeholder="Descripcion (opcional)" style={iStyle({ marginBottom: 8 })} />
              <div style={{ display: "flex", gap: 8 }}>
                <input type="number" value={inputFactura} onChange={e => setInputFactura(e.target.value)} onKeyDown={e => e.key === "Enter" && agregarIngreso()} placeholder="Monto $" autoFocus style={iStyle({ flex: 1, border: "1.5px solid " + C.primary })} />
                <Btn onClick={agregarIngreso} style={{ background: C.success, padding: "0 18px", color: "#fff", fontWeight: 600, fontSize: 15, height: 42, boxShadow: C.shadow }}>✓</Btn>
              </div>
            </div>
          )}
          {showHistorial && (
            <div style={{ borderTop: "1px solid " + C.border, paddingTop: 12 }}>
              <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>Historial</div>
              {safeIngresos.map(ing => (
                <div key={ing.id} style={{ marginBottom: 8 }}>
                  {editandoIngreso === ing.id ? (
                    <div style={{ display: "flex", gap: 8 }}>
                      <input type="number" value={editMonto} onChange={e => setEditMonto(e.target.value)} onKeyDown={e => e.key === "Enter" && guardarEdicionIngreso(ing.id)} autoFocus style={iStyle({ flex: 1, border: "1.5px solid " + C.primary })} />
                      <Btn onClick={() => guardarEdicionIngreso(ing.id)} style={{ background: C.success, padding: "0 12px", color: "#fff", fontWeight: 700, height: 42 }}>✓</Btn>
                      <Btn onClick={() => { setEditandoIngreso(null); setEditMonto(""); }} style={{ background: C.surface2, padding: "0 12px", color: C.text2, height: 42, border: "1.5px solid " + C.border }}>×</Btn>
                    </div>
                  ) : (
                    <div style={{ display: "flex", gap: 8 }}>
                      <div onClick={() => { setEditandoIngreso(ing.id); setEditMonto(String(ing.monto)); }} style={{ flex: 1, background: C.surface2, borderRadius: 10, padding: "10px 12px", cursor: "pointer", border: "1.5px solid " + C.border }}>
                        <div style={{ display: "flex", justifyContent: "space-between" }}>
                          <span style={{ fontSize: 12, color: C.text2 }}>{ing.label}</span>
                          <span style={{ fontSize: 13, fontWeight: 600, color: C.success }}>${fmt(ing.monto)}</span>
                        </div>
                        <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{ing.fecha} · toca para editar</div>
                      </div>
                      <Btn onClick={() => borrarIngreso(ing.id)} style={{ background: C.dangerBg, border: "1.5px solid #fca5a5", padding: "0 10px", color: C.danger, height: 42, fontWeight: 700 }}>✕</Btn>
                    </div>
                  )}
                </div>
              ))}
            </div>
          )}
        </div>
        <div style={{ display: "flex", gap: 8, marginBottom: 14, alignItems: "center" }}>
          <div style={{ background: C.surface, borderRadius: 14, padding: 4, display: "flex", gap: 2, flex: 1, boxShadow: C.shadow }}>
            {TABS.map(t => (
              <button key={t.id} onClick={() => { setTab(t.id); setReorderMode(false); }} style={{
                flex: 1, padding: "9px 4px", borderRadius: 10, border: "none", cursor: "pointer",
                fontSize: 10, fontFamily: "Inter, sans-serif", fontWeight: 600, textTransform: "uppercase", letterSpacing: 0.5,
                background: tab === t.id ? (t.id === "propiedades" ? C.success : C.primary) : "transparent",
                color: tab === t.id ? "#fff" : t.id === "propiedades" ? C.success : C.text2,
                boxShadow: tab === t.id ? C.shadow2 : "none", transition: "all 0.2s"
              }}>{t.label}</button>
            ))}
          </div>
          {tab === "personal" && (
            <button onClick={() => { setReorderMode(v => !v); setDragItem(null); setDragOver(null); }}
              style={{ padding: "9px 12px", borderRadius: 12, border: "1.5px solid " + (reorderMode ? C.primary : C.border), background: reorderMode ? C.primary : C.surface, color: reorderMode ? "#fff" : C.text2, fontSize: 16, cursor: "pointer", boxShadow: C.shadow, flexShrink: 0, fontFamily: "Inter, sans-serif" }}>
              ⇅
            </button>
          )}
        </div>
        {tab === "personal" && (
          <div style={{ animation: "fadeUp 0.3s ease" }}>
            <div style={{ fontSize: 10, color: reorderMode ? C.primary : C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>
              {reorderMode ? "⇅ Modo ordenar — arrastrá el ≡ para mover" : "Toca para marcar como pagado"}
            </div>
            {safePlan.map(item => {
              const esPagado  = safePagados.includes(item.id);
              const isOpen    = expandido === item.id;
              const subs      = safeSubGastos[item.id] || [];
              const gastado   = subs.reduce((a, b) => a + b.monto, 0);
              const restante  = item.expandible ? item.monto - gastado : item.monto;
              const pctItem   = item.expandible && item.monto > 0 ? Math.min(100, Math.round(gastado / item.monto * 100)) : 0;
              const acc       = itemAccent(item, esPagado);
              const showUte   = item.variable && !esPagado && editandoItem === item.id;
              const isDragging = dragItem === item.id;
              const isDragTarget = dragOver === item.id && dragItem !== item.id;
              return (
                <div key={item.id}
                  data-itemid={item.id}
                  style={{ marginBottom: 8, opacity: isDragging ? 0.35 : 1, transform: isDragTarget ? "translateY(-3px)" : "none", transition: "all 0.15s" }}
                  onDragOver={e => { e.preventDefault(); setDragOver(item.id); }}
                  onDrop={e => { e.preventDefault(); reorderPlan(dragItem, item.id); setDragItem(null); setDragOver(null); }}
                >
                  <div
                    onClick={() => { if (reorderMode) return; if (item.expandible) setExpandido(isOpen ? null : item.id); else if (!esPagado) marcarPago(item.id); }}
                    onDoubleClick={() => { if (reorderMode || item.expandible) return; if (esPagado) desmarcarPago(item.id); }}
                    style={{ background: isDragTarget ? C.primaryBg : esPagado ? "#f0fdf4" : C.surface, borderRadius: isOpen && !reorderMode ? "14px 14px 0 0" : 14, padding: "14px", boxShadow: isDragging ? C.shadow3 : isDragTarget ? C.shadow2 : C.shadow, border: "1.5px solid " + (isDragTarget ? C.primary : esPagado ? "#a7f3d0" : item.urgente ? "#fca5a5" : C.border), display: "flex", alignItems: "center", gap: 12, cursor: reorderMode ? "grab" : "pointer", transition: "all 0.15s",
                      userSelect: "none", WebkitUserSelect: "none" }}
                  >
                    {/* Handle drag - solo visible en modo reorden */}
                    {reorderMode && (
                      <div
                        onClick={e => e.stopPropagation()}
                        onTouchStart={e => {
                          e.stopPropagation();
                          e.preventDefault();
                          const scrollTop = window.scrollY;
                          e.currentTarget._scrollTop = scrollTop;
                          setDragItem(item.id);
                        }}
                        onTouchMove={e => {
                          e.stopPropagation();
                          e.preventDefault();
                          // Restaurar scroll si se movió
                          window.scrollTo(0, e.currentTarget._scrollTop || 0);
                          const touch = e.touches[0];
                          const els = document.elementsFromPoint(touch.clientX, touch.clientY);
                          for (const el of els) {
                            const id = el.getAttribute("data-itemid");
                            if (id && id !== item.id) {
                              setDragOver(id);
                              break;
                            }
                          }
                        }}
                        onTouchEnd={e => {
                          e.stopPropagation();
                          e.preventDefault();
                          if (dragOver && dragItem) reorderPlan(dragItem, dragOver);
                          setDragItem(null);
                          setDragOver(null);
                        }}
                        style={{ display: "flex", flexDirection: "column", justifyContent: "center", gap: 4, padding: "6px 10px 6px 0", cursor: "grab", flexShrink: 0, touchAction: "none", WebkitUserSelect: "none", userSelect: "none" }}
                      >
                        <div style={{ width: 18, height: 2.5, background: C.text3, borderRadius: 99 }} />
                        <div style={{ width: 18, height: 2.5, background: C.text3, borderRadius: 99 }} />
                        <div style={{ width: 18, height: 2.5, background: C.text3, borderRadius: 99 }} />
                      </div>
                    )}
                    <div style={{ width: 36, height: 36, borderRadius: 12, flexShrink: 0, background: esPagado ? C.success : acc.bg, display: "flex", alignItems: "center", justifyContent: "center", fontSize: esPagado ? 16 : 18, color: esPagado ? "#fff" : acc.color, fontWeight: 700, boxShadow: esPagado ? "0 2px 8px rgba(5,150,105,0.35)" : "none" }}>
                      {esPagado ? "✓" : (ITEM_ICONS[item.id] || (item.tipo === "extra" ? "⭐" : "📋"))}
                    </div>
                    <div style={{ flex: 1, minWidth: 0 }}>
                      <div style={{ fontSize: 13, fontWeight: 600, color: esPagado ? C.text3 : C.text, textDecoration: esPagado ? "line-through" : "none", whiteSpace: "nowrap", overflow: "hidden", textOverflow: "ellipsis" }}>{item.concepto}</div>
                      <div style={{ display: "flex", alignItems: "center", gap: 6, marginTop: 3 }}>
                        <span style={{ fontSize: 12, fontWeight: 600, color: C.text2 }}>{item.fecha}</span>
                        
                        {item.urgente && !esPagado && <Badge label="urgente" color={C.danger} bg={C.dangerBg} />}
                        {item.expandible && gastado > 0 && <Badge label={fmt(gastado) + " gastado"} color={C.primary} bg={C.primaryBg} />}
                      </div>
                      {item.expandible && gastado > 0 && (
                        <div style={{ height: 3, background: C.border, borderRadius: 99, marginTop: 6 }}>
                          <div style={{ height: "100%", width: pctItem + "%", background: pctItem > 80 ? C.danger : C.primary, borderRadius: 99, transition: "width 0.5s" }} />
                        </div>
                      )}
                    </div>
                    <div style={{ textAlign: "right", flexShrink: 0 }}
                      onTouchStart={e => {
                        if (item.variable && !esPagado && !item.expandible) {
                          e.stopPropagation();
                          const t = setTimeout(() => { setEditandoItem(item.id); }, 600);
                          e.currentTarget._lt = t;
                          e.currentTarget._fired = false;
                        }
                      }}
                      onTouchEnd={e => {
                        if (e.currentTarget._lt) {
                          clearTimeout(e.currentTarget._lt);
                          e.currentTarget._lt = null;
                        }
                      }}
                      onTouchMove={e => { clearTimeout(e.currentTarget._lt); e.currentTarget._lt = null; }}
                    >
                      {item.variable && !esPagado && !item.expandible && editandoItem === item.id ? (
                        <input
                          type="number"
                          autoFocus
                          defaultValue={item.monto}
                          onBlur={e => { const v = parseFloat(e.target.value); if (!isNaN(v) && v >= 0) setPlan(prev => (prev || PLAN_DEFAULT).map(i => i.id === item.id ? {...i, monto: v} : i)); setEditandoItem(null); }}
                          onKeyDown={e => { if (e.key === "Enter") e.target.blur(); if (e.key === "Escape") setEditandoItem(null); }}
                          onClick={e => e.stopPropagation()}
                          onTouchStart={e => e.stopPropagation()}
                          style={{ width: 80, textAlign: "right", fontSize: 14, fontWeight: 700, color: acc.color, background: C.primaryBg, border: "1.5px solid " + C.primary, borderRadius: 8, padding: "3px 6px", outline: "none", fontFamily: "Inter, sans-serif" }}
                        />
                      ) : (
                        <div style={{ fontSize: 14, fontWeight: 700, color: esPagado ? C.success : item.expandible ? (restante < 0 ? C.danger : restante === 0 ? C.text3 : acc.color) : acc.color }}>
                          {item.expandible ? (restante < 0 ? "-$" + fmt(Math.abs(restante)) : "$" + fmt(restante)) : "$" + fmt(item.monto)}
                        </div>
                      )}
                      {item.expandible && <div style={{ fontSize: 9, color: restante < 0 ? C.danger : C.text3 }}>{"de $" + fmt(item.monto)}</div>}
                      {item.usd > 0 && <div style={{ fontSize: 9, color: C.purple }}>{"+ U$S " + item.usd.toFixed(2)}</div>}
                    </div>
                    {item.tipo === "extra" && !esPagado && (
                      <div onClick={e => { e.stopPropagation(); borrarItem(item.id); }} style={{ color: C.text3, fontSize: 12, padding: "4px 6px", cursor: "pointer" }}>✕</div>
                    )}
                    {item.expandible && <div style={{ fontSize: 11, color: C.text3 }}>{isOpen ? "▲" : "▼"}</div>}
                  </div>

                  {isOpen && (
                    <div style={{ background: C.primaryBg, border: "1.5px solid #c7d2fe", borderTop: "none", borderRadius: "0 0 14px 14px", padding: "12px 14px" }}>
                      {subs.length > 0 && (
                        <div style={{ marginBottom: 12 }}>
                          {subs.map(s => (
                            <div key={s.id} style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "7px 0", borderBottom: "1px solid #e0e7ff" }}>
                              <div>
                                <span style={{ fontSize: 12, color: C.text }}>{s.desc}</span>
                                <span style={{ fontSize: 10, color: C.text3, marginLeft: 8 }}>{s.fecha}</span>
                              </div>
                              <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
                                <span style={{ fontSize: 12, fontWeight: 600, color: C.danger }}>-${fmt(s.monto)}</span>
                                <Btn onClick={() => borrarSubGasto(item.id, s.id)} style={{ background: "none", color: C.text3, fontSize: 11, padding: "2px 4px" }}>✕</Btn>
                              </div>
                            </div>
                          ))}
                          <div style={{ display: "flex", justifyContent: "space-between", marginTop: 10 }}>
                            <span style={{ fontSize: 11, color: restante < 0 ? C.danger : C.text2, fontWeight: 600 }}>
                              {restante < 0 ? "⚠️ Excedido" : "Restante"}
                            </span>
                            <span style={{ fontSize: 14, fontWeight: 700, color: restante < 0 ? C.danger : restante < item.monto * 0.2 ? C.warning : C.success }}>
                              {restante < 0 ? "-$" + fmt(Math.abs(restante)) : "$" + fmt(restante)}
                            </span>
                          </div>
                        </div>
                      )}
                      <div style={{ display: "flex", gap: 8 }}>
                        <input type="text" value={inputSub.desc} onChange={e => setInputSub(p => ({ ...p, desc: e.target.value }))} placeholder="Que fue?" style={iStyle({ border: "1.5px solid #c7d2fe", background: "#fff", flex: 1, width: "auto" })} />
                        <input type="number" value={inputSub.monto} onChange={e => setInputSub(p => ({ ...p, monto: e.target.value }))} onKeyDown={e => e.key === "Enter" && agregarSubGasto(item.id)} placeholder="$" style={iStyle({ border: "1.5px solid #c7d2fe", background: "#fff", width: 75 })} />
                        <Btn onClick={() => agregarSubGasto(item.id)} style={{ background: C.primary, padding: "0 12px", color: "#fff", fontWeight: 700, height: 42, boxShadow: C.shadow2 }}>✓</Btn>
                      </div>
                    </div>
                  )}
                </div>
              );
            })}
            {/* Gastos de mi casa - compartidos con Propiedades */}
            {(() => {
              const itemsCasa = safeItemsProp.filter(i => i.propId === "pocitos" && i.tipo === "gasto");
              if (itemsCasa.length === 0) return null;
              const iconCasa = { "gastos comunes":"🏢", "wifi":"📶", "ute (luz)":"💡", "impuesto":"🧾", "contribución":"🏛️", "primaria":"🏛️", "mantenimiento":"🔧" };
              return (
                <div style={{ marginTop: 20, marginBottom: 4 }}>
                  <div style={{ fontSize: 10, color: "#4f46e5", fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", marginBottom: 8, display: "flex", alignItems: "center", gap: 6 }}>
                    🏢 Gastos de mi casa
                  </div>
                  {itemsCasa.map(item => {
                    const pagado = safePagadosProp.includes(item.id);
                    return (
                      <div key={item.id} onClick={() => { if (!safePagadosProp.includes(item.id)) setPagadosProp(prev => [...prev, item.id]); }} onDoubleClick={() => { if (safePagadosProp.includes(item.id)) setPagadosProp(prev => prev.filter(x => x !== item.id)); }}
                        style={{ background: pagado ? "#f0fdf4" : C.surface, borderRadius: 14, padding: "13px 14px", marginBottom: 6, boxShadow: C.shadow, border: "1.5px solid " + (pagado ? "#a7f3d0" : "#4f46e533"), display: "flex", alignItems: "center", gap: 12, cursor: "pointer", transition: "all 0.15s" }}>
                        <div style={{ width: 36, height: 36, borderRadius: 12, flexShrink: 0, background: pagado ? C.success : "#4f46e515", display: "flex", alignItems: "center", justifyContent: "center", fontSize: pagado ? 16 : 18, color: pagado ? "#fff" : "#4f46e5", fontWeight: 700, boxShadow: pagado ? "0 2px 8px rgba(5,150,105,0.35)" : "none" }}>
                          {pagado ? "✓" : (iconCasa[item.concepto.toLowerCase()] || "📌")}
                        </div>
                        <div style={{ flex: 1 }}>
                          <div style={{ fontSize: 13, fontWeight: 600, color: pagado ? C.text3 : C.text, textDecoration: pagado ? "line-through" : "none" }}>{item.concepto}</div>
                          <div style={{ fontSize: 11, color: C.text3, marginTop: 2 }}>{item.fecha}{item.variable ? " · variable" : ""}</div>
                        </div>
                        <div style={{ fontSize: 13, fontWeight: 700, color: pagado ? C.success : "#4f46e5" }}>
                          {item.monto > 0 ? "-$" + fmt(item.monto) : <span style={{ color: C.text3 }}>$0</span>}
                        </div>
                      </div>
                    );
                  })}
                </div>
              );
            })()}

            {!showNuevoItem ? (
              <Btn onClick={() => setShowNuevoItem(true)} style={{ width: "100%", background: C.surface, border: "1.5px dashed " + C.border2, padding: "13px", color: C.text2, fontSize: 12, fontWeight: 500, boxShadow: C.shadow, marginTop: 4, textAlign: "center" }}>
                + Agregar gasto extra
              </Btn>
            ) : (
              <div style={{ background: C.surface, borderRadius: 14, padding: 16, boxShadow: C.shadow2, marginTop: 4, border: "1.5px solid " + C.border }}>
                <div style={{ fontSize: 11, color: C.text2, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1, marginBottom: 12 }}>Nuevo gasto</div>
                <input type="text" value={nuevoItem.concepto} onChange={e => setNuevoItem(p => ({ ...p, concepto: e.target.value }))} placeholder="Descripcion" style={iStyle({ marginBottom: 8 })} />
                <div style={{ display: "flex", gap: 8, marginBottom: 10 }}>
                  <input type="text" value={nuevoItem.fecha} onChange={e => setNuevoItem(p => ({ ...p, fecha: e.target.value }))} placeholder="Fecha" style={iStyle({ flex: 1, width: "auto" })} />
                  <input type="number" value={nuevoItem.monto} onChange={e => setNuevoItem(p => ({ ...p, monto: e.target.value }))} onKeyDown={e => e.key === "Enter" && agregarItem()} placeholder="Monto $" style={iStyle({ flex: 1, width: "auto" })} />
                </div>
                <div style={{ display: "flex", gap: 8 }}>
                  <Btn onClick={() => setShowNuevoItem(false)} style={{ flex: 1, background: C.surface2, padding: "11px", color: C.text2, fontWeight: 500, fontSize: 12, border: "1.5px solid " + C.border }}>Cancelar</Btn>
                  <Btn onClick={agregarItem} style={{ flex: 2, background: C.primary, padding: "11px", color: "#fff", fontWeight: 600, fontSize: 12, boxShadow: C.shadow2 }}>Agregar</Btn>
                </div>
              </div>
            )}
          </div>
        )}
        {tab === "propiedades" && (
          <TabPropiedades
            propiedades={safePropiedades} setPropiedades={setPropiedades}
            itemsProp={safeItemsProp}     setItemsProp={setItemsProp}
            pagadosProp={safePagadosProp} setPagadosProp={setPagadosProp}
          />
        )}
        {tab === "tarjetas" && (
          <div style={{ animation: "fadeUp 0.3s ease" }}>
            <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>Mis tarjetas</div>
            {safeTarjetas.map(t => {
              const esPag = safePagadosTarj.includes(t.id);
              return (
                <div key={t.id} onClick={() => { if (!safePagadosTarj.includes(t.id)) setPagadosTarj(prev => [...prev, t.id]); }} onDoubleClick={() => { if (safePagadosTarj.includes(t.id)) setPagadosTarj(prev => prev.filter(x => x !== t.id)); }}
                  style={{ background: esPag ? "#f0fdf4" : C.surface, borderRadius: 16, padding: 16, marginBottom: 10, boxShadow: esPag ? "0 2px 12px rgba(5,150,105,0.18)" : C.shadow2, border: "1.5px solid " + (esPag ? "#059669" : C.border), cursor: "pointer", transition: "all 0.2s" }}>
                  <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 12 }}>
                    <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                      <div style={{ width: 14, height: 14, borderRadius: "50%", background: t.color, boxShadow: "0 0 8px " + t.color + "66", flexShrink: 0, marginTop: 3 }} />
                      <div>
                        <div style={{ fontSize: 15, fontWeight: 700, color: esPag ? C.text3 : C.text, textDecoration: esPag ? "line-through" : "none" }}>{t.nombre} {esPag ? "✓" : ""}</div>
                        {t.vto ? <div style={{ fontSize: 12, fontWeight: 600, color: C.warning, marginTop: 3 }}>Vto: {t.vto}{t.urgente ? " 🔴" : ""}</div> : <div style={{ fontSize: 11, color: C.text3, marginTop: 3 }}>Sin vencimiento cargado</div>}
                        {t.nota ? <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{t.nota}</div> : null}
                      </div>
                    </div>
                    <div style={{ textAlign: "right" }}>
                      <div style={{ fontSize: 18, fontWeight: 700, color: esPag ? C.success : t.monto === 0 ? C.text3 : t.color, textDecoration: esPag ? "line-through" : "none" }}>
                        {t.monto === 0 ? "—" : "$" + fmt(t.monto)}
                      </div>
                      {t.usd > 0 && <div style={{ fontSize: 10, color: C.purple, marginTop: 2 }}>+ U$S {t.usd.toFixed(2)} (~${fmt(Math.round(t.usd * TC))})</div>}
                    </div>
                  </div>
                  <div style={{ height: 5, background: C.border, borderRadius: 99 }}>
                    <div style={{ height: "100%", width: t.monto > 0 ? Math.min(100, t.monto / 30000 * 100) + "%" : "0%", background: t.color, borderRadius: 99, transition: "width 0.8s" }} />
                  </div>
                </div>
              );
            })}
            {safeTarjetas.length > 0 && (
              <div style={{ background: C.surface, borderRadius: 14, padding: 14, boxShadow: C.shadow2, border: "1.5px solid " + C.primary + "44", marginTop: 4 }}>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 6 }}>
                  <span style={{ fontSize: 13, color: C.text2 }}>Total tarjetas</span>
                  <span style={{ fontSize: 17, fontWeight: 700, color: C.primary }}>${fmt(safeTarjetas.reduce((a, b) => a + b.monto, 0))}</span>
                </div>
                {safeTarjetas.some(t => t.usd > 0) && (
                  <div style={{ fontSize: 11, color: C.purple }}>
                    {"+ U$S " + safeTarjetas.reduce((a, b) => a + b.usd, 0).toFixed(2) + " (~$" + fmt(Math.round(safeTarjetas.reduce((a, b) => a + b.usd, 0) * TC)) + ")"}
                  </div>
                )}
                <div style={{ fontSize: 10, color: C.text3, marginTop: 6 }}>Toca cada tarjeta para marcar como pagada</div>
              </div>
            )}
            {/* Cuotas comprometidas dentro de Tarjetas */}
            <div style={{ marginTop: 16 }}>
              <button onClick={() => setExpandedMes(expandedMes === "__cuotas__" ? null : "__cuotas__")}
                style={{ width: "100%", background: C.surface, borderRadius: expandedMes === "__cuotas__" ? "14px 14px 0 0" : 14, padding: "14px 16px", border: "1.5px solid " + C.border, borderBottom: expandedMes === "__cuotas__" ? "none" : undefined, boxShadow: C.shadow2, display: "flex", justifyContent: "space-between", alignItems: "center", cursor: "pointer", fontFamily: "Inter, sans-serif" }}>
                <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                  <span style={{ fontSize: 20 }}>📅</span>
                  <div style={{ textAlign: "left" }}>
                    <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>Cuotas comprometidas</div>
                    <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>Ver desglose mes a mes</div>
                  </div>
                </div>
                <span style={{ fontSize: 16, color: C.text3 }}>{expandedMes === "__cuotas__" ? "▲" : "▼"}</span>
              </button>
              {expandedMes === "__cuotas__" && (
                <div style={{ background: C.surface2, border: "1.5px solid " + C.border, borderTop: "none", borderRadius: "0 0 14px 14px", padding: "12px 14px" }}>
                  {safeCuotas.map((c, idx) => {
                    const isOpenMes = expandedMes === c.mes;
                    const totalConUSD = c.total + Math.round((c.usd || 0) * TC);
                    const maxTotal = Math.max(...safeCuotas.map(x => x.total + Math.round((x.usd || 0) * TC)), 1);
                    const color = totalConUSD > 20000 ? C.danger : totalConUSD > 10000 ? C.warning : totalConUSD > 0 ? C.success : C.text3;
                    const mesKey = "cuota_" + c.mes;
                    const [mesOpen, setMesOpen] = [expandedMes === mesKey, () => setExpandedMes(expandedMes === mesKey ? "__cuotas__" : mesKey)];
                    return (
                      <div key={c.mes} style={{ marginBottom: idx < safeCuotas.length - 1 ? 8 : 0 }}>
                        <div onClick={setMesOpen} style={{ background: C.surface, borderRadius: mesOpen ? "10px 10px 0 0" : 10, padding: "12px 14px", border: "1.5px solid " + (totalConUSD === 0 ? C.border : color + "44"), borderBottom: mesOpen ? "none" : undefined, display: "flex", justifyContent: "space-between", alignItems: "center", cursor: "pointer" }}>
                          <div>
                            <div style={{ fontSize: 13, fontWeight: 700, color: C.text }}>{c.mes}</div>
                            <div style={{ fontSize: 10, color: C.text3 }}>{c.detalle.filter(d => d.monto > 0).length} tarjeta{c.detalle.filter(d => d.monto > 0).length !== 1 ? "s" : ""} {mesOpen ? "▲" : "▼"}</div>
                          </div>
                          <div style={{ textAlign: "right" }}>
                            <div style={{ fontSize: 15, fontWeight: 700, color: totalConUSD === 0 ? C.text3 : color }}>{totalConUSD === 0 ? "—" : "$" + fmt(totalConUSD)}</div>
                            {c.usd > 0 && <div style={{ fontSize: 9, color: C.purple }}>{"incl. U$S " + c.usd}</div>}
                          </div>
                        </div>
                        {mesOpen && (
                          <div style={{ background: "#fff", border: "1.5px solid " + color + "33", borderTop: "none", borderRadius: "0 0 10px 10px", padding: "8px 14px" }}>
                            {c.detalle.map((d, i) => (
                              <div key={d.tarjeta} style={{ display: "flex", alignItems: "center", gap: 10, padding: "8px 0", borderBottom: i < c.detalle.length - 1 ? "1px solid " + C.border : "none" }}>
                                <div style={{ width: 8, height: 8, borderRadius: "50%", background: d.color, flexShrink: 0 }} />
                                <div style={{ flex: 1 }}>
                                  <div style={{ fontSize: 12, fontWeight: 600, color: C.text }}>{d.tarjeta}</div>
                                  {d.vto ? <div style={{ fontSize: 10, color: C.text3 }}>Vto: {d.vto}</div> : null}
                                </div>
                                <div style={{ textAlign: "right" }}>
                                  <div style={{ fontSize: 13, fontWeight: 700, color: d.monto === 0 ? C.text3 : d.color }}>{d.monto === 0 ? "—" : "$" + fmt(d.monto)}</div>
                                  {d.usd > 0 && <div style={{ fontSize: 9, color: C.purple }}>{"+ U$S " + d.usd.toFixed(2)}</div>}
                                </div>
                              </div>
                            ))}
                          </div>
                        )}
                      </div>
                    );
                  })}
                </div>
              )}
            </div>
          </div>
        )}
        {tab === "cuotas" && (
          <div style={{ animation: "fadeUp 0.3s ease" }}>
            <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>Cuotas comprometidas</div>
            {safeCuotas.map(c => {
              const isOpen = expandedMes === c.mes;
              const maxTotal = Math.max(...safeCuotas.map(x => x.total + Math.round((x.usd || 0) * TC)), 1);
              const totalConUSD = c.total + Math.round((c.usd || 0) * TC);
              const color = totalConUSD > 20000 ? C.danger : totalConUSD > 10000 ? C.warning : totalConUSD > 0 ? C.success : C.text3;
              return (
                <div key={c.mes} style={{ marginBottom: 8 }}>
                  <div onClick={() => setExpandedMes(isOpen ? null : c.mes)}
                    style={{ background: C.surface, borderRadius: isOpen ? "14px 14px 0 0" : 14, padding: "14px", boxShadow: C.shadow, border: "1.5px solid " + (totalConUSD === 0 ? C.border : color + "44"), borderBottom: isOpen ? "none" : undefined, cursor: "pointer", transition: "all 0.2s" }}>
                    <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: totalConUSD > 0 ? 10 : 0 }}>
                      <div>
                        <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{c.mes}</div>
                        <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>
                          {c.detalle.filter(d => d.monto > 0).length} {c.detalle.filter(d => d.monto > 0).length === 1 ? "tarjeta" : "tarjetas"} · toca para ver detalle {isOpen ? "▲" : "▼"}
                        </div>
                      </div>
                      <div style={{ textAlign: "right" }}>
                        <div style={{ fontSize: 16, fontWeight: 700, color: totalConUSD === 0 ? C.text3 : color }}>
                          {totalConUSD === 0 ? "—" : "$" + fmt(totalConUSD)}
                        </div>
                        {c.usd > 0 && <div style={{ fontSize: 10, color: C.purple }}>{"incl. U$S " + c.usd + " (~$" + fmt(Math.round(c.usd * TC)) + ")"}</div>}
                      </div>
                    </div>
                    {totalConUSD > 0 && (
                      <div style={{ height: 5, background: C.border, borderRadius: 99 }}>
                        <div style={{ height: "100%", width: (totalConUSD / maxTotal * 100) + "%", background: color, borderRadius: 99, transition: "width 0.8s ease" }} />
                      </div>
                    )}
                  </div>
                  {isOpen && (
                    <div style={{ background: C.surface2, border: "1.5px solid " + color + "33", borderTop: "none", borderRadius: "0 0 14px 14px", padding: "10px 14px" }}>
                      {c.detalle.map((d, i) => (
                        <div key={d.tarjeta} style={{ display: "flex", alignItems: "center", gap: 12, padding: "10px 0", borderBottom: i < c.detalle.length - 1 ? "1px solid " + C.border : "none" }}>
                          <div style={{ width: 10, height: 10, borderRadius: "50%", background: d.color, flexShrink: 0, boxShadow: "0 0 6px " + d.color + "88" }} />
                          <div style={{ flex: 1 }}>
                            <div style={{ fontSize: 13, fontWeight: 600, color: C.text }}>{d.tarjeta}</div>
                            {d.vto ? <div style={{ fontSize: 10, color: C.text3, marginTop: 1 }}>Vto: {d.vto}</div> : <div style={{ fontSize: 10, color: C.text3 }}>Sin vencimiento</div>}
                          </div>
                          <div style={{ textAlign: "right" }}>
                            <div style={{ fontSize: 14, fontWeight: 700, color: d.monto === 0 ? C.text3 : d.color }}>{d.monto === 0 ? "—" : "$" + fmt(d.monto)}</div>
                            {d.usd > 0 && <div style={{ fontSize: 9, color: C.purple }}>{"+ U$S " + d.usd.toFixed(2)}</div>}
                          </div>
                        </div>
                      ))}
                    </div>
                  )}
                </div>
              );
            })}
            <div style={{ background: C.successBg, borderRadius: 12, padding: "12px 14px", border: "1.5px solid #a7f3d0", marginTop: 8 }}>
              <span style={{ fontSize: 11, color: C.success, fontWeight: 500 }}>Cargá los montos cuando tengas los estados de cuenta</span>
            </div>
          </div>
        )}
        {tab === "fijos" && (
          <div style={{ animation: "fadeUp 0.3s ease" }}>

            {/* Gastos de mi casa — comparten estado con Propiedades */}
            {(() => {
              const miCasa = safePropiedades.find(p => p.id === "pocitos");
              const itemsCasa = safeItemsProp.filter(i => i.propId === "pocitos" && i.tipo === "gasto");
              if (!miCasa || itemsCasa.length === 0) return null;
              const totalCasa = itemsCasa.reduce((a, b) => a + b.monto, 0);
              const pagadosCasa = itemsCasa.filter(i => safePagadosProp.includes(i.id)).reduce((a, b) => a + b.monto, 0);
              const iconCasa = { "gastos comunes":"🏢", "wifi":"📶", "ute (luz)":"💡", "impuesto":"🧾", "contribución":"🏛️", "primaria":"🏛️", "mantenimiento":"🔧" };
              return (
                <div style={{ marginBottom: 18 }}>
                  <div style={{ fontSize: 10, color: "#4f46e5", fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", marginBottom: 8, display: "flex", alignItems: "center", gap: 6 }}>
                    🏢 Gastos de mi casa — Apto Pocitos
                  </div>
                  {itemsCasa.map(item => {
                    const pagado = safePagadosProp.includes(item.id);
                    return (
                      <div key={item.id} onClick={() => { if (!safePagadosProp.includes(item.id)) setPagadosProp(prev => [...prev, item.id]); }} onDoubleClick={() => { if (safePagadosProp.includes(item.id)) setPagadosProp(prev => prev.filter(x => x !== item.id)); }}
                        style={{ background: pagado ? "#f0fdf4" : C.surface, borderRadius: 14, padding: "13px 14px", marginBottom: 6, boxShadow: C.shadow, border: "1.5px solid " + (pagado ? "#a7f3d0" : "#4f46e544"), display: "flex", alignItems: "center", gap: 12, cursor: "pointer", transition: "all 0.15s" }}>
                        <div style={{ width: 36, height: 36, borderRadius: 10, background: pagado ? C.success : "#4f46e522", display: "flex", alignItems: "center", justifyContent: "center", fontSize: pagado ? 16 : 18, color: pagado ? "#fff" : "#4f46e5", fontWeight: 700, boxShadow: pagado ? "0 2px 8px rgba(5,150,105,0.35)" : "none" }}>
                          {pagado ? "✓" : (iconCasa[item.concepto.toLowerCase()] || "📌")}
                        </div>
                        <div style={{ flex: 1 }}>
                          <div style={{ fontSize: 13, fontWeight: 600, color: pagado ? C.text3 : C.text, textDecoration: pagado ? "line-through" : "none" }}>{item.concepto}</div>
                          <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{item.fecha}{item.variable ? " · variable" : ""}</div>
                        </div>
                        <div style={{ fontSize: 13, fontWeight: 700, color: pagado ? C.success : C.text2 }}>
                          {item.monto > 0 ? "-$" + fmt(item.monto) : <span style={{ color: C.text3 }}>$0</span>}
                        </div>
                      </div>
                    );
                  })}
                  <div style={{ background: C.surface, borderRadius: 12, padding: "10px 14px", border: "1.5px solid #4f46e544", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
                    <span style={{ fontSize: 12, color: C.text2 }}>Pagado: <strong style={{ color: C.danger }}>${fmt(pagadosCasa)}</strong> de <strong>${fmt(totalCasa)}</strong></span>
                    <span style={{ fontSize: 12, fontWeight: 700, color: pagadosCasa >= totalCasa ? C.success : C.text2 }}>{pagadosCasa >= totalCasa ? "✓ Todo pago" : "Pendiente: $" + fmt(totalCasa - pagadosCasa)}</span>
                  </div>
                </div>
              );
            })()}

            {/* Préstamos */}
            {PRESTAMOS_DEFAULT.length > 0 && (
              <div style={{ marginBottom: 18 }}>
                <div style={{ fontSize: 10, color: C.warning, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", marginBottom: 8 }}>🏦 Préstamos</div>
                {PRESTAMOS_DEFAULT.map(p => {
                  const [openPrestamo, setOpenPrestamo] = [expandedMes === p.id, () => setExpandedMes(expandedMes === p.id ? null : p.id)];
                  const restanCuotas = p.totalCuotas - p.cuotaActual + 1;
                  const cuotasFuturas = Array.from({ length: restanCuotas }, (_, i) => {
                    const num = p.cuotaActual + i;
                    const mesNombre = ["", "Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep", "Oct", "Nov", "Dic"];
                    const mesBase = 4; // Abril
                    const mesIdx = ((mesBase - 1 + i) % 12) + 1;
                    const anio = 2026 + Math.floor((mesBase - 1 + i) / 12);
                    return { num, label: mesNombre[mesIdx] + " " + anio, vto: "11/" + String(mesIdx).padStart(2,"0") + "/" + anio };
                  });
                  return (
                    <div key={p.id} style={{ marginBottom: 8 }}>
                      <div onClick={setOpenPrestamo} style={{ background: openPrestamo ? "#fffbeb" : C.surface, borderRadius: openPrestamo ? "14px 14px 0 0" : 14, padding: "14px 16px", boxShadow: C.shadow2, border: "1.5px solid " + (openPrestamo ? C.warning : C.border), borderBottom: openPrestamo ? "none" : undefined, cursor: "pointer", transition: "all 0.2s" }}>
                        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
                          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                            <div style={{ width: 40, height: 40, borderRadius: 12, background: "#fef3c7", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 20 }}>🏦</div>
                            <div>
                              <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{p.nombre}</div>
                              <div style={{ fontSize: 11, color: C.text3, marginTop: 2 }}>{p.numero} · Cuota {p.cuotaActual}/{p.totalCuotas}</div>
                              <div style={{ fontSize: 11, color: C.warning, fontWeight: 600, marginTop: 2 }}>Próx. vto: {p.vtoProximo}</div>
                            </div>
                          </div>
                          <div style={{ textAlign: "right" }}>
                            <div style={{ fontSize: 16, fontWeight: 700, color: C.warning }}>${fmt(p.montoCuota)}</div>
                            <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{restanCuotas} cuota{restanCuotas !== 1 ? "s" : ""} restante{restanCuotas !== 1 ? "s" : ""}</div>
                            <div style={{ fontSize: 10, color: C.text3 }}>Total: ${fmt(p.montoCuota * restanCuotas)}</div>
                          </div>
                        </div>
                        <div style={{ marginTop: 10, display: "flex", alignItems: "center", justifyContent: "space-between" }}>
                          <div style={{ flex: 1, height: 6, background: C.border, borderRadius: 99, marginRight: 10 }}>
                            <div style={{ height: "100%", width: (p.cuotaActual / p.totalCuotas * 100) + "%", background: C.warning, borderRadius: 99, transition: "width 0.8s" }} />
                          </div>
                          <span style={{ fontSize: 10, color: C.text3 }}>{openPrestamo ? "▲ Ocultar" : "▼ Ver cuotas"}</span>
                        </div>
                      </div>
                      {openPrestamo && (
                        <div style={{ background: "#fffbeb", border: "1.5px solid " + C.warning + "44", borderTop: "none", borderRadius: "0 0 14px 14px", padding: "12px 16px" }}>
                          <div style={{ fontSize: 10, color: C.warning, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", marginBottom: 10 }}>Proyección de cuotas</div>
                          {cuotasFuturas.map((c, i) => (
                            <div key={c.num} style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "8px 0", borderBottom: i < cuotasFuturas.length - 1 ? "1px solid " + C.border : "none" }}>
                              <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
                                <div style={{ width: 28, height: 28, borderRadius: 8, background: i === 0 ? C.warning : C.surface2, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 11, fontWeight: 700, color: i === 0 ? "#fff" : C.text3 }}>{c.num}</div>
                                <div>
                                  <div style={{ fontSize: 13, fontWeight: i === 0 ? 700 : 500, color: i === 0 ? C.text : C.text2 }}>{c.label}</div>
                                  <div style={{ fontSize: 10, color: C.text3 }}>Vto: {c.vto}</div>
                                </div>
                              </div>
                              <div style={{ fontSize: 13, fontWeight: 600, color: i === 0 ? C.warning : C.text2 }}>${fmt(p.montoCuota)}</div>
                            </div>
                          ))}
                          <div style={{ marginTop: 10, paddingTop: 8, borderTop: "1px solid " + C.border, display: "flex", justifyContent: "space-between" }}>
                            <span style={{ fontSize: 12, color: C.text2 }}>Total restante</span>
                            <span style={{ fontSize: 14, fontWeight: 700, color: C.warning }}>${fmt(p.montoCuota * restanCuotas)}</span>
                          </div>
                        </div>
                      )}
                    </div>
                  );
                })}
              </div>
            )}

            {/* Gastos personales fijos */}
            <div style={{ fontSize: 10, color: C.text3, fontWeight: 600, letterSpacing: 1, textTransform: "uppercase", marginBottom: 8 }}>Gastos personales</div>
            {GASTOS_FIJOS.map(g => {
              const iconMap2 = {"Telefono":"📱","Comida":"🍔","Combustible":"🚗","Entretenimiento":"🎭","Gastos varios":"📦","Club Juana":"⚽","Entrenamiento Tizi":"🏃","Pension alimenticia":"👨‍👧","Patente auto":"🚗"};
              return (
                <div key={g.concepto} style={{ background: C.surface, borderRadius: 14, padding: "13px 14px", marginBottom: 8, boxShadow: C.shadow, border: "1.5px solid " + C.border, display: "flex", alignItems: "center", gap: 12 }}>
                  <div style={{ width: 36, height: 36, borderRadius: 10, background: C.primaryBg, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 20, flexShrink: 0 }}>
                    {iconMap2[g.concepto] || "📋"}
                  </div>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontSize: 13, fontWeight: 500, color: C.text }}>{g.concepto}</div>
                    <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{g.vto}</div>
                  </div>
                  <div style={{ fontSize: 13, fontWeight: 600, color: C.text2 }}>${fmt(g.monto)}</div>
                </div>
              );
            })}
            <div style={{ background: C.surface, borderRadius: 14, padding: 16, boxShadow: C.shadow2, border: "1.5px solid " + C.primary + "44", marginBottom: 18 }}>
              <div style={{ display: "flex", justifyContent: "space-between" }}>
                <span style={{ fontSize: 13, color: C.text2 }}>Total gastos personales</span>
                <span style={{ fontSize: 18, fontWeight: 700, color: C.primary }}>${fmt(GASTOS_FIJOS.reduce((a, b) => a + b.monto, 0))}</span>
              </div>
            </div>

            {/* Suscripciones OCA - solo informativas */}
            <div style={{ fontSize: 10, color: C.purple, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase", marginBottom: 8 }}>💳 Suscripciones OCA</div>
            <div style={{ background: "#fdf4ff", borderRadius: 10, padding: "8px 12px", marginBottom: 10, border: "1px solid #e9d5ff" }}>
              <span style={{ fontSize: 10, color: C.purple }}>Se descuentan automáticamente de la tarjeta OCA cada mes — no requieren pago manual</span>
            </div>
            {SUSCRIPCIONES_OCA.map(s => {
              const montoTotal = s.monto + s.monto2 + Math.round((s.usd || 0) * TC_SUSCR);
              return (
                <div key={s.concepto} style={{ background: C.surface, borderRadius: 14, padding: "13px 14px", marginBottom: 8, boxShadow: C.shadow, border: "1.5px solid #e9d5ff", display: "flex", alignItems: "center", gap: 12 }}>
                  <div style={{ width: 36, height: 36, borderRadius: 10, background: "#f5f3ff", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 18, flexShrink: 0 }}>
                    {s.icono}
                  </div>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontSize: 13, fontWeight: 500, color: C.text }}>{s.concepto}</div>
                    <div style={{ fontSize: 10, color: C.text3, marginTop: 2 }}>{s.nota}</div>
                  </div>
                  <div style={{ textAlign: "right" }}>
                    <div style={{ fontSize: 13, fontWeight: 600, color: C.purple }}>${fmt(montoTotal)}</div>
                    {s.usd > 0 && <div style={{ fontSize: 9, color: C.purple }}>U$S {s.usd.toFixed(2)}</div>}
                  </div>
                </div>
              );
            })}
            <div style={{ background: C.surface, borderRadius: 14, padding: 14, boxShadow: C.shadow2, border: "1.5px solid " + C.purple + "44" }}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
                <span style={{ fontSize: 13, color: C.text2 }}>Total suscripciones/mes</span>
                <span style={{ fontSize: 16, fontWeight: 700, color: C.purple }}>
                  ${fmt(SUSCRIPCIONES_OCA.reduce((a, s) => a + s.monto + s.monto2 + Math.round((s.usd||0)*TC_SUSCR), 0))}
                </span>
              </div>
              <div style={{ fontSize: 10, color: C.text3 }}>Se suman al próximo estado de cuenta OCA</div>
            </div>
          </div>
        )}
        <div style={{ marginTop: 20, display: "flex", gap: 8 }}>
          <button onClick={() => { setShowExport(v => !v); setShowImport(false); }} style={{ flex: 1, padding: "12px", borderRadius: 12, cursor: "pointer", fontFamily: "Inter, sans-serif", fontSize: 12, fontWeight: 600, background: showExport ? C.primaryBg : C.surface, color: C.primary, border: "1.5px solid " + C.primary, boxShadow: C.shadow }}>Backup</button>
          <button onClick={() => { setShowImport(v => !v); setShowExport(false); }} style={{ flex: 1, padding: "12px", borderRadius: 12, cursor: "pointer", fontFamily: "Inter, sans-serif", fontSize: 12, fontWeight: 600, background: showImport ? C.surface2 : C.surface, color: C.text2, border: "1.5px solid " + C.border, boxShadow: C.shadow }}>Restaurar</button>
        </div>
        {showExport && (
          <div style={{ background: C.surface, borderRadius: 14, padding: 16, marginTop: 10, border: "1.5px solid " + C.primary + "44", boxShadow: C.shadow }}>
            <div style={{ fontSize: 12, fontWeight: 600, color: C.text, marginBottom: 4 }}>Tu backup</div>
            <div style={{ fontSize: 11, color: C.text2, marginBottom: 10 }}>Manten presionado - Seleccionar todo - Copiar - Guardar en Notes</div>
            <textarea readOnly value={getBackupText()} rows={5} onClick={e => e.target.select()} onFocus={e => e.target.select()} style={{ width: "100%", background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 8, color: C.text, padding: "8px 10px", fontSize: 10, fontFamily: "monospace", resize: "none", outline: "none", cursor: "pointer" }} />
          </div>
        )}
        {showImport && (
          <div style={{ background: C.surface, borderRadius: 14, padding: 16, marginTop: 10, border: "1.5px solid " + C.border, boxShadow: C.shadow }}>
            <div style={{ fontSize: 12, fontWeight: 600, color: C.text, marginBottom: 4 }}>Restaurar backup</div>
            <textarea value={importText} onChange={e => setImportText(e.target.value)} placeholder="Pega el texto del backup aca..." rows={5} style={{ width: "100%", background: C.surface2, border: "1.5px solid " + C.border, borderRadius: 8, color: C.text, padding: "8px 10px", fontSize: 10, fontFamily: "monospace", resize: "none", outline: "none" }} />
            {importMsg && <div style={{ fontSize: 11, color: importMsg.startsWith("✓") ? C.success : C.danger, marginTop: 6, fontWeight: 600 }}>{importMsg}</div>}
            <div style={{ display: "flex", gap: 8, marginTop: 10 }}>
              <button onClick={() => { setShowImport(false); setImportText(""); }} style={{ flex: 1, padding: "10px", borderRadius: 10, border: "1.5px solid " + C.border, background: C.surface2, color: C.text2, cursor: "pointer", fontFamily: "Inter, sans-serif", fontSize: 12 }}>Cancelar</button>
              <button onClick={importarDatos} disabled={!importText} style={{ flex: 2, padding: "10px", borderRadius: 10, border: "none", background: importText ? C.primary : C.border, color: importText ? "#fff" : C.text3, cursor: importText ? "pointer" : "not-allowed", fontFamily: "Inter, sans-serif", fontSize: 12, fontWeight: 600 }}>Restaurar</button>
            </div>
          </div>
        )}
        <div style={{ marginTop: 10 }}>
          <button onClick={guardarTodo} style={{ width: "100%", padding: "15px", borderRadius: 14, cursor: "pointer", fontFamily: "Inter, sans-serif", fontSize: 13, fontWeight: 600, background: savedMsg === "ok" ? C.success : unsaved ? C.primary : C.surface2, color: savedMsg === "ok" ? "#fff" : unsaved ? "#fff" : C.text3, border: "1.5px solid " + (savedMsg === "ok" ? C.success : unsaved ? C.primary : C.border), boxShadow: savedMsg === "ok" ? C.shadow2 : unsaved ? C.shadow3 : C.shadow, transition: "all 0.3s" }}>
            {savedMsg === "saving" ? "Guardando..." : savedMsg === "ok" ? "✓ Guardado" : savedMsg === "error" ? "\u2717 Error" : unsaved ? "Guardando automaticamente..." : "✓ Todo guardado"}
          </button>
        </div>
      </div>
    </div>
  );
}
