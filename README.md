# DS 4320 Project 1 - MovieDataset
Ryan Ermovick - jph4dg

**DOI:** 10.5281/zenodo.19362900 
Linked [here](https://doi.org/10.5281/zenodo.19362900)

**Executive Summary:**
This repository contains the detailed creation of the MovieDataset, including the origin of the data, how it was transformed, and how it can be used. The README contains information on the problem being solved, domain exposition, data creation, and metadata. The repository also contains a RandomForestRegressor that is used to predict how users would rate a certain movie. The press release details how this model could be used. 


#### Important Links

| File/Resource | Link |
| :--- | :--- |
| Folder with data files | [Data Set](https://myuva-my.sharepoint.com/:f:/g/personal/jph4dg_virginia_edu/IgCkBPj3Qvv_RqwRktNJpfHWAd3jXUCRhCbCgvidEkNqbVM?e=aQWVLi) |
| Press Release | [press_release.md](documents/press_release.md) |
| Data Creation File | [data_preparation](data_preparation.ipynb) |
| Pipeline & Model File | [pipeline.ipynb](scripts/pipeline.md) |
| Ryan Ermovick MIT License | [license](LICENSE) | 

#### Quick Folder Guide
| Folder | Description |
| :--- | :--- |
| documents | Contains important documents, such as the Press Release |
| images | Contains images created in this project that can be used elsewhere |
| scripts | Contains the scripts used in this project |



## Problem Definition
**Initial Problem:** Netflix Content Recommendation

**Refined Specific Problem:** Predicting content that users will and enjoy and that Netflix should add to its platform.

#### Reasoning
The rationale behind this convergence is getting to the root of the problem. Netflix is one of many streaming services now available to users. As such, Netflix must keep their platform stocked with popular movies and tailored recommendation systems to promote these movies to users. So, the refined goal for content recommendation is to use past users' movie ratings to determine what movies should be added to netflix and what should be recommended to users. Having interesting and well liked content on Netflix's platform is beneficial to them as users will stay on the platform longer and possibly recommend the platform/specific movies to friends/families. Having tailored recommendations achieves this goal as well. Overall, the convergence of the problem comes from the root objective of keeping people engaged on the platform and the idea that personalized recommendations are superior to broad ones. 


The motivation for this project is personal. Watching tv is one of my favorite things to do, but I often am looking for new content to consume. Sometimes the recommendations that Netflix or other platforms provides are not effective. This is bad for me, because I am not getting the content that I want, and bad for Netflix because I am not engaged on their platform and might shift to other platforms. In the end, it will benefit us both to have an effective recommendation system and well liked content to ensure user engagement and satisfaction. 

**A better way to find Movies!** Read the full press release [here](documents/press_release.md).

## Domain Exposition
#### Terminology Table
| Term/KPI | Definition | Term or KPI |
| :--- | :--- | :--- |  
| Netflix | A video streaming platform. | Term |
| User | Subscribers of the Netflix streaming platform | Term |
| Content Recommendation | A personalized list of shows or movies for a user to watch on netflix | Term |
| Rating | How many stars the user rated the piece of content on Netflix | KPI |
| Content | A movie, show, or other piece of media on Netflix (or other streaming platform) | Term |
| Favorites List | If the user has the show saved on their favorite list of content on Netflix | Term |
| Previously Watched | If the user has already watched any part of the content or not | Term |

#### Domain Explanation
This project lives in the domains of entertainment and content recommendation algorithms. Since the goal is to give recommendations to users for entertainment, naturally this project would be important to entertainment companies. Specifically streaming services that want users to stay on their platform. This industry has gotten to be extremely competitive nowadays with many new streaming services popping up (HBO Max, Hulu, Disney +, Paramount +, etc.). Since these are fixed price services, they want people to enjoy their platform and have interesting content so that users remain subscribed and influence friends/family to subscribe. Also, the domain of content recommendations algorithms has had to evolve along with this. Since there is a huge amount of content on each platform, users need a way to find the ones that interest them in an easy manner.

#### Check out some background reading on the these fields below!

| Title | Description | Link |
| :--- | :--- | :--- |  
| We delve into the exciting history of Netflix: <Br> How a global entertainment leader was forged | This article reviews the timeline of Netflixs creation <br> and how it has evolved from a DVD service to a streaming platform. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQByl0XGM0rdQYFDzKTttHtIAcNBrgTSpqHvAxOrnX8H8b0?e=cYfaGe |
| Recommendations | This is a short article from Netflix itself about its reccomendations. <br> Reading this article could be helpful in determining Netflix's motivation <br> and logic behind decisions. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQBHxVlUQJXfQZIljCO6lfgGAY-ykEcAYR3Pphxz35upht8?e=1LLiV4 |
| Winning the Streaming Wars: Are Megahits Like Stranger Things the Answer? | This article describes the complex business landscape of <br> streaming services and how Netflix fits into it. Additionally, this article might <br> shed light as to why Netflix would want to update their reccomendation system. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQA6zyoxBaKzR5I74m2ogfCRAQqASUKhVumINAZMRsfls2Q?e=atWRUP |
| Netflix’s Recommendation Systems: Entertainment Made for You | This article describs how reccomendation systems actually work. <br>  Reading this article could <br> inspire new ideas for a novel reccomendation system. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQAE66UeCebQQ5joZwi4d_GWAdka0ia8yry3m7iONrkGXrU?e=rSOr41 |
| Is Netflix actually bad at recommendations… <br> or is the algorithm intentionally limiting what we see? | This is a reddit thread where users are describing the issues that they have <br> with Netflix's reccomendation system. These reviews are extremely helpful <br> in determining how to create the new reccomendation system. |  https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQA7hxjsUc_IQr52MJNlDJ5OAULHsILQ5_A72Gu-evt_goU?e=bWAZ3l |


All readings/files can be found [here](https://myuva-my.sharepoint.com/:f:/g/personal/jph4dg_virginia_edu/IgDHbT83FJg4SYEykl-iIskoAXHbN_KretkzzvqoBKgnKy0?e=TChx0l)


## Data Creation

#### Provenance
The data acquisition process for this project mainly entitled gathering data from the movie lens dataset (32 million record version). The files were then downloaded and unpacked. To start, the movies and the ratings files were split up. The movies file became mov and only contains information on movies (genre, title, release year). The users file has a unique row for each user. Then the users file was supplemented with synthetic data to mimic their jobs, salary, and age. The ratings data table became rat and has ratings from all the users (including timestamp). Finally, another data table was created that describes users watching history (e.g. minutes watched).

Another dataset was also used for this project. A dataset on Kaggle was found that contains the movies present on Netflix. This dataset was joined with the mov dataset to become net_mov, which is a subset that contains only movies present on Netflix. The net_mov dataset was filtered to have the same column types as the mov data table. 

#### Bias and Mitigation
For all of the gathered data (mov, net_mov, rat), there is some room for bias to occur. All of the ratings came from the movie lens dataset, and were provided by actual users. Bias could have come from people intentionally rating a movie high or low (love/hate the movie) or not including all movie watchers. For the synthetic data, bias could have come from not having real data or not accurately representing the human population. There is not room for bias in the user watching history table as that was just calculated data.

To combat the bias of possibly not including enough users, a dataset of more than 32 million reviews was used. For the synthetic data, care was taken to ensure that the numbers created were not totally random. Research was done as to what the top jobs are in the US and what their mean salaries were. This way the data here is representative of the US population. Finally, as stated before, there is not room for bias in the user watching history table as it is just calculated data.

#### Key Decisions
I decided to make a synthetic data table because an older version of this dataset had such characteristics about the users. It seemed that it would add an extra level of complexity to the data, so I decided to recreate it, despite knowing that it might cause bias down the line. Additionally, Netflix would not have this information about users unless they willingly provided, so if they wanted to recreate this model, it would have to be synthetic data as well. I also decided to create a calculated data table because I think that these would be good statistics to help with a recommendation model. I choose to make a Netflix data table mainly because it would help with data separation down the line. If the model were to be trained on only netflix movies or non netflix movies, the two groups would have already been split up. 


#### Data Files
| Code File | Description | Link |
| :--- | :--- | :--- |
| Data Creation | File that shows the data creation | [Data Preparation](scripts/data_preparation.ipynb) |


## Metadata
### Entity Relationship Diagram (ERD)
![erd](images/erd.png)

#### Below is a description of every data table in the database and a link to it
| Data Table | Description | Link |
| :--- | :--- | :--- |  
| Users | Data on each user and their lifestyle | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQDOfQ2NaHrJT7bTmOMNOk3hAenB2wNTW-Nb-6C6aZrlmEE?e=acLMMP |
| mov | The movie and its attributes | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQD0yurptZC5SpJnKEgKrlfGAffz3DmeVaixxJalGnBjd9Y?e=KWQOnT |
| net_mov | The movie and its attributes (which are on Netflix) | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQCfeClmyVVAQp0g7HayvDZlAfaHQqgVVvFcdZLJoxQ_xbQ?e=Q5ImnK |
| rat | The rating for each movie and who made it | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQAcgFOBUW4gTI02sv08j089AXlw0JSeCW_iUpCmzCKgR58?e=WFFwZZ |
| watch_history | The watch history of each user | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQCqrC6KYsPUS7dwKZXvHx5bAdbc4TjjBJ9wWSR5A37xUm8?e=2etYDf |

All data tables can be found in this folder [here](https://myuva-my.sharepoint.com/:f:/g/personal/jph4dg_virginia_edu/IgCkBPj3Qvv_RqwRktNJpfHWAd3jXUCRhCbCgvidEkNqbVM?e=aQWVLi).

#### Below is a data dictionary for each table in the database. 

**Users**
| Name | Datatype | Description | Example | Uncertainty |
| :--- | :--- | :--- | :--- | :--- |  
| userID | INT | Inique user identification code | 1 | N/A |
| job | INT | The job of the user | Customer Sales Representative | N/A |
| age | INT | The age of the user | 25 | +- 1 |
| salary | CHAR | The salary of the user | 33000 | +- 1000 |


**Rat**
| Name | Datatype | Description | Example | Uncertainty |
| :--- | :--- | :--- | :--- | :--- |
| userID | INT | The unique ID of the user | 1 | N/A |
| movieID | INT | The unique ID of the movie | 1 | N/A |
| rating | FLOAT | The rating the user gave the movie | 5.0 | N/A |
| timestamp | FLOAT | The time at which the review was made | 944249077.0 | N/A |

**watch_history**
| Name | Datatype | Description | Example | Uncertainty |
| :--- | :--- | :--- | :--- | :--- |
| userID | INT | The unique ID of the user | 1 | N/A |
| num_movies_watched | INT | The number of movies that user has watched | 141 | N/A |
| avg_rating | FLOAT | The average rating that a user gives a movie | 3.53 | +- 0.000005 |
| total_minutes_watched | INT | The total minutes this user has spent watching movies that they have reviewed | 16074 | +- 5 |


**mov**
| Name | Datatype | Description | Example | Uncertainty |
| :--- | :--- | :--- | :--- | :--- |
| movieID | INT | The unique ID of the movie | 1 | N/A |
| title | CHAR | The title of the movie | Toy Story | N/A |
| year | INT | The year the movie was released | 2000 | N/A |
| (no genres listed) | BOOL | If the movie did not have a genre listed | 1 | N/A |
| Action | BOOL | If the movie was Action genre | 1 | N/A |
| Adventure | BOOL | If the movie was Adventure genre | 0 | N/A |
| Animation | BOOL | If the movie was Animated | 1 | N/A |
| Children | BOOL | If the movie was Children genre | 0 | N/A |
| Comedy | BOOL | If the movie was Comedy genre | 1 | N/A |
| Crime | BOOL | If the movie was Crime genre | 1 | N/A |
| Documentary | BOOL | If the movie was Documentary genre | 1 | N/A |
| Drama | BOOL | If the movie was Drama genre | 1 | N/A |
| Fantasy | BOOL | If the movie was Fantasy genre | 0 | N/A |
| Film-noir | BOOL | If the movie was Film-noir genre | 1 | N/A |
| Horror | BOOL | If the movie was Horror genre | 1 | N/A |
| IMAX | BOOL | If the movie was IMAX genre | 1 | N/A |
| Musical | BOOL | If the movie was Musical genre | 0 | N/A |
| Mystery | BOOL | If the movie was Mystery genre | 0 | N/A |
| Romance | BOOL | If the movie was Romance genre | 0 | N/A |
| Sci-Fi | BOOL | If the movie was Sci-Fi genre | 1 | N/A |
| Thriller | BOOL | If the movie was Thriller genre | 1 | N/A |
| War | BOOL | If the movie was War genre | 0 | N/A |
| Western | BOOL | If the movie was Western genre | 1 | N/A |



**net_mov**
| Name | Datatype | Description | Example | Uncertainty |
| :--- | :--- | :--- | :--- | :--- |
| movieID | INT | The unique ID of the movie | 1 | N/A |
| title | CHAR | The title of the movie | Enola Homes | N/A |
| year | INT | The year the movie was released | 2000 | N/A |
| (no genres listed) | BOOL | If the movie did not have a genre listed | 1 | N/A |
| Action | BOOL | If the movie was Action genre | 1 | N/A |
| Adventure | BOOL | If the movie was Adventure genre | 0 | N/A |
| Animation | BOOL | If the movie was Animated | 1 | N/A |
| Children | BOOL | If the movie was Children genre | 0 | N/A |
| Comedy | BOOL | If the movie was Comedy genre | 1 | N/A |
| Crime | BOOL | If the movie was Crime genre | 1 | N/A |
| Documentary | BOOL | If the movie was Documentary genre | 1 | N/A |
| Drama | BOOL | If the movie was Drama genre | 1 | N/A |
| Fantasy | BOOL | If the movie was Fantasy genre | 0 | N/A |
| Film-noir | BOOL | If the movie was Film-noir genre | 1 | N/A |
| Horror | BOOL | If the movie was Horror genre | 1 | N/A |
| IMAX | BOOL | If the movie was IMAX genre | 1 | N/A |
| Musical | BOOL | If the movie was Musical genre | 0 | N/A |
| Mystery | BOOL | If the movie was Mystery genre | 0 | N/A |
| Romance | BOOL | If the movie was Romance genre | 0 | N/A |
| Sci-Fi | BOOL | If the movie was Sci-Fi genre | 1 | N/A |
| Thriller | BOOL | If the movie was Thriller genre | 1 | N/A |
| War | BOOL | If the movie was War genre | 0 | N/A |
| Western | BOOL | If the movie was Western genre | 1 | N/A |

#### Quantification of Uncertainty

There are multiple numerical columns that have no uncertainty associated with them as they are just identifiers. For example, all iD columns are numerical, but these are identifiers so there is no uncertainty associated with them. 

In the mov and net_mov data table, the year column has no uncertainty as it is just the year the movie was released. Each of the genres are just binary values (stored numerically), so they have no uncertainty. 

In the watch history data table, there is uncertainty for the avg_rating, as this was a calculated number and some numbers might be rounded off. As such, the uncertainty for this is +- 0.000005 rating points. The movies watched column is a sums and does not have any uncertainty. The total minutes watched column has some uncertainty as the length of each movie was synthetically generated. The uncertainty for this is +- 5 minutes. 

In the rating table, there is no uncertainty in the rating column because these are just user inputted values. There is also no uncertainty in the time stamp. 

 In the users data table, there is uncertainty in the age as this was a synthetic number generated. Any uncertainty is around if it accurately reflects the population of movie watchers. The uncertainty is +-1 year. The uncertainty for the salary is +-$1,000 as this is also a synthetically generated number.

 Each of these uncertainties are shown in the table above
