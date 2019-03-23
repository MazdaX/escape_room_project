# escape_room_project
How to choose the best escape room in Edinburgh!

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
require(tidyverse)
require(DT)
require(FactoMineR)
require(factoextra)
```

# Parsing the metadata 

The metadat was collected from escape rooms reviews on TripAdvisor as following:

 - Venues with more than 100, 5 Star reviews were filtered
 - Details of the game (min/max players) and difficulty/interesting scored was collected
 - Restuarants, bars and pubs within 15 min walking distance of the Venues were filtered for min 4.5 stars on google maps
 - Distance from work measured in miles via driving

# The dataframe

```{r,cache=TRUE}
erd<-read.csv(file = "/mnt/e/escape_room_edinburgh.csv",header = TRUE,stringsAsFactors = FALSE)
erd$Venue<-as.factor(erd$Venue)
erd$Game<-as.factor(erd$Game)
erd$URL<-paste0("<a href='",erd$URL,"'>",erd$URL,"</a>")
datatable(data = erd %>% arrange(desc(revs_tripadvisor)),
          escape = FALSE,
          style = "bootstrap",
          options = list( autoWidth = TRUE , 
                           pageLength = 10,
                           searchHighlight = TRUE,
                           scrollX = TRUE))
```


# Priciple component analysis:


```{r, fig.align="center",cache=TRUE}
#PCA prep
erd_scaled<-scale(x = erd[-c(1:2,10)],center = TRUE,scale = TRUE)
erd_ready<-cbind.data.frame(erd[1:2],erd_scaled)
erd_pca<-PCA(X = erd_ready,ncp = 3,quali.sup = c(1,2), graph = FALSE)
fviz_pca(erd_pca)
fviz_pca_ind(erd_pca,habillage = "Venue",repel = TRUE,palette = "lancet")
```
