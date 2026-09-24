<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <meta
    name="description"
    content="Interactive HTML5 diagram showing altitude, ear pressure, eardrum movement, and Eustachian tube airflow."
  >
  <title>Altitude and Ear Pressure</title>
  <style>
    :root {
      --navy: #17324d;
      --text: #30465b;
      --muted: #667b8e;
      --border: #d7e2ea;
      --panel: #ffffff;
      --page: #edf5f9;
      --outside: #1f77b4;
      --middle: #d26a32;
      --altitude: #247b69;
      --eardrum: #d1495b;
      --airflow: #218739;
      --inner-ear: #4f8e8a;
      --bone: #8f6426;
      --success: #218739;
      --warning: #b55b20;
      --danger: #d1495b;
      --shadow:
        0 14px 35px rgba(28, 63, 88, 0.1);
    }
    * {
      box-sizing: border-box;
    }
    html {
      color-scheme: light;
    }
    body {
      min-height: 100vh;
      margin: 0;
      padding: 24px;
      color: var(--text);
      background:
        radial-gradient(
          circle at top,
          #ffffff 0,
          #eef7fb 48%,
          #e7f0f5 100%
        );
      font-family:
        Inter,
        Arial,
        Helvetica,
        sans-serif;
    }
    button,
    input {
      font: inherit;
    }
    button {
      touch-action: manipulation;
    }
    .app {
      width: min(1380px, 100%);
      margin: 0 auto;
    }
    .page-header {
      margin-bottom: 22px;
      text-align: center;
    }
    .eyebrow {
      margin: 0 0 7px;
      color: var(--altitude);
      font-size: 0.78rem;
      font-weight: 800;
      letter-spacing: 0.11em;
      text-transform: uppercase;
    }
    .page-header h1 {
      margin: 0 0 8px;
      color: var(--navy);
      font-size: clamp(1.85rem, 4vw, 3rem);
      line-height: 1.1;
    }
    .page-header-description {
      max-width: 790px;
      margin: 0 auto;
      color: var(--muted);
      font-size: 1rem;
      line-height: 1.6;
    }
    .layout {
      display: grid;
      grid-template-columns:
        minmax(0, 1.72fr)
        minmax(320px, 0.68fr);
      gap: 20px;
      align-items: start;
    }
    .card {
      overflow: hidden;
      border: 1px solid var(--border);
      border-radius: 20px;
      background: var(--panel);
      box-shadow: var(--shadow);
    }
    /* Diagram */
    .diagram-card {
      padding: 16px;
    }
    .diagram-heading {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 12px;
    }
    .diagram-heading h2 {
      margin: 0;
      color: var(--navy);
      font-size: 1.2rem;
    }
    .diagram-heading p {
      margin: 4px 0 0;
      color: var(--muted);
      font-size: 0.84rem;
      line-height: 1.45;
    }
    .difference-badge {
      flex: 0 0 auto;
      min-width: 150px;
      padding: 9px 12px;
      border: 1px solid var(--border);
      border-radius: 12px;
      background: #f3f7f9;
      text-align: center;
    }
    .difference-badge span {
      display: block;
      color: var(--muted);
      font-size: 0.66rem;
      font-weight: 750;
      letter-spacing: 0.05em;
      text-transform: uppercase;
    }
    .difference-badge strong {
      display: block;
      margin-top: 3px;
      color: var(--success);
      font-size: 1rem;
    }
    .diagram-frame {
      overflow: hidden;
      border: 1px solid #dce8ef;
      border-radius: 16px;
      background: #f8fcff;
    }
    .ear-svg {
      display: block;
      width: 100%;
      height: auto;
      min-height: 440px;
    }
    .legend {
      display: flex;
      flex-wrap: wrap;
      gap: 12px 18px;
      margin-top: 14px;
      color: var(--muted);
      font-size: 0.82rem;
    }
    .legend-item {
      display: inline-flex;
      gap: 7px;
      align-items: center;
    }
    .legend-dot {
      width: 11px;
      height: 11px;
      border-radius: 50%;
    }
    .legend-line {
      width: 20px;
      height: 4px;
      border-radius: 999px;
      background: var(--eardrum);
    }
    .legend-arrow {
      color: var(--airflow);
      font-size: 1rem;
      font-weight: 900;
      line-height: 1;
    }
    /* Side panel */
    .side-panel {
      display: grid;
      gap: 18px;
    }
    .controls-card,
    .information-card {
      padding: 22px;
    }
    .card-title {
      margin: 0;
      color: var(--navy);
      font-size: 1.2rem;
    }
    .controls-intro {
      margin: 6px 0 23px;
      color: var(--muted);
      font-size: 0.86rem;
      line-height: 1.5;
    }
    .control-group + .control-group {
      margin-top: 23px;
    }
    .control-heading {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      margin-bottom: 10px;
    }
    .control-heading label {
      color: var(--navy);
      font-size: 0.9rem;
      font-weight: 750;
    }
    .value-badge {
      min-width: 88px;
      padding: 7px 10px;
      border-radius: 9px;
      color: #ffffff;
      text-align: center;
      white-space: nowrap;
      font-size: 0.8rem;
      font-weight: 800;
    }
    .value-badge.outside {
      background: var(--outside);
    }
    .value-badge.middle {
      background: var(--middle);
    }
    .value-badge.altitude {
      min-width: 142px;
      background: var(--altitude);
    }
    input[type="range"] {
      width: 100%;
      height: 8px;
      margin: 5px 0;
      cursor: pointer;
    }
    #outsidePressure {
      accent-color: var(--outside);
    }
    #middlePressure {
      accent-color: var(--middle);
    }
    #altitude {
      accent-color: var(--altitude);
    }
    .range-scale {
      display: flex;
      justify-content: space-between;
      gap: 10px;
      margin-top: 4px;
      color: var(--muted);
      font-size: 0.69rem;
    }
    .range-scale span:nth-child(2) {
      text-align: center;
    }
    .range-scale span:last-child {
      text-align: right;
    }
    .altitude-section {
      margin-top: 25px;
      padding-top: 23px;
      border-top: 1px solid var(--border);
    }
    .altitude-help {
      margin: 11px 0 0;
      color: var(--muted);
      font-size: 0.8rem;
      line-height: 1.5;
    }
    .tube-control {
      margin-top: 24px;
      padding: 15px;
      border: 1px solid var(--border);
      border-radius: 14px;
      background: #f5f8fa;
    }
    .tube-control-heading {
      margin-bottom: 12px;
    }
    .tube-control-heading strong {
      display: block;
      color: var(--navy);
      font-size: 0.9rem;
    }
    .tube-control-heading p {
      margin: 4px 0 0;
      color: var(--muted);
      font-size: 0.76rem;
      line-height: 1.45;
    }
    .tube-button {
      display: flex;
      width: 100%;
      min-height: 48px;
      align-items: center;
      justify-content: center;
      gap: 10px;
      padding: 11px 16px;
      border: 1px solid #9eafb8;
      border-radius: 11px;
      color: var(--navy);
      background: #ffffff;
      cursor: pointer;
      font-weight: 800;
      transition:
        color 160ms ease,
        border-color 160ms ease,
        background 160ms ease,
        transform 100ms ease;
    }
    .tube-button:hover {
      border-color: var(--altitude);
      background: #edf8f4;
    }
    .tube-button[aria-pressed="true"] {
      border-color: var(--altitude);
      color: #ffffff;
      background: var(--altitude);
    }
    .tube-button:active,
    .reset-button:active {
      transform: translateY(1px);
    }
    .tube-button-icon {
      width: 11px;
      height: 11px;
      border: 2px solid currentColor;
      border-radius: 50%;
      background: transparent;
    }
    .tube-button[aria-pressed="true"] .tube-button-icon {
      background: #ffffff;
      box-shadow:
        0 0 0 3px rgba(255, 255, 255, 0.22);
    }
    .airflow-status {
      margin: 12px 0 0;
      padding: 11px 12px;
      border: 1px solid #d8e3e7;
      border-radius: 11px;
      color: var(--muted);
      background: #f4f7f8;
      font-size: 0.78rem;
      font-weight: 700;
      line-height: 1.45;
      transition:
        color 180ms ease,
        border-color 180ms ease,
        background 180ms ease;
    }
    .airflow-status.active {
      border-color: #b9d9ca;
      color: #176451;
      background: #edf9f4;
    }
    .airflow-status.balanced {
      border-color: #cce1d2;
      color: var(--success);
      background: #f1faf3;
    }
    .reset-button {
      width: 100%;
      margin-top: 17px;
      padding: 12px 18px;
      border: 1px solid #b9c9d0;
      border-radius: 11px;
      color: var(--text);
      background: #ffffff;
      cursor: pointer;
      font-weight: 750;
      transition:
        background 150ms ease,
        border-color 150ms ease,
        transform 100ms ease;
    }
    .reset-button:hover {
      border-color: #8fa5af;
      background: #f2f7f9;
    }
    button:focus-visible,
    input:focus-visible {
      outline: 3px solid rgba(31, 119, 180, 0.25);
      outline-offset: 3px;
    }
    /* Information */
    .information-card .card-title {
      margin-bottom: 17px;
    }
    .condition-box {
      padding: 16px;
      border-left: 6px solid var(--success);
      border-radius: 14px;
      background: #effaf2;
      transition:
        border-color 180ms ease,
        background 180ms ease;
    }
    .condition-box h3 {
      margin: 0 0 7px;
      color: var(--navy);
      font-size: 1.08rem;
    }
    .condition-box p {
      margin: 0;
      color: var(--text);
      font-size: 0.88rem;
      line-height: 1.55;
    }
    .note {
      margin-top: 16px;
      padding: 13px 14px;
      border-radius: 12px;
      color: #52697b;
      background: #f2f6f9;
      font-size: 0.82rem;
      line-height: 1.5;
    }
    .footer {
      margin-top: 18px;
      color: var(--muted);
      text-align: center;
      font-size: 0.8rem;
    }
    /* SVG labels */
    .label-box {
      fill: rgba(255, 255, 255, 0.98);
      stroke: #cbd9e2;
      stroke-width: 2;
      filter: url("#labelShadow");
    }
    .label-box.eardrum-box {
      stroke: var(--eardrum);
    }
    .label-box.inner-box {
      stroke: var(--inner-ear);
    }
    .label-box.bone-box {
      stroke: var(--bone);
    }
    .label-box.outside-box {
      stroke: var(--outside);
    }
    .label-box.pressure-box {
      stroke: var(--middle);
    }
    .label-box.tube-box {
      stroke: #a66b49;
    }
    .svg-label {
      fill: var(--navy);
      font-size: 16px;
      font-weight: 800;
    }
    .svg-small-label {
      fill: #52677a;
      font-size: 12px;
      font-weight: 650;
    }
    .label-line {
      fill: none;
      stroke: #52677a;
      stroke-width: 2;
      stroke-linecap: round;
      stroke-linejoin: round;
    }
    .label-line.eardrum-line {
      stroke: var(--eardrum);
    }
    .label-line.inner-line {
      stroke: var(--inner-ear);
    }
    .label-line.bone-line {
      stroke: var(--bone);
    }
    .label-line.outside-line {
      stroke: var(--outside);
    }
    .label-line.pressure-line {
      stroke: var(--middle);
    }
    .label-line.tube-line {
      stroke: #9c6242;
    }
    .label-anchor {
      fill: #52677a;
      stroke: #ffffff;
      stroke-width: 1.5;
    }
    .label-anchor.eardrum-anchor {
      fill: var(--eardrum);
    }
    .label-anchor.inner-anchor {
      fill: var(--inner-ear);
    }
    .label-anchor.bone-anchor {
      fill: var(--bone);
    }
    .label-anchor.outside-anchor {
      fill: var(--outside);
    }
    .label-anchor.pressure-anchor {
      fill: var(--middle);
    }
    .label-anchor.tube-anchor {
      fill: #9c6242;
    }
    .pressure-arrow {
      transition:
        opacity 180ms ease,
        stroke-width 180ms ease;
    }
    #eardrum {
      transition: d 220ms ease;
    }
    #earBones {
      transform-box: fill-box;
      transform-origin: left center;
      transition: transform 220ms ease;
    }
    #flowDots,
    #airflowDirectionGroup,
    .airflow-direction-arrow {
      opacity: 0;
      transition: opacity 180ms ease;
    }
    #flowDots.visible,
    #airflowDirectionGroup.visible,
    .airflow-direction-arrow.visible {
      opacity: 1;
    }
    @media (max-width: 980px) {
      body {
        padding: 14px;
      }
      .layout {
        grid-template-columns: 1fr;
      }
      .side-panel {
        grid-template-columns:
          repeat(2, minmax(0, 1fr));
      }
    }
    @media (max-width: 700px) {
      .side-panel {
        grid-template-columns: 1fr;
      }
      .diagram-card,
      .controls-card,
      .information-card {
        padding: 14px;
      }
      .diagram-heading {
        align-items: stretch;
      }
      .difference-badge {
        width: 100%;
      }
      .value-badge.altitude {
        min-width: 124px;
        font-size: 0.72rem;
      }
      .diagram-frame {
        overflow-x: auto;
      }
      .ear-svg {
        width: 900px;
        max-width: none;
        min-height: 0;
      }
    }
    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        animation-duration: 0.001ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.001ms !important;
      }
    }
  </style>
</head>
<body>
  <main class="app">
    <header class="page-header">
      <p class="eyebrow">
        Interactive physiology model
      </p>
      <h1>Altitude and Ear Pressure</h1>
      <p class="page-header-description">
        Explore how altitude affects outside air pressure and how a
        pressure difference moves the eardrum. Open the Eustachian
        tube to equalize middle-ear pressure and observe airflow.
      </p>
    </header>
    <div class="layout">
      <section
        class="card diagram-card"
        aria-labelledby="diagramHeading"
      >
        <div class="diagram-heading">
          <div>
            <h2 id="diagramHeading">
              Whole-ear side view
            </h2>
            <p>
              The eardrum bends toward the side with lower pressure.
            </p>
          </div>
          <div class="difference-badge">
            <span>Pressure difference</span>
            <strong id="pressureDifference">
              0.0 kPa
            </strong>
          </div>
        </div>
        <div class="diagram-frame">
          <svg
            id="earDiagram"
            class="ear-svg"
            viewBox="0 0 1100 650"
            role="img"
            aria-labelledby="diagramTitle diagramDescription"
            xmlns="http://www.w3.org/2000/svg"
          >
            <title id="diagramTitle">
              Interactive side-view diagram of the human ear
            </title>
            <desc id="diagramDescription">
              A side-view ear diagram showing the ear flap, ear canal,
              eardrum, middle ear, ear bones, inner ear, Eustachian
              tube, pressure arrows, and airflow.
            </desc>
            <defs>
              <linearGradient
                id="skinGradient"
                x1="0"
                x2="1"
              >
                <stop
                  offset="0%"
                  stop-color="#f8c9b2"
                />
                <stop
                  offset="100%"
                  stop-color="#e79373"
                />
              </linearGradient>
              <linearGradient
                id="canalGradient"
                x1="0"
                x2="1"
              >
                <stop
                  offset="0%"
                  stop-color="#ffe9d8"
                />
                <stop
                  offset="100%"
                  stop-color="#f4bc9c"
                />
              </linearGradient>
              <linearGradient
                id="middleEarGradient"
                x1="0"
                x2="1"
              >
                <stop
                  offset="0%"
                  stop-color="#f4efff"
                />
                <stop
                  offset="100%"
                  stop-color="#dcd0f1"
                />
              </linearGradient>
              <linearGradient
                id="boneGradient"
                x1="0"
                x2="1"
              >
                <stop
                  offset="0%"
                  stop-color="#ffecad"
                />
                <stop
                  offset="100%"
                  stop-color="#dea245"
                />
              </linearGradient>
              <filter id="softShadow">
                <feDropShadow
                  dx="0"
                  dy="3"
                  stdDeviation="4"
                  flood-color="#17324d"
                  flood-opacity="0.2"
                />
              </filter>
              <filter id="labelShadow">
                <feDropShadow
                  dx="0"
                  dy="2"
                  stdDeviation="2"
                  flood-color="#17324d"
                  flood-opacity="0.12"
                />
              </filter>
              <marker
                id="outsideArrowhead"
                markerWidth="10"
                markerHeight="10"
                refX="8"
                refY="3"
                orient="auto"
              >
                <path
                  d="M0,0 L0,6 L9,3 Z"
                  fill="#1f77b4"
                />
              </marker>
              <marker
                id="middleArrowhead"
                markerWidth="10"
                markerHeight="10"
                refX="8"
                refY="3"
                orient="auto"
              >
                <path
                  d="M0,0 L0,6 L9,3 Z"
                  fill="#d26a32"
                />
              </marker>
              <marker
                id="airflowArrowhead"
                markerWidth="10"
                markerHeight="10"
                refX="8"
                refY="3"
                orient="auto"
              >
                <path
                  d="M0,0 L0,6 L9,3 Z"
                  fill="#218739"
                />
              </marker>
            </defs>
            <rect
              width="1100"
              height="650"
              rx="18"
              fill="#f9fdff"
            />
            
            <!-- Ear flap label -->
            <g aria-hidden="true">
              <rect
                class="label-box"
                x="20"
                y="20"
                width="125"
                height="48"
                rx="10"
              />
              <text
                class="svg-label"
                x="82.5"
                y="50"
                text-anchor="middle"
              >
                Ear flap
              </text>
              <path
                class="label-line"
                d="M82 68 V104 L112 144"
              />
              <circle
                class="label-anchor"
                cx="112"
                cy="144"
                r="4.5"
              />
            </g>

            <!-- Ear canal label -->
            <g aria-hidden="true">
              <rect class="label-box" x="180" y="20" width="145" height="48" rx="10" />
              <text class="svg-label" x="252.5" y="50" text-anchor="middle">Ear canal</text>
              <path class="label-line" d="M252.5 68 V105 H322 V271" />
              <circle class="label-anchor" cx="322" cy="271" r="4.5" />
            </g>
            
            <!-- Eardrum label -->
            <g aria-hidden="true">
              <rect class="label-box eardrum-box" x="350" y="20" width="140" height="48" rx="10" />
              <text x="420" y="50" fill="#d1495b" font-size="16" font-weight="800" text-anchor="middle">Eardrum</text>
              <path class="label-line eardrum-line" d="M420 68 V115 H483 V269" />
              <circle class="label-anchor eardrum-anchor" cx="483" cy="269" r="4.5" />
            </g>
            
            <!-- Ear bones label (Moved left to prevent overlap) -->
            <g aria-hidden="true">
              <rect class="label-box bone-box" x="515" y="20" width="140" height="48" rx="10" />
              <text x="585" y="50" fill="#704d1d" font-size="16" font-weight="800" text-anchor="middle">Ear bones</text>
              <path class="label-line bone-line" d="M585 68 V100 H550 V280" />
              <circle class="label-anchor bone-anchor" cx="550" cy="280" r="4.5" />
            </g>
            
            <!-- Middle ear label (Moved right to prevent overlap) -->
            <g aria-hidden="true">
              <rect class="label-box" x="680" y="20" width="145" height="48" rx="10" />
              <text class="svg-label" x="752.5" y="50" text-anchor="middle">Middle ear</text>
              <path class="label-line" d="M752.5 68 V115 H645 V229" />
              <circle class="label-anchor" cx="645" cy="229" r="4.5" />
            </g>

            <!-- Inner ear label -->
            <g aria-hidden="true">
              <rect class="label-box inner-box" x="870" y="105" width="155" height="50" rx="10" />
              <text x="947.5" y="136" fill="#376f6b" font-size="16" font-weight="800" text-anchor="middle">Inner ear</text>
              <path class="label-line inner-line" d="M870 130 H805 V200 H730 V255" />
              <circle class="label-anchor inner-anchor" cx="730" cy="255" r="4.5" />
            </g>

            <!-- Ear flap -->
            <g id="earFlap">
              <path
                d="
                  M80 156
                  C29 193 24 284 53 363
                  C69 407 101 446 140 468
                  C168 484 200 469 201 437
                  C202 411 180 399 171 377
                  C162 356 174 338 188 325
                  C211 302 224 269 215 228
                  C208 196 186 162 154 143
                  C127 128 100 135 80 156
                  Z
                "
                fill="url(#skinGradient)"
                stroke="#985947"
                stroke-width="5"
                filter="url(#softShadow)"
              />
              <path
                d="
                  M108 190
                  C77 221 74 280 91 323
                  C102 349 122 367 143 370
                  C162 373 176 358 168 340
                  C158 320 142 309 144 284
                  C145 254 170 242 176 222
                  C182 200 164 178 140 173
                  C127 170 116 180 108 190
                  Z
                "
                fill="none"
                stroke="#b86d58"
                stroke-width="10"
                stroke-linecap="round"
              />
              <ellipse
                cx="185"
                cy="298"
                rx="31"
                ry="45"
                fill="#845044"
              />
            </g>
            <!-- Tissue surrounding the ear canal -->
            <path
              d="
                M181 245
                C252 222 355 229 481 250
                L481 340
                C355 361 250 364 181 342
                C203 316 203 271 181 245
                Z
              "
              fill="#e9a080"
              stroke="#9e624f"
              stroke-width="5"
            />
            <!-- Ear canal lumen -->
            <path
              d="
                M185 261
                C265 243 371 250 479 267
                L479 323
                C369 341 262 346 185 326
                C198 306 198 281 185 261
                Z
              "
              fill="url(#canalGradient)"
              stroke="#c5795d"
              stroke-width="3"
            />
            <path
              d="M251 263 Q269 278 286 262"
              fill="none"
              stroke="#ce8d70"
              stroke-width="3"
              opacity="0.65"
            />
            <path
              d="M322 332 Q341 314 361 331"
              fill="none"
              stroke="#ce8d70"
              stroke-width="3"
              opacity="0.65"
            />
            <!-- Middle-ear cavity -->
            <path
              d="
                M496 227
                C550 205 632 212 686 254
                C720 281 724 331 691 360
                C650 397 560 400 503 367
                C488 358 480 339 480 319
                L480 269
                C480 249 485 232 496 227
                Z
              "
              fill="url(#middleEarGradient)"
              stroke="#806aa8"
              stroke-width="5"
            />
            <!-- Eardrum attachment -->
            <path
              d="M483 251 Q465 294 483 337"
              fill="none"
              stroke="#914754"
              stroke-width="4"
              opacity="0.82"
            />
            <!-- Eardrum -->
            <path
              id="eardrum"
              d="M484 269 Q490 294 484 319"
              fill="none"
              stroke="#d1495b"
              stroke-width="9"
              stroke-linecap="round"
              filter="url(#softShadow)"
            />
            <!-- Ear bones -->
            <g
              id="earBones"
              filter="url(#softShadow)"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <circle
                cx="522"
                cy="270"
                r="13"
                fill="url(#boneGradient)"
                stroke="#916526"
                stroke-width="3"
              />
              <path
                d="M514 280 L498 307"
                fill="none"
                stroke="#d79b3f"
                stroke-width="10"
              />
              <path
                d="M534 269 Q555 252 571 269 L560 297"
                fill="none"
                stroke="#d79b3f"
                stroke-width="11"
              />
              <path
                d="M561 297 L587 304"
                fill="none"
                stroke="#d79b3f"
                stroke-width="8"
              />
              <path
                d="M587 289 L587 318"
                fill="none"
                stroke="#d79b3f"
                stroke-width="7"
              />
              <path
                d="M587 290 Q604 304 587 318"
                fill="none"
                stroke="#d79b3f"
                stroke-width="6"
              />
            </g>
            <!-- Inner ear -->
            <g id="innerEar">
              <path
                d="
                  M618 249
                  C661 229 713 249 719 289
                  C724 324 696 349 661 347
                  C631 345 612 324 616 302
                  C620 282 643 275 658 287
                  C673 298 667 320 650 324
                "
                fill="none"
                stroke="#6ca6a2"
                stroke-width="13"
                stroke-linecap="round"
              />
              <path
                d="M689 252 Q726 230 758 246"
                fill="none"
                stroke="#6ca6a2"
                stroke-width="8"
                stroke-linecap="round"
              />
              <path
                d="M699 268 Q739 252 768 272"
                fill="none"
                stroke="#6ca6a2"
                stroke-width="8"
                stroke-linecap="round"
              />
            </g>
            <!-- Eustachian tube -->
            <path
              d="M625 370 Q710 431 820 491"
              fill="none"
              stroke="#d98f66"
              stroke-width="36"
              stroke-linecap="round"
            />
            <path
              d="M625 370 Q710 431 820 491"
              fill="none"
              stroke="#fff0df"
              stroke-width="16"
              stroke-linecap="round"
            />
            <!-- Throat -->
            <path
              d="M820 489 Q854 504 880 534"
              fill="none"
              stroke="#e8a783"
              stroke-width="31"
              stroke-linecap="round"
            />
            <!-- Outside-pressure arrows -->
            <g id="outsideArrows">
              <line
                class="pressure-arrow"
                x1="280"
                y1="277"
                x2="433"
                y2="277"
                stroke="#1f77b4"
                stroke-width="6"
                marker-end="url(#outsideArrowhead)"
              />
              <line
                class="pressure-arrow"
                x1="280"
                y1="311"
                x2="433"
                y2="311"
                stroke="#1f77b4"
                stroke-width="6"
                marker-end="url(#outsideArrowhead)"
              />
            </g>
            <!-- Middle-ear pressure arrows -->
            <g id="middleArrows">
              <line
                class="pressure-arrow"
                x1="674"
                y1="264"
                x2="536"
                y2="264"
                stroke="#d26a32"
                stroke-width="6"
                marker-end="url(#middleArrowhead)"
              />
              <line
                class="pressure-arrow"
                x1="676"
                y1="334"
                x2="536"
                y2="334"
                stroke="#d26a32"
                stroke-width="6"
                marker-end="url(#middleArrowhead)"
              />
            </g>

            <!-- Outside-pressure callout -->
            <g aria-hidden="true">
              <rect class="label-box outside-box" x="205" y="390" width="170" height="64" rx="11" />
              <text x="290" y="416" fill="#1f77b4" font-size="14" font-weight="800" text-anchor="middle">Outside pressure</text>
              <text id="outsideSvgValue" x="290" y="440" fill="#1f77b4" font-size="16" font-weight="800" text-anchor="middle">101.3 kPa</text>
              <!-- Anchor moved to center of the ear canal lumen -->
              <path class="label-line outside-line" d="M290 390 V360 H340 V300" />
              <circle class="label-anchor outside-anchor" cx="340" cy="300" r="4.5" />
            </g>

            <!-- Middle-ear pressure callout -->
            <g aria-hidden="true">
              <rect class="label-box pressure-box" x="415" y="420" width="210" height="68" rx="11" />
              <text x="520" y="446" fill="#d26a32" font-size="14" font-weight="800" text-anchor="middle">Middle-ear pressure</text>
              <text id="middleSvgValue" x="520" y="472" fill="#d26a32" font-size="16" font-weight="800" text-anchor="middle">101.3 kPa</text>
              <path class="label-line pressure-line" d="M520 420 V390 H570 V350" />
              <circle class="label-anchor pressure-anchor" cx="570" cy="350" r="4.5" />
            </g>

            <!-- Airflow arrows -->
            <path
              id="airflowOutArrow"
              class="airflow-direction-arrow"
              d="M666 399 Q722 439 789 476"
              fill="none"
              stroke="#218739"
              stroke-width="5"
              stroke-linecap="round"
              marker-end="url(#airflowArrowhead)"
              aria-hidden="true"
            />
            <path
              id="airflowInArrow"
              class="airflow-direction-arrow"
              d="M789 476 Q722 439 666 399"
              fill="none"
              stroke="#218739"
              stroke-width="5"
              stroke-linecap="round"
              marker-end="url(#airflowArrowhead)"
              aria-hidden="true"
            />
            <!-- Animated airflow particles -->
            <g
              id="flowDots"
              aria-hidden="true"
            >
              <circle
                class="flow-particle"
                r="6"
                fill="#218739"
              />
              <circle
                class="flow-particle"
                r="6"
                fill="#218739"
              />
              <circle
                class="flow-particle"
                r="6"
                fill="#218739"
              />
              <circle
                class="flow-particle"
                r="6"
                fill="#218739"
              />
            </g>
            <!-- Airflow direction label -->
            <g
              id="airflowDirectionGroup"
              aria-hidden="true"
            >
              <rect
                x="660"
                y="515"
                width="235"
                height="42"
                rx="10"
                fill="#edf9f4"
                stroke="#218739"
                stroke-width="2"
                filter="url(#labelShadow)"
              />
              <text
                id="airflowDirectionLabel"
                x="777.5"
                y="541"
                fill="#176451"
                font-size="13"
                font-weight="800"
                text-anchor="middle"
              >
                No airflow
              </text>
            </g>
            
            <!-- Eustachian tube label -->
            <g aria-hidden="true">
              <rect class="label-box tube-box" x="780" y="555" width="245" height="55" rx="11" />
              <text x="902.5" y="578" fill="#6b4529" font-size="16" font-weight="800" text-anchor="middle">Eustachian tube</text>
              <text x="902.5" y="597" class="svg-small-label" text-anchor="middle">Connects the middle ear to the throat</text>
              <path class="label-line tube-line" d="M850 555 V520 H770 V465" />
              <circle class="label-anchor tube-anchor" cx="770" cy="465" r="4.5" />
            </g>
            
            <!-- Eardrum condition -->
            <g>
              <rect
                x="230"
                y="555"
                width="475"
                height="48"
                rx="12"
                fill="#fff7f8"
                stroke="#e6b3bb"
                stroke-width="2"
              />
              <text
                id="eardrumLabel"
                x="467.5"
                y="585"
                fill="#d1495b"
                font-size="17"
                font-weight="800"
                text-anchor="middle"
                aria-live="polite"
              >
                Eardrum: neutral
              </text>
            </g>
          </svg>
        </div>
        <div
          class="legend"
          aria-label="Diagram color key"
        >
          <span class="legend-item">
            <span
              class="legend-dot"
              style="background: var(--outside);"
            ></span>
            Outside pressure
          </span>
          <span class="legend-item">
            <span
              class="legend-dot"
              style="background: var(--middle);"
            ></span>
            Middle-ear pressure
          </span>
          <span class="legend-item">
            <span class="legend-line"></span>
            Eardrum
          </span>
          <span class="legend-item">
            <span class="legend-arrow">➜</span>
            Airflow direction
          </span>
        </div>
      </section>
      <aside class="side-panel">
        <section
          class="card controls-card"
          aria-labelledby="controlsTitle"
        >
          <h2
            class="card-title"
            id="controlsTitle"
          >
            Model controls
          </h2>
          <p class="controls-intro">
            Adjust the pressures directly or change the altitude to
            calculate the outside air pressure.
          </p>
          <div class="control-group">
            <div class="control-heading">
              <label for="outsidePressure">
                Outside air pressure
              </label>
              <output
                id="outsidePressureValue"
                class="value-badge outside"
                for="outsidePressure"
              >
                101.3 kPa
              </output>
            </div>
            <input
              id="outsidePressure"
              type="range"
              min="20"
              max="110"
              step="0.1"
              value="101.3"
            >
            <div
              class="range-scale"
              aria-hidden="true"
            >
              <span>20 kPa</span>
              <span>65 kPa</span>
              <span>110 kPa</span>
            </div>
          </div>
          <div class="control-group">
            <div class="control-heading">
              <label for="middlePressure">
                Middle-ear pressure
              </label>
              <output
                id="middlePressureValue"
                class="value-badge middle"
                for="middlePressure"
              >
                101.3 kPa
              </output>
            </div>
            <input
              id="middlePressure"
              type="range"
              min="20"
              max="110"
              step="0.1"
              value="101.3"
            >
            <div
              class="range-scale"
              aria-hidden="true"
            >
              <span>20 kPa</span>
              <span>65 kPa</span>
              <span>110 kPa</span>
            </div>
          </div>
          <div class="altitude-section">
            <div class="control-heading">
              <label for="altitude">
                Altitude
              </label>
              <output
                id="altitudeValue"
                class="value-badge altitude"
                for="altitude"
              >
                0 ft (0 m)
              </output>
            </div>
            <input
              id="altitude"
              type="range"
              min="-2000"
              max="35000"
              step="100"
              value="0"
            >
            <div
              class="range-scale"
              aria-hidden="true"
            >
              <span>−2,000 ft</span>
              <span>Sea level</span>
              <span>35,000 ft</span>
            </div>
            <p class="altitude-help">
              Increasing altitude lowers outside pressure. Middle-ear
              pressure stays unchanged unless the Eustachian tube is
              open.
            </p>
          </div>
          <div class="tube-control">
            <div class="tube-control-heading">
              <strong>Eustachian tube</strong>
              <p>
                Open the tube to display airflow and gradually
                equalize the pressure.
              </p>
            </div>
            <button
              id="tubeButton"
              class="tube-button"
              type="button"
              aria-pressed="false"
            >
              <span
                class="tube-button-icon"
                aria-hidden="true"
              ></span>
              <span id="tubeButtonText">
                Open Eustachian tube
              </span>
            </button>
          </div>
          <p
            id="airflowStatus"
            class="airflow-status"
            aria-live="polite"
          >
            <strong>Tube closed:</strong>
            no air is moving through the Eustachian tube.
          </p>
          <button
            id="resetButton"
            class="reset-button"
            type="button"
          >
            Reset model
          </button>
        </section>
        <section
          class="card information-card"
          aria-labelledby="conditionCardTitle"
        >
          <h2
            class="card-title"
            id="conditionCardTitle"
          >
            What is happening?
          </h2>
          <div
            id="conditionBox"
            class="condition-box"
            aria-live="polite"
          >
            <h3 id="conditionTitle">
              Balanced pressure
            </h3>
            <p id="conditionExplanation">
              Pressure is approximately equal on both sides, so the
              eardrum is in its neutral position.
            </p>
          </div>
          <div class="note">
            This simplified educational model is intended for
            illustration and is not a medical diagnostic tool.
          </div>
        </section>
      </aside>
    </div>
    <footer class="footer">
      Simplified interactive model of the outer, middle, and inner ear.
    </footer>
  </main>
  <script>
    "use strict";
    const SEA_LEVEL_PRESSURE = 101.325;
    const FEET_TO_METERS = 0.3048;
    const BALANCED_THRESHOLD = 0.25;
    const FLOW_STOP_THRESHOLD = 0.06;
    const outsidePressureInput =
      document.getElementById("outsidePressure");
    const middlePressureInput =
      document.getElementById("middlePressure");
    const altitudeInput =
      document.getElementById("altitude");
    const outsidePressureValue =
      document.getElementById("outsidePressureValue");
    const middlePressureValue =
      document.getElementById("middlePressureValue");
    const outsideSvgValue =
      document.getElementById("outsideSvgValue");
    const middleSvgValue =
      document.getElementById("middleSvgValue");
    const altitudeValue =
      document.getElementById("altitudeValue");
    const pressureDifference =
      document.getElementById("pressureDifference");
    const tubeButton =
      document.getElementById("tubeButton");
    const tubeButtonText =
      document.getElementById("tubeButtonText");
    const resetButton =
      document.getElementById("resetButton");
    const eardrum =
      document.getElementById("eardrum");
    const eardrumLabel =
      document.getElementById("eardrumLabel");
    const earBones =
      document.getElementById("earBones");
    const outsideArrows =
      document.querySelectorAll(
        "#outsideArrows .pressure-arrow"
      );
    const middleArrows =
      document.querySelectorAll(
        "#middleArrows .pressure-arrow"
      );
    const flowDots =
      document.getElementById("flowDots");
    const flowParticles =
      document.querySelectorAll(".flow-particle");
    const airflowOutArrow =
      document.getElementById("airflowOutArrow");
    const airflowInArrow =
      document.getElementById("airflowInArrow");
    const airflowDirectionGroup =
      document.getElementById("airflowDirectionGroup");
    const airflowDirectionLabel =
      document.getElementById("airflowDirectionLabel");
    const airflowStatus =
      document.getElementById("airflowStatus");
    const conditionBox =
      document.getElementById("conditionBox");
    const conditionTitle =
      document.getElementById("conditionTitle");
    const conditionExplanation =
      document.getElementById("conditionExplanation");
    let tubeIsOpen = false;
    let equalizationTimer = null;
    let animationFrame = null;
    let animationStartTime = 0;
    let currentFlowDirection = 0;
    function clamp(value, minimum, maximum) {
      return Math.min(
        Math.max(value, minimum),
        maximum
      );
    }
    function formatPressure(value) {
      return value.toFixed(1) + " kPa";
    }
    function formatAltitude(feet) {
      const roundedFeet = Math.round(feet);
      const meters = Math.round(
        roundedFeet * FEET_TO_METERS
      );
      return (
        roundedFeet.toLocaleString() +
        " ft (" +
        meters.toLocaleString() +
        " m)"
      );
    }
    function pressureAtAltitude(altitudeFeet) {
      const altitudeMeters =
        altitudeFeet * FEET_TO_METERS;
      const base =
        1 - 2.25577e-5 * altitudeMeters;
      const pressure =
        SEA_LEVEL_PRESSURE *
        Math.pow(
          Math.max(base, 0.01),
          5.25588
        );
      return clamp(
        pressure,
        Number(outsidePressureInput.min),
        Number(outsidePressureInput.max)
      );
    }
    function altitudeFromPressure(pressure) {
      const safePressure =
        Math.max(pressure, 0.01);
      const altitudeMeters =
        (
          1 -
          Math.pow(
            safePressure / SEA_LEVEL_PRESSURE,
            1 / 5.25588
          )
        ) / 2.25577e-5;
      const altitudeFeet =
        altitudeMeters / FEET_TO_METERS;
      return clamp(
        altitudeFeet,
        Number(altitudeInput.min),
        Number(altitudeInput.max)
      );
    }
    function getCondition(difference) {
      if (
        Math.abs(difference) <=
        BALANCED_THRESHOLD
      ) {
        return "balanced";
      }
      return difference > 0
        ? "inward"
        : "outward";
    }
    function updateTubeButton() {
      tubeButton.setAttribute(
        "aria-pressed",
        String(tubeIsOpen)
      );
      tubeButtonText.textContent =
        tubeIsOpen
          ? "Close Eustachian tube"
          : "Open Eustachian tube";
    }
    function updateArrowStrength(difference) {
      const magnitude = clamp(
        Math.abs(difference) / 12,
        0,
        1
      );
      const outsideIsStronger =
        difference > BALANCED_THRESHOLD;
      const middleIsStronger =
        difference < -BALANCED_THRESHOLD;
      outsideArrows.forEach(function (arrow) {
        let opacity = 0.72;
        let width = 6;
        if (outsideIsStronger) {
          opacity = 0.7 + magnitude * 0.3;
          width = 6 + magnitude * 4;
        } else if (middleIsStronger) {
          opacity = 0.22;
          width = 5;
        }
        arrow.style.opacity = opacity;
        arrow.style.strokeWidth = width;
      });
      middleArrows.forEach(function (arrow) {
        let opacity = 0.72;
        let width = 6;
        if (middleIsStronger) {
          opacity = 0.7 + magnitude * 0.3;
          width = 6 + magnitude * 4;
        } else if (outsideIsStronger) {
          opacity = 0.22;
          width = 5;
        }
        arrow.style.opacity = opacity;
        arrow.style.strokeWidth = width;
      });
    }
    function updateCondition(condition, difference) {
      const magnitude =
        Math.abs(difference).toFixed(1);
      if (condition === "balanced") {
        eardrumLabel.textContent =
          "Eardrum: neutral";
        conditionBox.style.borderLeftColor =
          "#218739";
        conditionBox.style.background =
          "#effaf2";
        conditionTitle.textContent =
          "Balanced pressure";
        conditionExplanation.textContent =
          "Pressure is approximately equal on both sides, " +
          "so the eardrum is in its neutral position.";
        return;
      }
      if (condition === "outward") {
        eardrumLabel.textContent =
          "Eardrum bulges outward toward the ear canal";
        conditionBox.style.borderLeftColor =
          "#b55b20";
        conditionBox.style.background =
          "#fff4e8";
        conditionTitle.textContent =
          "Outward eardrum movement";
        conditionExplanation.textContent =
          "Middle-ear pressure is " +
          magnitude +
          " kPa higher than outside pressure. Opening the " +
          "Eustachian tube allows air to move from the " +
          "middle ear toward the throat.";
        return;
      }
      eardrumLabel.textContent =
        "Eardrum bulges inward toward the middle ear";
      conditionBox.style.borderLeftColor =
        "#d1495b";
      conditionBox.style.background =
        "#fdecef";
      conditionTitle.textContent =
        "Inward eardrum movement";
      conditionExplanation.textContent =
        "Outside pressure is " +
        magnitude +
        " kPa higher than middle-ear pressure. Opening the " +
        "Eustachian tube allows air to move from the throat " +
        "toward the middle ear.";
    }
    function clearFlowGraphics() {
      currentFlowDirection = 0;
      flowDots.classList.remove("visible");
      airflowOutArrow.classList.remove("visible");
      airflowInArrow.classList.remove("visible");
      airflowDirectionGroup.classList.remove("visible");
    }
    function quadraticPoint(
      start,
      control,
      end,
      progress
    ) {
      const inverse = 1 - progress;
      return {
        x:
          inverse * inverse * start.x +
          2 * inverse * progress * control.x +
          progress * progress * end.x,
        y:
          inverse * inverse * start.y +
          2 * inverse * progress * control.y +
          progress * progress * end.y
      };
    }
    function animateFlow(timestamp) {
      if (!animationStartTime) {
        animationStartTime = timestamp;
      }
      if (
        !tubeIsOpen ||
        currentFlowDirection === 0
      ) {
        animationFrame = null;
        animationStartTime = 0;
        return;
      }
      const duration = 1450;
      const elapsed =
        timestamp - animationStartTime;
      const baseProgress =
        (elapsed % duration) / duration;
      const start = {
        x: 625,
        y: 370
      };
      const control = {
        x: 710,
        y: 431
      };
      const end = {
        x: 820,
        y: 491
      };
      flowParticles.forEach(
        function (particle, index) {
          let progress =
            (
              baseProgress +
              index / flowParticles.length
            ) % 1;
          if (currentFlowDirection === -1) {
            progress = 1 - progress;
          }
          const point = quadraticPoint(
            start,
            control,
            end,
            progress
          );
          particle.setAttribute(
            "cx",
            point.x.toFixed(2)
          );
          particle.setAttribute(
            "cy",
            point.y.toFixed(2)
          );
        }
      );
      animationFrame =
        window.requestAnimationFrame(
          animateFlow
        );
    }
    function startFlowAnimation(direction) {
      currentFlowDirection = direction;
      if (animationFrame === null) {
        animationStartTime = 0;
        animationFrame =
          window.requestAnimationFrame(
            animateFlow
          );
      }
    }
    function stopFlowAnimation() {
      currentFlowDirection = 0;
      animationStartTime = 0;
      if (animationFrame !== null) {
        window.cancelAnimationFrame(
          animationFrame
        );
        animationFrame = null;
      }
    }
    function updateAirflowVisual(difference) {
      clearFlowGraphics();
      airflowStatus.classList.remove(
        "active",
        "balanced"
      );
      if (!tubeIsOpen) {
        stopFlowAnimation();
        airflowStatus.innerHTML =
          "<strong>Tube closed:</strong> " +
          "no air is moving through the Eustachian tube.";
        return;
      }
      if (
        Math.abs(difference) <
        FLOW_STOP_THRESHOLD
      ) {
        stopFlowAnimation();
        airflowDirectionLabel.textContent =
          "Pressure equalized — no airflow";
        airflowDirectionGroup.classList.add(
          "visible"
        );
        airflowStatus.classList.add(
          "balanced"
        );
        airflowStatus.innerHTML =
          "<strong>Pressure equalized:</strong> " +
          "there is no significant airflow.";
        return;
      }
      flowDots.classList.add("visible");
      airflowDirectionGroup.classList.add(
        "visible"
      );
      airflowStatus.classList.add("active");
      if (difference > 0) {
        startFlowAnimation(-1);
        airflowInArrow.classList.add(
          "visible"
        );
        airflowDirectionLabel.textContent =
          "Airflow: throat → middle ear";
        airflowStatus.innerHTML =
          "<strong>Airflow toward the middle ear:</strong> " +
          "outside pressure is higher, so air moves from " +
          "the throat through the Eustachian tube.";
        return;
      }
      startFlowAnimation(1);
      airflowOutArrow.classList.add(
        "visible"
      );
      airflowDirectionLabel.textContent =
        "Airflow: middle ear → throat";
      airflowStatus.innerHTML =
        "<strong>Airflow toward the throat:</strong> " +
        "middle-ear pressure is higher, so air moves down " +
        "the Eustachian tube.";
    }
    function updateDisplay() {
      const outsidePressure =
        Number.parseFloat(
          outsidePressureInput.value
        );
      const middlePressure =
        Number.parseFloat(
          middlePressureInput.value
        );
      const difference =
        outsidePressure - middlePressure;
      const condition =
        getCondition(difference);
      outsidePressureValue.textContent =
        formatPressure(outsidePressure);
      middlePressureValue.textContent =
        formatPressure(middlePressure);
      outsideSvgValue.textContent =
        formatPressure(outsidePressure);
      middleSvgValue.textContent =
        formatPressure(middlePressure);
      pressureDifference.textContent =
        Math.abs(difference).toFixed(1) +
        " kPa";
      pressureDifference.style.color =
        condition === "balanced"
          ? "#218739"
          : condition === "outward"
            ? "#b55b20"
            : "#d1495b";
      const bend = clamp(
        difference * 2.2,
        -24,
        24
      );
      const controlX = 490 + bend;
      eardrum.setAttribute(
        "d",
        "M484 269 Q" +
          controlX.toFixed(2) +
          " 294 484 319"
      );
      const boneMovement = clamp(
        bend * 0.11,
        -2.5,
        2.5
      );
      earBones.style.transform =
        "translateX(" +
        boneMovement.toFixed(2) +
        "px)";
      updateArrowStrength(difference);
      updateCondition(condition, difference);
      updateAirflowVisual(difference);
      updateTubeButton();
    }
    function stopEqualization() {
      if (equalizationTimer !== null) {
        window.clearInterval(
          equalizationTimer
        );
        equalizationTimer = null;
      }
    }
    function startEqualization() {
      stopEqualization();
      updateDisplay();
      const startingDifference =
        Number.parseFloat(
          outsidePressureInput.value
        ) -
        Number.parseFloat(
          middlePressureInput.value
        );
      if (
        Math.abs(startingDifference) <
        FLOW_STOP_THRESHOLD
      ) {
        return;
      }
      equalizationTimer =
        window.setInterval(function () {
          const outsidePressure =
            Number.parseFloat(
              outsidePressureInput.value
            );
          const middlePressure =
            Number.parseFloat(
              middlePressureInput.value
            );
          const difference =
            outsidePressure - middlePressure;
          if (
            Math.abs(difference) <
            FLOW_STOP_THRESHOLD
          ) {
            middlePressureInput.value =
              outsidePressure.toFixed(1);
            stopEqualization();
            updateDisplay();
            return;
          }
          const nextMiddlePressure =
            middlePressure +
            difference * 0.08;
          middlePressureInput.value =
            clamp(
              nextMiddlePressure,
              Number(middlePressureInput.min),
              Number(middlePressureInput.max)
            ).toFixed(1);
          updateDisplay();
        }, 80);
    }
    function updateAfterPressureChange() {
      updateDisplay();
      if (tubeIsOpen) {
        startEqualization();
      }
    }
    altitudeInput.addEventListener(
      "input",
      function () {
        const altitudeFeet =
          Number.parseFloat(
            altitudeInput.value
          );
        const outsidePressure =
          pressureAtAltitude(altitudeFeet);
        altitudeValue.textContent =
          formatAltitude(altitudeFeet);
        outsidePressureInput.value =
          outsidePressure.toFixed(1);
        updateAfterPressureChange();
      }
    );
    outsidePressureInput.addEventListener(
      "input",
      function () {
        const pressure =
          Number.parseFloat(
            outsidePressureInput.value
          );
        const calculatedAltitude =
          altitudeFromPressure(pressure);
        const roundedAltitude =
          Math.round(
            calculatedAltitude / 100
          ) * 100;
        altitudeInput.value =
          roundedAltitude.toString();
        altitudeValue.textContent =
          formatAltitude(roundedAltitude);
        updateAfterPressureChange();
      }
    );
    middlePressureInput.addEventListener(
      "input",
      updateAfterPressureChange
    );
    tubeButton.addEventListener(
      "click",
      function () {
        tubeIsOpen = !tubeIsOpen;
        updateTubeButton();
        if (tubeIsOpen) {
          startEqualization();
        } else {
          stopEqualization();
          stopFlowAnimation();
          updateDisplay();
        }
      }
    );
    resetButton.addEventListener(
      "click",
      function () {
        stopEqualization();
        stopFlowAnimation();
        tubeIsOpen = false;
        altitudeInput.value = "0";
        outsidePressureInput.value = "101.3";
        middlePressureInput.value = "101.3";
        altitudeValue.textContent =
          formatAltitude(0);
        updateTubeButton();
        updateDisplay();
      }
    );
    altitudeValue.textContent =
      formatAltitude(0);
    updateTubeButton();
    updateDisplay();
  </script>
</body>
</html>
