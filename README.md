# Criando-uma-Dashboard-da-Porsche-com-Agentes-de-IA

<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Painel de Vendas Porsche</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#f3f1ed; --surface:#fff; --surface2:#f8f6f2; --line:#e2ded7;
  --ink:#16171a; --ink2:#55575d; --ink3:#85878d;
  --accent:#b8892f; --accent-ink:#8a6420; --bar-bg:#ece8e1;
  --good:#1f7a5a; --prog:#b8892f; --bad:#c4202f;
  --shadow:0 1px 2px rgba(0,0,0,.04);
  --display:"Barlow Condensed","Arial Narrow","Roboto Condensed",sans-serif;
  --body:"Inter",system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;
}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){
  --bg:#0e0f11; --surface:#17181b; --surface2:#1d1f23; --line:#2b2d32;
  --ink:#f1efea; --ink2:#a9abb1; --ink3:#74767c;
  --accent:#d4a84b; --accent-ink:#e3bd68; --bar-bg:#26282d;
  --good:#3fb88b; --prog:#d4a84b; --bad:#ef5a67; --shadow:none;}}
:root[data-theme="dark"]{
  --bg:#0e0f11; --surface:#17181b; --surface2:#1d1f23; --line:#2b2d32;
  --ink:#f1efea; --ink2:#a9abb1; --ink3:#74767c;
  --accent:#d4a84b; --accent-ink:#e3bd68; --bar-bg:#26282d;
  --good:#3fb88b; --prog:#d4a84b; --bad:#ef5a67; --shadow:none;}
*{box-sizing:border-box;margin:0}
body{background:var(--bg);color:var(--ink);font:400 14px/1.45 var(--body);-webkit-font-smoothing:antialiased}
.wrap{max-width:1180px;margin:0 auto;padding:28px 16px 48px}
header{display:flex;justify-content:space-between;align-items:flex-end;gap:16px;flex-wrap:wrap;border-bottom:2px solid var(--ink);padding-bottom:14px}
.eyebrow{font:600 12px var(--body);letter-spacing:.16em;text-transform:uppercase;color:var(--accent-ink)}
h1{font:700 44px/1 var(--display);letter-spacing:.01em;text-transform:uppercase;margin-top:4px}
.sub{color:var(--ink2);max-width:46ch;text-align:right}
.filters{display:flex;gap:10px;flex-wrap:wrap;align-items:flex-end;margin:18px 0 20px}
.f{display:flex;flex-direction:column;gap:4px;min-width:150px;flex:1 1 150px}
.f label{font:600 11px var(--body);letter-spacing:.1em;text-transform:uppercase;color:var(--ink3)}
select{appearance:none;background:var(--surface) url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M1 1l4 4 4-4' fill='none' stroke='%2385878d' stroke-width='1.5'/%3E%3C/svg%3E") no-repeat right 12px center;
  color:var(--ink);border:1px solid var(--line);border-radius:6px;padding:9px 32px 9px 12px;font:500 14px var(--body);cursor:pointer;width:100%}
select:focus-visible,button:focus-visible{outline:2px solid var(--accent);outline-offset:2px}
select.on{border-color:var(--accent)}
.reset{background:none;border:1px solid var(--line);color:var(--ink2);border-radius:6px;padding:9px 14px;font:500 13px var(--body);cursor:pointer;height:40px}
.reset:hover{color:var(--ink);border-color:var(--ink3)}
.kpis{display:grid;grid-template-columns:repeat(5,1fr);gap:12px;margin-bottom:16px}
.kpi{background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:14px 16px;box-shadow:var(--shadow)}
.kpi .l{font:600 11px var(--body);letter-spacing:.1em;text-transform:uppercase;color:var(--ink3)}
.kpi .v{font:600 clamp(24px,2.6vw,34px)/1.1 var(--display);white-space:nowrap;margin-top:6px;font-variant-numeric:tabular-nums}
.kpi .n{font-size:12px;color:var(--ink2);margin-top:2px}
.kpi.hero{border-top:3px solid var(--accent)}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.card{background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:18px 20px 16px;box-shadow:var(--shadow);position:relative}
.card.wide{grid-column:1/-1}
.qn{font:600 11px var(--body);letter-spacing:.14em;color:var(--accent-ink);text-transform:uppercase}
.card h2{font:600 24px/1.15 var(--display);letter-spacing:.01em;margin:4px 0 4px}
.ans{color:var(--ink2);font-size:13px;margin-bottom:14px}
.ans b{color:var(--ink);font-weight:600}
.tg{position:absolute;top:14px;right:14px;background:none;border:1px solid var(--line);color:var(--ink3);border-radius:5px;font:500 11px var(--body);padding:4px 8px;cursor:pointer}
.tg:hover{color:var(--ink)}
.row{display:grid;grid-template-columns:96px 1fr 84px;gap:10px;align-items:center;padding:5px 0;cursor:default}
.row.p{grid-template-columns:118px 1fr 84px 84px}
.row .nm{font-weight:500;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.track{height:18px;background:var(--bar-bg);border-radius:4px;position:relative}
.fill{height:100%;background:var(--accent);border-radius:0 4px 4px 0;min-width:2px;transition:width .35s}
.row:hover .fill{filter:brightness(1.12)}
.row:hover{background:var(--surface2)}
.row .val{text-align:right;font-variant-numeric:tabular-nums;font-weight:600}
.row .val2{text-align:right;font-variant-numeric:tabular-nums;color:var(--ink2)}
.colh{display:grid;grid-template-columns:118px 1fr 84px 84px;gap:10px;font:600 10px var(--body);letter-spacing:.1em;text-transform:uppercase;color:var(--ink3);padding-bottom:4px;border-bottom:1px solid var(--line);margin-bottom:2px}
.colh span:nth-child(n+3){text-align:right}
.seg{display:flex;gap:2px;height:34px;margin:6px 0 16px}
.seg>div{min-width:3px;transition:flex .35s}
.seg>div:first-child{border-radius:5px 0 0 5px}.seg>div:last-child{border-radius:0 5px 5px 0}
.sts{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.st{border-left:3px solid;padding:2px 0 2px 12px}
.st .t{font-weight:600;display:flex;gap:6px;align-items:center}
.st .big{font:600 30px/1.1 var(--display);margin-top:2px;font-variant-numeric:tabular-nums}
.st .s{color:var(--ink2);font-size:12px}
.empty{color:var(--ink3);padding:24px 0;text-align:center}
table.t{width:100%;border-collapse:collapse;font-variant-numeric:tabular-nums}
table.t th,table.t td{text-align:right;padding:6px 8px;border-bottom:1px solid var(--line)}
table.t th:first-child,table.t td:first-child{text-align:left}
table.t th{font:600 10px var(--body);letter-spacing:.1em;text-transform:uppercase;color:var(--ink3)}
.tip{position:fixed;pointer-events:none;background:var(--ink);color:var(--bg);padding:8px 10px;border-radius:6px;font-size:12px;line-height:1.5;z-index:9;opacity:0;transition:opacity .08s;max-width:240px;box-shadow:0 4px 14px rgba(0,0,0,.25)}
.tip b{font-weight:600}
footer{margin-top:18px;color:var(--ink3);font-size:12px;line-height:1.6}
@media(max-width:900px){.kpis{grid-template-columns:repeat(2,1fr)}.kpi:first-child{grid-column:1/-1}.grid{grid-template-columns:1fr}.sub{text-align:left}.sts{grid-template-columns:1fr}h1{font-size:36px}}
@media(max-width:520px){.row{grid-template-columns:76px 1fr 70px}.row.p,.colh{grid-template-columns:96px 1fr 70px}.row.p .val2,.colh span:nth-child(4){display:none}.f{flex-basis:calc(50% - 5px);min-width:0}}
</style>
</head>
<body>
<div class="wrap">
<header>
  <div><div class="eyebrow">Concessionária · Análise comercial</div><h1>Painel de Vendas Porsche</h1></div>
  <p class="sub">Três perguntas de negócio sobre linha de modelos, pagamento e entrega. Use os filtros para recortar.</p>
</header>

<div class="filters" id="filters"></div>
<section class="kpis" id="kpis"></section>

<section class="grid">
  <article class="card" id="c1">
    <div class="qn">Pergunta 1</div><h2>Qual linha de modelos concentra a receita?</h2>
    <p class="ans" id="a1"></p><button class="tg" data-t="1">Ver tabela</button><div id="v1"></div>
  </article>
  <article class="card" id="c2">
    <div class="qn">Pergunta 2</div><h2>Qual forma de pagamento move mais receita, e com que ticket?</h2>
    <p class="ans" id="a2"></p><button class="tg" data-t="2">Ver tabela</button><div id="v2"></div>
  </article>
  <article class="card wide" id="c3">
    <div class="qn">Pergunta 3</div><h2>Quanto da receita já foi entregue e quanto está em risco?</h2>
    <p class="ans" id="a3"></p><button class="tg" data-t="3">Ver tabela</button><div id="v3"></div>
  </article>
</section>

<footer id="foot"></footer>
</div>
<div class="tip" id="tip"></div>

<script>
const DATA = [{"id": 6, "fam": "718", "model": "718 Cayman", "year": 2022, "price": 79500.0, "pay": "Credit Card", "city": "Boston", "state": "MA", "status": "Entregue", "raw": "Delivered"}, {"id": 7, "fam": "911", "model": "911 Turbo S", "year": 2024, "price": 235000.0, "pay": "Wire Transfer", "city": "Seattle", "state": "WA", "status": "Entregue", "raw": "Delivered"}, {"id": 8, "fam": "Cayenne", "model": "Cayenne Coupe", "year": 2023, "price": 112750.0, "pay": "Financing", "city": "Austin", "state": "TX", "status": "Em andamento", "raw": "In Transit"}, {"id": 9, "fam": "Macan", "model": "Macan S", "year": 2021, "price": 68900.0, "pay": "Cash", "city": "Denver", "state": "CO", "status": "Em andamento", "raw": "Pending"}, {"id": 10, "fam": "Taycan", "model": "Taycan 4S", "year": 2024, "price": 121000.0, "pay": "Bank Transfer", "city": "Los Angeles", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 11, "fam": "Panamera", "model": "Panamera 4", "year": 2023, "price": 104500.0, "pay": "Credit Card", "city": "Miami", "state": "FL", "status": "Cancelada", "raw": "Cancelled"}, {"id": 12, "fam": "911", "model": "911 Carrera S", "year": 2020, "price": 96300.0, "pay": "Lease", "city": "New York", "state": "NY", "status": "Entregue", "raw": "Delivered"}, {"id": 13, "fam": "Cayenne", "model": "Cayenne E-Hybrid", "year": 2022, "price": 89750.0, "pay": "Wire Transfer", "city": "San Diego", "state": "CA", "status": "Em andamento", "raw": "Pending Approval"}, {"id": 14, "fam": "718", "model": "718 Boxster", "year": 2021, "price": 73500.0, "pay": "Debit Card", "city": "Chicago", "state": "IL", "status": "Em andamento", "raw": "Shipped"}, {"id": 15, "fam": "Macan", "model": "Macan GTS", "year": 2024, "price": 95000.0, "pay": "Financing", "city": "Phoenix", "state": "AZ", "status": "Em andamento", "raw": "In Transit"}, {"id": 16, "fam": "Taycan", "model": "Taycan Turbo", "year": 2023, "price": 153200.5, "pay": "ACH Payment", "city": "Dallas", "state": "TX", "status": "Entregue", "raw": "Delivered"}, {"id": 17, "fam": "911", "model": "911 GT3", "year": 2024, "price": 241000.0, "pay": "Wire Transfer", "city": "Las Vegas", "state": "NV", "status": "Em andamento", "raw": "Pending"}, {"id": 18, "fam": "Panamera", "model": "Panamera Turbo S", "year": 2022, "price": 132000.0, "pay": "Cash", "city": "San Jose", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 19, "fam": "Cayenne", "model": "Cayenne Turbo GT", "year": 2024, "price": 188000.0, "pay": "Crypto Payment", "city": "Houston", "state": "TX", "status": "Em andamento", "raw": "Awaiting Delivery"}, {"id": 20, "fam": "911", "model": "911 Carrera Cabriolet", "year": 2023, "price": 127800.0, "pay": "Credit Card", "city": "Atlanta", "state": "GA", "status": "Entregue", "raw": "Delivered"}, {"id": 21, "fam": "Macan", "model": "Macan", "year": 2021, "price": 58900.0, "pay": "Bank Transfer", "city": "Orlando", "state": "FL", "status": "Em andamento", "raw": "Pending"}, {"id": 22, "fam": "718", "model": "718 Spyder RS", "year": 2024, "price": 164000.0, "pay": "Financing", "city": "Portland", "state": "OR", "status": "Em andamento", "raw": "In Transit"}, {"id": 23, "fam": "Taycan", "model": "Taycan Cross Turismo", "year": 2023, "price": 118500.0, "pay": "Wire Transfer", "city": "Charlotte", "state": "NC", "status": "Entregue", "raw": "Delivered"}, {"id": 24, "fam": "Cayenne", "model": "Cayenne S", "year": 2022, "price": 91300.0, "pay": "Credit Card", "city": "Nashville", "state": "TN", "status": "Em andamento", "raw": "Pending"}, {"id": 25, "fam": "911", "model": "911 Targa 4S", "year": 2024, "price": 158750.0, "pay": "Lease", "city": "Minneapolis", "state": "MN", "status": "Entregue", "raw": "Delivered"}, {"id": 26, "fam": "Panamera", "model": "Panamera", "year": 2020, "price": 72000.0, "pay": "Bank Transfer", "city": "Philadelphia", "state": "PA", "status": "Cancelada", "raw": "Cancelled"}, {"id": 27, "fam": "Macan", "model": "Macan Electric", "year": 2025, "price": 86500.0, "pay": "Wire Transfer", "city": "San Antonio", "state": "TX", "status": "Entregue", "raw": "Delivered"}, {"id": 28, "fam": "911", "model": "911 Dakar", "year": 2024, "price": 270000.0, "pay": "Cash", "city": "Salt Lake City", "state": "UT", "status": "Em andamento", "raw": "Awaiting Pickup"}, {"id": 29, "fam": "Taycan", "model": "Taycan GTS", "year": 2023, "price": 139000.0, "pay": "Financing", "city": "Raleigh", "state": "NC", "status": "Em andamento", "raw": "In Transit"}, {"id": 30, "fam": "Cayenne", "model": "Cayenne", "year": 2021, "price": 76800.0, "pay": "Credit Card", "city": "Detroit", "state": "MI", "status": "Entregue", "raw": "Delivered"}, {"id": 31, "fam": "718", "model": "718 Cayman GT4 RS", "year": 2024, "price": 173600.0, "pay": "Wire Transfer", "city": "Columbus", "state": "OH", "status": "Em andamento", "raw": "Pending"}, {"id": 32, "fam": "911", "model": "911 Carrera GTS", "year": 2022, "price": 119900.0, "pay": "Cash", "city": "Indianapolis", "state": "IN", "status": "Entregue", "raw": "Delivered"}, {"id": 33, "fam": "Panamera", "model": "Panamera 4 E-Hybrid", "year": 2023, "price": 109250.0, "pay": "Lease", "city": "Fort Worth", "state": "TX", "status": "Em andamento", "raw": "In Transit"}, {"id": 34, "fam": "Macan", "model": "Macan T", "year": 2022, "price": 82000.0, "pay": "Wire Transfer", "city": "Jacksonville", "state": "FL", "status": "Entregue", "raw": "Delivered"}, {"id": 35, "fam": "Taycan", "model": "Taycan Turbo S", "year": 2025, "price": 214000.0, "pay": "Crypto Payment", "city": "San Diego", "state": "CA", "status": "Em andamento", "raw": "Pending Review"}, {"id": 36, "fam": "911", "model": "911 Carrera", "year": 2024, "price": 124500.0, "pay": "Credit Card", "city": "Tampa", "state": "FL", "status": "Entregue", "raw": "Delivered"}, {"id": 37, "fam": "Cayenne", "model": "Cayenne S", "year": 2023, "price": 98200.0, "pay": "Bank Transfer", "city": "Sacramento", "state": "CA", "status": "Em andamento", "raw": "Pending"}, {"id": 38, "fam": "Macan", "model": "Macan", "year": 2022, "price": 67500.0, "pay": "Financing", "city": "Cleveland", "state": "OH", "status": "Entregue", "raw": "Delivered"}, {"id": 39, "fam": "Taycan", "model": "Taycan", "year": 2025, "price": 116900.0, "pay": "Wire Transfer", "city": "Milwaukee", "state": "WI", "status": "Em andamento", "raw": "In Transit"}, {"id": 40, "fam": "Panamera", "model": "Panamera 4S", "year": 2024, "price": 112000.0, "pay": "Cash", "city": "Kansas City", "state": "MO", "status": "Entregue", "raw": "Delivered"}, {"id": 41, "fam": "718", "model": "718 Boxster", "year": 2021, "price": 74000.0, "pay": "Debit Card", "city": "Omaha", "state": "NE", "status": "Cancelada", "raw": "Cancelled"}, {"id": 42, "fam": "911", "model": "911 Turbo", "year": 2024, "price": 198300.0, "pay": "Wire Transfer", "city": "Albuquerque", "state": "NM", "status": "Em andamento", "raw": "Awaiting Delivery"}, {"id": 43, "fam": "Cayenne", "model": "Cayenne Coupe", "year": 2023, "price": 103750.0, "pay": "Wire Transfer", "city": "Tucson", "state": "AZ", "status": "Em andamento", "raw": "Pending Approval"}, {"id": 44, "fam": "Macan", "model": "Macan GTS", "year": 2024, "price": 93600.0, "pay": "Financing", "city": "Fresno", "state": "CA", "status": "Em andamento", "raw": "Shipped"}, {"id": 45, "fam": "Taycan", "model": "Taycan 4S", "year": 2025, "price": 129000.0, "pay": "ACH Payment", "city": "Virginia Beach", "state": "VA", "status": "Em andamento", "raw": "In Transit"}, {"id": 46, "fam": "Panamera", "model": "Panamera Turbo", "year": 2022, "price": 136000.0, "pay": "Credit Card", "city": "Colorado Springs", "state": "CO", "status": "Entregue", "raw": "Delivered"}, {"id": 47, "fam": "911", "model": "911 GT3 RS", "year": 2024, "price": 286500.0, "pay": "Wire Transfer", "city": "Arlington", "state": "TX", "status": "Em andamento", "raw": "Pending"}, {"id": 48, "fam": "Cayenne", "model": "Cayenne E-Hybrid", "year": 2023, "price": 92800.0, "pay": "Lease", "city": "Bakersfield", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 49, "fam": "Macan", "model": "Macan T", "year": 2022, "price": 72400.0, "pay": "Cash", "city": "Mesa", "state": "AZ", "status": "Em andamento", "raw": "Awaiting Pickup"}, {"id": 50, "fam": "Taycan", "model": "Taycan Turbo", "year": 2025, "price": 158500.0, "pay": "Crypto Payment", "city": "Atlanta", "state": "GA", "status": "Entregue", "raw": "Delivered"}, {"id": 51, "fam": "718", "model": "718 Cayman", "year": 2021, "price": 69900.0, "pay": "Bank Transfer", "city": "Long Beach", "state": "CA", "status": "Em andamento", "raw": "Pending"}, {"id": 52, "fam": "911", "model": "911 Targa 4", "year": 2024, "price": 141250.0, "pay": "Financing", "city": "Oakland", "state": "CA", "status": "Em andamento", "raw": "In Transit"}, {"id": 53, "fam": "Panamera", "model": "Panamera", "year": 2020, "price": 71500.0, "pay": "Wire Transfer", "city": "Tulsa", "state": "OK", "status": "Entregue", "raw": "Delivered"}, {"id": 54, "fam": "Cayenne", "model": "Cayenne Turbo", "year": 2023, "price": 146800.0, "pay": "Credit Card", "city": "Wichita", "state": "KS", "status": "Em andamento", "raw": "Pending"}, {"id": 55, "fam": "Macan", "model": "Macan Electric", "year": 2025, "price": 89700.0, "pay": "Lease", "city": "New Orleans", "state": "LA", "status": "Entregue", "raw": "Delivered"}, {"id": 56, "fam": "911", "model": "911 Carrera S", "year": 2022, "price": 104600.0, "pay": "Bank Transfer", "city": "Honolulu", "state": "HI", "status": "Cancelada", "raw": "Cancelled"}, {"id": 57, "fam": "Taycan", "model": "Taycan GTS", "year": 2024, "price": 142000.0, "pay": "Wire Transfer", "city": "Anaheim", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 58, "fam": "Cayenne", "model": "Cayenne", "year": 2021, "price": 78400.0, "pay": "Cash", "city": "Henderson", "state": "NV", "status": "Em andamento", "raw": "Awaiting Review"}, {"id": 59, "fam": "718", "model": "718 Spyder RS", "year": 2025, "price": 169000.0, "pay": "Financing", "city": "Lexington", "state": "KY", "status": "Em andamento", "raw": "In Transit"}, {"id": 60, "fam": "911", "model": "911 Dakar", "year": 2024, "price": 268900.0, "pay": "Credit Card", "city": "Riverside", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 61, "fam": "Panamera", "model": "Panamera 4", "year": 2023, "price": 101300.0, "pay": "Wire Transfer", "city": "Corpus Christi", "state": "TX", "status": "Em andamento", "raw": "Pending"}, {"id": 62, "fam": "Macan", "model": "Macan S", "year": 2021, "price": 66750.0, "pay": "Cash", "city": "St. Louis", "state": "MO", "status": "Entregue", "raw": "Delivered"}, {"id": 63, "fam": "Taycan", "model": "Taycan Cross Turismo", "year": 2024, "price": 127900.0, "pay": "Lease", "city": "Pittsburgh", "state": "PA", "status": "Em andamento", "raw": "In Transit"}, {"id": 64, "fam": "Cayenne", "model": "Cayenne Turbo GT", "year": 2025, "price": 200000.0, "pay": "Wire Transfer", "city": "Cincinnati", "state": "OH", "status": "Entregue", "raw": "Delivered"}, {"id": 65, "fam": "911", "model": "911 Carrera Cabriolet", "year": 2023, "price": 132000.0, "pay": "Crypto Payment", "city": "Anchorage", "state": "AK", "status": "Em andamento", "raw": "Pending Review"}, {"id": 66, "fam": "718", "model": "718 Cayman GT4 RS", "year": 2024, "price": 176400.0, "pay": "Credit Card", "city": "Plano", "state": "TX", "status": "Entregue", "raw": "Delivered"}, {"id": 67, "fam": "Panamera", "model": "Panamera 4 E-Hybrid", "year": 2022, "price": 108500.0, "pay": "Bank Transfer", "city": "Newark", "state": "NJ", "status": "Cancelada", "raw": "Cancelled"}, {"id": 68, "fam": "Macan", "model": "Macan", "year": 2021, "price": 59000.0, "pay": "Financing", "city": "Greensboro", "state": "NC", "status": "Em andamento", "raw": "Awaiting Delivery"}, {"id": 69, "fam": "Taycan", "model": "Taycan Turbo S", "year": 2025, "price": 218000.0, "pay": "Wire Transfer", "city": "Lincoln", "state": "NE", "status": "Em andamento", "raw": "Pending"}, {"id": 70, "fam": "Cayenne", "model": "Cayenne S", "year": 2024, "price": 99950.0, "pay": "Debit Card", "city": "Jersey City", "state": "NJ", "status": "Entregue", "raw": "Delivered"}, {"id": 71, "fam": "911", "model": "911 Carrera GTS", "year": 2024, "price": 121750.0, "pay": "Credit Card", "city": "Chandler", "state": "AZ", "status": "Entregue", "raw": "Delivered"}, {"id": 72, "fam": "718", "model": "718 Boxster GTS", "year": 2023, "price": 91500.0, "pay": "Lease", "city": "Reno", "state": "NV", "status": "Em andamento", "raw": "Shipped"}, {"id": 73, "fam": "Panamera", "model": "Panamera Turbo S", "year": 2022, "price": 134000.0, "pay": "Wire Transfer", "city": "Buffalo", "state": "NY", "status": "Em andamento", "raw": "In Transit"}, {"id": 74, "fam": "Macan", "model": "Macan GTS", "year": 2024, "price": 96800.0, "pay": "ACH Payment", "city": "Durham", "state": "NC", "status": "Entregue", "raw": "Delivered"}, {"id": 75, "fam": "Taycan", "model": "Taycan 4S", "year": 2025, "price": 131600.0, "pay": "Wire Transfer", "city": "Laredo", "state": "TX", "status": "Em andamento", "raw": "Pending Approval"}, {"id": 76, "fam": "Cayenne", "model": "Cayenne E-Hybrid", "year": 2023, "price": 94300.0, "pay": "Cash", "city": "Madison", "state": "WI", "status": "Entregue", "raw": "Delivered"}, {"id": 77, "fam": "911", "model": "911 Turbo S", "year": 2025, "price": 242000.0, "pay": "Crypto Payment", "city": "Lubbock", "state": "TX", "status": "Em andamento", "raw": "Awaiting Pickup"}, {"id": 78, "fam": "718", "model": "718 Cayman S", "year": 2022, "price": 82750.0, "pay": "Credit Card", "city": "Toledo", "state": "OH", "status": "Cancelada", "raw": "Cancelled"}, {"id": 79, "fam": "Macan", "model": "Macan Electric", "year": 2026, "price": 91300.0, "pay": "Wire Transfer", "city": "Irvine", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 80, "fam": "Panamera", "model": "Panamera", "year": 2021, "price": 79900.0, "pay": "Financing", "city": "Garland", "state": "TX", "status": "Em andamento", "raw": "Pending"}, {"id": 81, "fam": "Cayenne", "model": "Cayenne Coupe", "year": 2024, "price": 111000.0, "pay": "Bank Transfer", "city": "Irving", "state": "TX", "status": "Em andamento", "raw": "In Transit"}, {"id": 82, "fam": "911", "model": "911 Targa 4S", "year": 2023, "price": 156500.0, "pay": "Cash", "city": "Chesapeake", "state": "VA", "status": "Entregue", "raw": "Delivered"}, {"id": 83, "fam": "Taycan", "model": "Taycan", "year": 2025, "price": 119900.0, "pay": "Lease", "city": "Scottsdale", "state": "AZ", "status": "Em andamento", "raw": "Pending"}, {"id": 84, "fam": "Macan", "model": "Macan T", "year": 2022, "price": 73200.0, "pay": "Wire Transfer", "city": "Norfolk", "state": "VA", "status": "Entregue", "raw": "Delivered"}, {"id": 85, "fam": "911", "model": "911 GT3", "year": 2024, "price": 224000.0, "pay": "Wire Transfer", "city": "Boise", "state": "ID", "status": "Em andamento", "raw": "Awaiting Delivery"}, {"id": 86, "fam": "911", "model": "911 Carrera", "year": 2024, "price": 126900.0, "pay": "Credit Card", "city": "Orlando", "state": "FL", "status": "Entregue", "raw": "Delivered"}, {"id": 87, "fam": "Cayenne", "model": "Cayenne", "year": 2023, "price": 84500.0, "pay": "Bank Transfer", "city": "San Jose", "state": "CA", "status": "Em andamento", "raw": "Pending"}, {"id": 88, "fam": "Macan", "model": "Macan S", "year": 2022, "price": 69800.0, "pay": "Financing", "city": "Tampa", "state": "FL", "status": "Entregue", "raw": "Delivered"}, {"id": 89, "fam": "Taycan", "model": "Taycan 4S", "year": 2025, "price": 132700.0, "pay": "Wire Transfer", "city": "Denver", "state": "CO", "status": "Em andamento", "raw": "In Transit"}, {"id": 90, "fam": "Panamera", "model": "Panamera", "year": 2021, "price": 81000.0, "pay": "Cash", "city": "Austin", "state": "TX", "status": "Entregue", "raw": "Delivered"}, {"id": 91, "fam": "718", "model": "718 Cayman", "year": 2023, "price": 78900.0, "pay": "Debit Card", "city": "Seattle", "state": "WA", "status": "Cancelada", "raw": "Cancelled"}, {"id": 92, "fam": "911", "model": "911 Turbo S", "year": 2026, "price": 249300.0, "pay": "Wire Transfer", "city": "Boston", "state": "MA", "status": "Em andamento", "raw": "Awaiting Delivery"}, {"id": 93, "fam": "Cayenne", "model": "Cayenne Coupe", "year": 2024, "price": 108750.0, "pay": "Wire Transfer", "city": "Phoenix", "state": "AZ", "status": "Em andamento", "raw": "Pending Approval"}, {"id": 94, "fam": "Macan", "model": "Macan Electric", "year": 2026, "price": 92600.0, "pay": "Financing", "city": "Chicago", "state": "IL", "status": "Em andamento", "raw": "Shipped"}, {"id": 95, "fam": "Taycan", "model": "Taycan Turbo", "year": 2025, "price": 164000.0, "pay": "ACH Payment", "city": "Dallas", "state": "TX", "status": "Em andamento", "raw": "In Transit"}, {"id": 96, "fam": "Panamera", "model": "Panamera 4S", "year": 2024, "price": 119000.0, "pay": "Credit Card", "city": "San Francisco", "state": "CA", "status": "Entregue", "raw": "Delivered"}, {"id": 97, "fam": "911", "model": "911 GT3", "year": 2026, "price": 232500.0, "pay": "Wire Transfer", "city": "Las Vegas", "state": "NV", "status": "Em andamento", "raw": "Pending"}, {"id": 98, "fam": "Cayenne", "model": "Cayenne E-Hybrid", "year": 2023, "price": 96800.0, "pay": "Lease", "city": "Charlotte", "state": "NC", "status": "Entregue", "raw": "Delivered"}, {"id": 99, "fam": "Macan", "model": "Macan T", "year": 2022, "price": 74400.0, "pay": "Cash", "city": "Mesa", "state": "AZ", "status": "Em andamento", "raw": "Awaiting Pickup"}, {"id": 100, "fam": "Taycan", "model": "Taycan GTS", "year": 2025, "price": 148500.0, "pay": "Crypto Payment", "city": "Atlanta", "state": "GA", "status": "Entregue", "raw": "Delivered"}, {"id": 101, "fam": "718", "model": "718 Boxster", "year": 2021, "price": 71900.0, "pay": "Bank Transfer", "city": "Long Beach", "state": "CA", "status": "Em andamento", "raw": "Pending"}, {"id": 102, "fam": "911", "model": "911 Targa 4", "year": 2024, "price": 143250.0, "pay": "Financing", "city": "Oakland", "state": "CA", "status": "Em andamento", "raw": "In Transit"}, {"id": 103, "fam": "Panamera", "model": "Panamera Turbo", "year": 2020, "price": 137500.0, "pay": "Wire Transfer", "city": "Tulsa", "state": "OK", "status": "Entregue", "raw": "Delivered"}, {"id": 104, "fam": "Cayenne", "model": "Cayenne Turbo GT", "year": 2025, "price": 204800.0, "pay": "Credit Card", "city": "Wichita", "state": "KS", "status": "Em andamento", "raw": "Pending"}, {"id": 105, "fam": "911", "model": "911 Dakar", "year": 2024, "price": 271700.0, "pay": "Lease", "city": "New Orleans", "state": "LA", "status": "Entregue", "raw": "Delivered"}];
const FAMS=['911','718','Cayenne','Macan','Taycan','Panamera'];
const $=s=>document.querySelector(s);
const brl=(n,d=0)=>n.toLocaleString('pt-BR',{minimumFractionDigits:d,maximumFractionDigits:d});
const usd=n=>'US$ '+brl(n);
const short=n=>n>=1e6?'US$ '+brl(n/1e6,2)+' mi':n>=1e3?'US$ '+brl(n/1e3,0)+' mil':'US$ '+brl(n);
const pct=(a,b)=>b?brl(a/b*100,0)+'%':'0%';
const sum=a=>a.reduce((s,r)=>s+r.price,0);

// ---------- filtros ----------
const uniq=(k,sortFn)=>[...new Set(DATA.map(r=>r[k]))].sort(sortFn);
const defs=[
 {k:'model',label:'Modelo',opts:()=>FAMS.map(f=>({g:'Linha '+f,items:uniq('model').filter(m=>DATA.find(r=>r.model===m).fam===f)}))},
 {k:'city',label:'Cidade',opts:()=>uniq('city')},
 {k:'year',label:'Ano do modelo',opts:()=>uniq('year',(a,b)=>b-a)},
 {k:'pay',label:'Pagamento',opts:()=>uniq('pay')},
 {k:'status',label:'Situação',opts:()=>['Entregue','Em andamento','Cancelada']},
];
const state={};
const fEl=$('#filters');
defs.forEach(d=>{
  const w=document.createElement('div');w.className='f';
  const id='f_'+d.k;
  let h=`<label for="${id}">${d.label}</label><select id="${id}"><option value="">Todos</option>`;
  d.opts().forEach(o=>{ if(o.g) h+=`<optgroup label="${o.g}">`+o.items.map(i=>`<option>${i}</option>`).join('')+'</optgroup>'; else h+=`<option>${o}</option>`;});
  w.innerHTML=h+'</select>';fEl.append(w);
  w.querySelector('select').onchange=e=>{state[d.k]=e.target.value;e.target.classList.toggle('on',!!e.target.value);render()};
});
const rb=document.createElement('button');rb.className='reset';rb.textContent='Limpar filtros';
rb.onclick=()=>{defs.forEach(d=>{state[d.k]='';const s=$('#f_'+d.k);s.value='';s.classList.remove('on')});render()};
fEl.append(rb);

// ---------- tooltip ----------
const tip=$('#tip');
function bindTip(root){
  root.querySelectorAll('[data-tip]').forEach(el=>{
    el.addEventListener('mousemove',e=>{tip.innerHTML=el.dataset.tip;tip.style.opacity=1;
      const x=Math.min(e.clientX+14,innerWidth-250);tip.style.left=x+'px';tip.style.top=(e.clientY+14)+'px'});
    el.addEventListener('mouseleave',()=>tip.style.opacity=0);
  });
}

// ---------- tabela alternativa ----------
const tableMode={};
document.querySelectorAll('.tg').forEach(b=>b.onclick=()=>{
  const t=b.dataset.t;tableMode[t]=!tableMode[t];b.textContent=tableMode[t]?'Ver gráfico':'Ver tabela';render()});
function table(head,rows){
  return `<table class="t"><thead><tr>${head.map(h=>`<th>${h}</th>`).join('')}</tr></thead><tbody>`+
   rows.map(r=>`<tr>${r.map(c=>`<td>${c}</td>`).join('')}</tr>`).join('')+'</tbody></table>';
}
const group=(rows,key)=>{const m=new Map();rows.forEach(r=>{const g=m.get(r[key])||{k:r[key],n:0,s:0};g.n++;g.s+=r.price;m.set(r[key],g)});return [...m.values()]};

// ---------- render ----------
function render(){
  const rows=DATA.filter(r=>defs.every(d=>!state[d.k]||String(r[d.k])===state[d.k]));
  const live=rows.filter(r=>r.status!=='Cancelada');
  const canc=rows.filter(r=>r.status==='Cancelada');
  const rev=sum(live), deliv=sum(live.filter(r=>r.status==='Entregue'));

  $('#kpis').innerHTML=`
   <div class="kpi hero"><div class="l">Receita</div><div class="v">${short(rev)}</div><div class="n">${usd(rev)} · sem canceladas</div></div>
   <div class="kpi"><div class="l">Vendas</div><div class="v">${live.length}</div><div class="n">de ${rows.length} registros no recorte</div></div>
   <div class="kpi"><div class="l">Ticket médio</div><div class="v">${live.length?short(rev/live.length):'—'}</div><div class="n">por venda ativa</div></div>
   <div class="kpi"><div class="l">Receita entregue</div><div class="v">${pct(deliv,rev)}</div><div class="n">${short(deliv)} · % da receita ativa</div></div>
   <div class="kpi"><div class="l">Canceladas</div><div class="v">${canc.length}</div><div class="n">${short(sum(canc))} perdidos</div></div>`;

  // Q1
  const g1=group(live,'fam').sort((a,b)=>b.s-a.s);
  const q1=$('#v1'),a1=$('#a1');
  if(!g1.length){a1.textContent='';q1.innerHTML='<div class="empty">Nenhuma venda neste recorte.</div>'}
  else{
    const top=g1[0],mx=top.s;
    a1.innerHTML=`A linha <b>${top.k}</b> lidera com <b>${short(top.s)}</b>, ${pct(top.s,rev)} da receita, em ${top.n} venda${top.n>1?'s':''}.`;
    q1.innerHTML=tableMode[1]?table(['Linha','Vendas','Receita','% receita'],g1.map(g=>[g.k,g.n,usd(g.s),pct(g.s,rev)])):
     g1.map(g=>`<div class="row" data-tip="<b>${g.k}</b><br>Receita: ${usd(g.s)}<br>${g.n} vendas · ${pct(g.s,rev)} do total<br>Ticket médio: ${usd(g.s/g.n)}">
       <span class="nm">${g.k}</span><div class="track"><div class="fill" style="width:${g.s/mx*100}%"></div></div><span class="val">${short(g.s)}</span></div>`).join('');
  }

  // Q2
  const g2=group(live,'pay').sort((a,b)=>b.s-a.s);
  const q2=$('#v2'),a2=$('#a2');
  if(!g2.length){a2.textContent='';q2.innerHTML='<div class="empty">Nenhuma venda neste recorte.</div>'}
  else{
    const top=g2[0],mx=top.s,tk=[...g2].sort((a,b)=>b.s/b.n-a.s/a.n)[0];
    a2.innerHTML=`<b>${top.k}</b> é o maior canal: ${short(top.s)} (${pct(top.s,rev)}). O maior ticket médio é de <b>${tk.k}</b>, ${short(tk.s/tk.n)}${tk.n<3?' (poucas vendas)':''}.`;
    q2.innerHTML=tableMode[2]?table(['Pagamento','Vendas','Receita','Ticket médio'],g2.map(g=>[g.k,g.n,usd(g.s),usd(g.s/g.n)])):
     `<div class="colh"><span>Método</span><span></span><span>Receita</span><span>Ticket</span></div>`+
     g2.map(g=>`<div class="row p" data-tip="<b>${g.k}</b><br>Receita: ${usd(g.s)}<br>${g.n} vendas · ${pct(g.s,rev)} do total<br>Ticket médio: ${usd(g.s/g.n)}">
       <span class="nm">${g.k}</span><div class="track"><div class="fill" style="width:${g.s/mx*100}%"></div></div><span class="val">${short(g.s)}</span><span class="val2">${short(g.s/g.n)}</span></div>`).join('');
  }

  // Q3 (inclui canceladas)
  const order=[['Entregue','--good','✓'],['Em andamento','--prog','…'],['Cancelada','--bad','✕']];
  const tot=sum(rows),q3=$('#v3'),a3=$('#a3');
  const gs=order.map(([k,c,i])=>{const r=rows.filter(x=>x.status===k);return{k,c,i,n:r.length,s:sum(r)}});
  if(!rows.length){a3.textContent='';q3.innerHTML='<div class="empty">Nenhuma venda neste recorte.</div>'}
  else{
    const andam=gs[1],cn=gs[2];
    a3.innerHTML=`Só <b>${pct(gs[0].s,tot)}</b> do valor bruto (${short(tot)}, incluindo canceladas) já foi entregue. <b>${short(andam.s)}</b> ainda dependem de entrega e <b>${short(cn.s)}</b> foram cancelados.`;
    q3.innerHTML=tableMode[3]?table(['Situação','Vendas','Valor','% do valor'],gs.map(g=>[g.k,g.n,usd(g.s),pct(g.s,tot)])):
     `<div class="seg">`+gs.filter(g=>g.s>0).map(g=>`<div style="flex:${g.s};background:var(${g.c})" data-tip="<b>${g.k}</b><br>${usd(g.s)} · ${pct(g.s,tot)}<br>${g.n} vendas"></div>`).join('')+`</div>
     <div class="sts">`+gs.map(g=>`<div class="st" style="border-color:var(${g.c})"><div class="t"><span style="color:var(${g.c})">${g.i}</span>${g.k}</div>
       <div class="big">${short(g.s)}</div><div class="s">${pct(g.s,tot)} do valor · ${g.n} venda${g.n!==1?'s':''}</div></div>`).join('')+`</div>`;
  }
  bindTip($('.grid'));
}
$('#foot').innerHTML=`Fonte: base de ${DATA.length} vendas. Receita, vendas e ticket excluem vendas canceladas (a Pergunta 3 as inclui). Valores em dólares (US$).
 Linha de modelo derivada do nome do modelo; situações de entrega agrupadas em Entregue / Em andamento (pendente, em trânsito, aguardando, enviado) / Cancelada.
 <br>Datas de venda não foram usadas como filtro: 24 registros têm data "INVALID" e muitas datas estão no futuro; por isso o filtro de ano é o <b>ano do modelo</b>.`;
render();
</script>
</body>
</html>
