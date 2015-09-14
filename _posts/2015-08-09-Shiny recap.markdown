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
Output function, such as `plotOutput()`, used collaboratively with 'render*' functions

- server.r
`server <- function(input, output){}`; Save output to output variable via `ouput$variable`; Use render function to generate output; can access the input via `input$variable`

- reactive functions
used in server.r, within the functions, pure R script, reactive to users input

- render*()
used within server.r, generate output for elements specified in ui.r, such as `renderPlot`

- page layout functions 
functions that are used to construct the layout of a page, such as `fluidPage()`

- Interface builder functions
Include html, css, javascript within shiny, functions like 

~~~
HTML()
tag()
include()
~~~

- Shiny dashboard
A shiny flavor implementation of [AdminLTE](https://almsaeedstudio.com/) 


Details can be found from the shiny reference [website](http://shiny.rstudio.com/reference/shiny/latest/)
