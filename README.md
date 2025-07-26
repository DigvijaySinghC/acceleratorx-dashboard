<!-- AcceleratorX Growth Planning Dashboard | Executive Sales Tracker for Aug–Oct 2025 -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AcceleratorX | Executive Growth Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f4f6fa;
      margin: 0;
    }
    nav {
      background: #1f2937;
      color: white;
      display: flex;
      padding: 1rem;
      justify-content: space-around;
    }
    nav a {
      color: white;
      font-weight: bold;
      text-decoration: none;
    }
    .page {
      display: none;
      padding: 2rem;
    }
    .active {
      display: block;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1.5rem;
    }
    .card {
      background: #fff;
      border-radius: 12px;
      padding: 1.5rem;
      box-shadow: 0 0 12px rgba(0,0,0,0.05);
      text-align: center;
    }
    input[type="number"] {
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.5rem;
      font-size: 1rem;
    }
    h2, h3 {
      color: #1f2937;
    }
    button {
      background: #1f2937;
      color: white;
      padding: 0.75rem 1.5rem;
      margin-top: 1rem;
      font-weight: bold;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    canvas {
      background: #fff;
      padding: 1rem;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
      margin-top: 2rem;
    }
    .chart-section {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 2rem;
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <nav>
    <a href="#" onclick="showPage('dashboard')">Dashboard</a>
    <a href="#" onclick="showPage('competitors')">Competitor Analysis</a>
  </nav>

  <div id="dashboard" class="page active">
    <h2>Sales Planning: August - October 2025</h2>
    <div class="grid">
      <div class="card">
        <h3>Target Revenue (₹)</h3>
        <input type="number" id="targetRevenue" placeholder="e.g., 2500000">
      </div>
      <div class="card">
        <h3>Total Application Leads</h3>
        <input type="number" id="appLeads" placeholder="e.g., 2000">
      </div>
      <div class="card">
        <h3>Total Webinar Leads</h3>
        <input type="number" id="webLeads" placeholder="e.g., 3000">
      </div>
      <div class="card">
        <h3>Application Lead Conversion Rate (%)</h3>
        <input type="number" id="appConvRate" placeholder="e.g., 4" step="0.1">
      </div>
      <div class="card">
        <h3>Webinar Lead Conversion Rate (%)</h3>
        <input type="number" id="webConvRate" placeholder="e.g., 1.2" step="0.1">
      </div>
      <div class="card">
        <h3>Experienced Agents</h3>
        <input type="number" id="expAgents" placeholder="e.g., 2">
      </div>
      <div class="card">
        <h3>Interns</h3>
        <input type="number" id="interns" placeholder="e.g., 4">
      </div>
      <div class="card">
        <h3>Custom Cost per Lead (₹)</h3>
        <input type="number" id="customCPL" placeholder="Optional: Enter your CPL">
      </div>
    </div>
    <button onclick="runProjection()">Run Projection</button>
    <div class="grid">
      <div class="card"><h3>Total Revenue Achieved</h3><p id="achieved">-</p></div>
      <div class="card"><h3>App Conversions</h3><p id="appConverted">-</p></div>
      <div class="card"><h3>Webinar Conversions</h3><p id="webConverted">-</p></div>
      <div class="card"><h3>Total Conversions</h3><p id="totalConversions">-</p></div>
      <div class="card"><h3>CPL (₹)</h3><p id="cpl">-</p></div>
      <div class="card"><h3>CAC (₹)</h3><p id="cac">-</p></div>
      <div class="card"><h3>ROI (%)</h3><p id="roi">-</p></div>
      <div class="card"><h3>Status</h3><p id="status">-</p></div>
    </div>
    <div class="chart-section">
      <canvas id="trendChart"></canvas>
      <canvas id="conversionChart"></canvas>
    </div>
  </div>

  <div id="competitors" class="page">
    <h2>Competitor Analysis – EdTech Market</h2>
    <div class="grid">
      <div class="card">
        <h3>BYJU’S</h3>
        <p>Approx 3000+ sales agents, large-scale webinars, app-based leads, ROAS ≈ 3x<br>Focus: School + Competitive</p>
      </div>
      <div class="card">
        <h3>Scaler Academy</h3>
        <p>300–500 agents, high-performance outbound + LinkedIn push, ROAS ≈ 5x<br>Focus: Software upskilling</p>
      </div>
      <div class="card">
        <h3>UpGrad</h3>
        <p>500–800 agents, strong remarketing & application-led growth, webinars avg. 5000+ registrations</p>
      </div>
      <div class="card">
        <h3>GrowthSchool</h3>
        <p>50–100 agents, influencer-led lead generation, high LTV webinars (10k+ viewers)</p>
      </div>
    </div>
    <canvas id="competitorChart" width="600" height="300"></canvas>
  </div>

  <script>
    function showPage(id) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function runProjection() {
      const rev = +document.getElementById('targetRevenue').value;
      const app = +document.getElementById('appLeads').value;
      const web = +document.getElementById('webLeads').value;
      const appRate = +document.getElementById('appConvRate').value || 4;
      const webRate = +document.getElementById('webConvRate').value || 1.2;
      const customCPL = +document.getElementById('customCPL').value || null;

      const appConv = Math.floor(app * (appRate / 100));
      const webConv = Math.floor(web * (webRate / 100));
      const totalConv = appConv + webConv;
      const achieved = totalConv * 25000;
      const totalLeads = app + web;
      const cpl = customCPL || (totalLeads > 0 ? Math.round(rev / totalLeads) : 0);
      const cac = totalConv > 0 ? Math.round(rev / totalConv) : 0;
      const roi = rev > 0 ? Math.round(((achieved - rev) / rev) * 100) + '%' : '0%';

      document.getElementById('achieved').textContent = '₹' + achieved.toLocaleString();
      document.getElementById('appConverted').textContent = appConv;
      document.getElementById('webConverted').textContent = webConv;
      document.getElementById('totalConversions').textContent = totalConv;
      document.getElementById('cpl').textContent = '₹' + cpl;
      document.getElementById('cac').textContent = '₹' + cac;
      document.getElementById('roi').textContent = roi;
      document.getElementById('status').textContent = achieved >= rev ? '✅ Target Met' : '❌ Below Target';

      new Chart(document.getElementById('trendChart'), {
        type: 'bar',
        data: {
          labels: ['Target Revenue', 'Achieved Revenue'],
          datasets: [{
            label: 'Revenue (₹)',
            data: [rev, achieved],
            backgroundColor: ['#1f77b4', '#2ca02c']
          }]
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { beginAtZero: true } }
        }
      });

      new Chart(document.getElementById('conversionChart'), {
        type: 'doughnut',
        data: {
          labels: ['App Conversions', 'Webinar Conversions'],
          datasets: [{
            label: 'Conversions',
            data: [appConv, webConv],
            backgroundColor: ['#007acc', '#ff9900']
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { position: 'bottom' }
          }
        }
      });
    }

    new Chart(document.getElementById('competitorChart'), {
      type: 'pie',
      data: {
        labels: ['BYJU’S', 'Scaler', 'UpGrad', 'GrowthSchool'],
        datasets: [{
          data: [35, 25, 25, 15],
          backgroundColor: ['#2d89ef', '#f39c12', '#27ae60', '#8e44ad']
        }]
      },
      options: {
        plugins: {
          legend: { position: 'right' }
        }
      }
    });
  </script>
</body>
</html>
