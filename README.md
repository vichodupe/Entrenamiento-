<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <title>PPL Pro Tracker</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet" />
  <style>
    body {
      background-color: #0b0f19;
      color: #f1f5f9;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    .card {
      background: #151c2c;
      border: 1px solid #222d45;
      border-radius: 1rem;
    }
    .nav-active {
      color: #38bdf8;
      border-top: 2px solid #38bdf8;
      background: rgba(56, 189, 248, 0.06);
    }
    .badge-push { background: rgba(239, 68, 68, 0.2); color: #f87171; border: 1px solid rgba(239, 68, 68, 0.4); }
    .badge-pull { background: rgba(59, 130, 246, 0.2); color: #60a5fa; border: 1px solid rgba(59, 130, 246, 0.4); }
    .badge-legs { background: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid rgba(16, 185, 129, 0.4); }
    .badge-rest { background: rgba(148, 163, 184, 0.15); color: #94a3b8; border: 1px solid rgba(148, 163, 184, 0.3); }
    input[type="number"]::-webkit-inner-spin-button,
    input[type="number"]::-webkit-outer-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
  </style>
</head>
<body class="pb-24 select-none">

  <!-- HEADER -->
  <header class="sticky top-0 z-30 bg-[#0b0f19]/90 backdrop-blur-md border-b border-slate-800 px-4 py-3 flex justify-between items-center">
    <div>
      <h1 class="text-lg font-extrabold tracking-tight text-white flex items-center gap-2">
        <i class="fa-solid fa-dumbbell text-sky-400"></i> PPL <span class="text-sky-400">HYPERTROPHY</span>
      </h1>
      <p class="text-xs text-slate-400">Octubre 2026 • Alta Macul</p>
    </div>
    <div class="flex items-center gap-2">
      <span id="streak-badge" class="text-xs font-bold bg-amber-500/20 text-amber-300 border border-amber-500/40 px-2.5 py-1 rounded-full">
        🔥 0 entrenos este mes
      </span>
    </div>
  </header>

  <!-- MAIN CONTAINER -->
  <main class="max-w-xl mx-auto p-4 space-y-5">

    <!-- TAB 1: ENTRENAMIENTO Y PESOS -->
    <section id="tab-workout" class="space-y-4">
      <div class="grid grid-cols-3 gap-2 bg-slate-900 p-1.5 rounded-xl border border-slate-800">
        <button onclick="switchSplit('push')" id="btn-split-push" class="py-2 rounded-lg font-bold text-sm transition-all">
          🔴 PUSH
        </button>
        <button onclick="switchSplit('pull')" id="btn-split-pull" class="py-2 rounded-lg font-bold text-sm transition-all">
          🔵 PULL
        </button>
        <button onclick="switchSplit('legs')" id="btn-split-legs" class="py-2 rounded-lg font-bold text-sm transition-all">
          🟢 LEGS
        </button>
      </div>

      <div class="card p-3.5 flex items-center justify-between">
        <div>
          <h2 id="split-title" class="font-bold text-sm text-white">Bloque Push</h2>
          <p class="text-xs text-slate-400">Placas de 4.5 kg y mancuernas de 5 a 30 kg.</p>
        </div>
        <button onclick="logTodaySession()" class="bg-sky-500 hover:bg-sky-400 active:scale-95 text-slate-950 font-extrabold text-xs px-3.5 py-2.5 rounded-xl transition-all shadow-lg shadow-sky-500/20">
          <i class="fa-solid fa-calendar-check mr-1"></i> Registrar Hoy
        </button>
      </div>

      <div id="volume-warning-banner" class="hidden"></div>
      <div id="exercises-container" class="space-y-4"></div>

      <div id="paused-exercises-wrapper" class="hidden space-y-2 pt-2">
        <h3 class="text-xs font-extrabold uppercase tracking-wider text-slate-400 px-1">
          🔄 En Rotación / Pausados Hoy
        </h3>
        <div id="paused-exercises-container" class="space-y-2"></div>
      </div>

      <button onclick="openExerciseModal(null)" class="w-full py-3 rounded-xl border border-dashed border-slate-700 text-slate-400 hover:text-white hover:border-slate-500 text-sm font-semibold transition-all">
        <i class="fa-solid fa-plus mr-1"></i> Agregar Ejercicio al Bloque
      </button>
    </section>

    <!-- TAB 2: CALENDARIO -->
    <section id="tab-calendar" class="hidden space-y-4">
      <div class="card p-4">
        <div class="flex justify-between items-center mb-4">
          <button onclick="changeMonth(-1)" class="p-2 text-slate-400 hover:text-white"><i class="fa-solid fa-chevron-left"></i></button>
          <h2 id="calendar-month-title" class="font-bold text-base text-white capitalize">Octubre 2026</h2>
          <button onclick="changeMonth(1)" class="p-2 text-slate-400 hover:text-white"><i class="fa-solid fa-chevron-right"></i></button>
        </div>

        <div class="grid grid-cols-7 gap-1 text-center text-xs font-semibold text-slate-400 mb-2">
          <div>Lun</div><div>Mar</div><div>Mié</div><div>Jue</div><div>Vie</div><div>Sáb</div><div>Dom</div>
        </div>

        <div id="calendar-grid" class="grid grid-cols-7 gap-1.5 text-center"></div>

        <div class="flex flex-wrap justify-center gap-3 mt-4 pt-3 border-t border-slate-800 text-xs">
          <span class="px-2 py-0.5 rounded badge-push">Push</span>
          <span class="px-2 py-0.5 rounded badge-pull">Pull</span>
          <span class="px-2 py-0.5 rounded badge-legs">Legs</span>
          <span class="px-2 py-0.5 rounded badge-rest">Descanso</span>
        </div>
      </div>

      <div id="calendar-day-detail" class="card p-4 space-y-3">
        <div class="flex justify-between items-center">
          <h3 id="selected-date-label" class="font-bold text-sm text-sky-400">Selecciona un día</h3>
          <span id="selected-date-status" class="text-xs text-slate-400">Sin registro</span>
        </div>
        <p class="text-xs text-slate-400">Marca qué bloque entrenaste en esta fecha:</p>
        <div class="grid grid-cols-4 gap-2">
          <button onclick="setDayStatus('push')" class="py-2 rounded-lg text-xs font-bold badge-push">Push</button>
          <button onclick="setDayStatus('pull')" class="py-2 rounded-lg text-xs font-bold badge-pull">Pull</button>
          <button onclick="setDayStatus('legs')" class="py-2 rounded-lg text-xs font-bold badge-legs">Legs</button>
          <button onclick="setDayStatus('rest')" class="py-2 rounded-lg text-xs font-bold badge-rest">Descanso</button>
        </div>
        <button onclick="setDayStatus(null)" class="w-full py-1.5 text-xs text-slate-500 hover:text-red-400">Limpiar día</button>
      </div>
    </section>

    <!-- TAB 3: SUPLEMENTOS Y ESQUEMA DE SERIES -->
    <section id="tab-supps" class="hidden space-y-4">
      <div class="card p-4 space-y-3">
        <div class="flex justify-between items-center">
          <div>
            <h2 class="font-bold text-base text-white">Stack Diario de Suplementos</h2>
            <p class="text-xs text-slate-400">Checklist del día</p>
          </div>
          <span id="supps-progress" class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-lg border border-emerald-500/30">0/4</span>
        </div>
        <div id="supps-list" class="space-y-2.5 pt-2"></div>
      </div>

      <div class="card p-4 space-y-2.5">
        <h3 class="font-bold text-sm text-sky-400 flex items-center gap-2">
          <i class="fa-solid fa-list-check"></i> Estructura de Series por Ejercicio
        </h3>
        <ul class="text-xs text-slate-300 space-y-2 leading-relaxed">
          <li>🔥 <strong class="text-sky-300">1 Top Set (Serie Efectiva Principal):</strong> Es la serie clave donde vas al fallo o RIR 1-2 con tu peso máximo. Es la única que define si subes de peso en la app.</li>
          <li>📉 <strong class="text-indigo-300">2 Series Back-off (Retroceso):</strong> Haces 2 series adicionales bajando una placa (4.5 kg) o un par de mancuernas (2.5 kg) para sumar volumen hipertrófico de calidad sin sobrecargar el sistema nervioso.</li>
          <li>💪 <strong class="text-purple-400">Bíceps y Tríceps:</strong> Máximo 2 ejercicios activos por sesión para evitar fatiga inútil.</li>
        </ul>
      </div>
    </section>

    <!-- TAB 4: PROGRESO Y RESPALDO -->
    <section id="tab-stats" class="hidden space-y-4">
      <div class="card p-4 space-y-3">
        <h2 class="font-bold text-base text-white">Resumen de Marcas Actuales</h2>
        <div id="top-marks-summary" class="space-y-2 text-xs"></div>
      </div>

      <div class="card p-4 space-y-3">
        <h2 class="font-bold text-base text-white">Historial de Cambios</h2>
        <div id="history-log" class="max-h-56 overflow-y-auto space-y-2 text-xs pr-1"></div>
      </div>

      <div class="card p-4 space-y-3">
        <h2 class="font-bold text-base text-white">Respaldo</h2>
        <div class="grid grid-cols-2 gap-2">
          <button onclick="copySummaryToClipboard()" class="bg-sky-500 text-slate-950 font-bold text-xs py-2.5 rounded-xl">
            <i class="fa-solid fa-copy mr-1"></i> Copiar para Gemini
          </button>
          <button onclick="resetToDefaults()" class="bg-slate-800 text-rose-400 border border-rose-500/30 font-bold text-xs py-2.5 rounded-xl">
            <i class="fa-solid fa-rotate-left mr-1"></i> Restaurar Base
          </button>
        </div>
        <p id="copy-feedback" class="text-center text-xs text-emerald-400 hidden">¡Copiado al portapapeles!</p>
      </div>
    </section>

  </main>

  <!-- MODAL CREAR / EDITAR EJERCICIO -->
  <div id="modal-ex" class="fixed inset-0 bg-black/80 z-50 hidden flex items-center justify-center p-4">
    <div class="card p-5 w-full max-w-sm space-y-3 max-h-[90vh] overflow-y-auto">
      <div class="flex justify-between items-center">
        <h3 id="modal-ex-title" class="font-bold text-base text-white">Editar Ejercicio</h3>
        <button onclick="closeExerciseModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark"></i></button>
      </div>

      <div>
        <label class="text-[11px] text-slate-400 block mb-1">Nombre del Ejercicio</label>
        <input id="ex-form-name" type="text" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white" />
      </div>

      <div class="grid grid-cols-2 gap-2">
        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Grupo Muscular</label>
          <select id="ex-form-muscle" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white">
            <option value="Pecho">Pecho</option>
            <option value="Hombro">Hombro</option>
            <option value="Tríceps">Tríceps</option>
            <option value="Espalda">Espalda</option>
            <option value="Bíceps">Bíceps</option>
            <option value="Cuádriceps">Cuádriceps</option>
            <option value="Isquios/Glúteo">Isquios/Glúteo</option>
            <option value="Gemelos">Gemelos</option>
          </select>
        </div>
        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Tipo de Carga</label>
          <select id="ex-form-type" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white" onclick="updateModalStepHint()">
            <option value="placas">Máquina (por Placas de 4.5kg)</option>
            <option value="mancuernas">Mancuernas (Salto 2.5kg)</option>
            <option value="kilos">Libre / Otro (kg)</option>
          </select>
        </div>
      </div>

      <div class="grid grid-cols-2 gap-2">
        <div>
          <label id="label-top-weight" class="text-[11px] text-slate-400 block mb-1">Valor Top Set</label>
          <input id="ex-form-top" type="number" step="1" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white" />
        </div>
        <div>
          <label id="label-back-weight" class="text-[11px] text-slate-400 block mb-1">Valor Back-off</label>
          <input id="ex-form-backoff" type="number" step="1" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white" />
        </div>
      </div>

      <div class="grid grid-cols-2 gap-2">
        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Reps Mín / Máx</label>
          <div class="grid grid-cols-2 gap-1">
            <input id="ex-form-min" type="number" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2 py-2 text-sm text-white text-center" />
            <input id="ex-form-max" type="number" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2 py-2 text-sm text-white text-center" />
          </div>
        </div>
        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Nota Técnica</label>
          <input id="ex-form-tip" type="text" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2 py-2 text-xs text-white" />
        </div>
      </div>

      <div class="flex justify-between items-center pt-2">
        <button id="btn-delete-ex" onclick="deleteExerciseFromModal()" class="text-xs text-rose-400 hover:text-rose-300 font-bold px-2 py-2 hidden">
          <i class="fa-solid fa-trash mr-1"></i> Eliminar
        </button>
        <div class="flex gap-2 ml-auto">
          <button onclick="closeExerciseModal()" class="px-3 py-2 text-xs text-slate-400">Cancelar</button>
          <button onclick="saveExerciseModal()" class="bg-sky-500 text-slate-950 font-extrabold px-4 py-2 rounded-lg text-xs">Guardar</button>
        </div>
      </div>
    </div>
  </div>

  <!-- BOTTOM NAVIGATION -->
  <nav class="fixed bottom-0 inset-x-0 z-40 bg-[#0b0f19]/95 backdrop-blur-md border-t border-slate-800 grid grid-cols-4 text-center text-xs text-slate-400">
    <button onclick="switchTab('workout')" id="nav-workout" class="py-3 flex flex-col items-center gap-1 nav-active">
      <i class="fa-solid fa-dumbbell text-base"></i>
      <span>Rutina & Pesos</span>
    </button>
    <button onclick="switchTab('calendar')" id="nav-calendar" class="py-3 flex flex-col items-center gap-1">
      <i class="fa-solid fa-calendar-days text-base"></i>
      <span>Calendario</span>
    </button>
    <button onclick="switchTab('supps')" id="nav-supps" class="py-3 flex flex-col items-center gap-1">
      <i class="fa-solid fa-capsules text-base"></i>
      <span>Suplementos</span>
    </button>
    <button onclick="switchTab('stats')" id="nav-stats" class="py-3 flex flex-col items-center gap-1">
      <i class="fa-solid fa-chart-line text-base"></i>
      <span>Progreso</span>
    </button>
  </nav>

  <script>
    const STORAGE_KEY = 'vicente_ppl_tracker_v3';
    let editingExerciseIndex = null;

    // Conversión exacta: 1 placa = 4.5 kg. Mancuernas: salto de 2.5 kg.
    const defaultData = {
      currentSplit: 'push',
      calendarYear: 2026,
      calendarMonth: 9,
      selectedDate: '2026-10-15',
      attendance: {},
      suppsByDate: {},
      history: [
        { date: '15 Oct', text: 'App v3 calibrada con sistema de placas (4.5kg c/u), mancuernas (5-30kg) y estructura de 1 Top Set + 2 Back-offs.' }
      ],
      workouts: {
        push: [
          {
            id: 'pec_fly',
            name: 'Pectoral Fly (Máquina)',
            muscle: 'Pecho',
            type: 'placas', // 15 placas = 67.5 kg
            topVal: 15,
            topReps: 10,
            backVal: 13,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Escápulas retraídas, codos semiflexionados. 1 Top Set + 2 Back-off (-2 placas).'
          },
          {
            id: 'db_press',
            name: 'Press con Mancuernas',
            muscle: 'Pecho',
            type: 'mancuernas', // 22.5 kg
            topVal: 22.5,
            topReps: 9,
            backVal: 20,
            backReps: 11,
            rir: 1,
            targetMin: 8,
            targetMax: 10,
            active: true,
            tip: 'Top Set con mancuernas de 22.5 kg y Back-off con 20 kg. Codos a 45°.'
          },
          {
            id: 'shoulder_press',
            name: 'Shoulder Press (Máquina)',
            muscle: 'Hombro',
            type: 'placas', // 39 kg / 4.5 = ~9 placas
            topVal: 9,
            topReps: 9,
            backVal: 8,
            backReps: 11,
            rir: 1,
            targetMin: 8,
            targetMax: 10,
            active: true,
            tip: 'Abdomen firme. 1 Top Set pesada y 2 Back-off.'
          },
          {
            id: 'lat_raise',
            name: 'Elevaciones Laterales',
            muscle: 'Hombro',
            type: 'mancuernas', // 10 kg
            topVal: 10,
            topReps: 12,
            backVal: 7.5,
            backReps: 15,
            rir: 1,
            targetMin: 12,
            targetMax: 15,
            active: true,
            tip: 'Mancuernas de 10 kg para Top Set y 7.5 kg para Back-off. Movimiento estricto.'
          },
          {
            id: 'face_pull',
            name: 'Face Pull en Polea',
            muscle: 'Hombro',
            type: 'placas', // 5 placas
            topVal: 5,
            topReps: 15,
            backVal: 4,
            backReps: 15,
            rir: 2,
            targetMin: 12,
            targetMax: 15,
            active: true,
            tip: '5 placas en Top Set y 4 placas en Back-off.'
          },
          {
            id: 'tricep_ext',
            name: 'Extensión de Tríceps en Polea',
            muscle: 'Tríceps',
            type: 'placas', // 44.5 kg / 4.5 = ~10 placas
            topVal: 10,
            topReps: 10,
            backVal: 9,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Codos fijos. 1 Top Set y 2 Back-off.'
          }
        ],
        pull: [
          {
            id: 'lat_pulldown',
            name: 'Lat Pulldown (Jalón al Pecho)',
            muscle: 'Espalda',
            type: 'placas', // 15 placas = 67.5 kg
            topVal: 15,
            topReps: 9,
            backVal: 13,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Usa tus straps Worx. 1 Top Set (15 placas) y 2 Back-off (13 placas).'
          },
          {
            id: 'db_row',
            name: 'Remo con Mancuerna Unilateral',
            muscle: 'Espalda',
            type: 'mancuernas', // 25 kg
            topVal: 25,
            topReps: 10,
            backVal: 22.5,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Mancuerna de 25 kg para Top Set con straps, 22.5 kg para Back-off.'
          },
          {
            id: 'seated_row',
            name: 'Remo Sentado en Polea',
            muscle: 'Espalda',
            type: 'placas', // 40.5 kg / 4.5 = 9 placas
            topVal: 9,
            topReps: 10,
            backVal: 8,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: '9 placas en Top Set y 8 placas en Back-off.'
          },
          {
            id: 'incline_curl',
            name: 'Curl de Bíceps Banco Inclinado',
            muscle: 'Bíceps',
            type: 'mancuernas', // 10 kg c/u
            topVal: 10,
            topReps: 10,
            backVal: 7.5,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Énfasis en estiramiento. Mancuernas de 10 kg (Top) y 7.5 kg (Back-off).'
          },
          {
            id: 'hammer_curl',
            name: 'Curl Martillo con Mancuernas',
            muscle: 'Bíceps',
            type: 'mancuernas', // 10 kg c/u
            topVal: 10,
            topReps: 11,
            backVal: 7.5,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Agarre neutro estricto. 10 kg y 7.5 kg.'
          },
          {
            id: 'machine_curl',
            name: 'Curl de Bíceps en Máquina',
            muscle: 'Bíceps',
            type: 'placas', // 22 kg / 4.5 = ~5 placas
            topVal: 5,
            topReps: 10,
            backVal: 4,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Alterna este con Inclinado para no hacer 3 ejercicios de bíceps el mismo día.'
          }
        ],
        legs: [
          {
            id: 'leg_press',
            name: 'Prensa Inclinada',
            muscle: 'Cuádriceps',
            type: 'placas', // ~26 placas o kg libres
            topVal: 120,
            topReps: 10,
            backVal: 100,
            backReps: 12,
            rir: 2,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: '1 Top Set pesada y 2 Back-off.'
          },
          {
            id: 'bulgarian_squat',
            name: 'Sentadilla Búlgara',
            muscle: 'Isquios/Glúteo',
            type: 'mancuernas', // 14 kg
            topVal: 14,
            topReps: 10,
            backVal: 12,
            backReps: 12,
            rir: 1,
            targetMin: 8,
            targetMax: 12,
            active: true,
            tip: 'Mancuernas de 14 kg (Top) y 12 kg (Back-off).'
          },
          {
            id: 'leg_ext',
            name: 'Extensión de Cuádriceps',
            muscle: 'Cuádriceps',
            type: 'placas', // 50 kg / 4.5 = ~11 placas
            topVal: 11,
            topReps: 12,
            backVal: 9,
            backReps: 14,
            rir: 1,
            targetMin: 10,
            targetMax: 15,
            active: true,
            tip: '11 placas en Top Set, 9 placas en Back-off.'
          },
          {
            id: 'leg_curl',
            name: 'Leg Curl (Curl Femoral)',
            muscle: 'Isquios/Glúteo',
            type: 'placas', // 45 kg / 4.5 = 10 placas
            topVal: 10,
            topReps: 11,
            backVal: 8,
            backReps: 13,
            rir: 1,
            targetMin: 10,
            targetMax: 14,
            active: true,
            tip: '10 placas en Top Set, 8 placas en Back-off.'
          },
          {
            id: 'calf_raise',
            name: 'Elevación de Talones (Gemelos)',
            muscle: 'Gemelos',
            type: 'placas', // 60 kg / 4.5 = ~13 placas
            topVal: 13,
            topReps: 14,
            backVal: 11,
            backReps: 15,
            rir: 1,
            targetMin: 12,
            targetMax: 16,
            active: true,
            tip: '13 placas en Top Set, 11 placas en Back-off.'
          }
        ]
      }
    };

    const supplementsCatalog = [
      { id: 'creatine', name: 'Creatina Monohidrato (5g)', desc: 'Saturación de fosfocreatina y fuerza en Top Sets' },
      { id: 'whey', name: 'Batido Whey Protein + Almendras', desc: 'Síntesis proteica y aporte de grasas saludables' },
      { id: 'ashwagandha', name: 'Ashwagandha', desc: 'Control de cortisol post-entreno y descanso' },
      { id: 'lions_mane', name: 'Melena de León (Lion’s Mane)', desc: 'Enfoque cognitivo para estudio y conexión mente-músculo' }
    ];

    let state = loadState();

    function loadState() {
      const saved = localStorage.getItem(STORAGE_KEY);
      if (saved) {
        try { return JSON.parse(saved); } catch (e) { return JSON.parse(JSON.stringify(defaultData)); }
      }
      return JSON.parse(JSON.stringify(defaultData));
    }

    function saveState() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      updateHeaderStreak();
    }

    function getDisplayWeight(ex, val) {
      if (ex.type === 'placas') {
        const kg = (val * 4.5).toFixed(1);
        return `${val} placas (${kg} kg)`;
      } else if (ex.type === 'mancuernas') {
        return `${val} kg c/u`;
      }
      return `${val} kg`;
    }

    function getStepValue(ex) {
      if (ex.type === 'placas') return 1; // 1 placa = 4.5 kg
      if (ex.type === 'mancuernas') return 2.5; // Salto de mancuerna
      return 5;
    }

    function getOverloadAdvice(ex) {
      const { topReps, rir, targetMin, targetMax, topVal } = ex;
      const step = getStepValue(ex);
      const unitLabel = ex.type === 'placas' ? 'placa(s)' : 'kg';

      if (topReps < targetMin || (topReps === targetMin && rir === 0)) {
        const dropVal = Math.max(1, topVal - step);
        return {
          color: 'bg-rose-500/15 border-rose-500/40 text-rose-300',
          icon: 'fa-hand',
          title: 'NO SUBIR PESO (Prioriza Técnica)',
          detail: `Estás en ${topReps} reps (RIR ${rir}). Mantén ${getDisplayWeight(ex, topVal)} hasta dominar ${targetMin}-${targetMax} reps estrictas.`
        };
      }

      if (topReps >= targetMax && rir >= 1) {
        const nextVal = +(topVal + step).toFixed(1);
        return {
          color: 'bg-emerald-500/15 border-emerald-500/40 text-emerald-300',
          icon: 'fa-arrow-trend-up',
          title: `¡LISTO PARA SUBIR! (+${step} ${unitLabel})`,
          detail: `Dominaste ${topReps} reps con RIR ${rir}. Sube tu Top Set a ${getDisplayWeight(ex, nextVal)} en la próxima sesión.`
        };
      }

      return {
        color: 'bg-amber-500/15 border-amber-500/40 text-amber-200',
        icon: 'fa-bullseye',
        title: 'CONSOLIDAR PESO ACTUAL',
        detail: `Zona óptima (${topReps} reps, RIR ${rir}). Mantén ${getDisplayWeight(ex, topVal)} y busca sumar +1 repetición en la siguiente sesión.`
      };
    }

    function renderVolumeWarning() {
      const split = state.currentSplit;
      const activeList = state.workouts[split].filter(e => e.active !== false);
      const counts = {};
      activeList.forEach(e => {
        const m = e.muscle || 'General';
        counts[m] = (counts[m] || 0) + 1;
      });

      const banner = document.getElementById('volume-warning-banner');
      const warnings = [];

      if ((counts['Bíceps'] || 0) >= 3) {
        warnings.push(`Tienes <strong>${counts['Bíceps']} ejercicios de Bíceps</strong> activos. Tras espalda, con <strong>2 ejercicios</strong> es suficiente para hipertrofia sin sobre-fatigar los tendones. Usa el botón <strong>"Rotar"</strong> en uno.`);
      }
      if ((counts['Tríceps'] || 0) >= 3) {
        warnings.push(`Tienes <strong>${counts['Tríceps']} ejercicios de Tríceps</strong> activos. Con 2 variantes bastas.`);
      }

      if (warnings.length > 0) {
        banner.className = 'p-3.5 rounded-xl bg-amber-500/15 border border-amber-500/40 text-amber-200 text-xs space-y-1.5';
        banner.innerHTML = `
          <div class="font-extrabold flex items-center gap-1.5 text-amber-300">
            <i class="fa-solid fa-triangle-exclamation"></i> Alerta de Optimización de Volumen
          </div>
          ${warnings.map(w => `<p class="leading-relaxed">${w}</p>`).join('')}
        `;
      } else {
        banner.className = 'hidden';
        banner.innerHTML = '';
      }
    }

    function renderExercises() {
      const split = state.currentSplit;
      const list = state.workouts[split];
      const container = document.getElementById('exercises-container');
      const pausedWrapper = document.getElementById('paused-exercises-wrapper');
      const pausedContainer = document.getElementById('paused-exercises-container');

      ['push', 'pull', 'legs'].forEach(s => {
        const btn = document.getElementById(`btn-split-${s}`);
        if (s === split) {
          btn.className = 'py-2 rounded-lg font-extrabold text-sm bg-slate-800 text-white shadow border border-slate-600 transition-all';
        } else {
          btn.className = 'py-2 rounded-lg font-semibold text-sm text-slate-400 hover:text-white transition-all';
        }
      });

      const titles = {
        push: 'Bloque Push (Pecho, Hombro y Tríceps)',
        pull: 'Bloque Pull (Espalda y Bíceps)',
        legs: 'Bloque Legs (Cuádriceps, Isquios, Glúteo y Gemelos)'
      };
      document.getElementById('split-title').textContent = titles[split];

      renderVolumeWarning();

      const activeItems = [];
      const pausedItems = [];

      list.forEach((ex, idx) => {
        if (ex.active === false) pausedItems.push({ ex, idx });
        else activeItems.push({ ex, idx });
      });

      container.innerHTML = activeItems.map(({ ex, idx }) => {
        const advice = getOverloadAdvice(ex);
        const step = getStepValue(ex);
        return `
          <div class="card p-4 space-y-3">
            <div class="flex justify-between items-start gap-2">
              <div>
                <div class="flex items-center gap-2 flex-wrap">
                  <span class="text-[10px] font-extrabold uppercase px-2 py-0.5 rounded bg-sky-500/15 text-sky-300 border border-sky-500/30">${ex.muscle || 'General'}</span>
                  <h3 class="font-bold text-base text-white">${ex.name}</h3>
                </div>
                <span class="text-xs text-slate-400">Meta: ${ex.targetMin}-${ex.targetMax} reps • Estructura: <strong class="text-white">1 Top Set + 2 Back-off</strong></span>
              </div>

              <div class="flex items-center gap-1">
                <button onclick="moveExercise('${split}', ${idx}, -1)" class="p-1.5 text-xs text-slate-400 hover:text-white bg-slate-900 rounded-lg border border-slate-800"><i class="fa-solid fa-arrow-up"></i></button>
                <button onclick="moveExercise('${split}', ${idx}, 1)" class="p-1.5 text-xs text-slate-400 hover:text-white bg-slate-900 rounded-lg border border-slate-800"><i class="fa-solid fa-arrow-down"></i></button>
                <button onclick="toggleExerciseActive('${split}', ${idx})" class="px-2 py-1.5 text-xs text-amber-300 bg-amber-500/10 border border-amber-500/30 rounded-lg font-semibold"><i class="fa-solid fa-pause mr-1"></i>Rotar</button>
                <button onclick="openExerciseModal(${idx})" class="p-1.5 text-xs text-sky-400 bg-sky-500/10 border border-sky-500/30 rounded-lg"><i class="fa-solid fa-pen"></i></button>
                <button onclick="toggleTip('${ex.id}')" class="p-1.5 text-xs text-slate-300 bg-slate-900 border border-slate-700 rounded-lg"><i class="fa-solid fa-circle-info"></i></button>
              </div>
            </div>

            <div id="tip-${ex.id}" class="hidden text-xs bg-slate-900/90 border border-slate-700 text-slate-300 p-2.5 rounded-lg">
              💡 <strong>Coaching Tip:</strong> ${ex.tip}
            </div>

            <!-- TOP SET (1 Serie) -->
            <div class="bg-slate-900/80 border border-slate-800 rounded-xl p-3 space-y-2">
              <div class="flex justify-between items-center">
                <span class="text-xs font-extrabold uppercase tracking-wider text-sky-400">🔥 1 Top Set (Serie Efectiva Máxima)</span>
                <span class="text-xs text-slate-400">${ex.targetMin}-${ex.targetMax} reps</span>
              </div>
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label class="text-[11px] text-slate-400 block mb-1">Carga (${ex.type === 'placas' ? 'Placas' : 'kg'})</label>
                  <div class="flex items-center justify-between bg-slate-950 border border-slate-700 rounded-lg p-1">
                    <button onclick="adjustValue('${split}', ${idx}, 'topVal', -${step})" class="w-8 h-8 rounded bg-slate-800 text-white font-bold">-</button>
                    <span class="font-extrabold text-xs text-white text-center">${getDisplayWeight(ex, ex.topVal)}</span>
                    <button onclick="adjustValue('${split}', ${idx}, 'topVal', ${step})" class="w-8 h-8 rounded bg-slate-800 text-sky-400 font-bold">+</button>
                  </div>
                </div>
                <div>
                  <label class="text-[11px] text-slate-400 block mb-1">Reps logradas</label>
                  <div class="flex items-center justify-between bg-slate-950 border border-slate-700 rounded-lg p-1">
                    <button onclick="adjustValue('${split}', ${idx}, 'topReps', -1)" class="w-8 h-8 rounded bg-slate-800 text-white font-bold">-</button>
                    <span class="font-extrabold text-sm text-white">${ex.topReps}</span>
                    <button onclick="adjustValue('${split}', ${idx}, 'topReps', 1)" class="w-8 h-8 rounded bg-slate-800 text-emerald-400 font-bold">+</button>
                  </div>
                </div>
              </div>
            </div>

            <!-- BACK-OFF SETS (2 Series) -->
            <div class="bg-slate-900/40 border border-slate-800/80 rounded-xl p-3 space-y-2">
              <div class="flex justify-between items-center">
                <span class="text-xs font-bold uppercase tracking-wider text-indigo-400">📉 2 Series Back-off (Volumen de Carga)</span>
                <button onclick="autoCalcBackoff('${split}', ${idx})" class="text-[11px] text-indigo-300 underline">Auto -1 paso</button>
              </div>
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label class="text-[11px] text-slate-400 block mb-1">Carga Back-off</label>
                  <div class="flex items-center justify-between bg-slate-950 border border-slate-800 rounded-lg p-1">
                    <button onclick="adjustValue('${split}', ${idx}, 'backVal', -${step})" class="w-7 h-7 rounded bg-slate-800 text-slate-300 font-bold">-</button>
                    <span class="font-bold text-xs text-slate-200 text-center">${getDisplayWeight(ex, ex.backVal)}</span>
                    <button onclick="adjustValue('${split}', ${idx}, 'backVal', ${step})" class="w-7 h-7 rounded bg-slate-800 text-slate-300 font-bold">+</button>
                  </div>
                </div>
                <div>
                  <label class="text-[11px] text-slate-400 block mb-1">Reps objetivo (2 series)</label>
                  <div class="flex items-center justify-between bg-slate-950 border border-slate-800 rounded-lg p-1">
                    <button onclick="adjustValue('${split}', ${idx}, 'backReps', -1)" class="w-7 h-7 rounded bg-slate-800 text-slate-300 font-bold">-</button>
                    <span class="font-bold text-xs text-slate-200">${ex.backReps} reps</span>
                    <button onclick="adjustValue('${split}', ${idx}, 'backReps', 1)" class="w-7 h-7 rounded bg-slate-800 text-slate-300 font-bold">+</button>
                  </div>
                </div>
              </div>
            </div>

            <!-- RIR -->
            <div class="flex items-center justify-between pt-1">
              <span class="text-xs text-slate-400">Esfuerzo Top Set (RIR):</span>
              <div class="flex gap-1.5">
                ${[0, 1, 2, 3].map(r => `
                  <button onclick="setRir('${split}', ${idx},${r})" class="px-2.5 py-1 rounded text-xs font-bold border ${ex.rir === r ? 'bg-sky-500 text-slate-950 border-sky-400' : 'bg-slate-900 text-slate-400 border-slate-700'}">
                    RIR ${r}${r === 0 ? ' (Fallo)' : ''}
                  </button>
                `).join('')}
              </div>
            </div>

            <!-- ALERTA SOBRECARGA -->
            <div class="p-3 rounded-xl border text-xs ${advice.color} flex items-start gap-2.5">
              <i class="fa-solid ${advice.icon} text-sm mt-0.5"></i>
              <div>
                <div class="font-extrabold">${advice.title}</div>
                <p class="opacity-90 leading-snug mt-0.5">${advice.detail}</p>
              </div>
            </div>
          </div>
        `;
      }).join('');

      if (pausedItems.length > 0) {
        pausedWrapper.classList.remove('hidden');
        pausedContainer.innerHTML = pausedItems.map(({ ex, idx }) => `
          <div class="card p-3 opacity-75 flex justify-between items-center">
            <div>
              <span class="text-[10px] font-bold uppercase px-1.5 py-0.5 rounded bg-slate-800 text-slate-400 mr-1">${ex.muscle}</span>
              <span class="font-bold text-sm text-slate-200">${ex.name}</span>
              <div class="text-xs text-slate-400">Marca: ${getDisplayWeight(ex, ex.topVal)} × ${ex.topReps} reps</div>
            </div>
            <div class="flex gap-1.5">
              <button onclick="toggleExerciseActive('${split}', ${idx})" class="bg-emerald-500/20 text-emerald-300 border border-emerald-500/40 px-2.5 py-1.5 rounded-lg text-xs font-bold"><i class="fa-solid fa-play mr-1"></i>Activar</button>
              <button onclick="openExerciseModal(${idx})" class="bg-slate-800 text-slate-300 px-2.5 py-1.5 rounded-lg text-xs"><i class="fa-solid fa-pen"></i></button>
            </div>
          </div>
        `).join('');
      } else {
        pausedWrapper.classList.add('hidden');
        pausedContainer.innerHTML = '';
      }
    }

    function toggleExerciseActive(split, idx) {
      const ex = state.workouts[split][idx];
      ex.active = !ex.active;
      addHistory(`${ex.name}: ${ex.active ? 'Activado' : 'Pausado'}`);
      saveState();
      renderExercises();
    }

    function moveExercise(split, idx, dir) {
      const list = state.workouts[split];
      const newIdx = idx + dir;
      if (newIdx < 0 || newIdx >= list.length) return;
      const temp = list[idx];
      list[idx] = list[newIdx];
      list[newIdx] = temp;
      saveState();
      renderExercises();
    }

    // MODAL
    function openExerciseModal(idx) {
      editingExerciseIndex = idx;
      const split = state.currentSplit;
      const modalTitle = document.getElementById('modal-ex-title');
      const deleteBtn = document.getElementById('btn-delete-ex');

      if (idx !== null) {
        const ex = state.workouts[split][idx];
        modalTitle.textContent = 'Editar Ejercicio';
        deleteBtn.classList.remove('hidden');
        document.getElementById('ex-form-name').value = ex.name;
        document.getElementById('ex-form-muscle').value = ex.muscle || 'Pecho';
        document.getElementById('ex-form-type').value = ex.type || 'placas';
        document.getElementById('ex-form-top').value = ex.topVal;
        document.getElementById('ex-form-backoff').value = ex.backVal;
        document.getElementById('ex-form-min').value = ex.targetMin;
        document.getElementById('ex-form-max').value = ex.targetMax;
        document.getElementById('ex-form-tip').value = ex.tip || '';
      } else {
        modalTitle.textContent = 'Nuevo Ejercicio';
        deleteBtn.classList.add('hidden');
        document.getElementById('ex-form-name').value = '';
        document.getElementById('ex-form-muscle').value = 'Pecho';
        document.getElementById('ex-form-type').value = 'placas';
        document.getElementById('ex-form-top').value = 10;
        document.getElementById('ex-form-backoff').value = 9;
        document.getElementById('ex-form-min').value = 8;
        document.getElementById('ex-form-max').value = 12;
        document.getElementById('ex-form-tip').value = '1 Top Set pesada + 2 Back-off.';
      }
      updateModalStepHint();
      document.getElementById('modal-ex').classList.remove('hidden');
    }

    function updateModalStepHint() {
      const type = document.getElementById('ex-form-type').value;
      const topLabel = document.getElementById('label-top-weight');
      const backLabel = document.getElementById('label-back-weight');
      if (type === 'placas') {
        topLabel.textContent = 'Top Set (Nº Placas)';
        backLabel.textContent = 'Back-off (Nº Placas)';
      } else if (type === 'mancuernas') {
        topLabel.textContent = 'Top Set (kg c/u)';
        backLabel.textContent = 'Back-off (kg c/u)';
      } else {
        topLabel.textContent = 'Top Set (kg)';
        backLabel.textContent = 'Back-off (kg)';
      }
    }

    function closeExerciseModal() {
      document.getElementById('modal-ex').classList.add('hidden');
      editingExerciseIndex = null;
    }

    function saveExerciseModal() {
      const split = state.currentSplit;
      const name = document.getElementById('ex-form-name').value.trim();
      if (!name) return;

      const muscle = document.getElementById('ex-form-muscle').value;
      const type = document.getElementById('ex-form-type').value;
      const topVal = parseFloat(document.getElementById('ex-form-top').value) || 5;
      const backVal = parseFloat(document.getElementById('ex-form-backoff').value) || 4;
      const targetMin = parseInt(document.getElementById('ex-form-min').value) || 8;
      const targetMax = parseInt(document.getElementById('ex-form-max').value) || 12;
      const tip = document.getElementById('ex-form-tip').value.trim();

      if (editingExerciseIndex !== null) {
        const ex = state.workouts[split][editingExerciseIndex];
        ex.name = name;
        ex.muscle = muscle;
        ex.type = type;
        ex.topVal = topVal;
        ex.backVal = backVal;
        ex.targetMin = targetMin;
        ex.targetMax = targetMax;
        ex.tip = tip;
        addHistory(`Editado: ${name}`);
      } else {
        state.workouts[split].push({
          id: 'custom_' + Date.now(),
          name,
          muscle,
          type,
          topVal,
          topReps: targetMin,
          backVal,
          backReps: targetMax,
          rir: 1,
          targetMin,
          targetMax,
          active: true,
          tip
        });
        addHistory(`Nuevo ejercicio: ${name}`);
      }

      saveState();
      closeExerciseModal();
      renderExercises();
    }

    function deleteExerciseFromModal() {
      if (editingExerciseIndex === null) return;
      const split = state.currentSplit;
      const ex = state.workouts[split][editingExerciseIndex];
      if (confirm(`¿Eliminar "${ex.name}"?`)) {
        state.workouts[split].splice(editingExerciseIndex, 1);
        addHistory(`Eliminado: ${ex.name}`);
        saveState();
        closeExerciseModal();
        renderExercises();
      }
    }

    function toggleTip(id) {
      const el = document.getElementById(`tip-${id}`);
      if (el) el.classList.toggle('hidden');
    }

    function adjustValue(split, idx, field, delta) {
      const ex = state.workouts[split][idx];
      const nextVal = Math.max(0, +(ex[field] + delta).toFixed(1));
      ex[field] = nextVal;
      if (field === 'topVal') {
        addHistory(`${ex.name}: Top Set actualizado a ${getDisplayWeight(ex, nextVal)}`);
      }
      saveState();
      renderExercises();
    }

    function autoCalcBackoff(split, idx) {
      const ex = state.workouts[split][idx];
      const step = getStepValue(ex);
      ex.backVal = Math.max(1, ex.topVal - step);
      saveState();
      renderExercises();
    }

    function setRir(split, idx, rirVal) {
      state.workouts[split][idx].rir = rirVal;
      saveState();
      renderExercises();
    }

    function switchSplit(split) {
      state.currentSplit = split;
      saveState();
      renderExercises();
    }

    // CALENDARIO Y SUPLEMENTOS
    function getTodayISO() {
      const now = new Date();
      return `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')}`;
    }

    function logTodaySession() {
      const today = getTodayISO();
      state.attendance[today] = state.currentSplit;
      state.selectedDate = today;
      addHistory(`Sesión de ${state.currentSplit.toUpperCase()} registrada.`);
      saveState();
      alert(`¡Sesión de ${state.currentSplit.toUpperCase()} registrada hoy!`);
    }

    function renderCalendar() {
      const year = state.calendarYear;
      const month = state.calendarMonth;
      const monthNames = ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio', 'Julio', 'Agosto', 'Septiembre', 'Octubre', 'Noviembre', 'Diciembre'];
      document.getElementById('calendar-month-title').textContent = `${monthNames[month]} ${year}`;

      const firstDay = new Date(year, month, 1).getDay();
      const offset = (firstDay + 6) % 7;
      const daysInMonth = new Date(year, month + 1, 0).getDate();

      const grid = document.getElementById('calendar-grid');
      let html = '';

      for (let i = 0; i < offset; i++) html += `<div class="h-10"></div>`;

      for (let day = 1; day <= daysInMonth; day++) {
        const dateStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
        const status = state.attendance[dateStr];
        const isSelected = state.selectedDate === dateStr;

        let badgeClass = 'bg-slate-900 text-slate-300 border border-slate-800';
        if (status === 'push') badgeClass = 'badge-push font-extrabold';
        if (status === 'pull') badgeClass = 'badge-pull font-extrabold';
        if (status === 'legs') badgeClass = 'badge-legs font-extrabold';
        if (status === 'rest') badgeClass = 'badge-rest';

        html += `<button onclick="selectCalendarDate('${dateStr}')" class="h-10 rounded-lg text-xs flex flex-col items-center justify-center transition-all ${badgeClass} ${isSelected ? 'ring-2 ring-sky-400' : ''}"><span>${day}</span></button>`;
      }
      grid.innerHTML = html;
      updateSelectedDatePanel();
    }

    function selectCalendarDate(dateStr) {
      state.selectedDate = dateStr;
      saveState();
      renderCalendar();
    }

    function updateSelectedDatePanel() {
      const dateStr = state.selectedDate;
      const status = state.attendance[dateStr];
      document.getElementById('selected-date-label').textContent = `Fecha: ${dateStr}`;
      const statusMap = { push: '🔴 PUSH', pull: '🔵 PULL', legs: '🟢 LEGS', rest: '⚪ Descanso' };
      document.getElementById('selected-date-status').textContent = status ? statusMap[status] : 'Sin registro';
    }

    function setDayStatus(status) {
      const dateStr = state.selectedDate;
      if (!status) delete state.attendance[dateStr];
      else state.attendance[dateStr] = status;
      saveState();
      renderCalendar();
    }

    function changeMonth(delta) {
      state.calendarMonth += delta;
      if (state.calendarMonth > 11) { state.calendarMonth = 0; state.calendarYear++; }
      else if (state.calendarMonth < 0) { state.calendarMonth = 11; state.calendarYear--; }
      saveState();
      renderCalendar();
    }

    function updateHeaderStreak() {
      const prefix = `${state.calendarYear}-${String(state.calendarMonth + 1).padStart(2, '0')}`;
      const trainedDays = Object.entries(state.attendance).filter(([k, v]) => k.startsWith(prefix) && v !== 'rest').length;
      document.getElementById('streak-badge').textContent = `🔥 ${trainedDays} entrenos este mes`;
    }

    function renderSupplements() {
      const today = getTodayISO();
      if (!state.suppsByDate[today]) state.suppsByDate[today] = {};
      const todaySupps = state.suppsByDate[today];
      const container = document.getElementById('supps-list');
      let completed = 0;

      container.innerHTML = supplementsCatalog.map(s => {
        const checked = !!todaySupps[s.id];
        if (checked) completed++;
        return `
          <div onclick="toggleSupp('${s.id}')" class="p-3 rounded-xl border flex items-center justify-between cursor-pointer transition-all ${checked ? 'bg-emerald-500/10 border-emerald-500/40' : 'bg-slate-900 border-slate-800'}">
            <div>
              <div class="font-bold text-sm ${checked ? 'text-emerald-300 line-through' : 'text-white'}">${s.name}</div>
              <div class="text-xs text-slate-400">${s.desc}</div>
            </div>
            <div class="w-6 h-6 rounded-full flex items-center justify-center border ${checked ? 'bg-emerald-500 border-emerald-400 text-slate-950' : 'border-slate-600'}">
              ${checked ? '<i class="fa-solid fa-check text-xs"></i>' : ''}
            </div>
          </div>
        `;
      }).join('');
      document.getElementById('supps-progress').textContent = `${completed}/${supplementsCatalog.length} hoy`;
    }

    function toggleSupp(id) {
      const today = getTodayISO();
      if (!state.suppsByDate[today]) state.suppsByDate[today] = {};
      state.suppsByDate[today][id] = !state.suppsByDate[today][id];
      saveState();
      renderSupplements();
    }

    function renderStats() {
      const summaryEl = document.getElementById('top-marks-summary');
      summaryEl.innerHTML = ['push', 'pull', 'legs'].map(sp => {
        const items = state.workouts[sp].map(e => `
          <div class="flex justify-between py-1 border-b border-slate-800/70">
            <span class="text-slate-300">${e.name} ${e.active === false ? '<em class="text-amber-400">(Pausado)</em>' : ''}</span>
            <span class="font-bold text-sky-400">Top: ${getDisplayWeight(e, e.topVal)} × ${e.topReps} (RIR ${e.rir})</span>
          </div>
        `).join('');
        return `<div class="mb-3"><div class="font-extrabold uppercase text-xs text-slate-400 mb-1">${sp}</div>${items}</div>`;
      }).join('');

      document.getElementById('history-log').innerHTML = state.history.slice().reverse().map(item => `
        <div class="bg-slate-900 border border-slate-800 p-2 rounded-lg">
          <span class="text-sky-400 font-semibold">${item.date}:</span> <span class="text-slate-300">${item.text}</span>
        </div>
      `).join('');
    }

    function addHistory(text) {
      const now = new Date();
      state.history.push({ date: now.toLocaleDateString('es-CL', { day: '2-digit', month: 'short' }), text });
      if (state.history.length > 60) state.history.shift();
    }

    function copySummaryToClipboard() {
      let lines = ['📊 RESUMEN PPL (Placas 4.5kg y Mancuernas 5-30kg):'];
      ['push', 'pull', 'legs'].forEach(sp => {
        lines.push(`\n[${sp.toUpperCase()}]`);
        state.workouts[sp].forEach(e => {
          lines.push(`- ${e.name}: Top ${getDisplayWeight(e, e.topVal)} x ${e.topReps} reps | Back-off ${getDisplayWeight(e, e.backVal)} x ${e.backReps} reps`);
        });
      });
      navigator.clipboard.writeText(lines.join('\n')).then(() => {
        const fb = document.getElementById('copy-feedback');
        fb.classList.remove('hidden');
        setTimeout(() => fb.classList.add('hidden'), 3000);
      });
    }

    function resetToDefaults() {
      if (confirm('¿Restaurar valores iniciales?')) {
        localStorage.removeItem(STORAGE_KEY);
        state = loadState();
        renderAll();
      }
    }

    function switchTab(tab) {
      ['workout', 'calendar', 'supps', 'stats'].forEach(t => {
        document.getElementById(`tab-${t}`).classList.toggle('hidden', t !== tab);
        const nav = document.getElementById(`nav-${t}`);
        if (t === tab) nav.classList.add('nav-active');
        else nav.classList.remove('nav-active');
      });
      if (tab === 'calendar') renderCalendar();
      if (tab === 'supps') renderSupplements();
      if (tab === 'stats') renderStats();
    }

    function renderAll() {
      renderExercises();
      renderCalendar();
      renderSupplements();
      updateHeaderStreak();
    }

    renderAll();
  </script>
</body>
</html>
