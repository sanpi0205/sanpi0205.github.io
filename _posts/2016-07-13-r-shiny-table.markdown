---
layout: post
title:  "R Shiny app for data table"
date:   2016-07-13 16:44:40
categories: R
---

Shiny集成web service, html, javascript, CSS 的快速数据产品原型开发工具，在需求不明确或
需求较为简单时，利用Shiny可以快速生成数据的可视化前端产品，同时可以将Shiny与其他工具结合，
开发功能和效果更强强大的产品。

本文以已离线数据文件为例，开发Shiny的前端展示报表。

{% highlight r %}
library(shiny)
library(DT)
library(dplyr)
library(reshape2)
library(scales)

mode = read.csv2('aaa.csv', header = F, sep='\t')
names(mode) = c('dt', 'mode', 'version', 'amount')

aa = mode %>% group_by(dt, mode) %>% summarise(sum(amount))
bb = mode %>% filter(version =='abc') %>% group_by(dt, mode) %>% summarise(sum(amount))
cc = left_join(aa, bb, by=c('dt'='dt','mode'='mode'))

cc[is.na(cc)] = 0
names(cc) = c('dt', 'mode', 'total', 'some')
cc = cc %>% mutate(updated_ratio = some / total)

dd = melt(cc, id.vars = c('dt','mode'), variable.name = 'indicator')
dd = dd %>% filter(dt>=20160706 & dt<=20160713 & mode %in% c('v1','v2','v3') )


final = dcast(dd, mode+indicator~dt, sum)
final[is.na(final)] = 0

head(final)
final_integer = comma(final[final$indicator %in% c('total', 'some'),])
final_percent = cbind(final[final$indicator == 'updated_ratio',1:2], apply(final[final$indicator == 'updated_ratio',3:10],2, function(x) percent(x) ))
final = rbind(final_integer, final_percent)
final = full_join(final[,1:2] %>% arrange(mode, indicator), final, by=c('mode'='mode','indicator'='indicator'))


# Define the overall UI
shinyUI = fluidPage(
    titlePanel("updated"),
    
    # Create a new Row in the UI for selectInputs
    fluidRow(
      column(4,
             selectInput("mode", "mode:", c("All", unique(as.character(final$mode))))
      ),
      column(4,
             selectInput("indicator", "indicator:", c("All", unique(as.character(final$indicator))))
      )
    ),
    # Create a new row for the table.
    fluidRow(
      DT::dataTableOutput("table")
    )
)

shinyServer = function(input, output) {
  
  # Filter data based on selections
  output$table <- DT::renderDataTable(DT::datatable({
    data <- final
    if (input$mode != "All") {
      data <- data[data$mode == input$mode,]
    }
    if (input$indicator != "All") {
      data <- data[data$indicator == input$indicator,]
    }
    data
  }))
  
}

shinyApp(ui = shinyUI, server = shinyServer)

{% endhighlight %}

在主程序完成之后，需要在 terminal 中运行程序，当然这里不是正在意义上的部署，
具体部署程序参见 [官网指南][shiny deploy]

`runApp(appDir = file, host="127.0.0.1", port=12345)`

在使用Shiny data table 时很重要的一点是数据的格式化，比如千分位用逗号分隔，已经百分数显示问题等，
这对用户体验还是非常重要的一个步骤，当然这也是比较耗费时间和精力的部分。因而使用`dplyr, reshape2, scales`
就非常必要了。



### 参考资料
[shiny deploy]:http://shiny.rstudio.com/deploy/

