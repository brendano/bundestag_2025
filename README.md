Yet another example that latent space models are really good at delineating political ideology.  The Bundestag elections yesterday included significant support for 7 or so parties (only 6, excluding BSW, met the threshold for Bundestag representation).

Here's a quick PCA analysis on just 12 data points, the percentage support across 12 districts in Berlin.  If you look at per-party support on a map (like [this animated version](https://x.com/umichvoter/status/1893766694029369742), based on [this interactive map](https://interaktiv.morgenpost.de/bundestagswahl-ergebnisse-berlin/), when you select "Wahlkreise"), there seem to be significant patterns.

First, the loadings of the first two principal components for the 7 parties from running PCA on the 12 data points.

![](blob/master/Screen%20Shot%202025-02-24%20at%204.59.57%20PM.png)

There seem to be two separate dimensions, on issues much remarked-on these days - an axis that looks like traditional left-to-right (maybe especially on immigration and/or cultural issues?), and an axis that looks like a populism versus establishment orientation, or what some commentators might call an anti-system vs. pro-system axis.  (Or more neutrally, I guess they're newer versus older parties.)

I was a little surprised to see the Greens (Grüne) further left than the Left (Die Linke) party, but I don't know enough substantively to really judge that piece.  I'm not familiar with the new BSW party; according to Wikipedia it splintered off from Die Linke with differences including right-wing culture/immigration stances (plus pro-Russian foreign policy, which I guess is in the right-wing populism bucket in current Europe/US politics).

Here is the correlation matrix of party support levels.  The same matrix is shown twice, ordered by the first PC, then again but ordered by the second PC.



The 4 parties of the establishment side of PC2 (top 4 in the loadings plot) look pretty clear. All ruling coalitions since 1961 have been among those 4 parties.

For what it's worth, if you use a hierarchical clustering dendrogram-based method to order the variables ("optimal leaf ordering", `'OLO'' in [corrgram](https://kwstat.github.io/corrgram/)), it produces a result much more like the 2nd PC than the 1st, though AfD and BSW anchor the populism end of the spectrum (not Die Linke).

