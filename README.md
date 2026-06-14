# Moshi-Moshi-Japan-zuupFARAWAY
predictive maintenance dashboard for Indian Railways track. Combines Hertz contact, Winkler bending, Paris Law crack propagation, and CWR thermal stress with a Gradient Boosting ML model (R²=0.993, 93.7% defect accuracy) across 68 IR divisions and 20 climate zones. Single self-contained HTML. Built for Far Away 2026 — Team Moshi Moshi Japan.
Important Context:
1. Z direction= direction of passage of train
2. Y direction= the vertical direction (loading direction)
3. X direction= lateral

The simulations are NOT extremely accurate; the actual modelling involves hertzian mechanics and crack propagation mechanics, which are not available to free users on simulation software. Those parts however, have been performed analytically and packaged inside the dashboard. The simulation involves thermomechanical dynamic transient analysis to mimic the effects of sun and a train passing over, a rail segment is under observation and a 5 second simulation is performed which corresponds to 4-5 axles passing over the rail segment.
