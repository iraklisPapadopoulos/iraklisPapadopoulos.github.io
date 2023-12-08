---
layout: post
title:  Plotting Longitudinal Data in R
date: 2023-12-07 21:01:00
description: Repeated measurements visualization
tags: longitudinal
categories: visualization
thumbnail: assets/img/posts/longit_plots/R.png
toc:
  beginning: true
---
The following graphs produced upon simulated data of SARSSURV study's data and for presentation purposes.

## BoxPlots

````markdown
```R
bxp <- ggboxplot(boost, x = "vax_brand", y = "logigg",
                 color = "vax_brand", palette = "jco",
                 #add = "jitter",
                 facet.by = "measur", 
                 short.panel.labs = T
)+
  labs(x = "Vaccines", y = "Log scale Anti-spike IgG (BAU/ml)")+  labs(color = "Vaccines")
 

bxp


library(rstatix)

stat.test <- boost %>%
  group_by(measur) %>%
  t_test(logigg ~ vax_brand) %>%
  adjust_pvalue(method = "bonferroni") %>%
  add_significance("p.adj")
stat.test

# Additional statistical test


# Add p-values of `stat.test` and `stat.test2`
# 1. Add stat.test
stat.test <- stat.test %>%
  add_xy_position(x = "vax_brand", dodge = 0.8)
bxp.complex <- bxp + stat_pvalue_manual(
  stat.test,  label = "p.adj.signif", tip.length = 0.03,step.increase = 0.1
)

significant_stat.test <- stat.test %>%
  filter(p.adj < 0.05)  # Adjust the significance level if needed

# Plot only the significant comparisons
bxp.complex <- bxp +
  stat_pvalue_manual(
    significant_stat.test,color="black", label = "p.adj.signif", tip.length = 0.02
  ) +
  theme_bw()
```
````


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/longit_plots/bxp.complex2.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
  <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/posts/longit_plots/bxp.complex.Nab.jpg" class="img-fluid rounded z-depth-1" zoomable=true%}
    </div>
</div>
<div class="caption">
    Visualizing by boxplots the anti-Spike IgG measurements by vaccine type and time period (left) and through time by vaccine type (right).
</div>

## Scatter plot

<div class="row mt-3">
        {% include figure.html path="assets/img/posts/longit_plots/IgG_boost_vaccine_loess.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
  
<div class="caption">
    Anti-Spike IgG through time by booster vaccination vaccine.
</div>
## Radar Plot
  
  <div class="row mt-3">
        {% include figure.html path="assets/img/posts/longit_plots/sdfg.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="caption">
    Prevalence of symptoms on three time points.
</div>