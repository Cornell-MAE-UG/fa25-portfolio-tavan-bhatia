---
layout: project
title: "Shocked: The Art of Suspense and the Science of Measuring It"
description: "MAE 3260 Final Group Work — Off-road Suspension System"
technologies: [MATLAB, Systems Modeling]
image: /assets/images/systems/figure-1-raw-dyno.png
---

<!-- Optional: MathJax for equations (remove if your site already includes MathJax) -->
<script>
  window.MathJax = {
    tex: { inlineMath: [['\\(', '\\)']], displayMath: [['\\[', '\\]']] }
  };
</script>
<script defer src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<section>
  <h2>Report</h2>
  <p><strong>Topic of Interest:</strong> Off-road Suspension System (Baja SAE)</p>
</section>

<section>
  <h2>Abstract</h2>
  <p>
    We are choosing to analyze the shock from a small off-road vehicle’s suspension system (Baja SAE).
    Using a quarter-car suspension model, our objective is to determine the key parameters of the system,
    including the effective damping coefficient, sprung and unsprung masses, and spring constant from prior
    shock dynamometer data. With these parameters, we will construct a mathematical model of the suspension,
    derive the system’s transfer function, and generate Bode plots to evaluate its frequency response
    characteristics. We will then compare this model to the systems we’ve seen in coursework that analyze
    passenger vehicle suspensions to see how our assumptions might change given this specific application
    and the smaller size of the vehicle whose suspension we are studying.
  </p>
</section>

<section>
  <h2>Students and Roles</h2>
  <table>
    <thead>
      <tr>
        <th>Student</th>
        <th>Task / Role</th>
        <th>Portfolio</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Greg Svensson</td>
        <td>
          Mathematical modelling of the system; passive design analysis; system modeling; analytical data comparison.
          Final result: simplified shock model and comprehensive comparison.
        </td>
        <td>
          <a href="https://cornell-mae-ug.github.io/spring-2025-portfolio-gregsvensson/projects/2022-trig-analysis/">
            cornell-mae-ug.github.io/spring-2025-portfolio-gregsvensson/…
          </a>
        </td>
      </tr>
      <tr>
        <td>Ava Rosenow</td>
        <td>Shock dissection and dynamometer data analysis.</td>
        <td></td>
      </tr>
      <tr>
        <td>Aniket Martins</td>
        <td>
          Build/analyze Bode plots for the shock system; contribute to shock teardown and overall group work.
        </td>
        <td>
          <a href="https://am2762.github.io/portfolio-fa25-martinsa/projects/2025-car-suspension/">
            am2762.github.io/portfolio-fa25-martinsa/…
          </a>
        </td>
      </tr>
      <tr>
        <td>Tavan Bhatia</td>
        <td>Help write equations of motion for the shock absorber system (1 DOF) and assist with data analysis.</td>
        <td></td>
      </tr>
    </tbody>
  </table>
</section>

<section>
  <h2>MAE 3260 Concepts and Skills Used</h2>
  <ul>
    <li>
      <strong>Models</strong>
      <ul>
        <li>ODEs</li>
        <li>Transfer Functions</li>
        <li>Bode Plots</li>
      </ul>
    </li>
    <li>
      <strong>Open-loop system</strong>
      <ul>
        <li>Parameter estimation</li>
        <li>Passive design</li>
      </ul>
    </li>
  </ul>
</section>

<section>
  <h2>Data Analysis</h2>

  <p>
    Figure 1 shows the raw dynamometer data for our shock. A force of increasing magnitude was applied to
    the full shock system and velocity was measured. The slope of this graph gives the damping coefficient
    of the shock in \(\mathrm{lb\cdot s/in}\).
  </p>

  <figure>
    <img
      src="{{ '/assets/images/systems/figure-1-raw-dyno.png' | relative_url }}"
      alt="Figure 1: Raw dynamometer data for 5 different tunes, F vs. avg. velocity"
      class="inline-image"
    />
    <figcaption>Figure 1: Raw dynamometer data for 5 different tunes, F vs. avg. velocity</figcaption>
  </figure>

  <p>
    Figure 2 shows a simplified version of our absolute force versus velocity, done by averaging all data
    points and linearizing the data to get a more confident estimate of the damping coefficient.
    From this graph, we get:
    \( b = 19.2\,\mathrm{lb\cdot s/in} = 3360\,\mathrm{N\cdot s/m} \).
  </p>

  <figure>
    <img
      src="{{ '/assets/images/systems/figure-2-linearized.png' | relative_url }}"
      alt="Figure 2: Linearized Graph"
      class="inline-image"
    />
    <figcaption>Figure 2: Linearized Graph</figcaption>
  </figure>
</section>

<section>
  <h2>Spring Constant and Quarter-Car Mass</h2>

  <p>
    The spring constant was experimentally determined from the shock spring using the setup shown in Figure 3.
    The spring was compressed and displacement was measured with a ruler while the force was measured using the scale.
    \(k_1 = 184.15\,\mathrm{lb/in}\).
  </p>

  <figure>
    <img
      src="{{ '/assets/images/systems/figure-3-spring-setup.png' | relative_url }}"
      alt="Figure 3: Experimental setup used to determine spring constants"
      class="inline-image"
    />
    <figcaption>Figure 3: Experimental setup used to determine spring constants</figcaption>
  </figure>

  <p>
    Vehicle mass is known to be 370 lbs. We divide this by 4 to obtain the relative mass for a quarter-car model:
    \(m = 92.5\,\mathrm{lb} = 41.957\,\mathrm{kg}\). This value was confirmed using corner scales to ensure the
    weight distribution wasn’t significantly different front-to-rear or left-to-right.
  </p>

  <p>Spring-rate calculations used in the report:</p>
  <div class="math-block">
    \[
      k_1 = \frac{29\,\mathrm{lb}}{0.4\,\mathrm{cm}} = 32345\,\mathrm{N/m} \\
      k_2 = \frac{20\,\mathrm{lb}}{1\,\mathrm{cm}} = 8879\,\mathrm{N/m} \\
      \frac{1}{k_{eq}} = \frac{1}{k_1} + \frac{1}{k_2} = \frac{1}{32345} + \frac{1}{8879} \\
      k_{eq} = 6975\,\mathrm{N/m}
    \]
  </div>
</section>

<section>
  <h2>Mathematical Model</h2>

  <p>Equation of motion:</p>
  <div class="math-block">
    \[
      m\ddot{x} + b\dot{x} + kx = b\dot{r} + kr
    \]
  </div>

  <p>Laplace transform:</p>
  <div class="math-block">
    \[
      (ms^2 + bs + k)X(s) = (bs + k)R(s)
    \]
  </div>

  <p>Transfer function:</p>
  <div class="math-block">
    \[
      G(s) = \frac{X(s)}{R(s)} = \frac{bs + k}{ms^2 + bs + k}
    \]
  </div>
</section>

<section>
  <h2>Comparison</h2>
  <table>
    <thead>
      <tr>
        <th>System</th>
        <th>m (kg)</th>
        <th>b (N·s/m)</th>
        <th>k (N/m)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Baja SAE Suspension</td>
        <td>41.95</td>
        <td>3360</td>
        <td>6897</td>
      </tr>
      <tr>
        <td>Group Work</td>
        <td>325</td>
        <td>300</td>
        <td>500</td>
      </tr>
    </tbody>
  </table>
</section>

<section>
  <h2>Bode Plot</h2>

  <figure>
    <img
      src="{{ '/assets/images/systems/figure-bode-plot.png' | relative_url }}"
      alt="Bode Plot of X(s)/R(s)"
      class="inline-image"
    />
    <figcaption>Bode Plot of X(s)/R(s)</figcaption>
  </figure>

  <h3>MATLAB Code</h3>
  <pre><code class="language-matlab">m = 41.95;      % kg
b = 3362;       % N*s/m
k = 6975;       % N/m

s = tf('s');
G = (b*s + k)/(m*s^2 + b*s + k);

bode(G); grid on;
title('Bode Plot of X(s)/R(s)');</code></pre>

  <h3>Sources</h3>
  <ul>
    <li><a href="https://www.mathworks.com/help/control/ref/tf.html">tf - Transfer function model - MATLAB</a></li>
    <li><a href="https://www.mathworks.com/help/control/ref/bode.html">bode - Bode frequency response of dynamic system - MATLAB</a></li>
  </ul>
</section>

<section>
  <h2>Conclusion</h2>
  <p>
    The mass of our car is far smaller than the mass of the car from the group work. The group work specifies that
    the car modeled is a passenger car, whereas our car is a single-seat buggy. Thus we reasonably expect the mass
    of our car to be substantially smaller than the group work car. This is consistent with our car’s mass being
    41.95 kg and the mass of the groupwork car being 325 kg (both measurements per corner, quarter car).
  </p>
  <p>
    From data we acquired by compressing the shock stack up used in pace with our dyno, we arrived at a k value of
    approximately 7000 (about 40 lb/in). This is in the range of target stiffness values for the application of a rally car
    rather than an on-road car. For those applications, the standard spring stiffness of 100–200 lb/in provides a much
    tighter suspension. In the off-road case, much more bump and collision can be expected; as such this validates the
    stiffness in our model suspension. This is much greater than the exemplar suspension, but we find that the exemplar
    (500 N/m, or about 2.8 lb/in) is an inaccurate estimation as it would not provide enough grip or restoring force for a vehicle.
  </p>
  <p>
    For b, we extracted a damping coefficient of ~3363 N·s/m from the dynamometer data, compared to 300 N·s/m from the
    in-class group work, which also seems reasonable. Passenger vehicle suspensions aim to make the ride as smooth as possible
    for passengers. Off-road vehicle suspensions must handle more aggressive terrain, so they’re designed to safely and quickly
    handle obstacles—necessitating a much higher damping coefficient. These shocks in particular even have a custom machined
    piston inside of them to further modify the tune beyond just things like the spring stack-up, and the dynamometer data was
    originally used to validate that damping is optimal for the specific application.
  </p>
</section>

<section>
  <h2>Figure 4</h2>
  <figure>
    <img
      src="{{ '/assets/images/systems/figure-4-ai-prof-campbell.png' | relative_url }}"
      alt="Figure 4: AI generated image of Professor Campbell with the model car"
      class="inline-image"
    />
    <figcaption>Figure 4: AI generated image of Professor Campbell with the model car</figcaption>
  </figure>
</section>
