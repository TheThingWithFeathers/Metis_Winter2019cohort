# Project McNulty Proposal: What's the probability a given SF police incident report will remain unresolved within 2 months?

**Natalia Lichagina**

### Scope

I am interesting in predicting whether a crime incident will not be resolved within a reasonable time frame (2 months) given day, time, location, and type of the crime.

I may change my objective after I've had the chance to explore the data more fully and instead try to predict what type of crime is most likely to happen at a particular time/day/location.

### Data

1. San Francisco Police Department Incident Reports 2008 to present  (download)

   [link]: https://data.sfgov.org/Public-Safety/Police-Department-Incident-Reports-2018-to-Present/wg3w-h783

### Considerations

1. After the initial report has been filed, it cannot be edited, so if a crime was resolved later, it will be logged as a new row - I will have to do some data management with my old friend SQL to fix that and have only one observation per crime in the set
2. The report also includes self-reported crimes (I'm going to exclude these) and vehicle-related crimes (will decide after I've dug into the data)
3. I'm sure Seattle has just as good of a dataset, but I don't want to get freaked out by knowing how much crime happens as I walk through the streets alone at night
4. In itself the resolution of a crime (e.g. arrest) will not necessarily indicate a good thing because, as we all know, there is discrimination in who gets arrested and why; also, the more high crime neighborhoods will probably have higher density of patrols and thus more immediate arrests?
