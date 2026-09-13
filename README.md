<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Ideas Bolivia</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: #f3f6fb;
      color: #17233a;
      font-family: system-ui, sans-serif;
    }
    main { max-width: 1050px; margin: auto; padding: 28px 18px; }
    h1 { margin-bottom: 8px; }
    .intro { color: #526078; line-height: 1.6; }
    .panel, article {
      background: white;
      padding: 22px;
      border-radius: 16px;
      box-shadow: 0 5px 20px #1423400a;
    }
    .filters {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
      gap: 16px;
    }
    label { display: block; font-weight: 650; }
    select, button {
      width: 100%;
      padding: 12px;
      border-radius: 9px;
      font: inherit;
      margin-top: 8px;
    }
    select { border: 1px solid #ccd5e2; background: white; }
    button {
      background: #156d4b;
      color: white;
      border: none;
      cursor: pointer;
      font-weight: 700;
    }
    button:hover { background: #10583c; }
    button:focus-visible, select:focus-visible {
      outline: 3px solid #e8aa19;
      outline-offset: 3px;
    }
    .notice {
      background: #fff5d9;
      padding: 15px;
      border-radius: 10px;
      line-height: 1.5;
      margin: 20px 0;
    }
    #results {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 18px;
    }
    article h2 { font-size: 1.2rem; margin-top: 12px; }
    article p { line-height: 1.55; }
    .tag {
      display: inline-block;
      background: #e8f5ef;
      color: #14583e;
      border-radius: 20px;
      padding: 5px 10px;
      font-size: .8rem;
    }
    .validation {
      border-top: 1px solid #e3e8f0;
      padding-top: 12px;
    }
    footer { margin-top: 28px; color: #526078; line-height: 1.6; }
  </style>
</head>
<body>
<main>
  <h1>🇧🇴 Ideas Bolivia</h1>
  <p class="intro">
    Explora oportunidades de negocio y aprende a validar
    necesidades antes de invertir.
  </p>

  <section class="panel" aria-label="Filtros de oportunidades">
    <div class="filters">
      <label>¿Dónde quieres emprender?
        <select id="zone">
          <option value="all">Cualquier zona</option>
          <option value="urban">Ciudad</option>
          <option value="periurban">Zona periurbana</option>
          <option value="rural">Área rural</option>
        </select>
      </label>

      <label>Presupuesto disponible
        <select id="budget">
          <option value="all">Cualquier presupuesto</option>
          <option value="low">Hasta Bs 5.000</option>
          <option value="medium">De Bs 5.001 a Bs 20.000</option>
          <option value="high">Más de Bs 20.000</option>
        </select>
      </label>

      <label>Sector de interés
        <select id="sector">
          <option value="all">Todos los sectores</option>
          <option value="digital">Servicios digitales</option>
          <option value="agro">Agro y alimentos</option>
          <option value="environment">Medioambiente</option>
          <option value="services">Servicios cotidianos</option>
        </select>
      </label>
    </div>
    <button id="generate">Buscar ideas para mí</button>
  </section>

  <div class="notice">
    <strong>Ideas para investigar, no oportunidades confirmadas.</strong>
    Esta versión utiliza un catálogo de hipótesis: no consulta datos
    en tiempo real ni comprueba que un negocio sea inexistente.
    Los presupuestos son orientativos para un piloto pequeño, no cotizaciones.
  </div>

  <p id="count" role="status" aria-live="polite"></p>
  <section id="results" aria-label="Ideas de negocio"></section>

  <footer>
    <strong>Antes de invertir:</strong> consulta información del INE,
    registros públicos de SEPREC y requisitos de tu municipio.
    Revisa también Google Maps, Facebook Marketplace y negocios del barrio:
    los registros formales no muestran toda la competencia.
    Si manipulas alimentos o residuos, verifica los requisitos
    sanitarios y ambientales aplicables.
  </footer>
</main>

<script>
const ideas = [
  {
    name: "Asistente digital para pequeños comercios",
    sector: "digital",
    zones: ["urban", "periurban", "rural"],
    budget: "low",
    need: "Algunos comercios podrían necesitar ayuda para organizar pedidos, catálogos y cuentas sin sistemas complicados.",
    solution: "Configurar WhatsApp Business, catálogos y una plantilla sencilla de ventas; ofrecer capacitación y soporte.",
    income: "Cobro por instalación y una mensualidad opcional de asistencia.",
    pilot: "Usar tu propio celular o computadora y herramientas gratuitas, sin desarrollar software al inicio.",
    validation: "Entrevista a 15 comercios de un mismo rubro. Ofrece 3 pilotos pagados y mide si ahorran tiempo.",
    competition: "Busca agencias, freelancers y sistemas de venta locales. Prueba diferenciarte por rubro, idioma o atención presencial."
  },
  {
    name: "Venta de excedentes alimentarios por reserva",
    sector: "agro",
    zones: ["urban", "periurban"],
    budget: "low",
    need: "Panaderías y otros comercios podrían tener productos aptos para consumo que no logran vender al final del día.",
    solution: "Publicar paquetes con descuento en un catálogo y permitir que el cliente los reserve y recoja.",
    income: "Comisión por venta o una tarifa acordada con cada comercio.",
    pilot: "Coordinar manualmente con 3 locales, sin almacén ni reparto propio.",
    validation: "Registra excedentes durante una semana y prueba reservas reales. Define alérgenos, conservación y responsabilidades sanitarias.",
    competition: "Revisa promociones de cierre, grupos de descuentos y servicios de entrega existentes."
  },
  {
    name: "Alquiler compartido de herramientas agrícolas",
    sector: "agro",
    zones: ["rural", "periurban"],
    budget: "medium",
    need: "Productores pequeños podrían requerir herramientas por pocos días sin poder justificar su compra.",
    solution: "Coordinar alquileres de herramientas pequeñas con mantenimiento, capacitación y reglas de devolución.",
    income: "Tarifa por día de uso o comisión por conectar propietarios y usuarios.",
    pilot: "Empezar con herramientas de terceros o pocas unidades, no con maquinaria pesada.",
    validation: "Consulta a 20 productores sobre herramientas, temporadas y precios aceptables. Consigue reservas antes de comprar.",
    competition: "Investiga alquileres existentes, asociaciones y equipos comunitarios. Considera transporte, daños y seguridad."
  },
  {
    name: "Recolección programada de reciclables para negocios",
    sector: "environment",
    zones: ["urban", "periurban"],
    budget: "medium",
    need: "Algunos negocios podrían necesitar retiros más predecibles de cartón, plástico y otros materiales aprovechables.",
    solution: "Organizar rutas de recolección selectiva y entregar comprobantes simples de retiro.",
    income: "Tarifa por servicio, más venta de materiales cuando resulte viable.",
    pilot: "Trabajar con un reciclador local y transporte contratado, sin comprar vehículo al inicio.",
    validation: "Habla con 10 negocios y 2 compradores de material. Calcula costos por ruta y confirma quién pagaría.",
    competition: "Mapea recicladores de base y operadores actuales. Prioriza alianzas y evita residuos peligrosos."
  },
  {
    name: "Mantenimiento domiciliario con visitas programadas",
    sector: "services",
    zones: ["urban", "periurban"],
    budget: "low",
    need: "Algunos hogares y pequeños locales podrían tener dificultades para encontrar técnicos puntuales y con precios claros.",
    solution: "Coordinar profesionales con referencias, presupuestos previos y seguimiento del trabajo.",
    income: "Comisión por trabajo o tarifa de coordinación claramente informada.",
    pilot: "Crear una red de 3 técnicos y gestionar reservas por teléfono o WhatsApp, sin oficina.",
    validation: "Realiza 5 servicios pagados. Mide puntualidad, reclamos y margen después de transporte y correcciones.",
    competition: "Compara técnicos independientes, empresas y grupos barriales. Verifica competencias para trabajos de riesgo."
  },
  {
    name: "Secado de productos agrícolas por encargo",
    sector: "agro",
    zones: ["rural", "periurban"],
    budget: "high",
    need: "En ciertas cadenas agrícolas podría existir interés en conservar excedentes o transformar productos de temporada.",
    solution: "Ofrecer secado o deshidratado por lotes mediante equipos adecuados al producto.",
    income: "Cobro por lote procesado y, posteriormente, venta de productos propios.",
    pilot: "Ensayar primero con equipos alquilados o un procesador habilitado antes de construir instalaciones.",
    validation: "Elige un solo producto. Confirma oferta estacional, compradores, inocuidad, consumo energético y costo por kilo final.",
    competition: "Investiga procesadores, asociaciones y marcas existentes. No compres equipos sin pruebas técnicas y comerciales."
  }
];

const sectorNames = {
  digital: "Servicios digitales",
  agro: "Agro y alimentos",
  environment: "Medioambiente",
  services: "Servicios cotidianos"
};

const budgetNames = {
  low: "Piloto: hasta Bs 5.000",
  medium: "Piloto: Bs 5.001–20.000",
  high: "Piloto: más de Bs 20.000"
};

function addParagraph(card, title, text, className = "") {
  const p = document.createElement("p");
  p.className = className;
  const strong = document.createElement("strong");
  strong.textContent = title + " ";
  p.append(strong, document.createTextNode(text));
  card.appendChild(p);
}

function generate() {
  const zone = document.getElementById("zone").value;
  const budget = document.getElementById("budget").value;
  const sector = document.getElementById("sector").value;
  const levels = { low: 1, medium: 2, high: 3 };

  const matches = ideas.filter(idea =>
    (zone === "all" || idea.zones.includes(zone)) &&
    (budget === "all" || levels[idea.budget] <= levels[budget]) &&
    (sector === "all" || idea.sector === sector)
  );

  const results = document.getElementById("results");
  results.replaceChildren();

  document.getElementById("count").textContent = matches.length
    ? `${matches.length} ideas compatibles. No están ordenadas por rentabilidad.`
    : "No hay coincidencias en este catálogo. Prueba ampliar los filtros.";

  for (const idea of matches) {
    const card = document.createElement("article");
    const tag = document.createElement("span");
    tag.className = "tag";
    tag.textContent = sectorNames[idea.sector];

    const title = document.createElement("h2");
    title.textContent = idea.name;
    card.append(tag, title);

    addParagraph(card, "Necesidad por comprobar:", idea.need);
    addParagraph(card, "Propuesta:", idea.solution);
    addParagraph(card, "Cómo cobrar:", idea.income);
    addParagraph(card, budgetNames[idea.budget] + ".", idea.pilot);
    addParagraph(card, "Cómo validar:", idea.validation, "validation");
    addParagraph(card, "Competencia:", idea.competition);

    results.appendChild(card);
  }
}

document.getElementById("generate").addEventListener("click", generate);
generate();
</script>
</body>
</html>
