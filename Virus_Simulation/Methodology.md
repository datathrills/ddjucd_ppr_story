This is a methodology document for the [simulation game](https://datathrills.github.io/) created as a part of a Intro to Data Journalism course at UCD.

## Sources used in the article when discussing possible infection and mortality rates:

- [New York State testing results 1](https://eu.usatoday.com/story/news/nation/2020/04/23/coronavirus-new-york-millions-residents-may-have-been-infected-antibody-test/3012920001/)
- [New York State testing results 2](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3590771)
- [Boston testing results](https://www.boston.gov/news/results-released-antibody-and-covid-19-testing-boston-residents)
- [Spain testing results](https://www.isciii.es/Noticias/Noticias/Paginas/Noticias/PrimerosDatosEstudioENECOVID19.aspx)
- [Slovenia testing results](https://www.gov.si/en/news/2020-05-06-first-study-carried-out-on-herd-immunity-of-the-population-in-the-whole-territory-of-slovenia/)
- [Critique of antibody tests](https://www.sciencemag.org/news/2020/04/antibody-surveys-suggesting-vast-undercount-coronavirus-infections-may-be-unreliable)
- [Estimating the real number of Covid-19 deaths](https://www.economist.com/graphic-detail/2020/04/16/tracking-covid-19-excess-deaths-across-countries)
- [Diamond Princess cruise ship deaths](https://www.mhlw.go.jp/stf/newpage_10870.html)
- [Imperial College model](https://www.imperial.ac.uk/media/imperial-college/medicine/sph/ide/gida-fellowships/Imperial-College-COVID19-NPI-modelling-16-03-2020.pdf) (PDF)
- [Stanford University model](https://www.medrxiv.org/content/10.1101/2020.05.13.20101253v1)
- [ONS study about COVID-19 deaths and socioeconomic deprivation](https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/deaths/bulletins/deathsinvolvingcovid19bylocalareasanddeprivation/deathsoccurringbetween1marchand17april#middle-layer-super-output-areas)

## Game logic

The program loops through all icons. For each icon it throws a dice to see if the person is infected or not. The chances of person being infected are determent by the user input. Out of those who are infected, the same process is repeated to see if they will die or not. 
