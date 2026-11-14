---
layout: page
title: Distilling Safety Filters for Filter-free Multi-agent RL
permalink: /ff-safemarl/
description:
nav: false
_styles: >
  .post-header { text-align: center; margin-bottom: 0.5rem; }
  .post-title { font-size: 2rem; line-height: 1.3; }
  .sm-authors { text-align: center; font-size: 1.1rem; margin-bottom: 0.2rem; }
  .sm-authors a { margin: 0 0.4rem; }
  .sm-affil { text-align: center; color: var(--global-text-color-light); margin-bottom: 1rem; }
  .sm-links { text-align: center; margin-bottom: 2rem; }
  .sm-links a, .sm-links span { display: inline-block; margin: 0 0.25rem; padding: 0.3rem 0.9rem;
    border: 1px solid var(--global-text-color); border-radius: 4px; color: var(--global-text-color); }
  .sm-links a:hover { background: var(--global-text-color); color: var(--global-bg-color); text-decoration: none; }
  .sm-links span { opacity: 0.45; }
  .sm-abstract { text-align: justify; }
  .sm-figure { margin: 0 0 1.5rem 0; }
  .sm-figure video { width: 100%; display: block; border: 1px solid var(--global-divider-color); border-radius: 4px; }
  .sm-figure figcaption { text-align: center; margin-top: 0.4rem; color: var(--global-text-color-light); }
  .sm-figure figcaption b { color: var(--global-text-color); }
  .sm-note { color: var(--global-text-color-light); margin-bottom: 1rem; }
  .sm-row { display: flex; flex-wrap: wrap; gap: 1rem; }
  .sm-row .sm-figure { flex: 1 1 300px; }
---

<div class="sm-authors">
  <a href="{{ '/' | relative_url }}">Srikar Babu Gadipudi</a>
  <a href="https://people.eecs.berkeley.edu/~sojoudi/">Somayeh Sojoudi</a>
  <a href="https://people.eecs.berkeley.edu/~tomlin/">Claire J. Tomlin</a>
</div>
<div class="sm-affil">University of California, Berkeley</div>

<div class="sm-links">
  <span title="Coming soon">Paper</span>
  <a href="https://github.com/srikarbabug/ff-safemarl">Code</a>
</div>

## Abstract

<p class="sm-abstract">
Reinforcement learning (RL) can teach robot teams to navigate shared spaces, but trial and error causes collisions that real robots cannot afford. Control barrier function (CBF) safety filters prevent these collisions by correcting unsafe actions, yet in a team the filter becomes a centralized optimization whose number of constraints grows quadratically with the team size. We propose an approach that builds the filter's safety into the RL policy and examine how such a system behaves during filter-free deployment. However, the filter that keeps training safe also hides what the policy must learn: it masks the cost of unsafe actions, which actor-critic methods then overvalue, and it keeps near-collision states out of the training data. Our approach therefore learns only from executed safe actions with implicit Q-learning and distills the filter into the policy at synthetic near-collision states. We prove that the filter distillation loss bounds the policy's constraint violation and hence how far agents can cross their safety margin, so errors never accumulate. When distillation states cover deployed states, the loss also bounds the collision probability at every step. Without a filter and acting only on local observations, our policies have no collisions in 1,800 test episodes with multiple agents and static obstacles. A policy trained with three agents transfers zero-shot to teams of twenty at constant per-agent computation and stays collision-free with up to eight agents. We test our policies on Crazyflie quadrotors with up to five flying at the same time.
</p>

## Hardware: Crazyflie Quadrotors

<p class="sm-note">All flights run the policy trained in simulation with 3 agents, with no safety filter. The inset shows the drones' positions and goals.</p>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/3n_4obs_full.mp4' | relative_url }}" poster="{{ '/assets/video/ff-safemarl/3n_4obs_full_poster.jpg' | relative_url }}" controls muted playsinline preload="metadata"></video>
  <figcaption><b>3 drones, 4 obstacles</b> &middot; full observation</figcaption>
</figure>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/3n_4obs_partial.mp4' | relative_url }}" poster="{{ '/assets/video/ff-safemarl/3n_4obs_partial_poster.jpg' | relative_url }}" controls muted playsinline preload="metadata"></video>
  <figcaption><b>3 drones, 4 obstacles</b> &middot; partial observation (local sensing)</figcaption>
</figure>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/5n_2obs_partial.mp4' | relative_url }}" poster="{{ '/assets/video/ff-safemarl/5n_2obs_partial_poster.jpg' | relative_url }}" controls muted playsinline preload="metadata"></video>
  <figcaption><b>5 drones, 2 obstacles</b> &middot; partial observation (local sensing), zero-shot in team size</figcaption>
</figure>

## Simulation

<p class="sm-note">Filter-free roll-outs with double-integrator agents. Open circles: starts; crosses: goals; red discs: obstacles.</p>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/ours_n3o4_sensing.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>Ours</b> &middot; 3 agents, 4 obstacles, local sensing (shaded: sensing radius)</figcaption>
</figure>

### Ablations

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/abl_sac_only.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>SAC only</b></figcaption>
</figure>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/abl_iql_only.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>IQL only</b></figcaption>
</figure>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/abl_iql_osfd.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>IQL + OSFD</b></figcaption>
</figure>

### Zero-shot Scaling

<p class="sm-note">A policy trained with 3 agents, deployed unchanged on larger teams.</p>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/zs_n12c_iql_vs_ours.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>12 agents, circle layout</b> &middot; IQL only vs. ours</figcaption>
</figure>

<div class="sm-row">
  <figure class="sm-figure">
    <video src="{{ '/assets/video/ff-safemarl/zs_n8c.mp4' | relative_url }}" autoplay loop muted playsinline></video>
    <figcaption><b>8 agents</b> &middot; circle layout</figcaption>
  </figure>
  <figure class="sm-figure">
    <video src="{{ '/assets/video/ff-safemarl/zs_n16c.mp4' | relative_url }}" autoplay loop muted playsinline></video>
    <figcaption><b>16 agents</b> &middot; circle layout</figcaption>
  </figure>
</div>

<figure class="sm-figure">
  <video src="{{ '/assets/video/ff-safemarl/zs_n20r.mp4' | relative_url }}" autoplay loop muted playsinline></video>
  <figcaption><b>20 agents</b> &middot; random layout</figcaption>
</figure>
