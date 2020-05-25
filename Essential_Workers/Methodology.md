This is a methodology document for the [data story](http://newslab.ie/ddjucd/) written as a part of a Intro to Data Journalism course at UCD.

## Table of Contents
* [Data Sources](#sources)
* [Data Processing](#processing)
* [Data Visualisation](#dataViz)

<a name="sources"></a>

## Data Sources

**1) Occupations and Earned Income**

**Table name:** IIA12 -- Median Earned Income of NACE Sectors by Detailed Occupational Group, Year and Statistic  
**Source:** CSO  
**Published:** 20/06/2019 11:00:00  
**Link:** https://statbank.cso.ie/px/pxeirestat/Statire/SelectVarVal/Define.asp?maintable=IIA12&PLanguage=0   
**Note:** Latest available data was for year 2016

Dataset consist of median income for 325 occupations in regards to in which industries does each occupation work. Dataset was slightly restructured in OpenRefine (see code at the end of the document).    

:moneybag::question:

Additional data about incomes of HSE healthcare workers:

**Table name:** January 2016 Revised Consolidated Payscales  
**Source:** HSE  
**Link:** https://www.hse.ie/eng/staff/benefitsservices/pay/

:ambulance:

<br>

**2) Number of People in Different Industries**

**Table name:** Population Aged 15 Years and Over in the Labour Force 2011 to 2016 by Age Group, Detailed Industrial Group and Census Year  
**Source:** CSO  
**Link:**  https://statbank.cso.ie/px/pxeirestat/Statire/SelectVarVal/Define.asp?MainTable=EB042&PLanguage=0&PXSId=0   

Dataset consist of number of people who are employed in 145 different industries. In order to estimate the number of people who work in essential industries, each data point was manually compared with the industries stated in the government list of essential services.  

**Table name:** List of essential service providers under new public health guidelines  
**Link:** https://www.gov.ie/en/publication/dfeb8f-list-of-essential-service-providers-under-new-public-health-guidelin/  
**Published:** 28 March 2020  


**3) Population structure**	

**Table name:** EZ002: Population Aged 15 Years and Over 2011 to 2016 by County and City, Sex, Principal Economic Status, Age Group and CensusYear  
**Source:** CSO  
**Link:**  https://statbank.cso.ie/px/pxeirestat/Statire/SelectVarVal/Define.asp?maintable=EZ002&PLanguage=0  

**Table name:** EY002: Population at Each Census from 1926 to 2016 by Age Group, Sex and CensusYear  
**Source:** CSO  
**Link:**  https://statbank.cso.ie/px/pxeirestat/Statire/SelectVarVal/Define.asp?maintable=EY002&PLanguage=0  

**Document name:** Update on payments awarded for COVID-19 Pandemic Unemployment Payment and Enhanced Illness Benefit 
**Source:** Department of Employment Affairs and Social Protection
**Link:** https://www.gov.ie/en/press-release/88a055-update-on-payments-awarded-for-covid-19-pandemic-unemployment-paymen/  

Estimate:

| Category                        | Number           | Number of figures|
| ------------------------------- |:----------------:|:---------: |
| Employer or own account worker  | 313404           |  -      |
| Employee                        | 1688549          |  -      |
| Total Population                | 4761865          |  -       |
| COVID-19 Pandemic Unemployment Payment; Live Register | 753000    |  ≈14 | 
|                                 |                  |            |
| Employed Estimate March 2020    | 1500000     |  ≈31      |
| Others Estimate March 2020       | 2700000             |  ≈55     |

<br>

<a name="processing"></a>

## Data Processing

Using OpenRefine on the CSO's "IIA12 -- Median Earned Income of NACE Sectors by Detailed Occupational Group, Year and Statistic" table

```JSON
[
  {
    "op": "core/column-rename",
    "oldColumnName": "Median Earned Income of NACE Sectors by Detailed Occupational Group,",
    "newColumnName": "Occupation",
    "description": "Rename column Median Earned Income of NACE Sectors by Detailed Occupational Group, to Occupation"
  },
  {
    "op": "core/text-transform",
    "engineConfig": {
      "facets": [
        {
          "type": "list",
          "name": "occupation",
          "expression": "value",
          "columnName": "occupation",
          "invert": false,
          "omitBlank": false,
          "omitError": false,
          "selection": [
            {
              "v": {
                "v": "Opticians",
                "l": "Opticians"
              }
            }
          ],
          "selectBlank": false,
          "selectError": false
        }
      ],
      "mode": "row-based"
    },
    "columnName": "Occupation",
    "expression": "value.trim()",
    "onError": "keep-original",
    "repeat": false,
    "repeatCount": 10,
    "description": "Text transform on cells in column Occupation using expression value.trim()"
  },
  {
    "op": "core/fill-down",
    "engineConfig": {
      "facets": [
        {
          "type": "list",
          "name": "occupation",
          "expression": "value",
          "columnName": "occupation",
          "invert": false,
          "omitBlank": false,
          "omitError": false,
          "selection": [
            {
              "v": {
                "v": "Opticians",
                "l": "Opticians"
              }
            }
          ],
          "selectBlank": false,
          "selectError": false
        }
      ],
      "mode": "record-based"
    },
    "columnName": "Occupation",
    "description": "Fill down cells in column Occupation"
  },
  {
    "op": "core/row-removal",
    "engineConfig": {
      "facets": [
        {
          "type": "list",
          "name": "occupation",
          "expression": "value",
          "columnName": "occupation",
          "invert": false,
          "omitBlank": false,
          "omitError": false,
          "selection": [
            {
              "v": {
                "v": "Opticians",
                "l": "Opticians"
              }
            }
          ],
          "selectBlank": false,
          "selectError": false
        },
        {
          "type": "list",
          "name": "Column 2",
          "expression": "isBlank(value)",
          "columnName": "Column 2",
          "invert": false,
          "omitBlank": false,
          "omitError": false,
          "selection": [
            {
              "v": {
                "v": true,
                "l": "true"
              }
            }
          ],
          "selectBlank": false,
          "selectError": false
        }
      ],
      "mode": "record-based"
    },
    "description": "Remove rows"
  }
]
```

<a name="dataViz"></a>

## Data Visualisation

Charts were done with Vega-lite and Inkscape  

Vega-lite JSON specs:
```JSON
{
  "schema": "https://vega.github.io/schema/vega-lite/v4.json",
  "width": 600,
  "height": {"step": 0.7},
  "data": {
    "values": "stats_h1",
    "name": "source"
  },
  "selection": {
    "a": {"type": "single"}
  },
  "mark": {"type": "bar"},
  "encoding": {
    "x": {"field": "Income", 
        "type": "quantitative"
       },
    "y": {"field": "Occupation and Industry Group", 
        "type": "nominal",
        "sort":{"op": "min", "order": "descending", "field": "Income"},
       "axis": {
         "labels": false,
         "ticks": false
       }
       },
    "tooltip": [
      {"field": "Occupation and Industry Group", "type": "nominal"},
      {"field": "Income", "type": "quantitative"},
    ],
    "color": {
      "field": "Type",
        "type": "nominal",
        "scale": {
          "range": [
            "#e0e1e0",
            "#e0e1e0",
            "#e0e1e0",
            "#dd4b4b",
            "#e0e1e0",
            "#e0e1e0",
            "#e0e1e0",
            "#e0e1e0"
          ]
        }
    }
  },
  "layer": [{
    "mark": "bar"
  }, {
    "transform": [
    {"filter": "datum.Type == 'Health'"} 
  ],
    "mark": {
      "type": "text",
      "align": "left",
      "baseline": "middle",
      "dx": 3
    },
    "encoding": {
      "text": {"field": "Occupation", "type": "nominal"}
    }
  }],
  "config": {
    "bar": {"discreteBandSize": 0.8},
    "view": {"stroke": "transparent"},
    "axis": {"domainWidth": 0}
    
  }
}
```
