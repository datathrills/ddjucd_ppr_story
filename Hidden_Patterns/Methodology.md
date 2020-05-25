This is a methodology document for the [data story](http://newslab.ie/ddjucd/) written as a part of a Intro to Data Journalism course at UCD.

# Data sources

CSO Census small area data and shape files: [https://www.cso.ie/en/census/census2016reports/census2016smallareapopulationstatistics/](https://www.cso.ie/en/census/census2016reports/census2016smallareapopulationstatistics/)

HSE Covid-19 Dublin map: [PDF](https://www.hse.ie/eng/services/news/newsfeatures/covid19-updates/covid-19-daily-operations-20-00-15-april-2020.pdf)

# Python code
```pyhton
import pandas as pd
import codes #clear_codes; virus_codes
# List of all small area codes which are 98% clear
# Manually selected all the clear spaces
# Edit > Copy Features > Paste Temp Strach Layer
# C/P to VS Code and edit (CTRL+A, ALT+SHIT+I)

data = pd.read_csv("SAPS.csv", encoding = "ISO-8859-1", dtype = str)
clear_data = data[data['GUID'].isin(clear_codes)].copy()

virus_data = data[data['GUID'].isin(virus_codes)].copy()
variables = list(data.columns.values)

# Data!
#From 4th column to the end: remove commas and turn into integrer
for col in data[data.columns[3 : ]]:
    data[col] = data[col].replace(",","", regex=True).astype(int)

#clear
for col in clear_data[clear_data.columns[3 : ]]:
    clear_data[col] = clear_data[col].replace(",","", regex=True).astype(int)

#virus
for col in virus_data[virus_data.columns[3 : ]]:
    virus_data[col] = virus_data[col].replace(",","", regex=True).astype(int)
    
#Remove rows with NaN
data_master = data.dropna(how='any')
clear_data_master = clear_data.dropna(how='any')
virus_data_master = virus_data.dropna(how='any')

#Adjusted to CSO Glossary, and new rate Dataframe
data_glossary = data_master.iloc[:, 3:]
data_rate_glossary = pd.DataFrame()

clear_data_glossary = clear_data_master.iloc[:, 3:]
clear_data_rate_glossary = pd.DataFrame()

virus_data_glossary = virus_data_master.iloc[:, 3:]
virus_data_rate_glossary = pd.DataFrame()

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

arr_totals_metadata = [
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
]

build(data_glossary, data_rate_glossary, arr_totals_metadata)
build(clear_data_glossary, clear_data_rate_glossary, arr_totals_metadata)
build(virus_data_glossary, virus_data_rate_glossary, arr_totals_metadata)

differences = pd.DataFrame( clear_data_rate_glossary.mean() - virus_data_rate_glossary.mean(), columns=[ "diff"])
sorted_differences = differences.sort_values("diff").round(4)
sorted_differences


```

