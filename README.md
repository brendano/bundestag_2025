### Latent left-to-right versus populism axes in Bundestag elections PCA
([Brendan O'Connor](http://brenocon.com) 2025-02-24)

Yet another example that latent space models are good at delineating political ideology or other significant structures underlying voting decisions --

In yesterday's Bundestag elections there are interesting results for 7 or so parties with significant support.  If you look at per-party support on a map of Berlin (like [this animated version](https://x.com/umichvoter/status/1893766694029369742), based on [this interactive map](https://interaktiv.morgenpost.de/bundestagswahl-ergebnisse-berlin/) - select "Wahlkreise" or scroll down for a static version), there seem to be significant patterns, even looking at results at the coarse granularity of 12 districts.  Does PCA recover anything interesting from their correlations, even on this small a dataset?

First, the loadings of the first two principal components for the 7 parties from running PCA on the 12 data points.

<img src="Screen%20Shot%202025-02-24%20at%204.59.57%20PM.png" width=400 />

There seem to be two dimensions on issues much remarked-on these days - one axis that looks like traditional left-to-right (maybe especially on immigration and/or cultural issues?), and another axis that looks like a populism versus establishment orientation, or what some commentators might call an anti-system vs. pro-system axis.  (Or more neutrally, lower parties are newer, and possibly [having younger voters](https://adamtooze.substack.com/p/chartbook-356-deutschland-2025-a).)

I was a little surprised to see the Greens (Grüne) further left than the Left party (Die Linke), but I don't know enough substantively to really judge that bit.  I'm not familiar with the new BSW party; [Wikipedia](https://en.wikipedia.org/wiki/Sahra_Wagenknecht_Alliance) describes some significant right-wing stances it has compared to the Left, which it splintered from.  There's also [pro-Russian foreign policy stances](https://www.theguardian.com/world/2022/sep/19/germanys-die-linke-on-verge-of-split-over-sanctions-on-russia) of BSW and AfD, which in the Europe/US context seem to go with the populist right; they occupy, roughly, the bottom-right of the loadings space.

Here is the correlation matrix of party support levels.  The same matrix is shown twice, ordered by the first PC, then again but ordered by the second PC.

<img src="Screen%20Shot%202025-02-24%20at%2010.24.37%20PM.png" width=300 /><img src="Screen%20Shot%202025-02-24%20at%2010.24.01%20PM.png" width=300 />

The second one looks a little clearer to me.  The 4 parties of the establishment side of PC2 (top 4 in the loadings plot) are grouped together with mostly mutual correlation. All ruling coalitions since 1961 have been among those 4 parties.

For what it's worth, if you use a hierarchical clustering dendrogram-based method to order the variables ("optimal leaf ordering" in [corrgram](https://kwstat.github.io/corrgram/)), which cares more about local pairwise distances than PCA does, it produces a result much more like the 2nd PC than the 1st, though AfD and BSW anchor the populism end of the spectrum instead of Die Linke.  The block structure looks better as you might expect from hiearchical clustering.  Maybe this is the better way to order the corr matrix.

<img src="Screen Shot 2025-02-24 at 10.42.45 PM.png" width=300 />

Finally here are the districts in the PC space (scores):

<img src="Screen Shot 2025-02-24 at 10.01.00 PM.png" width=300 />

I see various former West Berlin neighborhoods toward the top (some of the wealthier ones? but not all?). My intuitions about Berlin neighborhoods are too out of date or misinformed to attempt much interpretation but it could be interesting to look at demographics' correlations to the PC scores.  (C'burg=Charlottenburg, P'burg=Prenzlauer Berg, F'hain=Friedrichshain)  To compare, the [Tooze Chartbook blog post (linked above)](https://adamtooze.substack.com/p/chartbook-356-deutschland-2025-a) has lots of graphs of individual-level demographic correlations against individual parties.

Background on latent space voting models - I still like [Poole and Rosenthal 1989](https://people.cs.umass.edu/~brenocon/smacss2015/papers/PooleRosenthal1989Color.pdf)'s overview of [NOMINATE](https://en.wikipedia.org/wiki/NOMINATE_(scaling_method)).  We don't have individual decision data though, and in any case PCA is... really easy to run.

---


Data from [here](https://www.bundeswahlleiterin.de/en/bundestagswahlen/2025/ergebnisse/bund-99/land-11.html), copied into spreadsheet in this directory.  I used the 2nd stage voting totals.

some code snippets - incomplete

```
> library(ggrepel)
> library(corrgram)

> fix=function(x) ifelse(x=="-","0", x) %>% as.numeric
> d=read_excel("Book3.xlsx") %>% mutate(n1=fix(n1),p1=fix(p1),n2=fix(n2),p2=fix(p2))
> wl=c('Die Linke', 'CDU', 'GRÜNE', 'AfD', 'SPD', 'BSW', 'FDP')
> d=d %>% filter(type %in% wl) 

> x=d %>% select(place,type,p2) %>% pivot_wider(names_from=type,values_from=p2)
> s=data.frame(place=x$place,princomp(y)$scores[,1:2])

> ab=function(x) x %>% str_replace("Berlin-","") %>% str_replace("^[0-9]+: ","") %>% str_replace("Prenzlauer ","P'") %>% str_replace("Friedrichshain","F'hain") %>% str_replace("Charlotten","C'")

> ggplot(data.frame(party=row.names(q),q), aes(-Comp.1,Comp.2,label=party))+geom_point()+geom_text_repel()+xlab("-PC1: Left to right")+ylab("PC2: Populist vs pro-system")

> corrgram(y[,order(-princomp(y)$loadings[,1])])
> corrgram(y[,order(princomp(y)$loadings[,2])])

> ggplot(s, aes(-Comp.1,Comp.2,label=ab(place)))+geom_point()+geom_text_repel()

```
