<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - Olimpiadas Vonex 2026</title>
    <!-- Tailwind CSS para diseño responsivo e interactivo -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js para visualización de datos avanzada -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts - Plus Jakarta Sans -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background: radial-gradient(circle at top right, #1e1b4b 0%, #0f172a 60%, #020617 100%);
        }
        .premium-card {
            background: rgba(15, 23, 42, 0.45);
            border: 1px solid rgba(255, 255, 255, 0.04);
            backdrop-filter: blur(16px);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .premium-card:hover {
            transform: translateY(-4px);
            border-color: rgba(99, 102, 241, 0.25);
            box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5), 0 0 25px -5px rgba(99, 102, 241, 0.15);
        }
        .table-row-animate {
            transition: all 0.2s ease;
        }
        .table-row-animate:hover {
            background: rgba(255, 255, 255, 0.02);
            transform: scale(1.005);
        }
    </style>
</head>
<body class="text-slate-100 min-h-screen antialiased">

    <!-- Encabezado Estilo Barra de Cristal -->
    <header class="border-b border-slate-800/80 bg-slate-950/40 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3.5">
                <div class="bg-gradient-to-tr from-indigo-600 to-violet-500 p-2.5 rounded-xl text-white font-extrabold text-xl tracking-wider shadow-lg shadow-indigo-500/20">V</div>
                <div>
                    <h1 class="text-base font-extrabold text-white tracking-tight sm:text-lg">OLIMPIADAS VONEX 2026</h1>
                    <p class="text-xs text-indigo-400 font-semibold tracking-wide uppercase">Dashboard de Pagos Automatizado</p>
                </div>
            </div>
            <div class="flex items-center space-x-2.5">
                <span class="flex h-2.5 w-2.5 relative">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-emerald-500 shadow-[0_0_10px_#10b981]"></span>
                </span>
                <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-3 py-1 rounded-full border border-emerald-500/20 tracking-wider uppercase">Conectado en Vivo</span>
            </div>
        </div>
    </header>

    <!-- Alerta de Sincronización Estructurada -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-4 rounded-xl text-sm font-medium shadow-lg">
            ⚠️ Alerta de Sincronización: No se pueden leer los datos. Asegúrate de publicar la pestaña DATOS_WEB como CSV.
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">
        <!-- Tarjetas de Resumen Dinámicas -->
        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <!-- Avance General -->
            <div class="premium-card rounded-2xl p-6 flex flex-col justify-between shadow-xl relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-indigo-500 to-violet-500"></div>
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Avance General</p>
                    <h3 class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-indigo-400 to-violet-400 mt-2 tracking-tight" id="txt-avance-global">...</h3>
                </div>
                <div class="w-full bg-slate-800 rounded-full h-2 mt-5 overflow-hidden">
                    <div id="bar-avance-global" class="bg-gradient-to-r from-indigo-500 to-violet-500 h-full rounded-full transition-all duration-700" style="width: 0%"></div>
                </div>
            </div>
            
            <!-- Total Recaudado -->
            <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-emerald-500 to-teal-500"></div>
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Total Recaudado</p>
                <h3 class="text-3xl font-extrabold text-emerald-400 mt-2 tracking-tight" id="txt-recaudado-global">...</h3>
                <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2" id="txt-meta-global">Meta Total: ...</p>
            </div>

            <!-- Estudiantes Pagantes -->
            <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-sky-500 to-blue-500"></div>
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Estudiantes Pagantes</p>
                <h3 class="text-3xl font-extrabold text-sky-400 mt-2 tracking-tight" id="txt-pagantes-global">...</h3>
                <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2" id="txt-meta-alumnos">Meta Alumnos: ...</p>
            </div>

            <!-- Monto Pendiente -->
            <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-rose-500 to-red-500"></div>
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Monto Pendiente (Falta)</p>
                <h3 class="text-3xl font-extrabold text-rose-400 mt-2 tracking-tight" id="txt-falta-global">...</h3>
                <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2">Por recaudar</p>
            </div>
        </section>

        <!-- Bloque de Gráficos e Información Detallada -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <div class="lg:col-span-2 space-y-8">
                <!-- Gráfico de Comparativa -->
                <section class="premium-card rounded-2xl p-6 shadow-xl">
                    <h3 class="text-base font-bold text-white mb-5 flex items-center gap-2 tracking-tight">
                        <span class="p-2 bg-indigo-500/10 text-indigo-400 rounded-xl">📊</span> Comparativa de Recaudación (S/)
                    </h3>
                    <div class="relative h-80"><canvas id="chartTutors"></canvas></div>
                </section>

                <!-- Tabla Detallada Optimizada -->
                <section class="premium-card rounded-2xl overflow-hidden shadow-2xl">
                    <div class="p-5 border-b border-slate-800/80 bg-slate-950/20 flex items-center justify-between">
                        <h3 class="text-base font-bold text-white tracking-tight flex items-center gap-2">
                            <span>Detalle por Tutor</span>
                        </h3>
                        <span class="text-[11px] font-bold text-slate-400 bg-slate-800/80 px-3 py-1 rounded-full border border-slate-700/50 uppercase tracking-wider">Métricas en Vivo</span>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-950/50 text-slate-400 text-[11px] font-bold uppercase tracking-wider border-b border-slate-800">
                                    <th class="py-4 px-5">Tutor</th>
                                    <th class="py-4 px-4">Ciclo</th>
                                    <th class="py-4 px-4 text-center">Meta Est.</th>
                                    <th class="py-4 px-4 text-center">Pagantes</th>
                                    <th class="py-4 px-4 text-right">Meta (S/)</th>
                                    <th class="py-4 px-4 text-right text-slate-400/80">Efectivo</th>
                                    <th class="py-4 px-4 text-right text-slate-400/80">Yape</th>
                                    <th class="py-4 px-5 text-right text-emerald-400">Recaudado</th>
                                    <th class="py-4 px-5 text-center">% Avance</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800/40 text-sm" id="table-body-tutors">
                                <!-- Filas Dinámicas -->
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>

            <!-- Leaderboard / Ranking Columna Derecha -->
            <div class="lg:col-span-1 space-y-8">
                <section class="premium-card rounded-2xl p-6 shadow-xl flex flex-col h-full">
                    <div class="mb-5 border-b border-slate-800/80 pb-4">
                        <h3 class="text-base font-bold text-white flex items-center gap-2 tracking-tight">
                            <span class="p-2 bg-amber-500/10 text-amber-400 rounded-xl">🏆</span> Tabla de Posiciones
                        </h3>
                    </div>
                    <div class="space-y-3.5 flex-1" id="leaderboard-container">
                        <!-- Puestos Dinámicos -->
                    </div>
                </section>
            </div>
        </div>
    </main>

    <script>
        // Endpoint dinámico oficial directo de tu hoja Google Sheets (DATOS_WEB)
        const SHEET_JSON_URL = 'https://docs.google.com/spreadsheets/d/1z2qJzZMr5c_lTLUDU2uXEZpUJ8JhL-tq8COC5GXKcVQ/gviz/tq?tqx=out:json&gid=6209676';

        let myChart = null;

        function cleanNum(val) {
            if (!val) return 0;
            let clean = val.toString().replace(/[S\/%,\s]/g, '').trim();
            return parseFloat(clean) || 0;
        }

        function getVal(cell, isNum = false) {
            if (!cell) return isNum ? 0 : '';
            if (cell.v === null || cell.v === undefined) return isNum ? 0 : '';
            if (isNum) {
                let val = cell.v;
                if (typeof val === 'string') {
                    val = val.replace(/[S\/%,\s]/g, '').trim();
                    return parseFloat(val) || 0;
                }
                return parseFloat(val) || 0;
            }
            return cell.f || cell.v.toString();
        }

        async function loadDashboardData() {
            try {
                const response = await fetch(SHEET_JSON_URL);
                const text = await response.text();
                
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                const jsonString = text.substring(startIdx, endIdx + 1);
                const data = JSON.parse(jsonString);
                const rows = data.table.rows;

                let totalsIndex = rows.findIndex(r => r && r.c && r.c[0] && getVal(r.c[0]).trim().toUpperCase() === 'TOTALES');
                if (totalsIndex === -1) totalsIndex = rows.length - 2;

                let tRow = rows[totalsIndex + 1];
                if (!tRow && totalsIndex !== -1) tRow = rows[totalsIndex];

                if(tRow && tRow.c) {
                    document.getElementById('txt-meta-alumnos').innerText = `Meta Alumnos: ${getVal(tRow.c[2], true)}`;
                    document.getElementById('txt-pagantes-global').innerText = getVal(tRow.c[3], true).toLocaleString('es-PE');
                    document.getElementById('txt-meta-global').innerText = `Meta Total: S/ ${getVal(tRow.c[4], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-recaudado-global').innerText = `S/ ${getVal(tRow.c[7], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-falta-global').innerText = `S/ ${getVal(tRow.c[8], true).toLocaleString('es-PE')}`;
                    
                    let avanceStr = getVal(tRow.c[9]);
                    let avanceGlobalNum = 0;
                    if (avanceStr.includes('%')) {
                        avanceGlobalNum = parseInt(avanceStr);
                    } else {
                        let n = getVal(tRow.c[9], true);
                        avanceGlobalNum = n <= 1.5 ? Math.round(n * 100) : Math.round(n);
                    }
                    
                    document.getElementById('txt-avance-global').innerText = avanceGlobalNum + '%';
                    document.getElementById('bar-avance-global').style.width = avanceGlobalNum + '%';
                    document.getElementById('error-box').className = 'hidden';
                }

                const tutorsData = [];
                for (let i = 0; i < totalsIndex; i++) {
                    const row = rows[i];
                    if (!row || !row.c || !row.c[0]) continue;

                    let tutorName = getVal(row.c[0]).trim();
                    if (tutorName === '' || tutorName.toUpperCase() === 'TUTOR' || tutorName.toUpperCase() === 'TOTALES') continue;

                    let avanceStr = getVal(row.c[9]);
                    let avanceNum = 0;
                    if (avanceStr.includes('%')) {
                        avanceNum = parseInt(avanceStr);
                    } else {
                        let n = getVal(row.c[9], true);
                        avanceNum = n <= 1.5 ? Math.round(n * 100) : Math.round(n);
                    }

                    tutorsData.push({
                        tutor: tutorName,
                        ciclo: getVal(row.c[1]).trim(),
                        metaEst: getVal(row.c[2], true),
                        pagantes: getVal(row.c[3], true),
                        metaDinero: getVal(row.c[4], true),
                        efectivo: getVal(row.c[5], true),
                        yape: getVal(row.c[6], true),
                        recaudado: getVal(row.c[7], true),
                        falta: getVal(row.c[8], true),
                        avance: avanceNum
                    });
                }

                renderTable(tutorsData);
                renderChart(tutorsData);
                
                const rankedData = [...tutorsData].sort((a, b) => b.avance - a.avance);
                renderLeaderboard(rankedData);

            } catch (error) {
                console.error(error);
                document.getElementById('error-box').className = 'max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6 block';
            }
        }

        function renderTable(data) {
            const tbody = document.getElementById('table-body-tutors');
            tbody.innerHTML = '';
            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "table-row-animate border-b border-slate-800/40 text-slate-300";
                tr.innerHTML = `
                    <td class="py-4 px-5 font-semibold text-slate-100 whitespace-nowrap">
                        <div class="flex items-center space-x-2.5">
                            <div class="h-2 w-2 rounded-full ${row.avance >= 100 ? 'bg-emerald-400 shadow-[0_0_8px_rgba(52,211,153,0.6)]' : 'bg-indigo-400'}"></div>
                            <span>${row.tutor}</span>
                        </div>
                    </td>
                    <td class="py-4 px-4 whitespace-nowrap">
                        <span class="bg-slate-800/80 text-slate-300 text-[11px] font-bold px-2.5 py-0.5 rounded-md border border-slate-700/50 tracking-wide">${row.ciclo}</span>
                    </td>
                    <td class="py-4 px-4 text-center font-semibold text-slate-300">${row.metaEst}</td>
                    <td class="py-4 px-4 text-center font-semibold text-slate-300">${row.pagantes}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400 whitespace-nowrap">S/ ${row.metaDinero.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400/80 whitespace-nowrap">S/ ${row.efectivo.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400/80 whitespace-nowrap">S/ ${row.yape.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-5 text-right font-bold text-emerald-400 whitespace-nowrap">S/ ${row.recaudado.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-5 text-center">
                        <span class="px-2.5 py-1 rounded-full text-xs font-bold border ${row.avance >= 100 ? 'bg-emerald-500/10 text-emerald-400 border-emerald-500/30' : 'bg-indigo-500/10 text-indigo-400 border-indigo-500/30'}">${row.avance}%</span>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function renderLeaderboard(data) {
            const container = document.getElementById('leaderboard-container');
            container.innerHTML = '';
            data.forEach((row, index) => {
                const item = document.createElement('div');
                item.className = `flex items-center justify-between p-4 rounded-xl border transition-all duration-200 hover:translate-x-1 ${
                    index === 0 ? 'bg-gradient-to-r from-amber-500/10 to-slate-900/10 border-amber-500/20' : 
                    index === 1 ? 'bg-gradient-to-r from-slate-400/10 to-slate-900/10 border-slate-400/20' : 
                    index === 2 ? 'bg-gradient-to-r from-amber-700/10 to-slate-900/10 border-amber-700/20' : 'bg-slate-800/30 border-slate-800/60'
                }`;
                
                let medal = `<span class="text-xs font-bold text-slate-500 w-6 text-center">${index + 1}</span>`;
                if (index === 0) medal = `<span class="text-lg w-6 text-center">🥇</span>`;
                if (index === 1) medal = `<span class="text-lg w-6 text-center">🥈</span>`;
                if (index === 2) medal = `<span class="text-xl w-6 text-center">🥉</span>`;

                item.innerHTML = `
                    <div class="flex items-center space-x-3 truncate">
                        ${medal}
                        <div class="truncate">
                            <p class="text-sm font-bold text-slate-200 truncate tracking-tight">${row.tutor}</p>
                            <p class="text-[11px] font-medium text-slate-400 truncate">${row.ciclo}</p>
                        </div>
                    </div>
                    <div class="text-right ml-3 flex-shrink-0">
                        <p class="text-sm font-extrabold text-white">${row.avance}%</p>
                        <p class="text-[10px] text-slate-500 font-semibold">S/ ${row.recaudado.toLocaleString('es-PE')}</p>
                    </div>
                `;
                container.appendChild(item);
            });
        }

        function renderChart(data) {
            const ctx = document.getElementById('chartTutors').getContext('2d');
            if (myChart) { myChart.destroy(); }
            
            // Creación de degradados modernos para las barras gráficas
            const gradientMeta = ctx.createLinearGradient(0, 0, ctx.canvas.width, 0);
            gradientMeta.addColorStop(0, 'rgba(51, 65, 85, 0.3)');
            gradientMovie = 'rgba(71, 85, 105, 0.5)';

            const gradientRecaudado = ctx.createLinearGradient(0, 0, ctx.canvas.width, 0);
            gradient2 = 'rgba(99, 102, 241, 0.85)';

            myChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: data.map(r => r.tutor.split(' ')[0] + ' ' + (r.tutor.split(' ')[1] || '')),
                    datasets: [
                        { label: 'Meta Fija (S/.)', data: data.map(r => r.metaDinero), backgroundColor: 'rgba(71, 85, 105, 0.35)', borderRadius: 5, barThickness: 10 },
                        { label: 'Recaudado (S/.)', data: data.map(r => r.recaudado), backgroundColor: 'rgba(99, 102, 241, 0.85)', borderRadius: 4, barThickness: 10 }
                    ]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { labels: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', weight: '500' } } } },
                    scales: {
                        x: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#64748b' } },
                        y: { grid: { display: false }, ticks: { color: '#cbd5e1', font: { weight: '600' } } }
                    }
                }
            });
        }

        loadDashboardData();
        setInterval(loadDashboardData, 60000);
    </script>
</body>
</html> 
