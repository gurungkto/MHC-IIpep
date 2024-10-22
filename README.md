# MHC-IIpep
MHC associated peptide identification from non-pulsed THP-1 differentiated macrophages
#### **Description**
####THP-1 (DRB1*01:01/15:01) cells were differentiated into macrophages by stimulating with PMA (10 ng/ml) and IFN-gamma (200 activity units/ml). A total of 12 T-175 flasks were used to generate the macrophages from which approximately 0.3 grams cell pellet was obtained. The cell pellet was processed to extract cell membrane bound MHC-peptide complexes. The complexes were then ran through a tandem W632 and L243 affinity columns to purify MHC I and II complexes respectively. The eluted complexes were then acid boiled to release peptides from the MHC grooves. The peptides were then fractionated in RP-HPLC, dried up in speed vaccum, resuspended in 10% sequencing grade acetic acid spiked with iRT peptides, and ran on nanoLCMS (TTOF) system. The raw DDA data were denovo sequenced and peptides were identified using PEAKS software at 1% FDR. 
Before the cell pellet was processed, cells were tested for HLA-DRB1 expression by flow cytometry.

---
#### **MHC II expression increases following PMA and interferon gamma stimulation**
```{r, echo=FALSE, out.width="70%", fig.align="center"}
knitr::include_graphics(path = "C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/flow/drb1 flow.jpg")

```

\pagebreak

#### **A table of some *MHC I* peptides and including a Biognosys iRT peptide is shown below.**
```{r echo=TRUE, message=FALSE, warning=FALSE, out.width="100%", fig.align="center", strip.white=TRUE}

## For Class I
library(DT)
setwd("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g W632")
data.nopulse.class1 <- read.csv("peptide.csv", header = T)
datatable((data.nopulse.class1[ c(11:14), c(1:7,13:15)]), filter = "top")
```
**A total of `r length(data.nopulse.class1$Peptide)` class I peptides including 11 iRT peptides were detected of which most of them were 9-mers.**

```{r echo=TRUE, message=FALSE, warning=FALSE, out.width = "80%", fig.align="center"}
table.class1 <- table(data.nopulse.class1$Length)
dat.class1 <- data.frame(table.class1)

library(ggplot2)
library(plotly)
ggplot(dat.class1, aes(dat.class1$Var1, dat.class1$Freq, group = 1)) + geom_point() + 
  geom_line(color = "red") + 
  labs(x = "Peptide length" , y = "Frequency") +
  theme(axis.line = element_line(size = 1),plot.title = element_text(hjust = 0.5)) +
  ggtitle("Frequency of #mer THP-1 Mac MHC I peptides")
```

\pagebreak


#### HLA-A02:01 motif
```{r, echo=FALSE, out.width="60%", fig.align="center"}
knitr::include_graphics(path="C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g W632/HLA-A0201.png") 

```

#### THP-1 W632 motif
```{r, echo=FALSE, out.width="60%", fig.align="center"}
knitr::include_graphics("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g W632/THP-1 A0201.png")

```

\pagebreak

#### **A table of some *MHC II* peptides is shown below.**
```{r echo=TRUE, message=FALSE, warning=FALSE, fig.align="center", strip.white=TRUE}
## For class II
library(DT)
setwd("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g L243")
data.nopulse.class2 <- read.csv("peptide.csv", header = T)
datatable((data.nopulse.class2[c(10:14) , c(1:7,13:15)]), filter = "top")
```

**A total of `r length(data.nopulse.class2$Peptide)` class II peptides including 11 iRT peptides were detected of which most of them were 15-mers.**

```{r echo=TRUE, message=FALSE, warning=FALSE, fig.align="center"}
table.class2 <- table(data.nopulse.class2$Length)
dat.class2 <- data.frame(table.class2)

ggplot(dat.class2, aes(dat.class2$Var1, dat.class2$Freq, group = 1)) + geom_point() + 
  geom_line(color = "red") + 
  labs(x = "Peptide length" , y = "Frequency") +
  theme(axis.line = element_line(size = 1),plot.title = element_text(hjust = 0.5)) +
  ggtitle("Frequency of #mer THP-1 Mac MHC II peptides")
```

\pagebreak

#### HLA-DRB1*01:01 motif
```{r, echo=FALSE, out.width="60%", fig.align="center"}
knitr::include_graphics("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g L243/DRB1-0101 motif.png") 
                      
```

#### THP-1 L243 motif
```{r, echo=FALSE, out.width="60%", fig.align="center"}
knitr::include_graphics("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g L243/DRB1-0101 THP-1 L243 motif from 15mer.png")

```

#### Peptide sampling comparing the class I and class II derived peptides looked interesting. 
For class I a protein was sampled one time for about 73% of the proteome whereas for class II a protein was sampled one time for only about 37% of the proteome (50% less than class I).
```{r, echo=TRUE}
setwd("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g W632")
library(ggplot2)

class.one <- read.csv("proteins.csv", header = T)

class.one <- class.one[!grepl("Biognosys", class.one$Accession), ]
par(mfrow= c(1,1))

df1 <- data.frame(table(class.one$X.Peptides))
df1$percent <- (df1$Freq/sum(df1$Freq)) * 100
df1 <- df1[!df1$Var1 == 0, ]
ggplot(df1, aes(Var1, percent)) + geom_bar(stat = "identity") + 
  labs(x = "Number of peptides per protein" , y = "Percent total") +
  theme(axis.line = element_line(size = 1),plot.title = element_text(hjust = 0.5)) +
  ggtitle("Sampling frequency of THP-1 Mac MHC I peptides") +
  scale_y_continuous(expand = c(0, 0))

setwd("C:/Users/HGURUNG1/Desktop/HG/Projects/Cunningham and Clay/THP-1 mac No pulse 0.3g L243")
class.two <- read.csv("proteins.csv", header = T)

class.two <- class.two[!grepl("Biognosys", class.two$Accession), ]
par(mfrow= c(1,1))

df2 <- data.frame(table(class.two$X.Peptides))
df2$percent <- (df2$Freq/sum(df2$Freq)) * 100
ggplot(df2, aes(Var1, percent)) + geom_bar(stat = "identity")+
  labs(x = "Number of peptides per protein" , y = "Percent total") +
  theme(axis.line = element_line(size = 1),plot.title = element_text(hjust = 0.5)) +
  ggtitle("Sampling frequency of THP-1 Mac MHC II peptides") +
  scale_y_continuous(expand = c(0, 0))

```
