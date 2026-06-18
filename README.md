<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Retail Marketing & Customer Intelligence Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            background-color: #fcfcfc;
            color: #1f2937;
            font-family: ui-sans-serif, system-ui, sans-serif;
        }
        .accent-bg { background-color: #7a0010; } /* Sophisticated Burgundy Red */
        .accent-text { color: #7a0010; }
        .card-shadow { box-shadow: 0 4px 20px -2px rgba(122, 0, 16, 0.06), 0 2px 4px -1px rgba(0, 0, 0, 0.03); }
    </style>
</head>
<body class="p-6 lg:p-10">

    <header class="accent-bg text-white rounded-2xl p-6 mb-8 flex flex-col md:flex-row justify-between items-start md:items-center gap-4 shadow-xl">
        <div>
            <h1 class="text-2xl lg:text-3xl font-black tracking-tight">Retail Marketing & Customer Intelligence</h1>
            <p class="text-red-200 text-sm mt-1">End-to-End Advanced Analytics Dashboard Platform</p>
        </div>
        <div class="bg-white/10 backdrop-blur-md p-2 rounded-xl flex items-center gap-3 border border-white/20">
            <label for="categoryFilter" class="text-sm font-semibold text-white/90 uppercase tracking-wider pl-2">Filter Category:</label>
            <select id="categoryFilter" class="bg-white text-gray-900 font-bold px-4 py-2 rounded-lg outline-none cursor-pointer text-sm shadow" onchange="updateDashboard()">
                <option value="All">All Categories</option>
                <option value="Electronics">Electronics</option>
                <option value="Clothing">Clothing</option>
                <option value="Beauty">Beauty</option>
            </select>
        </div>
    </header>

    <section class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div class="bg-white border border-gray-100 p-6 rounded-2xl card-shadow flex items-center justify-between">
            <div>
                <p class="text-xs font-bold text-gray-400 uppercase tracking-widest">Total Gross Revenue</p>
                <h3 id="kpiRevenue" class="text-3xl font-black text-gray-900 mt-1">$0.00</h3>
            </div>
            <div class="p-3 bg-red-50 rounded-xl accent-text font-black text-xl">$</div>
        </div>
        <div class="bg-white border border-gray-100 p-6 rounded-2xl card-shadow flex items-center justify-between">
            <div>
                <p class="text-xs font-bold text-gray-400 uppercase tracking-widest">Total Volume Orders</p>
                <h3 id="kpiOrders" class="text-3xl font-black text-gray-900 mt-1">0</h3>
            </div>
            <div class="p-3 bg-red-50 rounded-xl accent-text font-black text-xl">📦</div>
        </div>
        <div class="bg-white border border-gray-100 p-6 rounded-2xl card-shadow flex items-center justify-between">
            <div>
                <p class="text-xs font-bold text-gray-400 uppercase tracking-widest">Total Units Sold</p>
                <h3 id="kpiUnits" class="text-3xl font-black text-gray-900 mt-1">0</h3>
            </div>
            <div class="p-3 bg-red-50 rounded-xl accent-text font-black text-xl">📈</div>
        </div>
    </section>

    <main class="grid grid-cols-1 xl:grid-cols-3 gap-8">
        
        <div class="bg-white border border-gray-100 p-6 rounded-2xl card-shadow xl:col-span-1 flex flex-col justify-between">
            <div>
                <h3 class="text-lg font-bold text-gray-800">Advanced Customer Segmentation</h3>
                <p class="text-xs text-gray-400 mt-0.5 mb-4">Distribution via K-Means Clustering Machine Learning</p>
            </div>
            <div class="relative w-full max-w-[280px] mx-auto p-2">
                <canvas id="clusterChart"></canvas>
            </div>
            <div class="mt-4 space-y-2 text-xs border-t border-gray-50 pt-4">
                <div class="flex justify-between"><span>🔴 Cluster 0: New / Low Spend</span><span class="font-bold text-gray-600">Recent</span></div>
                <div class="flex justify-between"><span>🔵 Cluster 1: At-Risk Spenders</span><span class="font-bold text-gray-600">Dormant</span></div>
                <div class="flex justify-between"><span>🟢 Cluster 2: Inactive Low-Value</span><span class="font-bold text-gray-600">Lost</span></div>
                <div class="flex justify-between"><span>🟡 Cluster 3: Champions Group</span><span class="font-bold text-gray-600">High Value</span></div>
            </div>
        </div>

        <div class="bg-white border border-gray-100 p-6 rounded-2xl card-shadow xl:col-span-2">
            <h3 class="text-lg font-bold text-gray-800 mb-1">Core Performance Metrics Matrix</h3>
            <p class="text-xs text-gray-400 mb-5">Granular view of demographic spending traits</p>
            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse text-sm">
                    <thead>
                        <tr class="border-b border-gray-100 text-gray-400 text-xs font-bold uppercase tracking-wider">
                            <th class="pb-3">Demographic Group</th>
                            <th class="pb-3">Transactions</th>
                            <th class="pb-3">Average Age</th>
                            <th class="pb-3">Revenue Contribution</th>
                            <th class="pb-3">Share %</th>
                        </tr>
                    </thead>
                    <tbody id="metricsTableBody" class="divide-y divide-gray-50 font-medium text-gray-700">
                        </tbody>
                </table>
            </div>
        </div>

    </main>

    <footer class="mt-12 text-center text-xs text-gray-400 font-medium">
        Project Built Successfully • June 2026 Target Milestones Reached
    </footer>

    <script>
        // Compiled directly from customer_segments.csv metrics
        const dataEngine = {
            "All": {
                revenue: 456000, orders: 1000, units: 2514,
                clusters: [130, 240, 480, 150], // counts for [C0, C1, C2, C3]
                rows: [
                    { segment: "Female Shoppers", tx: 510, age: 41.6, rev: 232840, share: "51.1%" },
                    { segment: "Male Shoppers", tx: 490, age: 41.1, rev: 223160, share: "48.9%" },
                    { segment: "Core Store Aggregates", tx: 1000, age: 41.4, rev: 456000, share: "100%" }
                ]
            },
            "Electronics": {
                revenue: 156905, orders: 342, units: 849,
                clusters: [42, 81, 168, 51],
                rows: [
                    { segment: "Female Shoppers", tx: 174, age: 41.8, rev: 79850, share: "50.9%" },
                    { segment: "Male Shoppers", tx: 168, age: 40.9, rev: 77055, share: "49.1%" },
                    { segment: "Electronics Aggregates", tx: 342, age: 41.3, rev: 156905, share: "34.4%" }
                ]
            },
            "Clothing": {
                revenue: 155555, orders: 351, units: 893,
                clusters: [46, 83, 171, 51],
                rows: [
                    { segment: "Female Shoppers", tx: 178, age: 41.2, rev: 78920, share: "50.7%" },
                    { segment: "Male Shoppers", tx: 173, age: 41.5, rev: 76635, share: "49.3%" },
                    { segment: "Clothing Aggregates", tx: 351, age: 41.4, rev: 155555, share: "34.1%" }
                ]
            },
            "Beauty": {
                revenue: 143540, orders: 307, units: 772,
                clusters: [42, 76, 141, 48],
                rows: [
                    { segment: "Female Shoppers", tx: 158, age: 41.9, rev: 74070, share: "51.6%" },
                    { segment: "Male Shoppers", tx: 149, age: 41.0, rev: 69470, share: "48.4%" },
                    { segment: "Beauty Aggregates", tx: 307, age: 41.5, rev: 143540, share: "31.5%" }
                ]
            }
        };

        let myChart = null;

        function updateDashboard() {
            const selectedCategory = document.getElementById("categoryFilter").value;
            const currentData = dataEngine[selectedCategory];

            // 1. Update the Big KPI Box Numbers dynamically
            document.getElementById("kpiRevenue").innerText = "$" + currentData.revenue.toLocaleString();
            document.getElementById("kpiOrders").innerText = currentData.orders.toLocaleString();
            document.getElementById("kpiUnits").innerText = currentData.units.toLocaleString();

            // 2. Clear out and populate the metrics datatable matrix rows
            const tbody = document.getElementById("metricsTableBody");
            tbody.innerHTML = "";
            currentData.rows.forEach(item => {
                const tr = document.createElement("tr");
                tr.className = "border-b border-gray-50 last:font-bold last:text-gray-900 last:bg-red-50/40";
                tr.innerHTML = `
                    <td class="py-3.5">${item.segment}</td>
                    <td class="py-3.5">${item.tx}</td>
                    <td class="py-3.5">${item.age} yrs</td>
                    <td class="py-3.5 font-bold text-gray-900">$${item.rev.toLocaleString()}</td>
                    <td class="py-3.5"><span class="px-2 py-1 bg-gray-100 text-gray-600 rounded text-xs font-semibold">${item.share}</span></td>
                `;
                tbody.appendChild(tr);
            });

            // 3. Render or animate the responsive Segmentation Donut chart
            const ctx = document.getElementById('clusterChart').getContext('2d');
            
            if (myChart) {
                myChart.destroy(); // Destroy previous instance to avoid visual overlapping glitches
            }

            myChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Cluster 0', 'Cluster 1', 'Cluster 2', 'Cluster 3'],
                    datasets: [{
                        data: currentData.clusters,
                        backgroundColor: ['#f87171', '#3b82f6', '#4ade80', '#fbbf24'],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    cutout: '70%'
                }
            });
        }

        // Initialize dashboard instantly on application startup window loading
        window.onload = updateDashboard;
    </script>
</body>
</html>

# Retail Marketing & Customer Intelligence Analytics Platform

An end-to-end data analytics and machine learning project focused on transaction optimization and customer segmentation using a transactional retail dataset.

## 🛠️ Tech Stack & Workshop Tools
- **Language:** Python 3.9+
- **Libraries:** Pandas, NumPy, Scikit-Learn (K-Means Clustering), Matplotlib, Seaborn
- **Visualization Front-End:** Interactive HTML5, Tailwind CSS, Chart.js Engine

## 📈 Key Business Insights Discovered
- **Balanced Product Portfolio:** Revenue contribution is uniformly distributed across Electronics (~34.4%), Clothing (~34.1%), and Beauty (~31.5%).
- **Demographic Equity:** Female consumers account for 51.1% of transactions, while male consumers drive 48.9%, displaying symmetrical brand engagement.
- **Seasonality Peak:** Transaction volumes hit an absolute performance maximum during October 2023.

## 📁 Project Directory Structure
- `/data/raw/` - Original untouched transactional data.
- `/data/processed/` - Cleaned datasets and machine learning output segments.
- `/notebooks/` - Jupyter notebooks tracking step-by-step cleaning, EDA charts, and clustering.
- `/dashboards/` - Interactive HTML/JS analytics dashboard.

## Author - Jitender Tehilyai.
