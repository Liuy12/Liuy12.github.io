---
title: 'Shiny recap'
comments: true
layout: post
tags:
    - web design
    - shiny
---

- ui.r
Input function, such as `sliderInput()`
Output function, such as `plotOutput()`

- server.r
`server <- function(input, output){}`; Save output to output variable via `ouput$variable`; Use render function to generate output; can access the input via `input$variable`

- reactive functions
used in server.r, within the functions, pure R script

### render*()
