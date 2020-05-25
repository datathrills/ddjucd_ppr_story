This is a methodology document for the [simulation game](https://datathrills.github.io/) created as a part of a Intro to Data Journalism course at UCD.

## Sources used in the article when discussing possible infection and mortality rates:

- [New York releases antibody testing data: 14% of population may be infected with coronavirus](https://eu.usatoday.com/story/news/nation/2020/04/23/coronavirus-new-york-millions-residents-may-have-been-infected-antibody-test/3012920001/)
- [SARS-CoV-2, COVID-19, Infection Fatality Rate (IFR) Implied by the Serology, Antibody, Testing in New York City](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3590771)
- [RESULTS RELEASED FOR ANTIBODY AND COVID-19 TESTING OF BOSTON RESIDENTS](https://www.boston.gov/news/results-released-antibody-and-covid-19-testing-boston-residents)
- [Coronavirus: los primeros datos de seroprevalencia estiman que un 5% de la población ha estado contagiada, con variabilidad según provincias](https://www.isciii.es/Noticias/Noticias/Paginas/Noticias/PrimerosDatosEstudioENECOVID19.aspx)
- [https://www.isciii.es/Noticias/Noticias/Paginas/Noticias/PrimerosDatosEstudioENECOVID19.aspx](https://www.gov.si/en/news/2020-05-06-first-study-carried-out-on-herd-immunity-of-the-population-in-the-whole-territory-of-slovenia/)
- [Antibody surveys suggesting vast undercount of coronavirus infections may be unreliable](https://www.sciencemag.org/news/2020/04/antibody-surveys-suggesting-vast-undercount-coronavirus-infections-may-be-unreliable)
- [Tracking covid-19 excess deaths across countries](https://www.economist.com/graphic-detail/2020/04/16/tracking-covid-19-excess-deaths-across-countries)
- [横浜港で検疫を行ったクルーズ船に関連した患者の死亡について](https://www.mhlw.go.jp/stf/newpage_10870.html)
- [Impact of non-pharmaceutical interventions (NPIs) to reduce COVID-19 mortality and healthcare demand](https://www.imperial.ac.uk/media/imperial-college/medicine/sph/ide/gida-fellowships/Imperial-College-COVID19-NPI-modelling-16-03-2020.pdf) (PDF)
- [The infection fatality rate of COVID-19 inferred from seroprevalence data](https://www.medrxiv.org/content/10.1101/2020.05.13.20101253v1)
- [Deaths involving COVID-19 by local area and socioeconomic deprivation: deaths occurring between 1 March and 17 April 2020](https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/deaths/bulletins/deathsinvolvingcovid19bylocalareasanddeprivation/deathsoccurringbetween1marchand17april#middle-layer-super-output-areas)

## Game logic

The program loops through all icons. For each icon it throws a dice to see if the person is infected or not. The chances of person being infected are determent by the user input. Out of those who are infected, the same process is repeated to see if they will die or not. 
