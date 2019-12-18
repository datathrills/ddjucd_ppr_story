This is a methodology document for the [data story](http://newslab.ie/ddjucd/) written as a part of a Intro to Data Journalism course at UCD.


[test](#jupyerLab)


## Data Sources

**1) The Irish Property Price Register – Geocoded to Small Areas**
    
Property Price Register (PPR) data from years 2012-2017 (approx 220k property sales geocoded by Shane Lynn and released on his blog as open data: https://www.shanelynn.ie/the-irish-property-price-register-geocoded-to-small-areas/

**2) Census 2016 Small Area Population Statistics**
    Electoral Divisions (Generalised 20M) data set whicih includes 799 variables:
    https://www.cso.ie/en/census/census2016reports/census2016smallareapopulationstatistics/
<br>

## Data Processing

#### Cleaning the PPR data set with Open Refine
    
+ Reordering columns for better overview
+ Removing blanks for *electoral_district_id* column
+ Include only Electoral Divisions with 50 or more property sales
+ Calculate average price per Electoral Division
+ reorder and remove unnecessary columns and rows
+ Include only data for Dublin 
    <br>
    Exact operation history (copy/paste the code inside Undo/Redo>Apply tab inside [OpenRefine](http://openrefine.org/download.html)):
    <br>

    ```
    [{
            "op": "core/column-reorder",
            "columnNames": [
                "Column",
                "electoral_district",
                "electoral_district_id",
                "price",
                "year",
                "input_string",
                "ppr_county",
                "not_full_market_price",
                "vat_exclusive",
                "description_of_property",
                "property_size_description",
                "date",
                "formatted_address",
                "accuracy",
                "latitude",
                "longitude",
                "postcode",
                "type",
                "geo_county",
                "region",
                "small_area"
            ],
            "description": "Reorder columns"
        },
        {
            "op": "core/column-reorder",
            "columnNames": [
                "electoral_district_id",
                "electoral_district",
                "price",
                "year",
                "input_string",
                "ppr_county",
                "not_full_market_price",
                "vat_exclusive",
                "description_of_property",
                "property_size_description",
                "date",
                "formatted_address",
                "accuracy",
                "latitude",
                "longitude",
                "postcode",
                "type",
                "geo_county",
                "region",
                "small_area"
            ],
            "description": "Reorder columns"
        },
        {
            "op": "core/row-removal",
            "engineConfig": {
                "facets": [{
                    "type": "list",
                    "name": "electoral_district_id",
                    "expression": "value",
                    "columnName": "electoral_district_id",
                    "invert": false,
                    "omitBlank": false,
                    "omitError": false,
                    "selection": [{
                        "v": {
                            "v": "NA",
                            "l": "NA"
                        }
                    }],
                    "selectBlank": false,
                    "selectError": false
                }],
                "mode": "row-based"
            },
            "description": "Remove rows"
        },
        {
            "op": "core/row-removal",
            "engineConfig": {
                "facets": [{
                    "type": "list",
                    "name": "electoral_district_id",
                    "expression": "grel:facetCount(value, \"value\", \"electoral_district_id\")  > 49",
                    "columnName": "electoral_district_id",
                    "invert": false,
                    "omitBlank": false,
                    "omitError": false,
                    "selection": [{
                        "v": {
                            "v": false,
                            "l": "false"
                        }
                    }],
                    "selectBlank": false,
                    "selectError": false
                }],
                "mode": "row-based"
            },
            "description": "Remove rows"
        },
        {
            "op": "core/row-reorder",
            "mode": "row-based",
            "sorting": {
                "criteria": [{
                    "valueType": "string",
                    "column": "electoral_district_id",
                    "blankPosition": 2,
                    "errorPosition": 1,
                    "reverse": false,
                    "caseSensitive": false
                }]
            },
            "description": "Reorder rows"
        },
        {
            "op": "core/blank-down",
            "engineConfig": {
                "facets": [],
                "mode": "row-based"
            },
            "columnName": "electoral_district_id",
            "description": "Blank down cells in column electoral_district_id"
        },
        {
            "op": "core/column-addition",
            "engineConfig": {
                "facets": [],
                "mode": "record-based"
            },
            "baseColumnName": "price",
            "expression": "grel:floor(sum(row.record.cells.price.value)/length(row.record.cells.price.value))",
            "onError": "set-to-blank",
            "newColumnName": "average_price",
            "columnInsertIndex": 3,
            "description": "Create column average_price at index 3 based on column price using expression grel:floor(sum(row.record.cells.price.value)/length(row.record.cells.price.value))"
        },
        {
            "op": "core/column-reorder",
            "columnNames": [
                "electoral_district_id",
                "average_price",
                "electoral_district",
                "ppr_county"
            ],
            "description": "Reorder columns"
        },
        {
            "op": "core/row-removal",
            "engineConfig": {
                "facets": [{
                    "type": "list",
                    "name": "electoral_district_id",
                    "expression": "isBlank(value)",
                    "columnName": "electoral_district_id",
                    "invert": false,
                    "omitBlank": false,
                    "omitError": false,
                    "selection": [{
                        "v": {
                            "v": true,
                            "l": "true"
                        }
                    }],
                    "selectBlank": false,
                    "selectError": false
                }],
                "mode": "row-based"
            },
            "description": "Remove rows"
        },
        {
            "op": "core/row-removal",
            "engineConfig": {
                "facets": [{
                    "type": "list",
                    "name": "ppr_county",
                    "expression": "value",
                    "columnName": "ppr_county",
                    "invert": true,
                    "omitBlank": false,
                    "omitError": false,
                    "selection": [{
                        "v": {
                            "v": "Dublin",
                            "l": "Dublin"
                        }
                    }],
                    "selectBlank": false,
                    "selectError": false
                }],
                "mode": "row-based"
            },
            "description": "Remove rows"
        }
    ]
    ```
<br>
<a name="jupyerLab"></a>
### Further data processing and analysis in JupyterLab
    
+ Preparing both data sets for merging
+ Calculating percentage for each Census variable
\
    Data structure:

    | Electoral Division       | Average price (€)| Variable 1 (%)|Variable 2 (%) |...| Variable 799 (%)|
    | ------------------------ |:-------------:|:---------: |:---------:|:---------:|-----:|
    | 00                 | 266258.0 |      0.55      |1.10	|...|13.32 |
    | 01                 | 261902.0      |  1.20          | 0.97|...|7.41 |
    | ...            | ...      |        ...    |... |...|... |
    | 303                 | 594632.0      |    0.85        |1.50 |...|2.04 |

<br>

+ Calculating correlation and p-value for each Average price vs each variable
+ Ordering data to see the biggest correlations
    \
    Python code:
    ```Python
        import pandas as pd
        from scipy.stats import pearsonr
        from IPython.core.display import HTML

        #Load data
        data_stats = pd.read_csv("ED_Stats.csv", encoding = "utf-8", dtype = str)

        #From 4th column to the end: remove commas and turn into integrer 
        for col in data_stats[data_stats.columns[3 : ]]:
            data_stats[col] = data_stats[col].replace(",","", regex=True).astype(int)
            
        #Edit the column for merging
        data_stats["GEOGID"] = data_stats["GEOGID"].str[7:]

        #Load second dataset
        data_prices = pd.read_csv("ED_Prices_Dublin_final.csv", encoding = "utf-8")

        #Edit the column for merging (fix later)
        data_prices["electoral_district_id"] = "0" + data_prices["electoral_district_id"].astype(str)

        #export for QGIS
        data_prices.to_csv("out.csv", encoding='utf-8', index=False)

        #Merge datasets
        data_master = pd.merge(data_prices, data_stats, left_on="electoral_district_id", right_on="GEOGID", how="outer")

        #Remove rows with NaN
        data_master = data_master.dropna(how='any')

        #Adjusted to CSO Glossary, and new rate Dataframe
        data_glossary = data_master.iloc[:, 7:]
        data_rate_glossary = pd.DataFrame()

        #quick bug fix: stupid helper function
        def percent(x, y):
            if y!=0:
                return round((x/y)*100, 2)
            else:
                return 0;

        # Transforms numbers to rates, builds new df
        # Array sturcture:[["Total_Age_Males", 0, 38], [...]]
        def build (df, new_df, array):
            for arr in array:
                prefix = arr[0]
                start = arr[1]
                end = arr[2]

                total_list = df.iloc[:, end].tolist()
                
                for i in range(end - start):
                    column_list = df.iloc[:, start+i].tolist()
                        
                    #new_rate_list = map(lambda x, y: round((x/y)*100, 2), column_list, total_list)
                    new_rate_list = [ percent(x, y) for x,y in zip(column_list, total_list)]
            
                    name = prefix + df.columns[start + i]
                    new_df[name] = new_rate_list

        # Manually created array based on the CSO glossary:
        # https://www.cso.ie/en/census/census2016reports/census2016smallareapopulationstatistics/
        build(data_glossary, data_rate_glossary, [
            ["_Age_Males_", 0, 34],
            ["_Age_Females_", 35, 69],
            ["_Age_All_", 70, 104],
            ["_Martial_Status_Males_", 105, 110],
            ["_Martial_Status_Females_", 110, 116],
            ["_Martial_Status_All_", 117, 122],
            ["_Birth_place_", 123, 129],
            ["_Nationality_", 130, 137],
            ["_Ethnic_background_", 138, 145],
            ["_Residence_1y_before_", 146, 150],
            ["_Religion_", 151, 155],
            ["_Foregin_language_speakers_", 156, 160],
            ["_Foregin_language_speakers_EN_", 161, 166],
            ["_Irish_language_speakers_", 167, 170],
            ["_Frequency_of_speaking_Irish_M", 171, 181],
            ["_Frequency_of_speaking_Irish_F", 182, 192],
            ["_Frequency_of_speaking_Irish_A", 193, 203],
            ["_Family_size_by_num_person_Family", 204, 209],
            ["_Family_size_by_num_person_Persons", 210, 215],
            ["_Family_size_by_num_person_Children", 216, 221],
            ["_Family_with_children_by_type_and_chi_age", 222, 227],
            ["_Family_with_children_chi_age_under_15", 222, 227],
            ["_Family_with_children_chi_age_over_15", 228, 233],
            ["_Family_with_children_chi_age_both", 234, 238],
            ["_Family_by_number_of_children", 239, 245],
            ["_Family_Types_and_child_Couples_no_families", 246, 249],
            ["_Family_Types_and_child_Mother_no_families", 250, 253],
            ["_Family_Types_and_child_Father_no_families", 254, 257],
            ["_Family_Types_and_child_Couples_no_child", 258, 261],
            ["_Family_Types_and_child_Mother_no_child", 262, 265],
            ["_Family_Types_and_child_Father_no_child", 266, 269],
            ["_Family_age_youngest_child_no_families", 270, 275],
            ["_Family_age_youngest_child_no_persons", 276, 281],
            ["_Family_cycle_no_families", 282, 290],
            ["_Family_cycle_no_persons", 291, 299],
            ["_Females_no_children_born", 300, 305],
            ["_Household_type_no_households", 306, 320],
            ["_Household_type_no_persons", 321, 335],
            ["_Household_size_no_households", 336, 344],
            ["_Household_size_no_persons", 345, 353],
            ["_Household_accom_type_no_households", 354, 359],
            ["_Household_accom_type_no_persons", 360, 365],
            ["_Household_year_built_no_households", 366, 376],
            ["_Household_year_built_no_persons", 377, 387],
            ["_Household_ownership_no_households", 388, 395],
            ["_Household_ownership_no_persons", 396, 403],
            ["_Household_rooms_no_households", 404, 413],
            ["_Household_rooms_no_persons", 414, 423],
            ["_Household_heating", 424, 434],
            ["_Household_water", 435, 441],
            ["_Household_sewerage", 442, 448],
            ["_Household_occupancy_status", 449, 453],
            # No totals for 454, 455
            ["_Economic_status_Males", 456 , 464],
            ["_Economic_status_Females", 465, 473],
            ["_Economic_status_All", 474, 482],
            ["_Social_class_Males", 483, 490],
            ["_Social_class_Females", 491, 498],
            ["_Social_class_All", 499, 506],
            ["_Socioeconomic_of_ref_pers_no_households", 507, 518],
            ["_Socioeconomic_of_ref_pers_no_persons", 519, 530],
            ["_Education_ceased_Males", 531, 540],
            ["_Education_ceased_Females", 541, 550],
            ["_Education_ceased_All", 551, 560],
            # No totals for 561 - 566
            ["_Education_field_Males", 567, 578],
            ["_Education_field_Females", 579, 590],
            ["_Education_field_Total", 591, 602],
            ["_Education_highest_level_Males", 603, 615],
            ["_Education_highest_level_Females", 616, 628],
            ["_Education_highest_level_Total", 629, 641],
            ["_Travel_Work", 642, 653],
            ["_Travel_School", 654, 665],
            ["_Travel_Total", 666, 677],
            ["_Travel_Time_leaving_house", 678, 687],
            ["_Travel_Time_journey_time", 688, 695],
            ["_Travel_Time_journey_time", 688, 695],
            # No totals for 696 - 701
            # Messed up health data 702 - 722
            ["_Occupation_Males", 723, 733],
            ["_Occupation_Females", 734, 744],
            ["_Occupation_All", 745, 755],
            ["_Industry_work_Males", 756, 764],
            ["_Industry_work_Females", 765, 773],
            ["_Industry_work_All", 774, 782],
            ["_Car_ownership", 783, 789],
            ["_PC_ownership", 790, 793],
            ["_Internet_ownership", 794, 798]    
        ])

        # Create Average price dataframe
        data_price = data_master.iloc[:, 1:2]
        # Merge into result
        data_fin = pd.concat([data_price, data_rate_glossary], axis=1, join='outer', ignore_index=False)

        # CORRELATION ANALYSIS
        data_corr = data_fin.drop("average_price", axis=1).apply(lambda x: x.corr(data_fin.average_price))
        analysis = data_corr.sort_values()

        # Checking p-values
        fin_clean = data_fin.dropna()
        new_data_corr = fin_clean.drop("average_price", axis=1).apply(lambda x: pearsonr(x, fin_clean.average_price))
        analysis2 = pd.DataFrame(list(new_data_corr), index=new_data_corr.index.values, columns=["corr", "p_value"])
        sorted_analysis = analysis2.sort_values("corr").round(4)

        # Display 100 strongest correlations
        rich_properties = sorted_analysis.tail(100)
        display(HTML(rich_properties.to_html()))
    ```

### Data Visualisation

1) Map was done using QGIS and refined with Inkscape
2) Charts were done with Python using Matplotlib and also refined with Inkscape
3) The illustrations were hand drawn in Adobe Illustrator 
