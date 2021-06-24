---
title: 'Weekly Exercises #5'
author: "Yunyang Zhong"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---




```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.


```r
records <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-05-25/records.csv')

track_graph <- records %>%
  group_by(track) %>%
  filter(shortcut == "No") %>%
  filter(time == min(time)) %>%
  filter(date == max(date)) %>%
  ggplot() +
  geom_col(aes(x = time,
               y = fct_reorder(track,time),
               text = track)) +
  labs(title="Which track is the fastest without shortcuts?",
       y = "") +
  scale_x_continuous(breaks = c(0, 15, 30, 45, 60, 75, 90, 105, 120))

ggplotly(track_graph,
         tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-e69a48b6750130cceac4" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-e69a48b6750130cceac4">{"x":{"data":[{"orientation":"v","width":[37.58,27.8,30.78,38.96,58.69,38.27,38.02,27.62,85.82,37.72,55.5,43.15,42.04,31.25,40.78,116.35],"base":[4.55,1.55,2.55,8.55,13.55,7.55,6.55,0.55,14.55,5.55,12.55,11.55,10.55,3.55,9.55,15.55],"x":[18.79,13.9,15.39,19.48,29.345,19.135,19.01,13.81,42.91,18.86,27.75,21.575,21.02,15.625,20.39,58.175],"y":[0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.899999999999999,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.899999999999999],"text":["time:  37.58<br />Luigi Raceway","time:  27.80<br />Moo Moo Farm","time:  30.78<br />Koopa Troopa Beach","time:  38.96<br />Kalimari Desert","time:  58.69<br />Toad's Turnpike","time:  38.27<br />Frappe Snowland","time:  38.02<br />Choco Mountain","time:  27.62<br />Mario Raceway","time:  85.82<br />Wario Stadium","time:  37.72<br />Sherbet Land","time:  55.50<br />Royal Raceway","time:  43.15<br />Bowser's Castle","time:  42.04<br />D.K.'s Jungle Parkway","time:  31.25<br />Yoshi Valley","time:  40.78<br />Banshee Boardwalk","time: 116.35<br />Rainbow Road"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":133.698630136986},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Which track is the fastest without shortcuts?","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-5.8175,122.1675],"tickmode":"array","ticktext":["0","15","30","45","60","75","90","105","120"],"tickvals":[0,15,30,45,60,75,90,105,120],"categoryorder":"array","categoryarray":["0","15","30","45","60","75","90","105","120"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"time","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,16.6],"tickmode":"array","ticktext":["Mario Raceway","Moo Moo Farm","Koopa Troopa Beach","Yoshi Valley","Luigi Raceway","Sherbet Land","Choco Mountain","Frappe Snowland","Kalimari Desert","Banshee Boardwalk","D.K.'s Jungle Parkway","Bowser's Castle","Royal Raceway","Toad's Turnpike","Wario Stadium","Rainbow Road"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16],"categoryorder":"array","categoryarray":["Mario Raceway","Moo Moo Farm","Koopa Troopa Beach","Yoshi Valley","Luigi Raceway","Sherbet Land","Choco Mountain","Frappe Snowland","Kalimari Desert","Banshee Boardwalk","D.K.'s Jungle Parkway","Bowser's Castle","Royal Raceway","Toad's Turnpike","Wario Stadium","Rainbow Road"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1ec059c190f":{"x":{},"y":{},"text":{},"type":"bar"}},"cur_data":"1ec059c190f","visdat":{"1ec059c190f":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```


```r
viewers <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-01/viewers.csv')

popularity_graph <- viewers %>% 
  ggplot(aes(x = episode_number_overall)) +
  geom_smooth(aes(y = viewers, color = "Viewers in millions"), se = FALSE) +
  geom_smooth(aes(y = rating_18_49, color = "Rating by viwers aged 18-49"), se = FALSE) +
  geom_smooth(aes(y = share_18_49, color = "Share of viewers aged 18-49"), se = FALSE) +
  labs(x = "episode number overall", y = "", title = "Popularity of the show over time") +
  theme(legend.title = element_blank(),
        legend.position = "none")

ggplotly(popularity_graph,
         tooltip = c("y", "x"))
```

```{=html}
<div id="htmlwidget-7403c5774e5d2ab0d74e" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-7403c5774e5d2ab0d74e">{"x":{"data":[{"x":[1,8.53164556962025,16.0632911392405,23.5949367088608,31.126582278481,38.6582278481013,46.1898734177215,53.7215189873418,61.253164556962,68.7848101265823,76.3164556962025,83.8481012658228,91.379746835443,98.9113924050633,106.443037974684,113.974683544304,121.506329113924,129.037974683544,136.569620253165,144.101265822785,151.632911392405,159.164556962025,166.696202531646,174.227848101266,181.759493670886,189.291139240506,196.822784810127,204.354430379747,211.886075949367,219.417721518987,226.949367088608,234.481012658228,242.012658227848,249.544303797468,257.075949367089,264.607594936709,272.139240506329,279.670886075949,287.20253164557,294.73417721519,302.26582278481,309.79746835443,317.329113924051,324.860759493671,332.392405063291,339.924050632911,347.455696202532,354.987341772152,362.518987341772,370.050632911392,377.582278481013,385.113924050633,392.645569620253,400.177215189873,407.708860759494,415.240506329114,422.772151898734,430.303797468354,437.835443037975,445.367088607595,452.898734177215,460.430379746835,467.962025316456,475.493670886076,483.025316455696,490.556962025316,498.088607594937,505.620253164557,513.151898734177,520.683544303797,528.215189873418,535.746835443038,543.278481012658,550.810126582278,558.341772151899,565.873417721519,573.405063291139,580.936708860759,588.46835443038,596],"y":[27.2111319528657,26.6867539113882,26.1692590344774,25.6584957247927,25.1543123849938,24.65655741774,24.1650792256911,23.6797262115066,23.2003467778459,22.7267893273685,22.2589022627342,21.7965205782324,21.3393509828578,20.8876874351498,20.4420107257309,20.0028016452233,19.5705409842497,19.1457095334324,18.7287880833939,18.3202574247566,17.9205983481431,17.5302916441757,17.1450515055064,16.7589178865541,16.3736040343879,15.9909000779192,15.6125961460595,15.2404823677201,14.8763488718125,14.521985787248,14.1791832429381,13.8497313677941,13.5348910764747,13.2329152692808,12.9433546983507,12.6660979383084,12.401033563778,12.1480501493834,11.9070362697486,11.6778804994976,11.4604714132543,11.2548403684351,11.0747551736525,10.92489369144,10.7989232059683,10.6905110014082,10.5933243619303,10.5010305717055,10.4072969149044,10.3057906756978,10.1901791382563,10.0582575373642,9.92675195824902,9.79790866689817,9.6713285495121,9.54661249229131,9.42336138143629,9.30117610314754,9.17965754362556,9.05840658907085,8.93704070396916,8.81664005261569,8.69800163742103,8.5810629803747,8.4657616034662,8.35203502868503,8.23982077802068,8.12905637346268,8.01967933700051,7.91162719062368,7.80483418861669,7.69924302751158,7.5948741439501,7.49175900703952,7.38992908588711,7.28941584960012,7.19025076728582,7.09246530805146,6.99609094100431,6.90115913525164],"text":["episode_number_overall:   1.000000<br />viewers: 27.211132","episode_number_overall:   8.531646<br />viewers: 26.686754","episode_number_overall:  16.063291<br />viewers: 26.169259","episode_number_overall:  23.594937<br />viewers: 25.658496","episode_number_overall:  31.126582<br />viewers: 25.154312","episode_number_overall:  38.658228<br />viewers: 24.656557","episode_number_overall:  46.189873<br />viewers: 24.165079","episode_number_overall:  53.721519<br />viewers: 23.679726","episode_number_overall:  61.253165<br />viewers: 23.200347","episode_number_overall:  68.784810<br />viewers: 22.726789","episode_number_overall:  76.316456<br />viewers: 22.258902","episode_number_overall:  83.848101<br />viewers: 21.796521","episode_number_overall:  91.379747<br />viewers: 21.339351","episode_number_overall:  98.911392<br />viewers: 20.887687","episode_number_overall: 106.443038<br />viewers: 20.442011","episode_number_overall: 113.974684<br />viewers: 20.002802","episode_number_overall: 121.506329<br />viewers: 19.570541","episode_number_overall: 129.037975<br />viewers: 19.145710","episode_number_overall: 136.569620<br />viewers: 18.728788","episode_number_overall: 144.101266<br />viewers: 18.320257","episode_number_overall: 151.632911<br />viewers: 17.920598","episode_number_overall: 159.164557<br />viewers: 17.530292","episode_number_overall: 166.696203<br />viewers: 17.145052","episode_number_overall: 174.227848<br />viewers: 16.758918","episode_number_overall: 181.759494<br />viewers: 16.373604","episode_number_overall: 189.291139<br />viewers: 15.990900","episode_number_overall: 196.822785<br />viewers: 15.612596","episode_number_overall: 204.354430<br />viewers: 15.240482","episode_number_overall: 211.886076<br />viewers: 14.876349","episode_number_overall: 219.417722<br />viewers: 14.521986","episode_number_overall: 226.949367<br />viewers: 14.179183","episode_number_overall: 234.481013<br />viewers: 13.849731","episode_number_overall: 242.012658<br />viewers: 13.534891","episode_number_overall: 249.544304<br />viewers: 13.232915","episode_number_overall: 257.075949<br />viewers: 12.943355","episode_number_overall: 264.607595<br />viewers: 12.666098","episode_number_overall: 272.139241<br />viewers: 12.401034","episode_number_overall: 279.670886<br />viewers: 12.148050","episode_number_overall: 287.202532<br />viewers: 11.907036","episode_number_overall: 294.734177<br />viewers: 11.677880","episode_number_overall: 302.265823<br />viewers: 11.460471","episode_number_overall: 309.797468<br />viewers: 11.254840","episode_number_overall: 317.329114<br />viewers: 11.074755","episode_number_overall: 324.860759<br />viewers: 10.924894","episode_number_overall: 332.392405<br />viewers: 10.798923","episode_number_overall: 339.924051<br />viewers: 10.690511","episode_number_overall: 347.455696<br />viewers: 10.593324","episode_number_overall: 354.987342<br />viewers: 10.501031","episode_number_overall: 362.518987<br />viewers: 10.407297","episode_number_overall: 370.050633<br />viewers: 10.305791","episode_number_overall: 377.582278<br />viewers: 10.190179","episode_number_overall: 385.113924<br />viewers: 10.058258","episode_number_overall: 392.645570<br />viewers:  9.926752","episode_number_overall: 400.177215<br />viewers:  9.797909","episode_number_overall: 407.708861<br />viewers:  9.671329","episode_number_overall: 415.240506<br />viewers:  9.546612","episode_number_overall: 422.772152<br />viewers:  9.423361","episode_number_overall: 430.303797<br />viewers:  9.301176","episode_number_overall: 437.835443<br />viewers:  9.179658","episode_number_overall: 445.367089<br />viewers:  9.058407","episode_number_overall: 452.898734<br />viewers:  8.937041","episode_number_overall: 460.430380<br />viewers:  8.816640","episode_number_overall: 467.962025<br />viewers:  8.698002","episode_number_overall: 475.493671<br />viewers:  8.581063","episode_number_overall: 483.025316<br />viewers:  8.465762","episode_number_overall: 490.556962<br />viewers:  8.352035","episode_number_overall: 498.088608<br />viewers:  8.239821","episode_number_overall: 505.620253<br />viewers:  8.129056","episode_number_overall: 513.151899<br />viewers:  8.019679","episode_number_overall: 520.683544<br />viewers:  7.911627","episode_number_overall: 528.215190<br />viewers:  7.804834","episode_number_overall: 535.746835<br />viewers:  7.699243","episode_number_overall: 543.278481<br />viewers:  7.594874","episode_number_overall: 550.810127<br />viewers:  7.491759","episode_number_overall: 558.341772<br />viewers:  7.389929","episode_number_overall: 565.873418<br />viewers:  7.289416","episode_number_overall: 573.405063<br />viewers:  7.190251","episode_number_overall: 580.936709<br />viewers:  7.092465","episode_number_overall: 588.468354<br />viewers:  6.996091","episode_number_overall: 596.000000<br />viewers:  6.901159"],"type":"scatter","mode":"lines","name":"Viewers in millions","line":{"width":3.77952755905512,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","legendgroup":"Viewers in millions","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1,8.53164556962025,16.0632911392405,23.5949367088608,31.126582278481,38.6582278481013,46.1898734177215,53.7215189873418,61.253164556962,68.7848101265823,76.3164556962025,83.8481012658228,91.379746835443,98.9113924050633,106.443037974684,113.974683544304,121.506329113924,129.037974683544,136.569620253165,144.101265822785,151.632911392405,159.164556962025,166.696202531646,174.227848101266,181.759493670886,189.291139240506,196.822784810127,204.354430379747,211.886075949367,219.417721518987,226.949367088608,234.481012658228,242.012658227848,249.544303797468,257.075949367089,264.607594936709,272.139240506329,279.670886075949,287.20253164557,294.73417721519,302.26582278481,309.79746835443,317.329113924051,324.860759493671,332.392405063291,339.924050632911,347.455696202532,354.987341772152,362.518987341772,370.050632911392,377.582278481013,385.113924050633,392.645569620253,400.177215189873,407.708860759494,415.240506329114,422.772151898734,430.303797468354,437.835443037975,445.367088607595,452.898734177215,460.430379746835,467.962025316456,475.493670886076,483.025316455696,490.556962025316,498.088607594937,505.620253164557,513.151898734177,520.683544303797,528.215189873418,535.746835443038,543.278481012658,550.810126582278,558.341772151899,565.873417721519,573.405063291139,580.936708860759,588.46835443038,596],"y":[5.08799747133113,5.29174761920706,5.48336933566469,5.66251603087244,5.82884111499871,5.98199799821187,6.12164009068035,6.24742080257253,6.35899354405681,6.45601172530158,6.53812875647525,6.60462240562395,6.65447109220082,6.68862056802666,6.70816210082846,6.71418695833322,6.70778640826795,6.69005171835963,6.66207415633526,6.62494498992184,6.57975548684637,6.5275879591453,6.45097975498152,6.33794350871191,6.19536573078845,6.0301329316631,5.84913162178784,5.65924831161461,5.46736951159541,5.28038173218218,5.1051714838269,4.94862527698153,4.80633006302178,4.65909396097486,4.50821903033056,4.35569530500676,4.20351281892133,4.05366160599214,3.90813170013707,3.768913135274,3.63799594532079,3.51737016419533,3.40721219204625,3.30458072966017,3.20844757732023,3.11783740838827,3.03177489622612,2.94928471419562,2.86939153565861,2.79112003397694,2.71349488251243,2.63604663621758,2.56232123836046,2.49271945969733,2.42664565044288,2.3635041608118,2.3026993410188,2.24363554127856,2.18571711180577,2.12834840281514,2.07093376452136,2.0145360221522,1.96061413219453,1.90903707963469,1.85967355540548,1.81239225043972,1.76706185567024,1.72355106202985,1.68172856045136,1.6414630418676,1.60264519172092,1.56538500504131,1.52976228379673,1.49582711817536,1.46362959836533,1.43321981455479,1.40464785693191,1.37796381568482,1.35321778100169,1.33045984307065],"text":["episode_number_overall:   1.000000<br />rating_18_49: 5.087997","episode_number_overall:   8.531646<br />rating_18_49: 5.291748","episode_number_overall:  16.063291<br />rating_18_49: 5.483369","episode_number_overall:  23.594937<br />rating_18_49: 5.662516","episode_number_overall:  31.126582<br />rating_18_49: 5.828841","episode_number_overall:  38.658228<br />rating_18_49: 5.981998","episode_number_overall:  46.189873<br />rating_18_49: 6.121640","episode_number_overall:  53.721519<br />rating_18_49: 6.247421","episode_number_overall:  61.253165<br />rating_18_49: 6.358994","episode_number_overall:  68.784810<br />rating_18_49: 6.456012","episode_number_overall:  76.316456<br />rating_18_49: 6.538129","episode_number_overall:  83.848101<br />rating_18_49: 6.604622","episode_number_overall:  91.379747<br />rating_18_49: 6.654471","episode_number_overall:  98.911392<br />rating_18_49: 6.688621","episode_number_overall: 106.443038<br />rating_18_49: 6.708162","episode_number_overall: 113.974684<br />rating_18_49: 6.714187","episode_number_overall: 121.506329<br />rating_18_49: 6.707786","episode_number_overall: 129.037975<br />rating_18_49: 6.690052","episode_number_overall: 136.569620<br />rating_18_49: 6.662074","episode_number_overall: 144.101266<br />rating_18_49: 6.624945","episode_number_overall: 151.632911<br />rating_18_49: 6.579755","episode_number_overall: 159.164557<br />rating_18_49: 6.527588","episode_number_overall: 166.696203<br />rating_18_49: 6.450980","episode_number_overall: 174.227848<br />rating_18_49: 6.337944","episode_number_overall: 181.759494<br />rating_18_49: 6.195366","episode_number_overall: 189.291139<br />rating_18_49: 6.030133","episode_number_overall: 196.822785<br />rating_18_49: 5.849132","episode_number_overall: 204.354430<br />rating_18_49: 5.659248","episode_number_overall: 211.886076<br />rating_18_49: 5.467370","episode_number_overall: 219.417722<br />rating_18_49: 5.280382","episode_number_overall: 226.949367<br />rating_18_49: 5.105171","episode_number_overall: 234.481013<br />rating_18_49: 4.948625","episode_number_overall: 242.012658<br />rating_18_49: 4.806330","episode_number_overall: 249.544304<br />rating_18_49: 4.659094","episode_number_overall: 257.075949<br />rating_18_49: 4.508219","episode_number_overall: 264.607595<br />rating_18_49: 4.355695","episode_number_overall: 272.139241<br />rating_18_49: 4.203513","episode_number_overall: 279.670886<br />rating_18_49: 4.053662","episode_number_overall: 287.202532<br />rating_18_49: 3.908132","episode_number_overall: 294.734177<br />rating_18_49: 3.768913","episode_number_overall: 302.265823<br />rating_18_49: 3.637996","episode_number_overall: 309.797468<br />rating_18_49: 3.517370","episode_number_overall: 317.329114<br />rating_18_49: 3.407212","episode_number_overall: 324.860759<br />rating_18_49: 3.304581","episode_number_overall: 332.392405<br />rating_18_49: 3.208448","episode_number_overall: 339.924051<br />rating_18_49: 3.117837","episode_number_overall: 347.455696<br />rating_18_49: 3.031775","episode_number_overall: 354.987342<br />rating_18_49: 2.949285","episode_number_overall: 362.518987<br />rating_18_49: 2.869392","episode_number_overall: 370.050633<br />rating_18_49: 2.791120","episode_number_overall: 377.582278<br />rating_18_49: 2.713495","episode_number_overall: 385.113924<br />rating_18_49: 2.636047","episode_number_overall: 392.645570<br />rating_18_49: 2.562321","episode_number_overall: 400.177215<br />rating_18_49: 2.492719","episode_number_overall: 407.708861<br />rating_18_49: 2.426646","episode_number_overall: 415.240506<br />rating_18_49: 2.363504","episode_number_overall: 422.772152<br />rating_18_49: 2.302699","episode_number_overall: 430.303797<br />rating_18_49: 2.243636","episode_number_overall: 437.835443<br />rating_18_49: 2.185717","episode_number_overall: 445.367089<br />rating_18_49: 2.128348","episode_number_overall: 452.898734<br />rating_18_49: 2.070934","episode_number_overall: 460.430380<br />rating_18_49: 2.014536","episode_number_overall: 467.962025<br />rating_18_49: 1.960614","episode_number_overall: 475.493671<br />rating_18_49: 1.909037","episode_number_overall: 483.025316<br />rating_18_49: 1.859674","episode_number_overall: 490.556962<br />rating_18_49: 1.812392","episode_number_overall: 498.088608<br />rating_18_49: 1.767062","episode_number_overall: 505.620253<br />rating_18_49: 1.723551","episode_number_overall: 513.151899<br />rating_18_49: 1.681729","episode_number_overall: 520.683544<br />rating_18_49: 1.641463","episode_number_overall: 528.215190<br />rating_18_49: 1.602645","episode_number_overall: 535.746835<br />rating_18_49: 1.565385","episode_number_overall: 543.278481<br />rating_18_49: 1.529762","episode_number_overall: 550.810127<br />rating_18_49: 1.495827","episode_number_overall: 558.341772<br />rating_18_49: 1.463630","episode_number_overall: 565.873418<br />rating_18_49: 1.433220","episode_number_overall: 573.405063<br />rating_18_49: 1.404648","episode_number_overall: 580.936709<br />rating_18_49: 1.377964","episode_number_overall: 588.468354<br />rating_18_49: 1.353218","episode_number_overall: 596.000000<br />rating_18_49: 1.330460"],"type":"scatter","mode":"lines","name":"Rating by viwers aged 18-49","line":{"width":3.77952755905512,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","legendgroup":"Rating by viwers aged 18-49","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1,8.53164556962025,16.0632911392405,23.5949367088608,31.126582278481,38.6582278481013,46.1898734177215,53.7215189873418,61.253164556962,68.7848101265823,76.3164556962025,83.8481012658228,91.379746835443,98.9113924050633,106.443037974684,113.974683544304,121.506329113924,129.037974683544,136.569620253165,144.101265822785,151.632911392405,159.164556962025,166.696202531646,174.227848101266,181.759493670886,189.291139240506,196.822784810127,204.354430379747,211.886075949367,219.417721518987,226.949367088608,234.481012658228,242.012658227848,249.544303797468,257.075949367089,264.607594936709,272.139240506329,279.670886075949,287.20253164557,294.73417721519,302.26582278481,309.79746835443,317.329113924051,324.860759493671,332.392405063291,339.924050632911,347.455696202532,354.987341772152,362.518987341772,370.050632911392,377.582278481013,385.113924050633,392.645569620253,400.177215189873,407.708860759494,415.240506329114,422.772151898734,430.303797468354,437.835443037975,445.367088607595,452.898734177215,460.430379746835,467.962025316456,475.493670886076,483.025316455696,490.556962025316,498.088607594937,505.620253164557,513.151898734177,520.683544303797,528.215189873418,535.746835443038,543.278481012658,550.810126582278,558.341772151899,565.873417721519,573.405063291139,580.936708860759,588.46835443038,596],"y":[31.2610452811186,30.4341960586789,29.6226839476658,28.8265437539138,28.0458102832575,27.2805183415313,26.5307027345699,25.7963982682077,25.0776397482792,24.3744619806189,23.6868997710615,23.0153454115449,22.3609345792967,21.7234689007381,21.1026330642382,20.498111758166,19.9095896708907,19.3367514907814,18.7792819062073,18.2368656055374,17.7091872771408,17.1959316093867,16.6967832906442,16.2121971421566,15.7438093394863,15.2907837961076,14.8521242491924,14.4268344359131,14.0139180934417,13.6123789589503,13.2212207696111,12.8394472625963,12.4660621750779,12.1035975169954,11.7600073480488,11.4345714994552,11.1261143929014,10.8334604500746,10.5554340926617,10.2908597423496,10.0385618208253,9.79736474977592,9.56609295088833,9.34884392012133,9.15480306703285,8.98067500974551,8.822714691806,8.67717705676102,8.54031704815725,8.40838960954139,8.27764968446014,8.14435221646017,8.00671971071835,7.87482709960647,7.75036012112628,7.63291712709359,7.52209646932422,7.41749649963399,7.3187155698387,7.22535203175417,7.13700423719621,7.05339082856439,6.97585439137108,6.90478944011158,6.84008257277383,6.78162038734576,6.72928948181531,6.68297645417043,6.64256790239905,6.60795042448911,6.5790105791725,6.55561667644825,6.53776511931871,6.52552665331844,6.518972023982,6.51817197684395,6.52319725743886,6.53411861130129,6.55100678396579,6.57393252096694],"text":["episode_number_overall:   1.000000<br />share_18_49: 31.261045","episode_number_overall:   8.531646<br />share_18_49: 30.434196","episode_number_overall:  16.063291<br />share_18_49: 29.622684","episode_number_overall:  23.594937<br />share_18_49: 28.826544","episode_number_overall:  31.126582<br />share_18_49: 28.045810","episode_number_overall:  38.658228<br />share_18_49: 27.280518","episode_number_overall:  46.189873<br />share_18_49: 26.530703","episode_number_overall:  53.721519<br />share_18_49: 25.796398","episode_number_overall:  61.253165<br />share_18_49: 25.077640","episode_number_overall:  68.784810<br />share_18_49: 24.374462","episode_number_overall:  76.316456<br />share_18_49: 23.686900","episode_number_overall:  83.848101<br />share_18_49: 23.015345","episode_number_overall:  91.379747<br />share_18_49: 22.360935","episode_number_overall:  98.911392<br />share_18_49: 21.723469","episode_number_overall: 106.443038<br />share_18_49: 21.102633","episode_number_overall: 113.974684<br />share_18_49: 20.498112","episode_number_overall: 121.506329<br />share_18_49: 19.909590","episode_number_overall: 129.037975<br />share_18_49: 19.336751","episode_number_overall: 136.569620<br />share_18_49: 18.779282","episode_number_overall: 144.101266<br />share_18_49: 18.236866","episode_number_overall: 151.632911<br />share_18_49: 17.709187","episode_number_overall: 159.164557<br />share_18_49: 17.195932","episode_number_overall: 166.696203<br />share_18_49: 16.696783","episode_number_overall: 174.227848<br />share_18_49: 16.212197","episode_number_overall: 181.759494<br />share_18_49: 15.743809","episode_number_overall: 189.291139<br />share_18_49: 15.290784","episode_number_overall: 196.822785<br />share_18_49: 14.852124","episode_number_overall: 204.354430<br />share_18_49: 14.426834","episode_number_overall: 211.886076<br />share_18_49: 14.013918","episode_number_overall: 219.417722<br />share_18_49: 13.612379","episode_number_overall: 226.949367<br />share_18_49: 13.221221","episode_number_overall: 234.481013<br />share_18_49: 12.839447","episode_number_overall: 242.012658<br />share_18_49: 12.466062","episode_number_overall: 249.544304<br />share_18_49: 12.103598","episode_number_overall: 257.075949<br />share_18_49: 11.760007","episode_number_overall: 264.607595<br />share_18_49: 11.434571","episode_number_overall: 272.139241<br />share_18_49: 11.126114","episode_number_overall: 279.670886<br />share_18_49: 10.833460","episode_number_overall: 287.202532<br />share_18_49: 10.555434","episode_number_overall: 294.734177<br />share_18_49: 10.290860","episode_number_overall: 302.265823<br />share_18_49: 10.038562","episode_number_overall: 309.797468<br />share_18_49:  9.797365","episode_number_overall: 317.329114<br />share_18_49:  9.566093","episode_number_overall: 324.860759<br />share_18_49:  9.348844","episode_number_overall: 332.392405<br />share_18_49:  9.154803","episode_number_overall: 339.924051<br />share_18_49:  8.980675","episode_number_overall: 347.455696<br />share_18_49:  8.822715","episode_number_overall: 354.987342<br />share_18_49:  8.677177","episode_number_overall: 362.518987<br />share_18_49:  8.540317","episode_number_overall: 370.050633<br />share_18_49:  8.408390","episode_number_overall: 377.582278<br />share_18_49:  8.277650","episode_number_overall: 385.113924<br />share_18_49:  8.144352","episode_number_overall: 392.645570<br />share_18_49:  8.006720","episode_number_overall: 400.177215<br />share_18_49:  7.874827","episode_number_overall: 407.708861<br />share_18_49:  7.750360","episode_number_overall: 415.240506<br />share_18_49:  7.632917","episode_number_overall: 422.772152<br />share_18_49:  7.522096","episode_number_overall: 430.303797<br />share_18_49:  7.417496","episode_number_overall: 437.835443<br />share_18_49:  7.318716","episode_number_overall: 445.367089<br />share_18_49:  7.225352","episode_number_overall: 452.898734<br />share_18_49:  7.137004","episode_number_overall: 460.430380<br />share_18_49:  7.053391","episode_number_overall: 467.962025<br />share_18_49:  6.975854","episode_number_overall: 475.493671<br />share_18_49:  6.904789","episode_number_overall: 483.025316<br />share_18_49:  6.840083","episode_number_overall: 490.556962<br />share_18_49:  6.781620","episode_number_overall: 498.088608<br />share_18_49:  6.729289","episode_number_overall: 505.620253<br />share_18_49:  6.682976","episode_number_overall: 513.151899<br />share_18_49:  6.642568","episode_number_overall: 520.683544<br />share_18_49:  6.607950","episode_number_overall: 528.215190<br />share_18_49:  6.579011","episode_number_overall: 535.746835<br />share_18_49:  6.555617","episode_number_overall: 543.278481<br />share_18_49:  6.537765","episode_number_overall: 550.810127<br />share_18_49:  6.525527","episode_number_overall: 558.341772<br />share_18_49:  6.518972","episode_number_overall: 565.873418<br />share_18_49:  6.518172","episode_number_overall: 573.405063<br />share_18_49:  6.523197","episode_number_overall: 580.936709<br />share_18_49:  6.534119","episode_number_overall: 588.468354<br />share_18_49:  6.551007","episode_number_overall: 596.000000<br />share_18_49:  6.573933"],"type":"scatter","mode":"lines","name":"Share of viewers aged 18-49","line":{"width":3.77952755905512,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","legendgroup":"Share of viewers aged 18-49","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":22.648401826484},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Popularity of the show over time","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-28.75,625.75],"tickmode":"array","ticktext":["0","200","400","600"],"tickvals":[0,200,400,600],"categoryorder":"array","categoryarray":["0","200","400","600"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"episode number overall","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.166069428831743,32.757574553021],"tickmode":"array","ticktext":["0","10","20","30"],"tickvals":[0,10,20,30],"categoryorder":"array","categoryarray":["0","10","20","30"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1ec0588e4ab":{"x":{},"y":{},"colour":{},"type":"scatter"},"1ec0743a1176":{"x":{},"y":{},"colour":{}},"1ec05ba661e6":{"x":{},"y":{},"colour":{}}},"cur_data":"1ec0588e4ab","visdat":{"1ec0588e4ab":["function (y) ","x"],"1ec0743a1176":["function (y) ","x"],"1ec05ba661e6":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
library(zoo)

delay_graph <- small_trains %>% 
  mutate(date = as.yearmon(paste(small_trains$year, small_trains$month), "%Y %m")) %>% 
  group_by(date) %>% 
  summarise(total_delay_depart = sum(num_late_at_departure)) %>% 
  ggplot() +
  geom_col(aes(x = date,
               y = total_delay_depart,
               text = as.factor(date))) +
  labs(y = "",
       title = "Number of delayed departures each month") +
  theme(legend.position = "none")

ggplotly(delay_graph,
         tooltip = c("text", "total_delay_depart"))
```

```{=html}
<div id="htmlwidget-b12853c2609ecfa10551" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-b12853c2609ecfa10551">{"x":{"data":[{"orientation":"v","width":[0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181,0.0749999999998181],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[2015,2015.08333333333,2015.16666666667,2015.25,2015.33333333333,2015.41666666667,2015.5,2015.58333333333,2015.66666666667,2015.75,2015.83333333333,2015.91666666667,2016,2016.08333333333,2016.16666666667,2016.25,2016.33333333333,2016.41666666667,2016.5,2016.58333333333,2016.66666666667,2016.75,2016.83333333333,2016.91666666667,2017,2017.08333333333,2017.16666666667,2017.25,2017.33333333333,2017.41666666667,2017.5,2017.58333333333,2017.66666666667,2017.75,2017.83333333333,2017.91666666667,2018,2018.08333333333,2018.16666666667,2018.25,2018.33333333333,2018.41666666667,2018.5,2018.58333333333,2018.66666666667,2018.75,2018.83333333333],"y":[13788,15900,12120,16992,15762,20490,28110,22938,18684,17670,18036,15516,12012,13044,15558,16314,18552,24012,25572,20436,18654,21966,22704,22512,22272,18936,18828,21936,22662,32322,31404,27486,24330,23274,24876,28998,59676,63612,61392,46068,23760,28296,81306,70860,60666,61134,61146],"text":["total_delay_depart: 13788<br />Jan 2015","total_delay_depart: 15900<br />Feb 2015","total_delay_depart: 12120<br />Mar 2015","total_delay_depart: 16992<br />Apr 2015","total_delay_depart: 15762<br />May 2015","total_delay_depart: 20490<br />Jun 2015","total_delay_depart: 28110<br />Jul 2015","total_delay_depart: 22938<br />Aug 2015","total_delay_depart: 18684<br />Sep 2015","total_delay_depart: 17670<br />Oct 2015","total_delay_depart: 18036<br />Nov 2015","total_delay_depart: 15516<br />Dec 2015","total_delay_depart: 12012<br />Jan 2016","total_delay_depart: 13044<br />Feb 2016","total_delay_depart: 15558<br />Mar 2016","total_delay_depart: 16314<br />Apr 2016","total_delay_depart: 18552<br />May 2016","total_delay_depart: 24012<br />Jun 2016","total_delay_depart: 25572<br />Jul 2016","total_delay_depart: 20436<br />Aug 2016","total_delay_depart: 18654<br />Sep 2016","total_delay_depart: 21966<br />Oct 2016","total_delay_depart: 22704<br />Nov 2016","total_delay_depart: 22512<br />Dec 2016","total_delay_depart: 22272<br />Jan 2017","total_delay_depart: 18936<br />Feb 2017","total_delay_depart: 18828<br />Mar 2017","total_delay_depart: 21936<br />Apr 2017","total_delay_depart: 22662<br />May 2017","total_delay_depart: 32322<br />Jun 2017","total_delay_depart: 31404<br />Jul 2017","total_delay_depart: 27486<br />Aug 2017","total_delay_depart: 24330<br />Sep 2017","total_delay_depart: 23274<br />Oct 2017","total_delay_depart: 24876<br />Nov 2017","total_delay_depart: 28998<br />Dec 2017","total_delay_depart: 59676<br />Jan 2018","total_delay_depart: 63612<br />Feb 2018","total_delay_depart: 61392<br />Mar 2018","total_delay_depart: 46068<br />Apr 2018","total_delay_depart: 23760<br />May 2018","total_delay_depart: 28296<br />Jun 2018","total_delay_depart: 81306<br />Jul 2018","total_delay_depart: 70860<br />Aug 2018","total_delay_depart: 60666<br />Sep 2018","total_delay_depart: 61134<br />Oct 2018","total_delay_depart: 61146<br />Nov 2018"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":40.1826484018265},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Number of delayed departures each month","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[2014.76708333333,2019.06625],"tickmode":"array","ticktext":["Jan 2015","Jan 2016","Jan 2017","Jan 2018","Jan 2019"],"tickvals":[2015,2016,2017,2018,2019],"categoryorder":"array","categoryarray":["Jan 2015","Jan 2016","Jan 2017","Jan 2018","Jan 2019"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-4065.3,85371.3],"tickmode":"array","ticktext":["0","20000","40000","60000","80000"],"tickvals":[0,20000,40000,60000,80000],"categoryorder":"array","categoryarray":["0","20000","40000","60000","80000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1ec01c701ecf":{"x":{},"y":{},"text":{},"type":"bar"}},"cur_data":"1ec01c701ecf","visdat":{"1ec01c701ecf":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
graph3 <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>% 
  group_by(variety) %>% 
  mutate(cum = cumsum(daily_harvest_lb)) %>% 
  ggplot(aes(x = date, y = cum)) +
  geom_area(aes(fill = fct_reorder(variety, desc(cum)))) +
  theme(legend.title = element_blank()) +
  labs(title = "Cumulative harvest (lb)", 
       subtitle = "Date: {frame_along}",
       x = "",
       y = "",
       color = "vegetable") +
  transition_reveal(date)

anim_save("graph3.gif", graph3)
```


```r
knitr::include_graphics("graph3.gif")
```

![](graph3.gif)<!-- -->

## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.


```r
library(ggimage)

mallorca_map <- get_stamenmap(
  bbox = c(left = 2.28, bottom = 39.41, right = 3.03, top = 39.8),
  maptype = "terrain",
  zoom = 11)

image_link <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"

mallorca_bike_day7 <- mallorca_bike_day7 %>%
  mutate(image = image_link)
```


```r
graph4 <- ggmap(mallorca_map) +
  geom_image(data = mallorca_bike_day7,
             aes(image = image,
                 x = lon,
                 y = lat),
             size = 0.1) +
  geom_path(data = mallorca_bike_day7,
            aes(color = ele)) +
  theme_map() +
  theme(legend.background = element_blank(),
        plot.title = element_text(face = "bold.italic")) +
  transition_reveal(time) +
  labs(title = "Bike ride path", 
       subtitle = "Time: {frame_along}")

anim_save("graph4.gif", graph4)
```


```r
knitr::include_graphics("graph4.gif")
```

![](graph4.gif)<!-- -->

> I prefer this to the static map, because it is more similiar to a reproduce of the actual bike ride process and provides more information about elevation and time. 

  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
image_run <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/runner.png"

image_swim <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/swimmer.jpg"

panama_all <- panama_bike %>% 
  bind_rows(panama_run) %>% 
  bind_rows(panama_swim) %>%
  mutate(image = ifelse(event == "Bike", image_link, ifelse(event == "Run", image_run, image_swim)))

panama_map <- get_stamenmap(
  bbox = c(left = -79.56, bottom = 8.91, right = -79.51, top = 8.99),
  maptype = "terrain",
  zoom = 14)
```


```r
graph5 <- ggmap(panama_map) +
  geom_image(data = panama_all,
             aes(image = image,
                 x = lon,
                 y = lat),
             size = 0.1) +
  geom_path(data = panama_all,
            aes(color = ele)) +
  theme_map() +
  theme(legend.background = element_blank(),
        plot.title = element_text(face = "bold.italic")) +
  transition_reveal(time) +
  labs(title = "Panama race path", 
       subtitle = "Time: {frame_along}")

anim_save("graph5.gif", graph5)
```


```r
knitr::include_graphics("graph5.gif")
```

![](graph5.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
graph6 <- covid19 %>% 
  group_by(state) %>% 
  mutate(new = cases - replace_na(lag(cases, 7, order_by = date), 0)) %>% 
  filter(cases >= 20) %>% 
  ggplot(aes(x = cases, y = new, group = state)) +
  geom_path(color = "grey") +
  geom_point() +
  geom_text(aes(label = state), check_overlap = TRUE) +
  scale_x_log10(labels = scales::comma) +
  scale_y_log10(labels = scales::comma) +
  transition_reveal(date) +
  labs(title = "Covid cases", 
       subtitle = "Date: {frame_along}")

animate(graph6, nframes = 200, duration = 30)

anim_save("graph6.gif", graph6)
```


```r
knitr::include_graphics("graph6.gif")
```

![](graph6.gif)<!-- -->
  
> Texas, Florida, California, and New York are the four states that have the most cumulative cases as well as the most new cases; while Virgin Islands, Vermont, and Northern Mariana Islands have the least cumulative cases and the least new cases. The number of new cases keeps fluctuating.
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
```


```r
graph7 <- covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(cases_per_10000 = cases / est_pop_2018 * 10000,
         wday = wday(date, label = TRUE)) %>% 
  filter(wday == "Fri") %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = str_to_lower(state),
               fill = cases_per_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_states(date, transition_length = 0) +
  labs(title = "cumulative number of COVID-19 cases per 10,000 people",
       subtitle = "Date: {closest_state}")

animate(graph7, nframes = 200, end_pause = 10)

anim_save("graph7.gif", graph7)
```


```r
knitr::include_graphics("graph7.gif")
```

![](graph7.gif)<!-- -->

> The cumulative number of cases increased much faster starting from fall 2020.

## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.

> [github](https://github.com/yzhong0620/STAT-112-Weekly-Exercises-5/blob/master/05_exercises.md)
