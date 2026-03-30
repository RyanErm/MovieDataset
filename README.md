# DS 4320 Project 1 - MovieDataset
Ryan Ermovick - jph4dg

Executive Summary:
WRITE ME OUT

CREATE A DOI


also note that external users must download the data and adjust the file paths @!!!!


[Data Set](https://myuva-my.sharepoint.com/:f:/g/personal/jph4dg_virginia_edu/IgAh618Sh_cBTq5wyoayZTHZAQEe_mdXZ4E6tP_DfwqapeA?e=29mNt4)

[Press Release](press_release.md)
[Pipeline](pipeline.md)
[License](LICENSE) WHAT DO I Name it???


## Problem Definition
Initial Problem: Netflix Content Reccommendation

Refined Specific Problem - What content should be recommended for Netflix to add to their platform increase user engagement in the platform?


The rationale behind this convergence is getting to the root of the problem. Netflix is one of many streaming services now available to users. As such, Netflix must keep their platform stocked with popular movies and tailored recommendation systems to promotes these movies to users. So, the refined goal for content recommendation is to use past user's movie ratings to determine what movies should be added to netflix and what should be recommended to users. Having interesting and well liked content on Netflix's platform is beneficial to them as users will stay on the platform longer and possibly recommend the platform/specific movies to friends/families. Additionally, having tailored recommendations achieves this goal as well. Overall, the convergence of the problem comes from the root objective of keeping people engaged on the platform and the idea that personalized recommendations are superior to broad ones. 


The motivation for this project is personal. Watching tv is one of my favorite things to do, but I often am looking for new content to consume. Sometimes the recommendations that Netflix or other platforms provides are not effective. This is bad for me, because I am not getting the content that I want, and for Netflix because I am not engaged on their platform and might shift to platforms. In the end, it is of the benefit to the both of us to have an effective recommendation system and well liked content to ensure user engagement and satisfaction. 


[A better way to find tv!](press_release.md)

## Domain Exposition
Jargon Table
| Term/KPI | Definition | Term or KPI |
| :--- | :--- | :--- |  
| Netflix | A video streaming platform. | Term |
| User | Subscribers of the Netflix streaming platform | Term |
| Content Recommendation | A personalized list of shows or movies for a user to watch on netflix | Term |
| Rating | How many stars the user rated the piece of content on Netflix | KPI |
| Content | A movie, show, or other piece of media on Netflix (or other streaming platform) | Term |
| Favorites List | If the user has the show saved on their favorite list of content on Netflix | Term |
| Previously Watched | If the user has already watched any part of the content or not | Term |


This project lives in the domains of entertainment and content recommendation algorithms. Since the goal is to give recommendations to users for entertainment, naturally this project would be important to entertainment companies. Specifically streaming services that want users to stay on their platform. This industry has gotten to be extremely competitive nowadays with many new streaming services popping up (HBO Max, Hulu, Disney +, Paramount +, etc.). Since these are fixed price services, they want people to enjoy their platform and have interesting content so that users will get other people to subscribe. Also, the domain of content recommendations algorithms has had to evolve along with this. Since there is a huge amount of shows on each platform, users need a way to find the ones that interest them in an easy manner.

[Background Reading](https://myuva-my.sharepoint.com/:f:/g/personal/jph4dg_virginia_edu/IgDHbT83FJg4SYEykl-iIskoAXHbN_KretkzzvqoBKgnKy0?e=nBd5rL)

| Title | Description | Link |
| :--- | :--- | :--- |  
| We delve into the exciting history of Netflix: <Br> How a global entertainment leader was forged | This article reviews the timeline of Netflixs creation <br> and how it has evolved from a DVD service to a streaming platform. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQByl0XGM0rdQYFDzKTttHtIAcNBrgTSpqHvAxOrnX8H8b0?e=cYfaGe |
| Recommendations | This is a short article from Netflix itself about its reccomendations. <br> Reading this article could be helpful in determining Netflix's motivation <br> and logic behind decisions. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQBHxVlUQJXfQZIljCO6lfgGAY-ykEcAYR3Pphxz35upht8?e=1LLiV4 |
| Winning the Streaming Wars: Are Megahits Like Stranger Things the Answer? | This article describes the complex business landscape of <br> streaming services and how Netflix fits into it. Additionally, this article might <br> shed light as to why Netflix would want to update their reccomendation system. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQA6zyoxBaKzR5I74m2ogfCRAQqASUKhVumINAZMRsfls2Q?e=atWRUP |
| Netflix’s Recommendation Systems: Entertainment Made for You | This article describs how reccomendation systems actually work. <br>  Reading this article could <br> inspire new ideas for a novel reccomendation system. | https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQAE66UeCebQQ5joZwi4d_GWAdka0ia8yry3m7iONrkGXrU?e=rSOr41 |
| Is Netflix actually bad at recommendations… <br> or is the algorithm intentionally limiting what we see? | This is a reddit thread where users are describing the issues that they have <br> with Netflix's reccomendation system. These reviews are extremely helpful <br> in determining how to create the new reccomendation system. |  https://myuva-my.sharepoint.com/:w:/g/personal/jph4dg_virginia_edu/IQA7hxjsUc_IQr52MJNlDJ5OAULHsILQ5_A72Gu-evt_goU?e=bWAZ3l |

## Data Creation

The data aquisition process for this project mainly entitled gathering data from the movie lens dataset (32 million record version). The files were then downloaded and unpacked. To start, the movies and the ratings files were split up. The movies file became mov and only contains information on movies. The users file has a unique row for each user. Then the users file was supplemented with synthetic data to mimic their jobs, salary, and age. The ratings data table became rat and has ratings from all the users. Finally, another datatable was created that describes users watching history.

Another dataset was also used for this project. A dataset on Kaggle was found that contains the movies present on netflix. This dataset became the net_mov dataset after it was filtered and joined to have the same column types as the mov datatable. 

For all of the gathered data, there is some room for bias to occur. All of the ratings came from the movie lens dataset, and were provided by actual users. Bias could have come from people intentionally rating a movie high or low (love/hate the movie) or not including all movie watchers. For the synthetic data, bias could have come from not having random data or not accurately representing the human population. There is not room for bias in the user watching history table as that was just calculated data.


To combat the bias of possibly not including enough users, a dataset of more than 32 million reviews was used. For the synthetic data, care was taken to ensure that the numbers created were not totally random. Research was done as to what the top jobs are in the world and what their mean salaries were. This way the data here is representative of the global population. Finally, as stated before, there is not room for bias in the user watching history table as it is just calculated data.

I decided to make a synthetic data table because an older version of this dataset had such characteristics about the users. It seemed that it would add an extra level of complexity to the data, so I decided to recreate it, despite knowing that it might cause bias down the line. Additionally, Netflix would not have this information about users unless they willingly provided, so if they wanted to recreate this model, it would have to be synthetic data as well. I also decided to create a calculated data table because I think that these would be good statistics to help with a recommendation model. I choose to make a Netflix datatable mainly because it would help with data separation down the line. If the model were to be trained on only netflix movies or non netflix movies, the two groups would have already been split up. 

## Metadata
![erd](erd.png)


ADD IN NEW LINKS ADD IN NEW LINKS
| Data Table | Description | Link |
| :--- | :--- | :--- |  
| Users | Data on each user and their lifestyle | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQCIeRJJ3Jk2Rb5333RJ2jEBAZeZkViJv8N9qzHHO1un06w?e=fYcVfD |
| mov | The movie and its attributes | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQAC9-oW7ojSTqXB7PYewQzfAWTznKasmnefbAJKhhWaHO4?e=OZ0Kbu |
| net_mov | The movie and its attributes (which are on Netflix) | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQAEIofVw4x9SqmcVEosZinWAd03AKwx_VGwmaDww9sv9DA?e=PMZum7 |
| rat | The rating for each movie and who made it | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQBmU-SqcfthTqMW1l4XOJdcAUuE-7Q0ZWimtSZikxVIutA?e=bn1hfg |
| watch_history | The watch history of each user | https://myuva-my.sharepoint.com/:u:/g/personal/jph4dg_virginia_edu/IQDJCLuXHYLLSqEgRQDB_KsFAYDmYoZCvq711399jV4m-T0?e=uAqY8j |


Users
| Name | Datatype | Description | Example |
| :--- | :--- | :--- | :--- |
| userID | INT | Inique user identification code | 1 |
| job | INT | The job of the user | Customer Sales Representative |
| age | INT | The age of the user | 25 |
| salary | CHAR | The salary of the user | 33000 |


Rat
| Name | Datatype | Description | Example |
| :--- | :--- | :--- | :--- |
| userID | INT | The unique ID of the user | 1 |
| movieID | INT | The unique ID of the movie | 1 |
| rating | FLOAT | The rating the user gave the movie | 5.0 |
| timestamp | FLOAT | The time at which the review was made | 944249077.0 |

watch_history
| Name | Datatype | Description | Example |
| :--- | :--- | :--- | :--- |
| userID | INT | The unique ID of the user | 1 |
| num_movies_watched | INT | The number of movies that user has watched | 141 |
| avg_rating | FLOAT | The average rating that a user gives a movie | 3.53 |
| total_minutes_watched | INT | The total minutes this user has spent watching movies that they have reviewed | 16074 |


mov

| Name | Datatype | Description | Example |
| :--- | :--- | :--- | :--- |
| movieID | INT | The unique ID of the movie | 1 |
| title | CHAR | The title of the movie | Toy Story |
| year | INT | The year the movie was released | 2000 |
| (no genres listed) | BOOL | If the movie did not have a genre listed | 1 |
| Action | BOOL | If the movie was Action genre | 1 |
| Adventure | BOOL | If the movie was Adventure genre | 0 |
| Animation | BOOL | If the movie was Animated | 1 |
| Children | BOOL | If the movie was Children genre | 0 |
| Comedy | BOOL | If the movie was Comedy genre | 1 |
| Crime | BOOL | If the movie was Crime genre | 1 |
| Documentary | BOOL | If the movie was Documentary genre | 1 |
| Drama | BOOL | If the movie was Drama genre | 1 |
| Fantasy | BOOL | If the movie was Fantasy genre | 0 |
| Film-noir | BOOL | If the movie was Film-noir genre | 1 |
| Horror | BOOL | If the movie was Horror genre | 1 |
| IMAX | BOOL | If the movie was IMAX genre | 1 |
| Musical | BOOL | If the movie was Musical genre | 0 |
| Mystery | BOOL | If the movie was Mystery genre | 0 |
| Romance | BOOL | If the movie was Romance genre | 0 |
| Sci-Fi | BOOL | If the movie was Sci-Fi genre | 1 |
| Thriller | BOOL | If the movie was Thriller genre | 1 |
| War | BOOL | If the movie was War genre | 0 |
| Western | BOOL | If the movie was Western genre | 1 |



net_mov

| Name | Datatype | Description | Example |
| :--- | :--- | :--- | :--- |
| movieID | INT | The unique ID of the movie | 1 |
| title | CHAR | The title of the movie | Enola Homes |
| year | INT | The year the movie was released | 2000 |
| (no genres listed) | BOOL | If the movie did not have a genre listed | 1 |
| Action | BOOL | If the movie was Action genre | 1 |
| Adventure | BOOL | If the movie was Adventure genre | 0 |
| Animation | BOOL | If the movie was Animated | 1 |
| Children | BOOL | If the movie was Children genre | 0 |
| Comedy | BOOL | If the movie was Comedy genre | 1 |
| Crime | BOOL | If the movie was Crime genre | 1 |
| Documentary | BOOL | If the movie was Documentary genre | 1 |
| Drama | BOOL | If the movie was Drama genre | 1 |
| Fantasy | BOOL | If the movie was Fantasy genre | 0 |
| Film-noir | BOOL | If the movie was Film-noir genre | 1 |
| Horror | BOOL | If the movie was Horror genre | 1 |
| IMAX | BOOL | If the movie was IMAX genre | 1 |
| Musical | BOOL | If the movie was Musical genre | 0 |
| Mystery | BOOL | If the movie was Mystery genre | 0 |
| Romance | BOOL | If the movie was Romance genre | 0 |
| Sci-Fi | BOOL | If the movie was Sci-Fi genre | 1 |
| Thriller | BOOL | If the movie was Thriller genre | 1 |
| War | BOOL | If the movie was War genre | 0 |
| Western | BOOL | If the movie was Western genre | 1 |

