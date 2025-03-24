<h1>Hospital System Simulation</h1>
<p><em>(Collaboratively developed with a classmate)</em></p>

<h2>PHASE 1</h2>
<h2>1. Brief Overview</h2>
<p>This document outlines a detailed hospital system model comprising seven interconnected departments. The system manages two patient categories (elective and urgent) with distinct arrival patterns, prioritization rules, and treatment pathways. Key challenges include resource allocation, patient routing under uncertainty, and handling critical scenarios like power outages.</p>

<h2>2. Hospital Departments & Bed Capacity</h2>
<table>
  <thead>
    <tr>
      <th>Department</th>
      <th>Number of Beds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pre-Operative Unit</td>
      <td>25</td>
    </tr>
    <tr>
      <td>Emergency Room (ER)</td>
      <td>10</td>
    </tr>
    <tr>
      <td>Laboratory</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Operating Rooms (ORs)</td>
      <td>50</td>
    </tr>
    <tr>
      <td>General Ward</td>
      <td>40</td>
    </tr>
    <tr>
      <td>Intensive Care Unit (ICU)</td>
      <td>10</td>
    </tr>
    <tr>
      <td>Cardiac Care Unit (CCU)</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

<h2>3. Patient Classification & Arrival Process</h2>
<h3>3.1 Patient Types</h3>
<ul>
  <li><strong>Elective Patients (75%):</strong> Scheduled in advance, admitted to the Pre-Operative Unit 2 days before surgery.</li>
  <li><strong>Urgent Patients (25%):</strong> Require immediate care, prioritized for surgery. Admitted via the ER.</li>
</ul>

<h3>3.2 Arrival Rates</h3>
<ul>
  <li><strong>Elective Patients:</strong> Poisson arrival rate of 1 patient/hour.</li>
  <li><strong>Urgent Patients:</strong> Poisson arrival rate of 4 patients/hour.</li>
  <li><strong>Group Arrivals:</strong> 0.5% of urgent patients arrive in groups of 2–5 (uniformly distributed) only if ER has capacity.</li>
  <li><strong>ER Queue Limit:</strong> Up to 10 patients can wait in ambulances for ER beds.</li>
</ul>

<h2>4. Testing Process</h2>
<h3>Administrative Wait Time:</h3>
<ul>
  <li><strong>Elective (Pre-Operative):</strong> 60 minutes before lab transfer.</li>
  <li><strong>Urgent (ER):</strong> 10 minutes before lab transfer.</li>
</ul>

<h3>Laboratory Testing:</h3>
<ul>
  <li><strong>Tests:</strong> All patients undergo tests (blood/sugar).</li>
  <li><strong>Testing Time:</strong> Uniform distribution (28–32 minutes).</li>
  <li><strong>Priority:</strong> Urgent patients take precedence over elective in the lab.</li>
</ul>

<h3>Post-Testing Routing:</h3>
<ul>
  <li><strong>Elective:</strong> Return to Pre-Operative Unit for 2 days before OR transfer.</li>
  <li><strong>Urgent:</strong> Stay in ER for a triangular-distributed time (min=5 min, mode=75 min, max=100 min) before OR transfer.</li>
</ul>

<h2>5. Surgery Process</h2>
<h3>5.1 OR Setup</h3>
<ul>
  <li><strong>Setup Time:</strong> 10 minutes after each surgery.</li>
  <li><strong>Prioritization:</strong> Urgent patients bypass elective patients for OR access.</li>
</ul>

<h3>5.2 Surgery Types & Distribution</h3>
<table>
  <thead>
    <tr>
      <th>Surgery Type</th>
      <th>Probability</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Simple (e.g., appendectomy)</td>
      <td>50%</td>
    </tr>
    <tr>
      <td>Moderate (e.g., cholecystectomy)</td>
      <td>45%</td>
    </tr>
    <tr>
      <td>Complex (e.g., open-heart surgery)</td>
      <td>5%</td>
    </tr>
  </tbody>
</table>
<p><strong>Surgery Duration:</strong> Follows type-specific distributions (historical data referenced but parameters unspecified).</p>

<h2>6. Post-Surgery Routing</h2>
<h3>6.1 Post-Operative Transfers</h3>
<ul>
  <li><strong>Simple Surgery:</strong> Directly to General Ward.</li>
  <li><strong>Moderate Surgery:</strong>
    <ul>
      <li>General Ward (70%), ICU (10%), CCU (20%).</li>
    </ul>
  </li>
  <li><strong>Complex Surgery:</strong>
    <ul>
      <li>75% to ICU (non-cardiac) or 25% to CCU (cardiac).</li>
    </ul>
  </li>
  <li><strong>Complications:</strong> 1% of ICU/CCU patients deteriorate and require urgent reoperation.</li>
  <li><strong>Mortality:</strong> 10% die during surgery; sent to morgue (outside the system).</li>
</ul>

<h3>6.2 Stay Duration</h3>
<ul>
  <li><strong>General Ward:</strong> Exponential distribution (mean = 50 hours).</li>
  <li><strong>ICU/CCU:</strong> Exponential distribution (mean = 25 hours) before transfer to General Ward.</li>
</ul>

<h2>7. Additional Considerations</h2>
<ul>
  <li><strong>Power Outages:</strong>
    <ul>
      <li>Occurs once/month at random.</li>
      <li><strong>Generators:</strong> Support all ORs but only 80% of ICU/CCU beds.</li>
    </ul>
  </li>
  <li><strong>Patient Deterioration:</strong> Minimal cases of ward-to-ICU/CCU transfers (neglected in the model).</li>
  <li><strong>Discharge:</strong> Patients leave the hospital after completing their stay in the General Ward.</li>
</ul>

<p>This model integrates probabilistic events, prioritization rules, and resource constraints to simulate real-world hospital operations. Critical parameters (e.g., surgery time distributions) can be calibrated using historical data for accuracy.</p>


<h2>PHASE 2 & PHASE 3</h2>

<h2>Project Overview</h2>
<p>This project focuses on modeling and optimizing surgical patient flow in a hospital system. In Phase 1, a conceptual model of the surgical workflow was designed. Phase 2 extends this by:</p>
<ul>
  <li>Identifying the service time distribution from historical patient data</li>
  <li>Implementing a Python simulation to compute critical performance metrics required by hospital management, along with statistical confidence intervals</li>
</ul>


<h3>PHASE 2: Service Distribution Identification</h3>
<p><strong>Objective:</strong> Determine the statistical distribution (e.g., exponential, normal, log-normal) of patient service times across hospital departments (pre-op, lab, OR, ICU).</p>
<h4>Approach:</h4>
<ul>
  <li>Use Kernel Density Estimation (KDE) or Goodness-of-Fit tests (e.g., Kolmogorov-Smirnov) and chi square test to identify the best-fit distribution</li>
  <li>Apply sampling methods to generate synthetic data:
    <ul>
      <li>Inverse Transform Method for closed-form distributions</li>
      <li>Acceptance-Rejection Sampling for empirical/complex distributions</li>
    </ul>
  </li>
  <li>Validate results with visualizations (histograms, Q-Q plots)</li>
</ul>

<h3>PHASE 3: Python Simulation & Performance Metrics</h3>
<p>The Python program will calculate the following metrics with 95% confidence intervals (α = 0.05):</p>

<table>
  <thead>
    <tr>
      <th>Metric</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1. Mean Time in System</td>
      <td>Average duration a patient spends in the hospital (from arrival to discharge)</td>
    </tr>
    <tr>
      <td>2. ER Queue Saturation Probability</td>
      <td>Likelihood that the emergency queue operates at full capacity</td>
    </tr>
    <tr>
      <td>3. Queue-Specific Statistics</td>
      <td>For pre-op, lab, OR, and ICU queues:<br>
        - Max/Average Queue Length<br>
        - Mean Waiting Time</td>
    </tr>
    <tr>
      <td>4. Reoperation Rate</td>
      <td>Average number of repeat surgeries for patients with complex procedures</td>
    </tr>
    <tr>
      <td>5. Bed Productivity</td>
      <td>Utilization rate of beds in each department (occupied vs. total beds)</td>
    </tr>
    <tr>
      <td>6. Average Number of Patient Deaths</td>
      <td>Average number of dead patients during surgery</td>
    </tr>
  </tbody>
</table>

<h2>Key Features & Outputs</h2>
<h3>1. Simulation Engine</h3>
<ul>
  <li><strong>Event-Driven Design:</strong> Prioritized event handling (e.g., surgery start, lab test completion)</li>
  <li><strong>Random Sampling:</strong>
    <ul>
      <li>Uniform random numbers generated via numpy or random libraries</li>
      <li>Non-uniform distributions use transform methods (e.g., inverse CDF for exponential)</li>
    </ul>
  </li>
</ul>

<h2>PHASE 4</h2>
<h2>Objective</h2>
<p>Redesign surgical workflows to stabilize hospital operations using simulation principles.</p>

<h2>Project Tasks</h2>

<h3>Task 1: System Design</h3>
<p>Develop two operationally stable systems by applying the following policies:</p>
<ul>
    <li><strong>Capacity Upgrades:</strong> Increase bed and equipment capacity in critical departments (e.g., Operating Rooms, Pre-Operative units).</li>
    <li><strong>Pre-Operative Stay Reduction:</strong> Optimize workflows to reduce pre-surgical hospitalization time for elective patients.</li>
    <li><strong>Surgery Time Reduction:</strong> Achieve at least a 10% reduction in moderate and complex surgery durations through OR equipment upgrades.</li>
</ul>
<p><strong>Constraints:</strong> Both systems must ensure operational stability across all departments, maintaining finite queue lengths and steady resource utilization.</p>

<h3>Task 2: Steady-State Analysis</h3>
<p>Perform the following analyses for both systems:</p>
<ul>
    <li><strong>Cold Analysis:</strong> Theoretical or closed-form evaluation.</li>
    <li><strong>Warm Analysis:</strong> Simulation under stochastic variability.</li>
</ul>
<p><strong>Long-Term Metrics:</strong></p>
<ul>
    <li>Mean queue length (<em>Lq</em>).</li>
    <li>Mean waiting time (<em>Wq</em>).</li>
</ul>

<h3>Task 3: Independent Sampling Comparison</h3>
<p>Compare the two systems using independent sampling methods.</p>
<p><strong>Metrics of Interest:</strong></p>
<ul>
    <li><em>Lq</em> (mean queue length).</li>
    <li><em>Wq</em> (mean waiting time).</li>
</ul>
<p><strong>Statistical Framework:</strong></p>
<ul>
    <li>Significance level: <strong>α = 0.05</strong>.</li>
    <li>95% confidence intervals.</li>
    <li>Hypothesis testing: Two-sample t-test or nonparametric alternatives (e.g., Mann-Whitney).</li>
</ul>
<p>Identify the system with statistically superior performance for elective patients.</p>

<h3>Task 4: Secondary Metric Evaluation</h3>
<p>Define an additional performance metric (e.g., resource utilization, reoperation rate) and repeat Task 3’s comparison for this metric.</p>

<h3>Task 5: Management Recommendation</h3>
<p>Based on the results of Tasks 3 and 4, provide a data-driven recommendation to hospital management. Include:</p>
<ul>
    <li>Cost-benefit analysis of proposed changes.</li>
    <li>Risks and scalability considerations.</li>
</ul>

<h2>Simulation Methods and Guidelines</h2>
<ul>
    <li>Utilize <strong>discrete-event simulation</strong> principles for cold and warm analyses.</li>
    <li>Apply <strong>Monte Carlo sampling</strong> or random variate generation for stochastic components.</li>
    <li>Validate results using steady-state convergence criteria (e.g., batch means method).</li>
</ul>
<p>This project framework integrates core simulation principles to address real-world operational challenges effectively.
