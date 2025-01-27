SHUFFLE Survey Data V2
================


# set working directory and print option

# Packages & data

## Frage 1 Welche der unten aufgeführten individuellen Umstände wirken sich erschwerend auf Ihr Studium aus?

``` r
v <- c(157, 28, 20, 287, 189, 24, 60, 197, 12, 48, 170, 48)
w <- c('familiäre Verpflichtungen','Pflegeverantwortung', 'Deutschen Sprache', 'Studium', 'Job', 'Krankheit', 'Chronisch-körperliche Erkrankung', 'Psychische Erkrankung', 'Teilleistungsstörung', 'Neurodiversität', 'Keine', 'Sonstiges')

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Barriere','Anzahl')
```

``` r
# updated from sonstiges
v <- c(158, 28, 20, 287, 189, 24, 63, 201, 12, 49)
w <- c('Familiäre Verpflichtungen','Pflegeverantwortung', 'Sprachschwierigkeiten (deutsch)', 'Schwierigkeiten mit dem Studium', 'Vereinbarkeit Job/Studium', 'Länger andauernde Krankheit', 'Chronisch-körperliche Erkrankung/Behinderung', 'Psychische Erkrankung', 'Teilleistungsstörung', 'Neurodiversität')

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Barriere','Anzahl')

nb.cols <- 10
cs1 <- c("#325494", "#5494cc", "#4a7432", "#05a404", "#e49c04", "#a45404", "#dc0404", "#543aa3")
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

p <- ggplot(df, aes(x= reorder(Barriere, Anzahl), y=Anzahl, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 350), breaks = seq(0, 350, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  coord_flip() +
  ggtitle ('Zusammenfassung der individuellen Umstände  \n \n') +
  xlab('\n\nUmstände') + ylab('\nAnzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold", size=8, color="black")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm"))
  #ggplot2::ggsave(filename = 'frage_1.png',path = "figures2/Frage_1/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> \###
Frage 1 Sonstiges

``` r
# add data
df <- data.frame(
  group = c("Private Lebensumstände", "Folgen durch Corona", 'Folgen der Online-Lehre', 'Studienorganisation', 'andere'),
  value = c(26, 9, 2, 2, 2)
  )

# Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Individuellen Umstände (Sonstiges) \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=2) +
  scale_fill_manual(values=c("#385095", "#5494cc", "#4a7432", "#05a404","#a45404", "#dc0404")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_1_sonstiges_pie2.png',path = "figures2/Frage_1")
pie
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Frage 1 Welche der unten aufgeführten individuellen Umstände wirken sich erschwerend auf Ihr Studium aus? Percentages

``` r
df <- data.frame(
   Barriere = c("Private Lebensumstände", "Folgen durch Corona", 'Folgen der Online-Lehre', 'Studienorganisation', 'andere'),
  Anzahl = c(26, 9, 2, 2, 2)
  )

df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)

df
```

    ##                  Barriere Anzahl perc
    ## 1  Private Lebensumstände     26   63
    ## 2     Folgen durch Corona      9   22
    ## 3 Folgen der Online-Lehre      2    5
    ## 4     Studienorganisation      2    5
    ## 5                  andere      2    5

``` r
cs2 <- c("#7433a2", "#e49c04", "#a45404", "#5494cc", "#05a404" )

title <- "Individuellen Umstände (Sonstiges) \n "

p <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 70), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", hjust =
    ## -0.3, : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage1_sonstiges2_prozent.png', path = "figures2/Frage_1/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### Chart yes vs no Umstande

``` r
# create data
df <- data.frame(
  group = c("Ja", "Nein"),
  value = c(525, 170)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Zusammenfassung der individuellen Umstände  \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=5) +
  scale_fill_manual(values=c("#385095", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  ggplot2::ggsave(filename = 'frage_1_ ja_nein2.png',path = "figures2/")
```

    ## Saving 7 x 5 in image

``` r
pie
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

### Chart 2: Summary by Number of Identified Circumstances

``` r
k <- c(214, 177, 83, 40, 7, 2, 2)
l <- c('1','2','3','4','5','6','7')
df2 <- data.frame(l,k)
colnames(df2) <- c('Umstände','Anzahl')

cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#4a7432", "#05a404" )

df2
```

    ##   Umstände Anzahl
    ## 1        1    214
    ## 2        2    177
    ## 3        3     83
    ## 4        4     40
    ## 5        5      7
    ## 6        6      2
    ## 7        7      2

``` r
p2 <- ggplot(df2, aes(x= factor(Umstände), y=Anzahl, fill=Umstände)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 300), breaks = seq(0, 400, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der individuellen Umstände  \n \n') +
  xlab('\n\nAnzahl der Umstände') + ylab('\nAnzahl der Studierenden') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

  #ggplot2::ggsave(filename = 'Frage_1_Anzahl_Umstände2.png',path = "figures2/")

p2
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

### FRAGE 1.2

### Welche chronisch-körperliche Erkrankung, Beeinträchtigung oder Behinderung haben Sie? FREQUENCY

``` r
# 3 added to chron from sonstiges
v <- c(9, 6, 12, 3, 31, 2)
w <- c('Motorische Beeinträchtigung','Sehbeeinträchtigung', 'Hörbeeinträchtigung', 'Sprach-/Sprechbeeinträchtigung', 'Chronische körperliche Erkrankungen', 'Keine Angabe')

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Barriere','Anzahl')

cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#4a7432", "#05a404" )

title <- "Studierende mit Chronisch-körperliche Erkrankung,\n Beeinträchtigung  oder Behinderung \n "

p <- ggplot(df, aes(x= reorder(Barriere,Anzahl), y=Anzahl, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 50), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=Anzahl), face="bold", hjust= -0.3) +
  ggtitle (title) +
  xlab('Barrieren') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = Anzahl), face = "bold", hjust = -0.3):
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage1_2_zahl2.png',path = "figures2/Frage_1/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- --> \###
FRAGE 1.2 \### Welche chronisch-körperliche Erkrankung, Beeinträchtigung
oder Behinderung haben Sie? PERCENTAGES

``` r
# 3 added to chron from sonstiges
v <- c(9, 6, 12, 3, 31, 2)
w <- c('Motorische Beeinträchtigung','Sehbeeinträchtigung', 'Hörbeeinträchtigung', 'Sprach-/Sprechbeeinträchtigung', 'Chronische körperliche Erkrankungen', 'Keine Angabe')

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Barriere','Anzahl')

df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#05a404" )

df
```

    ##                              Barriere Anzahl perc
    ## 1         Motorische Beeinträchtigung      9   14
    ## 2                 Sehbeeinträchtigung      6   10
    ## 3                 Hörbeeinträchtigung     12   19
    ## 4      Sprach-/Sprechbeeinträchtigung      3    5
    ## 5 Chronische körperliche Erkrankungen     31   49
    ## 6                        Keine Angabe      2    3

``` r
title <- "Studierende mit Chronisch-körperliche Erkrankung,\n Beeinträchtigung  oder Behinderung \n "

p <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 60), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  #geom_text(aes(label=scales::percent(perc)),fontface="bold",size=3,  hjust= -0.2) +
  ggtitle (title) +
  xlab('Barrieren') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank")) 
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage1_2_prozent2.png', path = "figures2/Frage_1/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

### FRAGE 2

## cs2

``` r
data <- read_csv("~/survey/frage_2_4levels.csv")
```

    ## Rows: 28 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): 1, 2
    ## dbl (1): 3
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
colnames(data) <-c("Type", "Rating", "Number")
data$Rating <- factor(data$Rating, levels=c('(Gar)Nicht Gut', 'Nicht beantworten', 'Teilweise', '(Sehr)Gut'))


p3 <- ggplot(data = data, 
       aes(x = Type, y = Number, fill = Rating)) + 
  theme_classic() +
  coord_flip() +
  theme(legend.title = element_blank()) +
  theme(plot.title = element_text(size=14, face = "bold", vjust = 2, hjust=0.5)) +
  theme(legend.position = "bottom") +
  theme(axis.text.y = element_text(size=10)) +
  theme(axis.text.x = element_text(size=10)) +
  geom_bar(position="fill",stat = "identity") + 
  xlab("Aspekte") +
  ylab("Prozentsatz der Studierenden") +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(t = 20))) +
  theme(axis.text.y = element_text(size=10, face="bold")) +
  theme(axis.text.x = element_text(size=10, face="bold")) +
  # red, blue, yellow, green
  scale_fill_discrete(guide=guide_legend(reverse=T), type = c("#dc0404", "#385095" ,"#e49c04","#4a7432")) +
  ggtitle("Inwieweit kommen Sie mit den \n folgenden Aspekten zurecht? \n ") 
  

  #ggplot2::ggsave(filename = 'frage2_barchart2.png',path = "figures2/Frage_2/")

p3
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
\### FRAGE 3 Nehmen die von Ihnen wahrgenommenen Barrieren in digitalen
Formaten (z. B. Lehrveranstaltungen, Prüfungen, Organisation) eher zu
oder ab? Do the barriers you perceive in digital formats (e.g., courses,
exams, organization) tend to increase or decrease?

``` r
v <- c(199, 199, 193, 104)
w <- c('Sie nehmen eher zu ','Sie nehmen eher ab', 'Weder noch', 'Kann ich nicht benennen')

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Antwort','Anzahl')

cs2 <- c("#7433a2", "#e49c04", "#a45404", "#5494cc")

title <- "Barrieren in digitalen Formaten \n \n"
p <- ggplot(df, aes(x= reorder(Antwort,Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 200), expand = c(0,.5)) +
  scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 16)) +
  geom_bar(stat='identity', show.legend = FALSE, width = 0.7) +
  ggtitle (title) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(plot.title = element_text(size=16, face = "bold",vjust = 2, hjust = 0.7)) +
  theme(axis.line.y = element_line(color ="black", size = .5, linetype="solid")) +
  theme(axis.title.x = element_text(color="black", size=10, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=10, face="bold")) +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning: The `size` argument of `element_line()` is deprecated as of ggplot2 3.4.0.
    ## ℹ Please use the `linewidth` argument instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
#ggplot2::ggsave(filename = 'frage3_barchart2.png',path = "figures2/Frage_3/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
\### FRAGE 4: In welchen der nachfolgenden Bereiche stoßen Sie auf
Barrieren bei der Nutzung von Lernmaterialien? \### Frequency \# add
values from sonstiges

``` r
v <- c(73, 86, 211, 203,212, 34, 125, 201, 37, 12, 86, 257, 86)
w <- c('Visuelle Wahrnehmbarkeit','Auditive Wahrnehmbarkeit', 'Reizüberflutung', 'Technische Probleme', 'Verständlichkeit', 'Sprachverständnis', 'Nutzbarkeit', 'Zeitliche Verzögerung', 'Kompatibilitätsprobleme (assistive Technologien)', 'Unzureichende Tastaturbedienbarkeit', 'Unzureichende Unterstützung', 'Unzureichende Bearbeitungszeit', 'Aufwand für Barrierefreiheit')

df <- data.frame(w,v)
colnames(df) <- c('Antwort','Anzahl')

nb.cols <- 14
mycolors <- colorRampPalette(cs2)(nb.cols)

p4 <-ggplot(df, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 300), breaks = seq(0, 300, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ('In welchen der nachfolgenden Bereiche \n stoßen Sie auf Barrieren bei der Nutzung \n von Lernmaterialien? \n') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold"))

  #ggplot2::ggsave(filename = 'frage4_barchart2.png',path = "figures2/Frage_4/")

p4
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
\### Frage 4 Sonstiges pie

``` r
df <- data.frame(
  group = c("Unübersichtlichkeit", "Fehlende Lernmaterialien", 'Schlechte/fehlerhafte Kommunikation', 'Auswirkungen auf den Gesundheitszustand', 'Fehlende/teure/schwer zugängliche Software', 'Nicht zutreffend'),
  value = c(6, 6, 5, 5, 3, 9)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#05a404" )
# basic piechart
pie <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Nutzung von Lernmaterialien (Sonstiges) \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=3) +
  scale_fill_manual(values=cs2) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_4_sonstiges_pie.png',path = "figures2/Frage_4")
pie
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
\### Frage 4 Sonstiges bar

``` r
df <- data.frame(
  Barriere = c("Unübersichtlichkeit", "Fehlende Lernmaterialien", 'Schlechte/fehlerhafte Kommunikation', 'Auswirkungen auf den Gesundheitszustand', 'Fehlende/teure/schwer zugängliche Software', 'Nicht zutreffend'),
  Anzahl = c(6, 6, 5, 5, 3, 9)
  )

  # Compute the position of labels
#perc <- df$Anzahl/sum(df$Anzahl)
df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#05a404" )

title <- "Nutzung von Lernmaterialien (Sonstiges) \n "

p <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 40), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=13, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=13, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage4_sonstiges_prozent.png', path = "figures2/Frage_4/")
p
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

#### Frage 4 Ja-Nein

``` r
df <- data.frame(
  group = c("Ja", "Nein"),
  value = c(574, 121)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Barrieren bei der Nutzung von Lernmaterial  \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#4a7432", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_4_ ja_nein.png',path = "figures2/Frage_4/")
pie
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

### FRAGE 4 create chart with number of barriers

``` r
k <- c(251, 154, 126,71, 54,  22, 7, 5,1,1 )
l <- c('1','2','3','4','5','6','7', '8', '9', '10')
df2 <- data.frame(l,k)
colnames(df2) <- c('Bereiche','Anzahl')
df2$Bereiche <- factor(df2$Bereiche,levels = c('1','2','3','4','5','6','7', '8', '9', '10'))

nb.cols <- 10
cs2 <- c("#e49c04", "#a45404", "#dc0404","#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2")
mycolors <- colorRampPalette(cs2)(nb.cols)

p4 <- ggplot(df2, aes(x= factor(Bereiche), y=Anzahl, fill=Bereiche)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 300), breaks = seq(0, 300, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der identifizierte Bereiche  \n \n') +
  xlab('\n\nAnzahl der Bereiche') + ylab('\nAnzahl der Studierenden') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

#ggplot2::ggsave(filename = 'frage4_anzahl_bereiche.png',path = "figures2/Frage_4/")

p4
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
\### FRAGE 5

``` r
# add 2 from sonstiges
v <- c(427, 193, 220, 39, 15, 3, 2, 41, 254)
w <- c('Vortrags-/Seminaraufzeichnungen','Barrierefreie Folien', 'Transkripte', 'Untertitel (deutsch)', 'Untertitel (englisch)', 'Gebärdensprache','Lautsprachbegleitende Gebärden', 'Audiodeskription', 'Lernvideos')

nb.cols <- 9
mycolors <- colorRampPalette(cs2)(nb.cols)

df <- data.frame(w,v)
# change col names
colnames(df) <- c('Antwort','Anzahl')
df
```

    ##                           Antwort Anzahl
    ## 1 Vortrags-/Seminaraufzeichnungen    427
    ## 2            Barrierefreie Folien    193
    ## 3                     Transkripte    220
    ## 4            Untertitel (deutsch)     39
    ## 5           Untertitel (englisch)     15
    ## 6                 Gebärdensprache      3
    ## 7  Lautsprachbegleitende Gebärden      2
    ## 8                Audiodeskription     41
    ## 9                      Lernvideos    254

``` r
p5 <-ggplot(df, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 450), breaks = seq(0, 450, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ('Haben Sie bei der Nutzung von Lernmaterialien \n  einen Bedarf an...? \n') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=10, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=10, face="bold"))

  #ggplot2::ggsave(filename = 'frage5_barchart.png',path = "figures2/Frage_5/")

p5
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
\### Frage 5 sonstiges pie

``` r
df <- data.frame(
  group = c("Komprimierte Inhalte zur Vorlesung", "Austausch", 'Übersichtliche Materialien', 'Mehr Materialien', 'Nicht zutreffend'),
  value = c(4, 3, 2, 1, 4)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#05a404" )
# basic piechart
pie5 <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Nutzung von Lernmaterialien (Sonstiges) \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=3) +
  scale_fill_manual(values=cs2) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_5_sonstiges_pie.png',path = "figures2/Frage_5")
pie5
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

### Frage 5 sonstiges bar

``` r
df <- data.frame(
  Barriere = c("Komprimierte Inhalte zur Vorlesung", "Austausch", 'Übersichtliche Materialien', 'Mehr Materialien', 'Nicht zutreffend'),
  Anzahl= c(4, 3, 2, 1, 4)
  )

  # Compute the position of labels
df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
cs2 <- c("#7433a2", "#e49c04", "#a45404","#385095", "#5494cc", "#05a404" )

title <- "Nutzung von Lernmaterialien (Sonstiges) \n "

p5 <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 40), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=13, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=13, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage5_sonstiges_prozent.png', path = "figures2/Frage_5/")
p5
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

### Frage 5 Ja-Nein

no= 158, yes = 537

``` r
df <- data.frame(
  group = c("Ja", "Nein"),
  value = c(537, 158)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie5 <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Barrieren bei der Nutzung von Lernmaterial  \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#4a7432", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_5_ ja_nein.png',path = "figures2/Frage_5/")
pie5
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
\### Frage 5 Combos

``` r
k <- c(312,186,119,57,12,2,1 )
l <- c('1','2','3','4','5','6','9')

df5 <- data.frame(l,k)
colnames(df5) <- c('Bereiche','Anzahl')
df5$Bereiche <- factor(df5$Bereiche,levels = c('1','2','3','4','5','6','9'))

nb.cols <- 7
cs2 <- c("#e49c04", "#a45404", "#dc0404","#385095", "#5494cc", "#4a7432", "#05a404")
mycolors <- colorRampPalette(cs2)(nb.cols)

p5 <- ggplot(df5, aes(x= factor(Bereiche), y=Anzahl, fill=Bereiche)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 350), breaks = seq(0, 350, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der identifizierten Bedürfnisse: \n Lernmaterialien \n') +
  xlab('Bedürfnisse') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

#ggplot2::ggsave(filename = 'frage5_anzahl_Bedürfnisse.png',path = "figures2/Frage_5/")

p5
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

### FRAGE 6.2

barchart \# added from sonstiges

``` r
v <- c(7, 185, 38, 11, 6, 89, 12, 4, 15, 31)
w <- c('Nicht zugänglich','Navigationsschwierigkeiten', 'Visuelle Wahrnehmbarkeit', 'Auditive Wahrnehmbarkeit', 'Sprachbarrieren', 'Reizüberflutung ', 'Kompatibilitätsprobleme (assistive Technologien)', 'Unzureichende Tastaturbedienbarkeit', 'Unzureichende Hardware', 'Unzureichende Unterstützung')

nb.cols <- 10
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

df6 <- data.frame(w,v)
# change col names
colnames(df6) <- c('Antwort','Anzahl')
df
```

    ##   group value     prop     ypos
    ## 1  Nein   158 22.73381 11.36691
    ## 2    Ja   537 77.26619 61.36691

``` r
p6 <-ggplot(df6, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 200), breaks = seq(0, 200, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ('Auf welche Barrieren stoßen Sie \n bei der Nutzung von Lernplattformen? \n') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(t = 0, r = 20, b = 0, l = 0)))

  #ggplot2::ggsave(filename = 'frage6_2_barchart.png',path = "figures2/Frage_6/")

p6
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

### FRAGE 6.2 sonstiges pie

``` r
df <- data.frame(
  group = c("Unübersichtlichkeit", 'Uneinheitlichkeit der Nutzung/Strukur', 'Neues ILIAS schlechter als zurvor', 'Zu viele verschiedene LMS', 'Schwieriger Zugang zu Inhalten', 'Schwieriger Austausch über LMS', 'andere', 'Nicht zutreffend'),
  value = c(12, 6, 4, 4, 3,3, 8, 2)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
# basic piechart
pie6 <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Lernmaterialien (Sonstiges) \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=2.5) +
  scale_fill_manual(values=cs2) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_6_sonstiges_pie.png',path = "figures2/Frage_6")
pie6
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

### FRAGE 6.2 sonstiges bar

``` r
df <- data.frame(
  Barriere = c("Unübersichtlichkeit", 'Uneinheitlichkeit der Nutzung/Strukur', 'Neues ILIAS schlechter als zurvor', 'Zu viele verschiedene LMS', 'Schwieriger Zugang zu Inhalten', 'Schwieriger Austausch über LMS', 'andere', 'Nicht zutreffend'),
  Anzahl = c(12, 6, 4, 4, 3,3, 8, 2)
  )

  # Compute the position of labels
df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")

title <- "Lernmaterialien (Sonstiges) \n "

p6 <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 40), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold")) +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage6_sonstiges_prozent.png', path = "figures2/Frage_6/")
p6
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

### FRAGE 6.2

piechart

278 yes, 350 no, 67 no answer = 695

``` r
df6 <- data.frame(
  group = c("Ja", "Nein"),
  value = c(278, 350)
  )

  # Compute the position of labels
df6 <- df6 %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df6$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie6 <- ggplot(df6, aes(x="", y=value, fill=group)) +
  ggtitle ('Barrieren bei der Nutzung von Lernplattformen  \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#4a7432", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_6_2_ja_nein.png',path = "figures2/Frage_6/")
pie6
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->
\### FRAGE 6.2 combos

``` r
k <- c(512,71,25,3,1)
l <- c('1','2','3','4','5')
df6 <- data.frame(l,k)
colnames(df6) <- c('Barrieren','Anzahl')

df6$Barrieren <- factor(df6$Barrieren,levels = c('1','2','3','4','5'))

nb.cols <- 5
cs2 <- c("#e49c04", "#a45404", "#dc0404","#385095", "#05a404")
mycolors <- colorRampPalette(cs2)(nb.cols)

p6 <- ggplot(df6, aes(x= factor(Barrieren), y=Anzahl, fill=Barrieren)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 550), breaks = seq(0, 550, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der identifizierten Barrieren: \n Lernplattformen \n') +
  xlab('Barrieren') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

  #ggplot2::ggsave(filename = 'frage6_2_anzahl_Barrieren.png',path = "figures2/Frage_6/")

p6
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
\### Frage 7

yes-no E-Prüfungen

``` r
df7 <- data.frame(
  group = c("Ja", "Nein"),
  value = c(387, 308)
  )

  # Compute the position of labels
df7 <- df7 %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df7$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie7 <- ggplot(df7, aes(x="", y=value, fill=group)) +
  ggtitle ('Haben Sie bereits an einer E-Prüfung \n (auf einer Lernplattform) teilgenommen? \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#385095", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_7_ja_nein.png',path = "figures2/Frage_7/")
pie7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->
\### 7 sonstiges pie

``` r
df <- data.frame(
  group = c("Internetprobleme", 'Psychische Belastung', 'Technische Probleme', 'Fehlende Navigationsmöglichkeiten', 'Schwerere Prüfungen als in Präsenz', 'Unzureichende Prüfungsorganisation', 'Eigene fehlende Kompetenz', 'Andere', 'Nicht zutreffend'),
  value = c(11, 11, 6, 4, 3, 2, 2, 1, 2)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

nb.cols <- 9
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

# basic piechart
pie7 <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle (' E-Prüfung (Sonstiges) \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=2) +
  scale_fill_manual(values=mycolors) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  # ggplot2::ggsave(filename = 'frage_7_sonstiges_pie.png',path = "figures2/Frage_7")
pie7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

### 7 sonstiges bar

``` r
df <- data.frame(
  Barriere = c("Internetprobleme", 'Psychische Belastung', 'Technische Probleme', 'Fehlende Navigationsmöglichkeiten', 'Schwerere Prüfungen als in Präsenz', 'Unzureichende Prüfungsorganisation', 'Eigene fehlende Kompetenz', 'Andere', 'Nicht zutreffend'),
  Anzahl = c(11, 11, 6, 4, 3, 2, 2, 1, 2)
  )

  # Compute the position of labels
df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
nb.cols <- 9
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

title <- "E-Prüfung (Sonstiges) \n "

p7 <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 40), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
   geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(t = 0, r = 20, b = 0, l = 20))) +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage7_sonstiges_prozent.png', path = "figures2/Frage_7/")
p7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

### Frage 7_2

# added sonstiges

barchart

``` r
v <- c(4,2,9,5,2,33,5,16,27,25,135)
w <- c('Nicht zugänglich', 'Teilnahme nur mit Assistenz', 'Visuelle Wahrnehmbarkeit', 'Auditive Wahrnehmbarkeit', 'Sprachbarrieren', 'Reizüberflutung', 'Kompatibilitätsprobleme (assistive Technologien)', 'Unzureichende Tastaturbedienbarkeit', 'Unzureichende Hardware oder Software', 'Unzureichende Unterstützung', 'Bearbeitungszeit')

nb.cols <- 12
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

df7 <- data.frame(w,v)
colnames(df7) <- c('Antwort','Anzahl')


p7 <-ggplot(df7, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 350), breaks = seq(0, 350, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ('Auf welche Barrieren stoßen Sie \n bei der Teilnahme an digitalen Prüfungen? \n') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 20)))

  #ggplot2::ggsave(filename = 'frage7_2_barchart.png',path = "figures2/Frage_7/")

p7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->
\### Frage 7_2 piechart no = 193, na = 308, yes= 194

``` r
df7 <- data.frame(
  group = c("Ja", "Nein"),
  value = c(194, 193)
  )

  # Compute the position of labels
df7 <- df7 %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df7$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie7 <- ggplot(df7, aes(x="", y=value, fill=group)) +
  ggtitle ('Barrieren bei der digitalen Prüfungen \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#4a7432", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_7_2_ja_nein.png',path = "figures2/Frage_7/")
pie7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->
\### Frage 7_2 combos

``` r
k <- c(310, 37,11,7)
l <- c('1','2','3','4')
df7 <- data.frame(l,k)
colnames(df7) <- c('Barrieren','Anzahl')

df7$Barrieren <- factor(df7$Barrieren,levels = c('1','2','3','4'))

nb.cols <- 4
cs2 <- c("#e49c04", "#dc0404","#385095", "#05a404")
mycolors <- colorRampPalette(cs2)(nb.cols)

p7 <- ggplot(df7, aes(x= factor(Barrieren), y=Anzahl, fill=Barrieren)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 350), breaks = seq(0, 350, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der identifizierten Barrieren: \n digitalen Prüfungen \n ') +
  xlab('Barrieren') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

  #ggplot2::ggsave(filename = 'frage7_2_anzahl_Barrieren.png',path = "figures2/Frage_7/")

p7
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

### FRAGE 8.2

## barchart

``` r
v <- c(71,145,30,0,56,85,155,21,187,93, 13, 6, 12, 179)
w <- c('Visuelle Wahrnehmbarkeit', 'Auditive Wahrnehmbarkeit', 'Fehlende Untertitel','Fehlende Deutsche Gebärdensprache', 'Fehlende Transkripte', 'Reizüberflutung', '
Technische Barrieren', 'Sprachbarrieren', 'Schwierigkeiten, der Videokonferenz zu folgen',  'Teilnahmeschwierigkeiten', 'Kompatibilitätsprobleme (assistive Technologien)', 'Unzureichende Tastaturbedienbarkeit', 'Unzureichende Unterstützung', 'Unzureichendes Begleitmaterial')

df8 <- data.frame(w,v)
colnames(df8) <- c('Barriere','Anzahl')

nb.cols <- 14
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

df8 <- data.frame(w,v)
colnames(df8) <- c('Antwort','Anzahl')


p8 <-ggplot(df8, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 200), breaks = seq(0, 200, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ('Auf welche Barrieren stoßen Sie bei \n der Nutzung von Videokonferenztools? \n') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 20)))

  #ggplot2::ggsave(filename = 'frage8_2_barchart.png',path = "figures2/Frage_8/")

p8
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->
\### FRAGE 8 sonstiges

``` r
df <- data.frame(
  Barriere = c("Microsoft Teams", 'Discord', 'Skype', 'Cisco Webex', 'Alfaview', 'Edudip', 'Jitsi', 'Google Meet', 'Andere'),
  Anzahl = c(30, 14, 11, 6, 5, 4, 4, 2, 8)
  )

  # Compute the position of labels
df$perc <- df$Anzahl/sum(df$Anzahl) * 100
df[, 3] <- round(df[, 3], digits = 0)
nb.cols <- 9
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

title <- "Videokonferenztools (Sonstiges) \n "

p8 <- ggplot(df, aes(x= reorder(Barriere,perc), y=perc, fill=Barriere)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 50), expand = c(0,.5)) +
  scale_x_discrete(expand = c(0, .5)) +
  geom_bar(stat='identity', show.legend = FALSE) +
  geom_col(width = 0.5, show.legend = FALSE) +
  coord_flip() +
  geom_text(aes(label=paste0(perc, "%")), face="bold", color="black", hjust= -0.3, size=4) +
  ggtitle (title) +
  xlab('Categorien') + ylab('Anzahl in Prozent') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(t = 0, r = 20, b = 0, l = 20))) +
  theme(axis.text.y = element_text(size=12, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=12, face="bold", color="black")) +
  theme(panel.grid.major = element_line(linetype="blank"))
```

    ## Warning in geom_text(aes(label = paste0(perc, "%")), face = "bold", color =
    ## "black", : Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage8_sonstiges_prozent.png', path = "figures2/Frage_8/")
p8
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

\#pie chart

``` r
df8 <- data.frame(
  group = c("Ja", "Nein"),
  value = c(463, 231)
  )

  # Compute the position of labels
df8 <- df8 %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df8$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie8 <- ggplot(df8, aes(x="", y=value, fill=group)) +
  ggtitle ('Barrieren bei der Nutzung von Videokonferenztools \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=4) +
  scale_fill_manual(values=c("#4a7432", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_8_2_ja_nein.png',path = "figures2/Frage_8/")
pie8
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->
\#combos

``` r
k <- c(397,134,86,45,19,7,5,1)
l <- c('1','2','3','4', '5', '6', '7', '9')
df8 <- data.frame(l,k)
colnames(df8) <- c('Barrieren','Anzahl')

df8$Barrieren <- factor(df8$Barrieren,levels = c('1','2','3','4', '5', '6', '7', '9'))

cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")



p8 <- ggplot(df8, aes(x= factor(Barrieren), y=Anzahl, fill=Barrieren)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 450), breaks = seq(0, 400, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Anzahl der identifizierten Barrieren: \n Videokonferenztools \n ') +
  xlab('Barrieren') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=14, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=14, face="bold", margin = margin(r = 20))) +
  theme(axis.text = element_text(face="bold"))

  #ggplot2::ggsave(filename = 'frage8_2_anzahl_Barrieren.png',path = "figures2/Frage_8/")

p8
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->
\### FRAGE 9

# barchart

``` r
v <- c(312, 244, 90, 32, 10, 10, 9, 6)
w <- c('Keine Barrieren','Ich weiß es nicht', 'Dozierende', 'Kommiliton*innen', 'Technischer Support', 'Behindertenbeauftragte', 'Fachschaft', 'Sonstiges')

df9 <- data.frame(w,v)
# change col names
colnames(df9) <- c('Antwort','Anzahl')

title <- "An wen wenden Sie sich,\n wenn Sie auf Barrieren stoßen? \n \n"
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")


p9 <-ggplot(df9, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 350), breaks = seq(0, 350, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ("An wen wenden Sie sich,\n wenn Sie auf Barrieren stoßen? \n \n") +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold")) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 20)))

  #ggplot2::ggsave(filename = 'frage9_barchart.png',path = "figures2/Frage_9/")
p9
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

### Frage 10

## bar chart

``` r
v <- c(162, 343, 420, 448, 453, 412, 9)
w <- c('Keine Registrierung meines Bedarfs','Bedarf soll nicht sichtbar sein', 'Freiwilligkeit der Nutzung', 'Schutz meiner Privatsphäre', 'Datenschutz', 'Transparenz der Funktionen', 'Sonstiges')

df10 <- data.frame(w,v)
# change col names
colnames(df10) <- c('Antwort','Anzahl')

cs2 <- c("#385095", "#5494cc", "#4a7432", "#7433a2", "#e49c04", "#a45404", "#dc0404")


p10 <-ggplot(df10, aes(x= reorder(Antwort, Anzahl), y=Anzahl, fill=Antwort)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  coord_flip() +
  scale_y_continuous(limits = c(0, 500), breaks = seq(0, 500, 50)) +
  scale_x_discrete(expand = c(0, .7)) +
  geom_bar(stat='identity',show.legend = FALSE, width=0.9) +
  ggtitle ("Was ist Ihnen dabei wichtig?\n") +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  xlab('Antwort') + ylab('Anzahl') +
  theme(axis.text.y = element_text(size=10, face="bold", color="black")) +
  theme(axis.text.x = element_text(size=10, face="bold", color="black")) +
  theme(legend.title = element_text( size=8), legend.text=element_text(size=6)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold",margin = margin(t = 20))) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 20)))

  #ggplot2::ggsave(filename = 'frage10_barchart.png',path = "figures2/Frage_10/")
p10
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->
\### Frage 10 sonstiges bar

``` r
#### NOT ENOUGH INFO FOR A CHART
```

### FRAGE 11

``` r
k <- c(36, 138, 160, 161, 141, 50, 9 )
l <- c('1 (Ich weiß nichts darüber)','2','3','4','5','6','7 (Ich bin Profi)')

df11 <- data.frame(l,k)
colnames(df11) <- c('Bewertung','Anzahl')
df11$Bereiche <- factor(df11$Bewertung,levels = c('1 (Ich weiß nichts darüber)','2','3','4','5','6','7 (Ich bin Profi)'))

cs2 <- c("#e49c04", "#a45404", "#dc0404","#385095", "#5494cc", "#4a7432", "#05a404")

p11 <- ggplot(df11, aes(x= factor(Bewertung), y=Anzahl, fill=Bewertung)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 200), breaks = seq(0, 200, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = TRUE, width=0.9) +
  geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Kenntnisstand zum Thema digitaler Barrierefreiheit \n') +
  xlab('Bewertung') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 10))) +
  theme(axis.text.y = element_text(face="bold")) +
  theme(axis.text.x = element_blank()) + theme(axis.ticks.x = element_blank()) 

#ggplot2::ggsave(filename = 'frage11_barchart.png',path = "figures2/Frage_11/")

p11
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->
\### Frage 12 Würden Sie gerne einen Kurs zum Thema digitale
Barrierefreiheit besuchen?

``` r
# create data
df <- data.frame(
  group = c("Ja", "Nein"),
  value = c(338, 357)
  )

  # Compute the position of labels
df <- df %>% 
  arrange(desc(group)) %>%
  mutate(prop = value / sum(df$value) *100) %>%
  mutate(ypos = cumsum(prop)- 0.5*prop)

blank_theme <- theme_minimal()+
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        panel.border = element_blank(),
        panel.grid=element_blank(),
        axis.ticks = element_blank())

# basic piechart
pie12 <- ggplot(df, aes(x="", y=value, fill=group)) +
  ggtitle ('Würden Sie gerne einen Kurs zum Thema \n digitale Barrierefreiheit besuchen? \n') +
  # geom_bar(stat="identity", width=1, color="white") +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  theme_void() + # remove background, grid, numeric labels
  geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), 
            label = percent(value/sum(value))), face = "bold", color= "white", size=5) +
  scale_fill_manual(values=c("#385095", "#e49c04")) +
  theme(legend.title = element_blank()) +
  theme(plot.background = element_rect(fill = "white", colour = "white")) +
  theme(panel.background = element_rect(fill = "white", colour = "white")) +
  theme(plot.margin = unit(c(1,1,1,1), "cm")) +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
```

    ## Warning in geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]), :
    ## Ignoring unknown parameters: `face`

``` r
  #ggplot2::ggsave(filename = 'frage_12_ ja_nein.png',path = "figures2/Frage_12/")
pie12
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->
\### FRAGE 15 An welcher Hochschule studieren Sie?

``` r
# added answers from sonstiges
k <- c(182, 125, 178, 108, 3)
l <- c('Universität Bielefeld','PH Heidelberg','HdM Stuttgart','PH Freiburg','Andere')

df15 <- data.frame(l,k)
colnames(df15) <- c('Hochschule','Anzahl')
#df15$Hochschule <- factor(df15$Hochschule,levels = c('Universität Bielefeld','PH Heidelberg','HdM Stuttgart','PH Freiburg','Andere'))

cs2 <- c("#e49c04", "#a45404", "#385095", "#5494cc", "#05a404")

p15 <-ggplot(df15, aes(x= reorder(Hochschule, -Anzahl), y=Anzahl, fill=Hochschule)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 200), breaks = seq(0, 200, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = TRUE, width=0.9) +
  # geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('An welcher Hochschule studieren Sie?\n') +
  xlab('Hochschule') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold", margin = margin(r = 30))) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 10))) +
  theme(axis.text.y = element_text(face="bold")) +
  theme(axis.text.x = element_blank()) + theme(axis.ticks.x = element_blank()) 

 # ggplot2::ggsave(filename = 'frage15_barchart.png',path = "figures2/Frage_15/")

p15
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-42-1.png)<!-- -->
\### FRAGE 16 Welchen Abschluss streben Sie in Ihrem derzeitigen
Studiengang an?

``` r
k <- c(467, 188, 26, 14)
l <- c('Bachelor','Master','Staatsexamen','Promotion')

df16 <- data.frame(l,k)
colnames(df16) <- c('Abschluss','Anzahl')
#df15$Hochschule <- factor(df15$Hochschule,levels = c('Universität Bielefeld','PH Heidelberg','HdM Stuttgart','PH Freiburg','Andere'))

cs2 <- c("#e49c04", "#a45404", "#385095", "#5494cc", "#05a404")

p16 <-ggplot(df16, aes(x= reorder(Abschluss, -Anzahl), y=Anzahl, fill=Abschluss)) +
  theme_classic() +
  scale_fill_manual(values=cs2) +
  scale_y_continuous(limits = c(0, 500), breaks = seq(0, 500, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = TRUE, width=0.9) +
  # geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('Welchen Abschluss streben Sie in Ihrem \n derzeitigen Studiengang an? \n') +
  xlab('Abschluss') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold", margin = margin(t = 15))) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 10))) +
  theme(axis.text.y = element_text(face="bold")) +
  theme(axis.text.x = element_blank()) + theme(axis.ticks.x = element_blank()) 

  ggplot2::ggsave(filename = 'frage16_barchart.png',path = "figures2/Frage_16/")
```

    ## Saving 7 x 5 in image

``` r
p16
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->
\###Frage 16 pie

``` r
# value <- c(467, 188, 26, 14)
# group <- c('Bachelor','Master','Staatsexamen','Promotion')
# 
# df16 <- data.frame(group,value)
# #colnames(df16) <- c('Abschluss','Anzahl')
# 
#   # Compute the position of labels
# df16 <- df16 %>% 
#   arrange(desc(group)) %>%
#   mutate(prop = value / sum(df$value) *100) %>%
#   mutate(ypos = cumsum(prop)- 0.5*prop) 
# 
# blank_theme <- theme_minimal()+
#   theme(axis.title.x = element_blank(),
#         axis.title.y = element_blank(),
#         panel.border = element_blank(),
#         panel.grid=element_blank(),
#         axis.ticks = element_blank())
# 
# cs2 <- c("#7433a2", "#e49c04", "#a45404", "#05a404" )
# # basic piechart
# pie16 <- ggplot(df16, aes(x="", y=value, fill=group)) +
#   ggtitle ('Welchen Abschluss streben Sie in Ihrem \n derzeitigen Studiengang an? \n') +
#   # geom_bar(stat="identity", width=1, color="white") +
#   geom_bar(stat="identity", width=1) +
#   coord_polar("y", start=0) +
#   theme_void() + # remove background, grid, numeric labels
#   geom_text(aes(y = value/2 + c(0, cumsum(value)[-length(value)]),
#             label = percent(value/sum(value))), face = "bold", color= "white", size=3) +
#   scale_fill_manual(values=cs2) +
#   theme(legend.title = element_blank()) +
#   theme(plot.background = element_rect(fill = "white", colour = "white")) +
#   theme(panel.background = element_rect(fill = "white", colour = "white")) +
#   theme(plot.margin = unit(c(1,1,1,1), "cm")) +
#   theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) 
#   # geom_label_repel(data = df16,
#   #                  aes(y = ypos, label = paste0(value, "%")),
#   #                  size = 4.5, nudge_x = 1, show.legend = FALSE) +
#    
#   # guides(fill = guide_legend(title = "Group")) 
#   # ggplot2::ggsave(filename = 'frage_16_pie.png',path = "figures2/Frage_16/")
# pie16
```

### FRAGE 17 In welchem (Hochschul-)Semester befinden Sie sich zurzeit?

``` r
k <- c(137, 46, 118, 37, 106, 39, 71, 29, 35, 16, 22, 10, 9, 6, 7, 7)
l <- c('1','2','3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16')

df17 <- data.frame(l,k)
colnames(df17) <- c('Semester','Anzahl')
df17$Semester <- factor(df17$Semester,levels = c('1','2','3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16'))

nb.cols <- 16
cs2 <- c("#385095", "#5494cc", "#4a7432", "#05a404", "#7433a2", "#e49c04", "#a45404", "#dc0404")
mycolors <- colorRampPalette(cs2)(nb.cols)

p17 <- ggplot(df17, aes(x= factor(Semester), y=Anzahl, fill=Semester)) +
#p17 <-ggplot(df17, aes(x= reorder(Semester, Anzahl), y=Anzahl, fill=Semester)) +
  theme_classic() +
  scale_fill_manual(values=mycolors) +
  scale_y_continuous(limits = c(0, 200), breaks = seq(0, 200, 50), expand = c(0,6)) +
  scale_x_discrete(expand = c(0, .7))  +
  geom_bar(stat='identity', show.legend = FALSE, width=0.9) +
  # geom_text(aes(label=Anzahl), fontface = "bold", vjust = -0.3) +
  ggtitle ('In welchem (Hochschul-)Semester befinden Sie sich zurzeit? \n') +
  xlab('Semester') + ylab('Anzahl') +
  theme(plot.title.position = 'plot', plot.title = element_text(size=16, face = "bold",hjust = 0.5)) +
  theme(axis.title.x = element_text(color="black", size=12, face="bold", margin = margin(t = 15))) +
  theme(axis.title.y = element_text(color="black", size=12, face="bold", margin = margin(r = 10))) +
  theme(axis.text = element_text(face="bold")) 


  #ggplot2::ggsave(filename = 'frage17_barchart.png',path = "figures2/Frage_17/")

p17
```

![](charts_smibs_v2_files/figure-gfm/unnamed-chunk-45-1.png)<!-- -->
