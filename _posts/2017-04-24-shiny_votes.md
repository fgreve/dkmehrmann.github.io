---
layout: post
title: "interactive Web App of the Chilean Senate Votes with Shiny (R Studio)"
author: "Fernando Greve"
date: "April 24, 2017"
categories: blog
---

[Shiny](https://shiny.rstudio.com/) is a R package that allows to make interactive web applications. Thus, I used it to show the Senate Votes in Chile. The novelty of this Web application is that gives the opportunity to have the votes of a particular senator for certain amount of time.    

This application is on the next link: [Shiny-App](https://fgreve.shinyapps.io/shinyapp/).




{% highlight r %}
#server.R
library("dplyr")
library("shiny")

load("VotosSenado.Rda")

function(input, output) {
  datasetInput <- reactive({
    switch(input$dataset,
           "TODOS" = head(select(filter(VotosSenado, grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),
           "Allamand Z. Andres"=head(select(filter(VotosSenado, Senador_name=="Allamand_Z_Andres", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Alvear V. Soledad"=head(select(filter(VotosSenado, Senador_name=="Alvear_V_Soledad", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Arancibia R. Jorge"=head(select(filter(VotosSenado, Senador_name=="Arancibia_R_Jorge", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Avila C. Nelson"=head(select(filter(VotosSenado, Senador_name=="Avila_C_Nelson", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Cantero O. Carlos"=head(select(filter(VotosSenado, Senador_name=="Cantero_O_Carlos", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, 
           
           ...

           "Ossandon I. Manuel Jose"=head(select(filter(VotosSenado, Senador_name=="Ossandon_I_Manuel_Jose", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Van Rysselberghe H. Jacqueline"=head(select(filter(VotosSenado, Senador_name=="Van_Rysselberghe_H_Jacqueline", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Quinteros L. Rabindranath"=head(select(filter(VotosSenado, Senador_name=="Quinteros_L_Rabindranath", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100),
           "Matta A. Manuel Antonio"=head(select(filter(VotosSenado, Senador_name=="Matta_A_Manuel_Antonio", grepl(input$txt, Tema, ignore.case=TRUE), fecha >= input$dateRange[1], fecha <= input$dateRange[2] ),Senador, Tema, Voto, fecha, Sesion, Legislatura),100))
  })
  
  output$table <- renderTable({
    datasetInput()
  })
  
  
}

{% endhighlight %}


{% highlight r %}

library("dplyr")
library("shiny")

fluidPage(
  titlePanel('Buscador Votacion Senado'),
  sidebarLayout(
    sidebarPanel(
      selectInput("dataset", "Senador:", 
                  choices = c("TODOS",
                              "Walker P. Ignacio", 
                              "Allamand Z. Andres",
                              "Alvear V. Soledad",
                              "Arancibia R. Jorge",
                              "Avila C. Nelson",
                              "Cantero O. Carlos",
                                
                                ...

                              "Ossandon I. Manuel Jose",
                              "Van Rysselberghe H. Jacqueline",
                              "Quinteros L. Rabindranath",
                              "Matta A. Manuel Antonio")),   
          
      dateRangeInput('dateRange',
                     label = 'Rango de fechas: yyyy-mm-dd',
                     start = Sys.Date() - 360, end = Sys.Date() 
      ),      
      
      
      #sliderInput("range", "Periodo del Proyecto de Ley:",
                  #min = 2000, max = 2017, value = c(2015,2017)),
      
      textInput("txt", "Buscar Proyecto por palabra clave:", ""),
      
      helpText("Nota: max de 100 votos x busqueda.",
               "Fuente: http://www.senado.cl/",
               "App desarrollada por:",
               "Fernando Greve",
               "fgreve@gmail.com",
               "https://fgreve.github.io/")
      
    ),
    mainPanel(
      tableOutput('table')
    )
  )
)

{% endhighlight %}