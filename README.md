<h1>🏗️ HR Attrition &amp; Headcount Data Engineering Pipeline</h1>
<h3>Automated ETL using Airflow, Docker, GCP (BigQuery + GCS), Python &amp; Terraform</h3>

<p>
This project automates the <b>ingestion, transformation, storage, and visualization</b> of HR Headcount (HC) and Attrition data for a large IT organization.  
The goal is to ensure that management always has an <b>up-to-date analytics dashboard</b> with insights on employee attrition, retention, and workforce distribution.
</p>

<p>
The pipeline orchestrates data extraction, ETL, cloud storage, BigQuery loading, and dashboard refreshes using <b>Apache Airflow</b> running on <b>Docker</b>.  
A complete HR analytics dashboard is built in <b>Looker Studio</b> on top of <b>BigQuery</b>.
</p>

<br>

<h2>📁 Repository Contents</h2>

<table>
  <tr><th>File</th><th>Description</th></tr>
  <tr><td><b>docker-compose.yaml</b></td><td>Airflow environment running on Docker (scheduler, webserver, workers, PostgreSQL, etc.).</td></tr>
  <tr><td><b>main.tf</b></td><td>Terraform script used to create the Google Cloud Storage (GCS) bucket and related resources.</td></tr>
  <tr><td><b>DE shanistha (1).pdf</b></td><td>Full project documentation with architecture diagrams, screenshots, and detailed explanation.</td></tr>
  <tr><td><b>README.md</b></td><td>Project documentation (this file).</td></tr>
</table>

<br>

<h2>📌 Project Overview</h2>

<p>A large IT company is experiencing rising employee attrition at a specific location. Management needs an automated, reliable dashboard to monitor:</p>

<ul>
  <li>👥 Headcount (HC)</li>
  <li>📉 Attrition trends over time</li>
  <li>❓ Reasons for attrition</li>
  <li>🏢 Department-level breakdowns</li>
  <li>📊 Distribution of workforce across segments</li>
</ul>

<p>
HR regularly provides fresh CSV data, and this project ensures that the underlying data warehouse and dashboard update <b>automatically</b>, with <b>no manual data processing</b>.
</p>

<br>

<h2>🔥 Tools &amp; Technologies</h2>

<ul>
  <li>🌀 <b>Apache Airflow</b> (Dockerized) – workflow orchestration</li>
  <li>🐍 <b>Python</b> – ETL processing &amp; data transformation</li>
  <li>☁️ <b>Google Cloud Storage (GCS)</b> – raw &amp; processed data storage</li>
  <li>🗄️ <b>Google BigQuery</b> – data warehouse for analytics</li>
  <li>📊 <b>Looker Studio</b> – interactive HR dashboards</li>
  <li>🧱 <b>Terraform</b> – infrastructure as code (GCS bucket, configs)</li>
  <li>🐳 <b>Docker</b> – containerized Airflow deployment</li>
</ul>

<br>

<h2>🏗️ Architecture Overview</h2>

<p>The solution is built as an automated ETL pipeline with the following high-level stages:</p>

<ol>
  <li>📥 <b>Data Ingestion</b> – Fetch HR CSV files from source server</li>
  <li>🧮 <b>ETL Processing</b> – Clean &amp; transform data with Python</li>
  <li>☁️ <b>Cloud Storage</b> – Store processed files in GCS</li>
  <li>🗄️ <b>Data Warehouse</b> – Load data into BigQuery tables</li>
  <li>📊 <b>Reporting Layer</b> – Visualize metrics in Looker Studio</li>
</ol>

<br>

<h2>🛠️ End-to-End Workflow (Airflow DAG)</h2>

<p>The Airflow DAG consists of the following tasks:</p>

<h3>▶️ Task 1 — Download CSV from Server (BashOperator)</h3>
<ul>
  <li>Fetch the latest HR CSV file from an internal file server or shared location.</li>
  <li>Rename the file dynamically using the execution date (e.g., <code>20240201.csv</code>).</li>
</ul>

<h3>▶️ Task 2 — ETL Processing (PythonOperator)</h3>
<ul>
  <li>Convert categorical → numerical fields where needed (e.g., Yes/No, attrition flags).</li>
  <li>Remove unnecessary columns.</li>
  <li>Standardize column names and formats.</li>
  <li>Create a cleaned file named <code>employee_hr_data_YYYYMMDD.csv</code>.</li>
</ul>

<h3>▶️ Task 3 — Upload to GCS (PythonOperator)</h3>
<ul>
  <li>Upload the cleaned CSV to a designated <b>GCS bucket</b>.</li>
  <li>Use folder structure or naming conventions for partitioning and history tracking.</li>
</ul>

<h3>▶️ Task 4 — Create BigQuery Table If Not Exists</h3>
<ul>
  <li>Check if the BigQuery destination table exists.</li>
  <li>If not, create it using the schema inferred or predefined in the DAG.</li>
</ul>

<h3>▶️ Task 5 — Load Data from GCS → BigQuery</h3>
<ul>
  <li>Load the latest processed CSV file from GCS into BigQuery.</li>
  <li>Append data to historical records.</li>
  <li>Ensure the dashboard always reflects the latest HR information.</li>
</ul>

<p>The Airflow UI shows all tasks executing successfully in sequence, validating the pipeline orchestration.</p>

<br>

<h2>☁️ Google Cloud Setup with Terraform</h2>

<p><b>Terraform</b> is used to provision the necessary GCP infrastructure, primarily the GCS bucket.</p>

<p>The <code>main.tf</code> file includes:</p>

<ul>
  <li>Provider configuration for Google Cloud</li>
  <li>Variables for:
    <ul>
      <li><code>bucket_name</code></li>
      <li><code>bucket_location</code></li>
      <li><code>project_id</code></li>
      <li><code>storage_class</code></li>
    </ul>
  </li>
  <li>A <code>google_storage_bucket</code> resource such as:</li>
</ul>

<pre><code>resource "google_storage_bucket" "hr_bucket" {
  name          = var.bucket_name
  location      = var.bucket_location
  storage_class = var.storage_class
}
</code></pre>

<p>
Once applied, Terraform creates the storage bucket used by Airflow to store processed HR CSVs.
</p>

<br>

<h2>🗄️ BigQuery Integration</h2>

<ul>
  <li>Airflow loads processed data directly from GCS into a BigQuery table.</li>
  <li>The table is created automatically on the first run if it does not already exist.</li>
  <li>Subsequent DAG runs append new HR records, maintaining a historical dataset.</li>
</ul>

<p>
This setup enables fast analytical queries and supports multiple dashboards &amp; reports.
</p>

<br>

<h2>📊 Looker Studio Dashboard</h2>

<p>A complete HR analytics dashboard is built in <b>Looker Studio</b> on top of BigQuery.</p>

<h3>Dashboards Include:</h3>

<ul>
  <li>👥 <b>Active Headcount Overview</b> – department, gender, age bands, tenure</li>
  <li>📉 <b>Attrition Overview</b> – overall attrition rate, monthly trends</li>
  <li>🔍 <b>Attrition Deep Dives</b> – performance rating vs attrition, job role, satisfaction metrics</li>
  <li>🌍 <b>Location &amp; department segmentation</b></li>
</ul>

<p>
Charts used include pie charts, bar charts, and trend lines to help stakeholders quickly interpret HR metrics.
</p>

<br>

<h2>🎯 Results &amp; Business Impact</h2>

<ul>
  <li>✅ <b>Fully automated ETL pipeline</b><br>
      No manual steps. HR team simply uploads or refreshes the CSV → Dashboard updates automatically.</li>
  <li>✅ <b>Cloud-native &amp; scalable</b><br>
      GCS + BigQuery provide reliable, scalable storage and fast analytics.</li>
  <li>✅ <b>Improved HR decision-making</b><br>
      Management can monitor:
      <ul>
        <li>Attrition by department</li>
        <li>Reasons for leaving</li>
        <li>High-risk employee groups</li>
        <li>Headcount distribution and trends</li>
      </ul>
  </li>
  <li>✅ <b>Production-ready design</b><br>
      Airflow ensures workflows are repeatable, testable, and maintainable.</li>
</ul>

<br>

<h2>🚀 Future Enhancements</h2>

<ul>
  <li>📧 Add email or Slack alerts when attrition crosses thresholds</li>
  <li>☸ Deploy Airflow on Kubernetes for higher scalability &amp; resilience</li>
  <li>📐 Integrate <b>dbt</b> for modular data modeling in BigQuery</li>
  <li>🧬 Add Slowly Changing Dimensions (SCD) to track employee lifecycle changes</li>
  <li>🤖 Integrate predictive modeling to forecast attrition risk</li>
</ul>

<br>

<h2>⭐ Summary</h2>

<p>
This project demonstrates a complete <b>Data Engineering pipeline</b> combining:
</p>

<ul>
  <li>✔ Airflow orchestration</li>
  <li>✔ Dockerized infrastructure</li>
  <li>✔ Cloud storage &amp; data warehousing on GCP</li>
  <li>✔ Automated ETL for HR data</li>
  <li>✔ Real-time dashboards for HR analytics</li>
</ul>

<p>
This project is a strong example of my <b>production-grade data engineering</b> for HR analytics and business intelligence.
</p>
