# Data Cleaning Challenge- FIFA 21
A messy and raw dataset dataset of EA Sports' latest instalment of their hit FIFA series - FIFA21 cleaned using Power Query in Power BI


![download](https://user-images.githubusercontent.com/111463558/226396225-219c7ed0-7436-438c-ae7b-75c32cb10c8e.png)


## Introduction
Identifying and fixing errors in data is what data cleaning is all about. Cleaning data involves transforming any data that is inaccurate, incomplete, duplicated, inaccurate, or irrelevant to the project's analysis. In this way, we can maintain high levels of data integrity. 
This a documentation of the cleaning process of FIFA 21 dataset, the dataset contains a total of 77 columns and 18,979 rows of player statistics, contract information, demography and URL that directs the analyst to the player profile on Soffia to give further information in process of analysis.
The dataset can be found on [kaggle](https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring) alongside the data dictionary.



#### Aim of Project
1. Ensure accuracy of data
2. Eradicate errors in data
3. Ensure completeness of data
4. Make data easier to interpret for further analysis

The ETL process was used in the cleaning of the data, preferred tool for cleaning process was Power Query in Power BI.

ETL process:
- Extract
- Transform 
- Load



### Extract
The first step of the ETL process is "Extract", this is done by importing the CSV file into Power BI to start the transformation process, but first an audit of the data was carried out.

![Initial Import](https://user-images.githubusercontent.com/111463558/226297874-105de1cc-d1b1-4f9a-a325-aa43a2901cf2.png)

A data audit involves the understanding of the data in order to identify the inconsistencies, errors and missing values in the dataset. Following the Audit, the following columns were marked for transformation; Name, Longname , Age , OVA , POT , BOV,  Club , Contract , Positions , Height , Weight , Best position , Joined , Loan End date , Value, wage , release clause , W/F , SM , IR , and Hits.




### Transform
Before cleaning columns as stated above, the whitespaces in the dataset were removed by unchecking the "Show Whitespace" box in the View tab

![Whitespaces removed](https://user-images.githubusercontent.com/111463558/226299036-d32bcc3b-e18f-4a3e-907d-831117515da2.png)

The following steps were carried out.



#### 1. Names
The "Names" and "Longname" columns were cleaned and corrected to "Commentator Name" and "Full Name", Commentator name is the name used by commentators, pundits and fans in the footballing world as players' full names are not used when talking about a player, it therefore makes for better comprehension during visualization process by readers and analyst alike. Full name column was extracted from the URL column of players to get a cleaner version of dialectic names so as not to encounter problems with sorting during visualizations. Double commentator names like Neymar Jr, Van Dijk were kept as is.

| Before | After |
|--------|-------|
![Name](https://user-images.githubusercontent.com/111463558/226304063-409c8441-5a10-4347-ac98-cee41e1bbd65.png)|![Name clean](https://user-images.githubusercontent.com/111463558/226341431-00d273bd-7fcd-457a-a43d-3fe4a8e5449a.png)



#### 2. Age
Ages recorded for players in the dataset were from 2020, to ensure relevancy and the up-to-date nature of the data with respect to 2023, 3 years were added to each age
Note - FIFA 21 was released August 2020, so data collected was of 2020.

| Before | After |
|--------|-------|
![Age](https://user-images.githubusercontent.com/111463558/226343065-c2543986-a050-4a76-9fcf-1a49b42de0ce.png)|![Age Clean](https://user-images.githubusercontent.com/111463558/226343186-41fc7565-18ef-4154-8ef8-d98330466748.png)



#### 3. Percentages
These include the players’ overall analyses, Players’ potentials and the best overall which described by the data dictionary are recorded in %, headers were also changed to enhance comprehension.

| Before | After |
|--------|-------|
![Percentages](https://user-images.githubusercontent.com/111463558/226364033-7045b106-d6a8-432a-8b44-469f2de32569.png)|![Percentages clean](https://user-images.githubusercontent.com/111463558/226364114-a5f13b9c-7134-4964-84f1-036303ca301b.png)



#### 4. Clubs
Some clubs had number inaccurately inputted with their names as seen in the before picture, this was corrected using an if statement and split by delimiter

| Before | After |
|--------|-------|
![Club](https://user-images.githubusercontent.com/111463558/226362280-8fd28409-0fd0-4558-bc9b-7f88cf6a08a7.png)|![Club Clean](https://user-images.githubusercontent.com/111463558/226362330-48943060-28d9-4899-8227-09c271681067.png)


Also like in the "Name" column, some clubs had names with accents, and this would affect the sorting alphabetically and visualizations, The accents were removed and replaced by creating a function in power query.

Function by [DenisSipchenko](https://community.powerbi.com/t5/user/viewprofilepage/user-id/380661)

```
= (textToConvert) =>
let 
    textBinary = Text.ToBinary  (textToConvert,              1251 ),
    textASCII  = Text.FromBinary(textBinary   , TextEncoding.Ascii)
in    
    textASCII
```

![DenisSipchenko_0-1650635522413](https://user-images.githubusercontent.com/111463558/226362046-732bd5f5-835d-40a9-8be9-638c06d2c891.png)



| Before | After |
|--------|-------|
![club 2](https://user-images.githubusercontent.com/111463558/226362501-4efbd291-2460-4b26-ac31-32a4a7b580f2.png)|![Club 2 clean](https://user-images.githubusercontent.com/111463558/226362566-f6978fca-4468-4425-b721-206886369403.png)




#### 5. Contract
The contract situation data recorded for players was split across 3 columns namely: "Contracts”, "Joined" , and "Loan Date End". The "Contracts" column contains the start and end year of a contract and the end loan year of players on loan. "Joined" column shows the year a player joined a club, and the "Loan Date End" column shows the loan end date of a player. Using the filter view players whose "Contract" specify they are on loan had similar values with the “Loan Date End”, and players whose contract specify they are on free contracts corresponds with clubless players with no value, wage or release clause. 


Firstly, the "Contract" column was split by delimiter to "Contract Start" and "Contract End" and using the "Joined" and "Loan Date End" column to fill out the contract start and years for players on loan. A column was then create to specify the type of contract a player was on (Permanent, Loan or free)

"Joined" and "Loan Date End" were then dropped, and Contract columns kept in text format as formatting it as a date column will lead to incorrect months and day values.

| Before | After |
|--------|-------|
![Contract](https://user-images.githubusercontent.com/111463558/226374272-a3357f06-554c-4322-be6e-2ed509706615.png)|![Contract Clean](https://user-images.githubusercontent.com/111463558/226374324-963cbddd-d519-40b9-81fd-898e10ce6fa7.png)




#### 6. Position
There was a decision to drop the “Positions” column because the best position had already been given to each footballer and would be sufficient for analysis considering there was not sufficient data to determine the frequency of alternative positions played in.

| Before | After |
|--------|-------|
![Best Position](https://user-images.githubusercontent.com/111463558/226375329-a2956d84-a3ac-4efd-91f6-b246bc27b6fa.png)|![Best Position Clean](https://user-images.githubusercontent.com/111463558/226375387-93f4e81e-45bd-4e65-9b5e-c2abf8c93a23.png)




#### 7. Height and Weight
- Height
Values in the height column were recorded in 2 different units namely: centimetres (cm), feet (ft), and inches (inch). Decision was made to have all values in cm. This was achieved by splitting columns by delimiter into "cm", "ft" and "inches", converting "ft" and "inches" to "cm", rounding up to the nearest whole number, and merging the columns after dropping "cm" text from other values. 
Datatype was formatted to numeric after merge completion.

Formula for conversion of feet and inches to cm: 
(([ft]*12) + [inch]) * 2.54


- Weight
Like Height, values were recorded in kilograms (kg) and pounds (lbs). Decision was made to have all values in kg, this was achieved by splitting the columns into the 2 different units and multiplying the "lbs" column by 0.454, rounding up to the nearest whole number and merging the columns after dropping the "kg" text from other values.
Datatype was formatted to numeric after merge completion.


| Before | After |
|--------|-------|
![Height and Weight ](https://user-images.githubusercontent.com/111463558/226385595-8d721506-a889-4e05-a885-f61c22c6e079.png)|![Height and Weight clean](https://user-images.githubusercontent.com/111463558/226385659-8299df58-cb15-421c-9a28-020f57dcb97d.png)




#### 8. Value, Wage and Release clause.
There are three columns containing euro values in shortening format: 1,5k and 1.5m for 1 million. Due to the goal of standardizing the columns and converting to US dollars, the euro symbol was dropped from all rows using find and replace, then if a custom columns was created to multiply values with K and M by 1000 and 1,000,000 respectively.
After standardizing values, conversion from euros to USD was made by multiplying values by 1.183 (This was the conversion rate of euros to USD as of the time data was recorded)

| Before | After |
|--------|-------|
![Money](https://user-images.githubusercontent.com/111463558/226388614-f5caabed-4150-4b90-b939-d2c554cb8407.png)|![Money Clean](https://user-images.githubusercontent.com/111463558/226388658-219fbd86-5777-43ed-965d-46856b495f7e.png)




#### 9. WF, S/M, IR
Columns WF, S/M and IR refereed to the rating of the player's weak foot ability, skill moves and injury ration on a scale of 1-5, The row symbols were however encoded as stars, which were removed with the replace function and the data type changed to numeric. Headers were also changed for better comprehension of readers and analysts for visualization purposes.

| Before | After |
|--------|-------|
![Star Ratings](https://user-images.githubusercontent.com/111463558/226390284-1d40317c-c91c-4236-8a4b-bb34501889ab.png)|![Star Ratings clean](https://user-images.githubusercontent.com/111463558/226390330-5884d800-9d1c-4f44-96ec-96cdf90da1ea.png)


#### 10. Hits
Similar to the "Value", "Wages" and "Release Clause" column, some values were encoded as 1.5k to mean 1500. formula was used to standardize values by multiplying values with k by 1000.

| Before | After |
|--------|-------|
![Hits](https://user-images.githubusercontent.com/111463558/226391055-136dbfb8-93a2-4d56-aa92-9d13dad0553d.png)|![Hits clean](https://user-images.githubusercontent.com/111463558/226391096-affa01f2-b9d6-484f-a6ce-a67502a88af4.png)


### Load
After transformation of data using power query, data was loaded to Power BI platform for visualization using the "Close & Apply” button.



## Conclusion
Data cleaning is the very foundation of efficient data analysis, I am always enthusiastic when I can sharpen up my analytical skills. I am open to connecting with fellow analyst for suggestions, feedback, and collaboration.

I can be reached via [LinkedIn](https://www.linkedin.com/in/mega-erivona/) and email at megaerivona@gmail.com









