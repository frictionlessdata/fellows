## Introduction

Before joining the Frictionless Data Fellowship Programme, I did not realise the importance of research reproducibility. To tell the truth, I really did not have such a concept in my professional vocabulary despite having an MSc degree in Social Science Research Methods and working in different social research projects. But, maybe, that was the reason why I did not know this concept and never practised it in my research projects. Like many of my social science colleagues, especially the ones working with qualitative - and often sensitive -  data, for me it was important to ensure that data I collect are safely stored in a password-protected platform and then - upon completion of a project - are deleted. But now working for the *Frictionless Data Fellowship Programme* and managing different sorts of data, including bibliometric metadata, I see that if we want social sciences and humanities to progress, it is vital to integrate such practices as reproducing, replicating, and reusing data into our research. 

So, in this blog, I will try to explain my first attempt to reproduce my Frictionless fellow’s dataset, which is openly shared in [the GitHub repository](https://github.com/vyelnats/frictionless-v).  

## Reproducing Openly Shared Data using Frictionless browser tools

First, I contacted Victoria - my partner in data trading - to learn about her dataset and datapackage location. After a while, she sent me the link to her GitHub with the names of the files that she wanted me to reproduce. Feeling equipped with all the information that I needed, I opened Victoria’s Frictionless Fellow repository in GitHub. There were so many interesting things that caught my attention. By clicking and exploring some files, I finally could locate the dataset that I needed. It was a [weather.csv](https://github.com/vyelnats/frictionless-v/blob/main/weather.csv) file. Already reading the name of the dataset, I knew that I would deal with all sorts of information about weather and felt so excited. So, I copied the raw data path and loaded it into the Frictionless [Data Package Creator](https://create.frictionlessdata.io) tool. 

The Data Package tool inferred 22 fields with metadata about minimum and maximum temperatures, rainfall, wind direction and speed, humidity, and other weather related information. I looked through the fields, checked the schema, and corrected the inconsistencies in data types. Then, I quickly filled in the metadata part and pushed the *Validate* button. After I received the *Data package is valid* message, I was good to go to validate the dataset itself. For that purpose, I used the Frictionless [Goodbtables](http://goodtables.io) tool. 

In Goodtables, I first validated the data table without a schema. Not surprisingly, the data table came out to be valid. Then, I decided to validate the data against the table schema, which I copied from the datapackage.json file downloaded from the Data Package creator tool a few steps earlier. I uploaded the path to the schema json file, and pushed the validate button. This time I received a red-line message indicating 12 [type errors](https://framework.frictionlessdata.io/docs/references/errors-reference/#type-error) in the data. I wanted to fix them using a frictionless-py framework but was unsuccessful. However, there were not so many errors, so I manually fixed them, and revalidated the dataset. Finally, I received a long-awaited green-colour message, saying “Valid Table”. 

## Reproducing Openly Shared Data using frictionless-py tools

Although Goodtables is a quick and easy way to validate a dataset, it still did not make me give up my eagerness to learn about cleaning data using python. So, I asked Lilly to go through this process together during our call. 

We looked through the dataset by using describe, extract, and validate frictionless-py functions. Unsurprisingly, we got the same 12 type errors. I thought I could fix them by just changing the wrong data type to the correct one by using the following code: 


`resource.schema.get_field('WindSpeed9am').type = 'any'`

`resource.schema.get_field('WindGustSpeed').type = 'any'`

`resource.schema.get_field('Sunshine').type = 'any'`

`pprint(resource)`

`resource.to_yaml("weather.yaml")`

Running this code, I wanted the frictionless-py to get the fields where errors were located and to change their type to *any*. In the last code, I also asked it to save the changed dataset as a yaml file for later use. 

But reading the message line in the code, Lilly noticed that the true cause of the errors were *missing values* in the cells. That was an *aha* moment for us. So, Lilly opened the [frictionless-py documentation](https://framework.frictionlessdata.io) and found a new (at least for me) function called `Detector`, which, as its name says, detects missing values and asks the frictionless-py to consider them as ‘null’. 

We used the following code for that: 

from pprint import pprint
from frictionless import Detector, describe

`detector = Detector(field_missing_values=["", "NA"])`

`resource = describe("https://raw.githubusercontent.com/vyelnats/frictionless-v/main/weather.csv", detector=detector)`

`pprint(resource.schema.missing_values)`

`pprint(resource.read_rows())`

We ran the code and then re-validated the dataset. 
And as the stats below show, this time frictionless-py detected no error and the dataset was clean, valid, and ready to be uploaded into an open data repository. 

`{'errors': [],
 'stats': {'errors': 0, 'tasks': 1},
 'tasks': [{'errors': [],
            'partial': False,
            'resource': {'encoding': 'utf-8',
                         'format': 'csv',
                         'hashing': 'md5',
                         'name': 'weather',
                         'path': 'https://raw.githubusercontent.com/vyelnats/frictionless-v/main/weather.csv',
                         'profile': 'tabular-data-resource',
                         'schema': {'fields': [{'name': 'MinTemp',
                                                'type': 'number'},
                                               {'name': 'MaxTemp',
                                                'type': 'number'},
                                               {'name': 'Rainfall',
                                                'type': 'number'},
                                               {'name': 'Evaporation',
                                                'type': 'number'},
                                               {'name': 'Sunshine',
                                                'type': 'number'},
                                               {'name': 'WindGustDir',
                                                'type': 'string'},
                                               {'name': 'WindGustSpeed',
                                                'type': 'integer'},
                                               {'name': 'WindDir9am',
                                                'type': 'string'},
                                               {'name': 'WindDir3pm',
                                                'type': 'string'},
                                               {'name': 'WindSpeed9am',
                                                'type': 'integer'},
                                               {'name': 'WindSpeed3pm',
                                                'type': 'integer'},
                                               {'name': 'Humidity9am',
                                                'type': 'integer'},
                                               {'name': 'Humidity3pm',
                                                'type': 'integer'},
                                               {'name': 'Pressure9am',
                                                'type': 'number'},
                                               {'name': 'Pressure3pm',
                                                'type': 'number'},
                                               {'name': 'Cloud9am',
                                                'type': 'integer'},
                                               {'name': 'Cloud3pm',
                                                'type': 'integer'},
                                               {'name': 'Temp9am',
                                                'type': 'number'},
                                               {'name': 'Temp3pm',
                                                'type': 'number'},
                                               {'name': 'RainToday',
                                                'type': 'string'},
                                               {'name': 'RISK_MM',
                                                'type': 'number'},
                                               {'name': 'RainTomorrow',
                                                'type': 'string'}],
                                    'missingValues': ['', 'NA']},
                         'scheme': 'https',
                         'stats': {'bytes': 29462,
                                   'fields': 22,
                                   'hash': 'e9f5a995fe15cf248a228ad6313793de',
                                   'rows': 366}},
            'scope': ['hash-count-error',
                      'byte-count-error',
                      'field-count-error',
                      'row-count-error',
                      'blank-header',
                      'extra-label',
                      'missing-label',
                      'blank-label',
                      'duplicate-label',
                      'incorrect-label',
                      'blank-row',
                      'primary-key-error',
                      'foreign-key-error',
                      'extra-cell',
                      'missing-cell',
                      'type-error',
                      'constraint-error',
                      'unique-error'],
            'stats': {'errors': 0},
            'time': 0.806,
            'valid': True}],
 'time': 0.806,
 'valid': True,
 'version': '4.22.3'}`


## Conclusion 

This data trading practice was very useful for me. It taught me how by using openly shared data we can locate errors, fix them, and reuse other people’s data to progress science and research in multiple different contexts. 

