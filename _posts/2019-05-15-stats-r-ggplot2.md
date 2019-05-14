---
layout: post
title: "stats R ggplot" #post의 제목
description:         
headline: 
modified: 2019-05-15           #post의 날짜
category: webdevelopment       #post의 카테고리
tags: [jekyll]                 #post의 태그
imagefeature: cover10.jpg      #post의 앞에 나오는 사진
mathjax: 
chart: 
comments: true                 #post의 댓글 사용 여부
share: true                    #post의 댓글 공유가능 여부
featured: true                 #post가 중요한 글인지 여부
---
ggplot2

### what is ggplot?
1. ggplot is an R package for data visulaization. 
2. It utilizes a scheme for construction of plots called the Grammar of Graphics due to Leland Wilinson.
3. It provides a systematic approach to construction of graphs
4. It can be used instead of traditional or base graphics in R but provides additional funtionality.
5. It use the grid graphics engine and can create plots conditional on one or more variables in the same manner as lattice.


### Resourse
* Hadley Wickham, ggplot2: Elegant Graphics for Data Analysis, Springer, 2009
* Paul Murrell, R Graphic, Second Edition, CRC Press, 2011
* R Cookbook, Graphs by Winston Chang: http://www.cookbook-r.com/Graphs/


### BASICS
__ggplot()__  :define the data using basic funtion to plot, `create empty plot object`.  
__geoms___    :`represent data` and add `plot type` after it, e.g) geom_point(), geom_line()  
              
   >list of geom : http://ggplot2.tidyverse.org/reference/#section-layer-geoms
               
   >*color,
   >*size : size of the shape for points,
          height for text,
          width for lines (units of millimetres)
   >*group aesthetics
            
            
__aes()__     :aesthetics of the data values (x-, y- location of data)
           
           *aes() function can be included as an argument to ggplot()
                  inclusion of aes() in ggplot() is useful when there are multiple geoms
           e.g.
           ggplot(Orange) + geom_line(aes(age, circumference, colour = Tree)) +
           geom_point(aes(age, circumference, colour = Tree))
           -->>>>
           ggplot(Orange, aes(age, circumference, colour=Tree)) + geom_line() + geom_point()
           
           *when an aes is added to a geom, that overrides the aes defined in the ggplot() 

__scales__    :to change axix components
           
           *discrete, continuous,date, date time, log, reverse scales
           
           *continuous(arguments:limits,labels,breaks,na.value)
                      breakes affect the background grid lines, since these lines are linked to tick marks.
                      e.g.  +scale_y_continous(name = "Number of Students")
      
           legend : -automatically created and determinded by the scale
                    -name: control legend title
                    -labels: control legend item labels
                    -legend.position
                    -legend.justification
                    
__stats__     : a stat takes a dataset and produces a new dataset so can add new variables to an existing dataset.
            
            one example of this is the stat_bin which bin data, as is necessary for construction of a histogram
            stat_bin produces three new variables `count`, `density` and `x`(the centre of the bins)
            
            *Note that the ordinary histogram command also produces these values but invisibly.
            These new values can be used, but may need to be `surrounded with ..` to ensure the correct variable is chosen.
            e.g.
            ggplot(course.df) + 
            geom_histogram(aes(x=Exam, y= __..density..__), binwidth = 5)
            e.g.
            ggplot(course.df) +
            stat_smooth(aes(x = Test, y = Exam), method = "loess")

__group__     : 
            
            *ggplot(course.df, aes( x = Test, y = Exam)) + geom_point() + 
            stat_smooth(aes(group = Gender, color = Gender), method ="lm")
            

### using dataset

__Fisher's Iris Data__    

str(iris) 

    'data.frame': 150 obs. of  5 variables:  
    $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...  
    $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...  
    $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...  
    $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...  
    $ Species     : Factor w/ 3 levels "setosa","versicolor",..:  

head(iris)  

    Sepal.Length Sepal.Width Petal.Length Petal.Width     Species  
        1          5.1         3.5          1.4     0.2   setosa  
        2          4.9         3.0          1.4     0.2   setosa  
        3          4.7         3.2          1.3     0.2   setosa  
        4          4.6         3.1          1.5     0.2   setosa  
        5          5.0         3.6          1.4     0.2   setosa  
        6          5.4         3.9          1.7     0.4   setosa  
    
    
library(ggplot2)

#use iris data in R   
#use variable Sepal.Length, Petal.Length  
  
    ggplot(iris, aes(Sepal.Length, Petal.Length)) + geom_point()  
    ggplot(iris, aes(Sepal.Length, Petal.Length, colour = Species)) + geom_point()  



----------

str(Orange)  

    Classes 'nfnGroupedData', 'nfGroupedData', 'groupedData' and 'data.frame': 35 obs. of 3 variables:  
    $ Tree : Ord.factor w/ 5 levels "3"<"1"<"5"<"2"<..: 2 2 2 2 2 2 2 4 4 4 ...  
    $ age : num 118 484 664 1004 1231 ...  
    $ circumference: num 30 58 87 115 120 142 145 33 69 111 ...  
    - attr(*, "formula")=Class 'formula' language circumference ~ age | Tree  
    .. ..- attr(*, ".Environment")=<environment: R_EmptyEnv>  
    - attr(*, "labels")=List of 2  
    ..$ x: chr "Time since December 31, 1968"         
    ..$ y: chr "Trunk circumference"        
    - attr(*, "units")=List of 2  
    ..$ x: chr "(days)" 
    ..$ y: chr "(mm)"   

head(Orange)  

    Tree age circumference
    1 1   118           30
    2 1   484           58
    3 1   664           87
    4 1   1004         115
    5 1   1231         120
    6 1   1372         142


    ggplot(Orange, aes(age, circumference, color=Tree) + geom_line() + facet_grid(.~Tree)  
    +scale_x_continuous(breaks=c(800,1600)) + theme_bw()  



------------

library(s20x)

    data(course.df)
    p <- ggplot(course.df)
    p <- p + geom_point(aes(x=Test, y=Exam))
    print(p)

    ggplot(course.df) + geom_point(aes(x = Test, y = Exam, shape = Gender),size =2)

--> shows difference between 'setting' an aes and 'mapping' an aes
    Gender variable was mapped to the shape aes, 
    but the size aes was set to the constant value 2, which doubled the size of everything (text, plotting symbols)

__Distribution : geom_hist(), geom_density(), geom_boxplot()__
    
    ggplot(course.df) + geom_histogram(aes(Exam),binwidth = 5)

--------
__Exercise__

#adjust : make less smooth density (1:normal, 1/2, 1/6.. )

    ggplot(mtcars) + geom_density(aes(mpg),adjust=1/2, fill="pink")

#create a boxplot of the variable mpg using ggplot2
#create a variable in mtcars dataset which has a single value for all observations

    mtcars$all <- "all" 

#coord_flip : make boxplot horizontal

    ggplot(mtcars) + geom_boxplot(aes(all,mpg),coord_flip())



