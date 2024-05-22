# Election_Data_Analysis 📊

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/e2b2baf9-fd92-4c75-b0c3-d8ca30f6956b)

### Contents 📖
- Problem statement
- Tools used
- Data cleaning
- Data analysis & Visualization
- Recommendations

### Problem Statement  
AtliQ Media, a private media company wants to telecast a show on Lok Sabha elections in 2024 in India. Unlike other channel they do not want to have debate on who is going to win the elections, they rather wanted to present the insights from 2014 and 2019 elections without any bias and discuss the less explored themes like voter turnout percentage in India.

### Tools used 🛠️

- Programming Language: Python
- Libraries: Pandas, Numpy, Matplotlib, Seaborn
- IDE: Jupyter Notebook

### Data Cleaning

**1. Importing python libraries**

```python
import pandas as pd                                          # Pandas is a tool in python for data analysis
import numpy as np                                           # Numpy is a library in python for mathematical functions
import matplotlib.pyplot as plt                              # Matplotlib library in python for visualizations
import seaborn as sns                                        # Seaborn is a library in python for statistical graphs 
import plotly.express as px                                  # Plotly is a library in python for interactive visualizations
```

**2. Reading file which contains 2014 election data**

```python
df_2014=pd.read_csv(r"C:\Users\91961\Downloads\constituency_wise_results_2014.csv")     # reading 2014 file
df_2014.head(2)                                                                         # displaying 2 records of file
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/eb6112ee-6763-4133-b8e5-5eb1a5de32c4)


**3. Reading file which contains 2019 election data**

```python
df_2019=pd.read_csv(r"C:\Users\91961\Downloads\constituency_wise_results_2019.csv")      # reading 2019 file
df_2019.head(2)                                                                          # displaying 2 records of file
```
Output -

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/f50152ad-5a3d-46ef-8f82-fa22e8ced43b)


**4. Reading file which contains state codes**

```python
df_sc=pd.read_csv(r"C:\Users\91961\Downloads\dim_states_codes.csv")       # Reading state_codes file
df_sc.head(2)                                                             # displaying 2 records of file

df_sc.rename(columns = {'state_name':'state'}, inplace = True)            # Replacing column name from state_name to state to sync with other files
df_sc.head(2)                                                             # displaying 2 records of file after changing column name
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/34972ad4-cdde-4da8-b5d6-3a477193bb6d)

- Pandas.```read_csv()``` function is used to load a comma-separated values (CSV) file into a DataFrame object. We have to provide the path where the csv file is stored.
- Pandas ```head()``` method is used to return top n (5 by default) rows of a data frame or series.
- ```inplace=True``` is used to make changes to the original data without returning a copy of the data.


**5. Checking the datatypes of all columns in 2014 and 2019 files**

```python
df_2014.dtypes                                                 # Obtaining data types of all columns of 2014 file
df_2019.dtypes                                                 # Obtaining data types of all columns of 2019 file
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/891950eb-d37b-4ca3-b300-7abb6b2adf1c)

- In the process of data cleaning, it is essential to check the data types of all columns. dtypes is used to find out the data type (dtype) of each column in the given DataFrame
- A column's datatype can be changed if we find that the current data type is not relevant to the values in the column. In this project we are not changing any column's data types since they are already suitable for the values present.


**6.Verifying for all null values in columns**

```python
df_2014.isnull().sum()                                         # Null vales found in sex, age & category columns because of 'NOTA"
df_2019.isnull().sum()                                         # Null vales found in sex, age, category & party_symbol columns because of 'NOTA"
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/e45f65aa-b525-4109-bd43-a72a9d64f227)

- Null values in the data can skew the results resulting in inaccurate conclusions.
- In the process of data cleaning, we can replace the null values with median values in case of numerical values and mode in case of categorical values.
- We can also delete the rows or columns containing null values if they are not necessary for the analysis.
- In this project, we are retaining the null values, since those rows belong to NOTA category.

**7. Obtaining unique values**

```python
# Obtaining unique state values and their count
df_2014['state'].nunique()                       # nunique is used to obtain count of unique values in a column in dataframe
df_2014['state'].unique()                        # unique is used to obtain unique values in a column in dataframe
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b13e3f01-8cce-4313-9a4f-1cfbf87aa557)

**8. Replacing state name for Telengana constituencies in 2014 dataframe**

```python
# Obtaining constituency names from Telengana state from 2019 dataframe
df_2019[df_2019['state']=='Telangana']['pc_name'].unique()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b019292e-3ba3-4f14-9bd2-e4ff15062625)


```python
# Replacing the state name as Telegana from Andhra Pradesh to respective constituencies in 2014 dataframe becuase of bifurcation

telangana_pc_names=['Adilabad ', 'Peddapalle ', 'Karimnagar ', 'Nizamabad',
       'Zahirabad', 'Medak', 'Malkajgiri', 'Secundrabad', 'Hyderabad',
       'CHEVELLA', 'Mahbubnagar', 'Nagarkurnool', 'Nalgonda', 'Bhongir ',
       'Warangal', 'Mahabubabad  ', 'Khammam ']
df_2014.loc[df_2014['pc_name'].isin(telangana_pc_names), 'state'] = 'Telengana'
df_2014['state'].unique()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b35256ab-7661-4643-aeca-9ca8932243b6)


**9. Replacing constituency names** 

```python
# Few states have same constituency names. So replacing constituency name followed by the respective state name to differentiate

counts = df_2014.groupby('pc_name').size()                                        # Obtaining the count of pc_name by using groupby

for index, row in df_2014.iterrows():                                             # Iterating through the DataFrame and appending the state name
    if counts[row['pc_name']] > 1:
        df_2014.at[index, 'pc_name'] = f"{row['pc_name']}_{row['state']}"
```

```python
# Few states have same constituency names. So replacing constituency name followed by the respective state name to differentiate

counts = df_2019.groupby('pc_name').size()                                         # Obtaining the count of pc_name by using groupby

for index, row in df_2019.iterrows():                                               # Iterating through the DataFrame and appending the state name
    if counts[row['pc_name']] > 1:
        df_2019.at[index, 'pc_name'] = f"{row['pc_name']}_{row['state']}"
```

**10. Obtaining the deecriptive statistics for the dataframe

```python
df_2014.describe()                           # describe function provides details of descriptive statistics of all numerical columns 
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/4e8583ba-000e-40d2-bc84-44febc3ae2b3)


### Data analysis & visualization

**1. List top 5/bottom 5 constituencies of 2014 and 2019 in terms of voter turnout ratio**  

```python
voter_turnout_ratio_2014=round(((df_2014.groupby('pc_name').total_votes.sum())/
                           (df_2014.groupby('pc_name').total_electors.max()))*100,2)         # Calculating voter turn out ratio by constituency
#voter_turnout_ratio_2014

voter_turnout_ratio_sorted_2014 = voter_turnout_ratio_2014.sort_values(ascending=False)      # Sorting voter turn out ratio

top_5_2014 = voter_turnout_ratio_sorted_2014.head(5)                                         # Obtaining top 5 and bottom 5 records
top_5_2014

bottom_5_2014 = voter_turnout_ratio_sorted_2014.tail(5)
bottom_5_2014

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))                                  # Creating a figure with two plots

top_5_2014.plot(kind='bar', ax=axes[0], color='skyblue')                                     # Plotting the top 5 rows with bar chart
axes[0].set_title('Top 5 constituencies with highest voter turnout ratio in 2014')
axes[0].set_xlabel('Parliamentary Constituency')
axes[0].set_ylabel('Voter Turnout Ratio (%)')

bottom_5_2014.plot(kind='bar', ax=axes[1], color='salmon')                                   # Plotting the bottom 5 rows with bar chart
axes[1].set_title('Bottom 5 constituencies with lowest voter turnout ratio in 2014')
axes[1].set_xlabel('Parliamentary Constituency')
axes[1].set_ylabel('Voter Turnout Ratio (%)')
plt.tight_layout()                                                                           # Adjusting the layout
plt.show()                                                                                   # Displaying the plots
```

output :

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/a45e2747-4fb9-4c2d-9cf0-a7da52cfc0d7)


![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/5d1da9b2-8e06-44a8-ad7e-e8a12b8f4106)

```python
# Top 5 / Bottom 5 constituencies of 2019 in terms of voter turnout ratio

voter_turnout_ratio_2019=round(((df_2019.groupby('pc_name').total_votes.sum())/
                           (df_2019.groupby('pc_name').total_electors.max()))*100,2)         # Calculating voter turn out ratio by constituency

voter_turnout_ratio_sorted_2019 = voter_turnout_ratio_2019.sort_values(ascending=False)      # Sorting voter turn out ratio

top_5_2019 = voter_turnout_ratio_sorted_2019.head(5)                                         # Obtaining top 5 and bottom 5 records
bottom_5_2019 = voter_turnout_ratio_sorted_2019.tail(5)

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))                                  # Creating a figure with two plots

top_5_2019.plot(kind='bar', ax=axes[0], color='skyblue')                                     # Plotting the top 5 rows
axes[0].set_title('Top 5 constituencies with highest voter turnout ratio in 2019 ')
axes[0].set_xlabel('Parliamentary Constituency')
axes[0].set_ylabel('Voter Turnout Ratio (%)')

bottom_5_2019.plot(kind='bar', ax=axes[1], color='salmon')                                   # Plotting the bottom 5 rows
axes[1].set_title('Bottom 5 constituencies with lowest voter turnout ratio in 2019')
axes[1].set_xlabel('Parliamentary Constituency')
axes[1].set_ylabel('Voter Turnout Ratio (%)')

plt.tight_layout()                                                                           # Adjusting the layout

plt.show()                                                                                   # Displaying the plots
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/356b1b67-3641-490a-8746-ac72a12d093b)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/deed0ead-9295-4dd4-aeb7-31280d9c690a)

**2. List top 5/bottom 5 states of 2014 and 2019 in terms of voter turnout ratio**  

```python
# Top 5/ Bottom 5 states of 2019 in terms of voter turnout ratio
                                                                                
unique_df_2014 = df_2014.drop_duplicates(subset=['pc_name'])                                    # Dropping duplicate rows based on pc_name column
#unique_df

total_electors_by_state_2014 = unique_df_2014.groupby('state')['total_electors'].sum()          # Calculating total electors by state
#print(total_electors_by_state)

total_votes_by_states_2014 = df_2014.groupby('state')['total_votes'].sum()                      # Calculating total votes by state
#total_votes_by_states

voter_turnout_ratio_2014=round((total_votes_by_states/total_electors_by_state)*100,2)           # Calculating voter turnout ratio
#voter_turnout_ratio_2014

voter_turnout_ratio_2014_sorted = voter_turnout_ratio_2014.sort_values(ascending=False)         # Sorting voter turn out ratio

top_5_2014 = voter_turnout_ratio_2014_sorted.head(5)                                            # Obtaining top 5 and bottom 5 records
top_5_2014
bottom_5_2014 = voter_turnout_ratio_2014_sorted.tail(5)
bottom_5_2014

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))                                     # Creating a figure with two plots

top_5_2014.plot(kind='bar', ax=axes[0], color='turquoise')                                      # Plotting the top 5 rows
axes[0].set_title('Top 5 states with highest voter turnout ration in 2014')
axes[0].set_xlabel('state')
axes[0].set_ylabel('Voter Turnout Ratio (%)')
 
bottom_5_2014.plot(kind='bar', ax=axes[1], color='pink')                                        # Plotting the bottom 5 rows
axes[1].set_title('Bottom 5 states with lowest voter turnout ratio in 2014')
axes[1].set_xlabel('state')
axes[1].set_ylabel('Voter Turnout Ratio (%)')

plt.tight_layout()                                                                              # Adjusting the layout

plt.show()                                                                                      # Displaying the plots
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b97d9cca-e92a-47b0-869e-ef4f6ae174f4)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/c94e9001-8a48-49a2-ae41-760211fd768f)

```python
# Top 5/ Bottom 5 states of 2019 in terms of voter turnout ratio

unique_df_2019 = df_2019.drop_duplicates(subset=['pc_name'])                                      # Dropping duplicate rows based on pc_name column
#unique_df

total_electors_by_state_2019 = unique_df_2019.groupby('state')['total_electors'].sum()            # Calculating total electors by state
#print(total_electors_by_state)

total_votes_by_states_2019 = df_2019.groupby('state')['total_votes'].sum()                        # Calculating total votes by state
#total_votes_by_states

voter_turnout_ratio_2019=round((total_votes_by_states/total_electors_by_state)*100,2)             # Calculating voter turn out ratio
#voter_turnout_ratio_2019

voter_turnout_ratio_2019_sorted = voter_turnout_ratio_2019.sort_values(ascending=False)           # Sorting voter turn out ratio

top_5_2019 = voter_turnout_ratio_2019_sorted.head(5)                                              # Obtaining top 5 and bottom 5 records
top_5_2019
bottom_5_2019 = voter_turnout_ratio_2019_sorted.tail(5)
bottom_5_2019

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))                                       # Creating a figure with two plots

top_5_2019.plot(kind='bar', ax=axes[0], color='turquoise')                                        # Plotting the top 5 rows
axes[0].set_title('Top 5 states with highest voter turnout ration in 2019')
axes[0].set_xlabel('state')
axes[0].set_ylabel('Voter Turnout Ratio (%)')
 
bottom_5_2019.plot(kind='bar', ax=axes[1], color='pink')                                          # Plotting the bottom 5 rows
axes[1].set_title('Bottom 5 states with lowest voter turnout ratio in 2019')
axes[1].set_xlabel('state')
axes[1].set_ylabel('Voter Turnout Ratio (%)')

plt.tight_layout()                                                                                # Adjusting the layout

plt.show()                                                                                        # Displaying the plots
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/965c3a2d-28af-4773-ab58-d945aca8b757)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/95a4338c-a583-430c-8926-4738ce2ce17f)


**Creating a choropleth map to show voter turnout ration per states in 2019

```python
#Reading indian states shape file
shp_gdf = gpd.read_file('/kaggle/input/india-states/Indian_states.shp')  
#shp_gdf.head()

#Converting voter turn out ratio series into a dataframe with columns state & voter turnout ratio
df_sorted = pd.DataFrame({"State": voter_turnout_ratio_2019_sorted.index, "Voter Turnout Ratio": voter_turnout_ratio_2019_sorted.values})
#print(df_sorted.head())

merging shape df and voter turnout ration df using state column
merged = shp_gdf.set_index('st_nm').join(df_sorted.set_index('State'))
#merged.head()

#Plotting choropleth map
fig, ax = plt.subplots(1, figsize=(12, 12))
ax.axis('off')
ax.set_title('Voter turnout ratio per state in 2019',
             fontdict={'fontsize': '15', 'fontweight' : '3'})
fig = merged.plot(column='Voter Turnout Ratio', cmap='GnBu', linewidth=0.5, ax=ax, edgecolor='0.2',legend=True)
plt.show()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/a1e36981-2d31-4897-814a-8b43f490b3a7)




**3. Which constituencies have elected the same party for two consecutive elections, rank them by % of votes to that winning party in 2019**

```python
winning_party_2014_df=df_2014.loc[df_2014.groupby('pc_name')['total_votes'].idxmax()]     #Filtering 2014 data to obtain winning party per constituency

winning_party_2019_df=df_2019.loc[df_2019.groupby('pc_name')['total_votes'].idxmax()]     #Filtering 2019 data to obtain winning party per constituency

merged_df = pd.merge(winning_party_2014_df,winning_party_2019_df,on='pc_name',suffixes=('_2014','_2019')) # Merging 2014 & 2019 datframes by pc_name

matched_parties_df = merged_df[merged_df['party_2014'] == merged_df['party_2019']]        # Filtering the rows where party names match in both years

matched_parties_df['percentage_change'] = ((matched_parties_df['total_votes_2019'] -      # Calculating percentage change in votes from 2014 to 2019
                                            matched_parties_df['total_votes_2014']) /
                                           matched_parties_df['total_votes_2019']) * 100

ranked_df = matched_parties_df.sort_values(by='percentage_change', ascending=False)       # Sorting constituencies based on percentage change in votes

print(ranked_df[['pc_name', 'party_2014','total_votes_2014','party_2019','total_votes_2019', 'percentage_change']])
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/20b2f9fe-f01c-48f9-ab24-bc0ccbc4788a)

```python
#Plotting constituencies that elected same party for consecutive years by % Change in Votes

filtered_ranked_df=ranked_df.head(30)                                                                # Filtering first 30 rows

labels = [f"{constituency}-{party}" for constituency, party in 
          zip(filtered_ranked_df['pc_name'], filtered_ranked_df['party_2014'])]                      # Concatenating constituency and party for labels

plt.figure(figsize=(12, 8))
plt.plot(labels,filtered_ranked_df['percentage_change'], color='red', marker='o')                    # Plotting a line chart
plt.xlabel('Constituency-Party')
plt.ylabel('Percentage Change in Votes')
plt.title('Constituencies that elected same party for consecutive years by % Change in Votes')
plt.tick_params(axis='x', labelrotation=90) 
plt.grid(True)
plt.tight_layout()                                                                                   # Adjusting layout to prevent trimming of labels
plt.show()
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/270e090a-866d-45ba-bb44-c439dfd8fe8f)

**4. Which constituencies have voted for different partiesin two elections (list top 10 based on the difference (2019-2014) in voter percentage in two elections)**


```python
winning_party_2014_df=df_2014.loc[df_2014.groupby('pc_name')['total_votes'].idxmax()]     #Filtering 2014 data to obtain winning party per constituency

winning_party_2019_df=df_2019.loc[df_2019.groupby('pc_name')['total_votes'].idxmax()]     #Filtering 2019 data to obtain winning party per constituency

merged_df = pd.merge(winning_party_2014_df, winning_party_2019_df, on='pc_name', suffixes=('_2014', '_2019'))  # Matching pc_names  from 2014 and 2019

merged_df['max_total_votes']=merged_df[['total_votes_2014','total_votes_2019']].max(axis=1)   # Finding the maximum total votes between 2014 and 2019

different_parties_df = merged_df[merged_df['party_2014'] != merged_df['party_2019']]        # Filtering the rows where party names match in both years

different_parties_df['percentage_change'] = ((different_parties_df['total_votes_2019'] -      # Calculating percentage change in votes from 2014 to 2019
                                            different_parties_df['total_votes_2014']) /
                                           different_parties_df['max_total_votes']) * 100

different_ranked_df = different_parties_df.sort_values(by='percentage_change', ascending=False)    # Sorting constituencies based on % change in votes

print(different_ranked_df[['pc_name', 'party_2014','total_votes_2014','party_2019','total_votes_2019', 'percentage_change']])
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b54ac35e-07aa-4b5f-8e0b-38c6c57ae4df)

```python
#Plotting percentage Change in Votes from 2014 to 2019 by Constituency-Party Pair
filtered_ranked2_df=different_ranked_df.head(30)                                                     # Filtering first 30 rows

labels = [f"{constituency}-{party}" for constituency, party in 
          zip(filtered_ranked2_df['pc_name'], filtered_ranked2_df['party_2014'])]                    # Concatenating constituency and party for labels

plt.figure(figsize=(12, 8))
plt.plot(labels,filtered_ranked2_df['percentage_change'], color='blue',marker='o')                   # Plotting line chart
plt.xlabel('Constituency-Party')
plt.ylabel('Percentage Change in Votes')
plt.title('Percentage Change in Votes from 2014 to 2019 by Constituency-Party Pair')
plt.tick_params(axis='x', labelrotation=90)  
plt.grid(True)
plt.tight_layout()                                                                                   # Adjusting layout to prevent trimming of labels
plt.show()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/61bef1db-8b0e-4ea4-86f6-45b0a4635a63)


**5. Top 5 candidates based on margin difference with runners in 2014 and 2019**

```python
# Calculating the margin difference between the winner and runner for each constituency in 2014
winner_runner_margin_2014 = df_2014.groupby('pc_name').apply(lambda x: x.nlargest(2, 'total_votes', 'all')).reset_index(drop=True)
winner_runner_margin_2014['margin_difference'] = winner_runner_margin_2014.groupby('pc_name')['total_votes'].diff().abs()

# Sorting candidates based on margin difference and selecting top 5 candidates
top_5_candidates_2014 = winner_runner_margin_2014.nlargest(5, 'margin_difference')

print("Top 5 candidates based on margin difference with runners in 2014:")
print(top_5_candidates_2014[['pc_name', 'candidate', 'party', 'total_votes', 'margin_difference']])

#------------------------------------------------------------------------------------------------------------------------------------------------------#

# Calculating the margin difference between the winner and runner for each constituency in 2019
winner_runner_margin_2019 = df_2019.groupby('pc_name').apply(lambda x: x.nlargest(2, 'total_votes', 'all')).reset_index(drop=True)
winner_runner_margin_2019['margin_difference'] = winner_runner_margin_2019.groupby('pc_name')['total_votes'].diff().abs()

# Sorting candidates based on margin difference and selecting top 5 candidates
top_5_candidates_2019 = winner_runner_margin_2019.nlargest(5, 'margin_difference')

print("Top 5 candidates based on margin difference with runners in 2019:")
print(top_5_candidates_2019[['pc_name', 'candidate', 'party', 'total_votes', 'margin_difference']])
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/1d3d303b-ca1f-4fb8-ae11-e033993ce87e)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/8f44ebbb-c056-4a0d-b185-4f5d6f384789)







**6. % Split of votes of parties between 2014 vs 2019 at national level**

```python
party_votes_2014_df=df_2014.groupby('party')['total_votes'].sum().reset_index()                   # Calcualting total votes gained by a party
total_votes_2014=party_votes_2014_df['total_votes'].sum()                                         # Calculating total votes gained by all parties
party_votes_2014_df['vote_share_2014']=(party_votes_2014_df['total_votes']/total_votes_2014)*100  # Calculating vote share of each party
sorted_2014_df = party_votes_2014_df.sort_values(by='vote_share_2014', ascending=False).head(10)  # Sorting and filtering first 10 parties 

party_votes_2019_df=df_2019.groupby('party')['total_votes'].sum().reset_index()
total_votes_2019=party_votes_2019_df['total_votes'].sum()
party_votes_2019_df['vote_share_2019']=(party_votes_2019_df['total_votes']/total_votes_2019)*100
sorted_2019_df = party_votes_2019_df.sort_values(by='vote_share_2019', ascending=False).head(10)

# Creating two subplots one for 2014 and other for 2019 showing top 10 parties and their respective vote share

labels_2014 = sorted_2014_df['party']
sizes_2014 = sorted_2014_df['vote_share_2014']

labels_2019 = sorted_2019_df['party']
sizes_2019 = sorted_2019_df['vote_share_2019']

colors_2014 = ['skyblue','lightgreen','lightcoral','lightskyblue','lightpink','lightgreen','lightsalmon','lightblue']
colors_2019 = ['lightcoral','lightskyblue','lightgreen','lightpink','lightblue','lightsalmon','skyblue','lightgreen']

fig, axes = plt.subplots(nrows=1,ncols=2,figsize=(14, 6))                               

axes[0].pie(sizes_2014,labels=labels_2014,autopct='%1.1f%%',pctdistance=0.85,colors=colors_2014)        # Plotting a pie chart
axes[0].set_title('Vote Share by Party in 2014')

axes[1].pie(sizes_2019,labels=labels_2019,autopct='%1.1f%%',pctdistance=0.85,colors=colors_2019)
axes[1].set_title('Vote Share by Party in 2019')

plt.tight_layout()                                                                         
plt.show()
```
      
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/bc74e7fb-be3b-4fee-b5de-035d9d39a89a)

**7. % Split of votes of parties between 2014 vs 2019 at state level**

```python
state_votes_2014_df=df_2014.groupby(['state','party'])['total_votes'].sum().reset_index()                # Calcualting total votes gained by a party
total_state_votes_2014=state_votes_2014_df['total_votes'].sum()                                          # Calculating total votes gained by all parties
state_votes_2014_df['vote_share_2014']=(state_votes_2014_df['total_votes']/total_state_votes_2014)*100   # Calculating vote share of each party
sorted_states_2014_df = state_votes_2014_df.sort_values(by='vote_share_2014', ascending=False).head(20)  # Sorting and filtering parties 

# Creating an interactive horizontal bar chart using Plotly for vote share of parties in 2014
fig = px.bar(sorted_states_2014_df,x='vote_share_2014',y='state',color='party',             
             orientation='h',
             labels={'state': 'State', 'vote_share_2014': 'Vote Share (%)', 'party': 'Party'},
             title='Vote Share of Parties in Top States in 2014',
             hover_data={'vote_share_2014': ':.2f'})

fig.update_layout(yaxis={'categoryorder': 'total ascending'},
                  xaxis=dict(title='Vote Share (%)'),
                  legend_title='Party')
fig.show()

state_votes_2019_df=df_2019.groupby(['state','party'])['total_votes'].sum().reset_index()                # Calcualting total votes gained by a party
total_state_votes_2019=state_votes_2019_df['total_votes'].sum()                                          # Calculating total votes gained by all parties
state_votes_2019_df['vote_share_2019']=(state_votes_2019_df['total_votes']/total_state_votes_2019)*100   # Calculating vote share of each party
sorted_states_2019_df = state_votes_2019_df.sort_values(by='vote_share_2019', ascending=False).head(20)  # Sorting and filtering parties

# Creating an interactive horizontal bar chart using Plotly for vote share of parties in 2019
fig = px.bar(sorted_states_2019_df,x='vote_share_2019',y='state',color='party',             
             orientation='h',
             labels={'state': 'State','vote_share_2019': 'Vote Share (%)', 'party': 'Party'},
             title='Vote Share of Parties in Top States in 2019',
             hover_data={'vote_share_2019': ':.2f'})

fig.update_layout(yaxis={'categoryorder': 'total ascending'},
                  xaxis=dict(title='Vote Share (%)'),
                  legend_title='Party')

fig.show()
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/24e929b7-ad14-4dbf-be70-88115ea1bc44)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b9c934b7-d979-45c6-9a47-6282b2729418)

**8. List top 5 constituencies for two major national parties where they have gained vote share in 2019 as compared to 2014**

```python
national_parties = ['BJP', 'INC']    # Two major national parties

# Grouping by parliamentary constituency and party for both 2014 and 2019 by total votes
party_votes_2014 = df_2014.groupby(['pc_name', 'party'])['total_votes'].sum().unstack(fill_value=0)
party_votes_2019 = df_2019.groupby(['pc_name', 'party'])['total_votes'].sum().unstack(fill_value=0)

# Calculating vote share gained for each party from 2014 to 2019
vote_share_gained = party_votes_2019 - party_votes_2014

# Filtering records for only the two major national parties
national_party_votes_2014 = vote_share_gained[national_parties]

# Finding top 5 constituencies where each party gained vote share
top_constituencies_party1 = national_party_votes_2014.sort_values(by='BJP', ascending=False).head(5)
top_constituencies_party2 = national_party_votes_2014.sort_values(by='INC', ascending=False).head(5)

print("Top 5 constituencies where BJP gained vote share:")
print(top_constituencies_party1)

print("\nTop 5 constituencies where INC gained vote share:")
print(top_constituencies_party2)
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/b27d0bad-b902-4a73-a4d2-fe3a452b5b96)

```python
# Plotting horizontal bar chart for top 5 Constituencies where BJP Gained Vote Share
plt.figure(figsize=(10, 6))
plt.barh(top_constituencies_party1.index, top_constituencies_party1['BJP'], color='turquoise')
plt.xlabel('Vote Share Gained')
plt.title('Top 5 Constituencies where BJP Gained Vote Share')
plt.gca().invert_yaxis()                                                                    # Inverting y-axis to display top constituencies at the top
plt.show()

# Plotting horizontal bar chart for top 5 Constituencies where INC Gained Vote Share
plt.figure(figsize=(10, 6))
plt.barh(top_constituencies_party2.index, top_constituencies_party2['INC'], color='pink')
plt.xlabel('Vote Share Gained')
plt.title('Top 5 Constituencies where INC Gained Vote Share')
plt.gca().invert_yaxis() 
plt.show()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/4cc0345c-66a1-4124-ba3c-072d2e2f3cee)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/7a22dec6-045e-4814-9d1d-74e7f9039ca3)

**9. List top 5 constituencies for two major national parties where they have lost vote share in 2019 as compared to 2014**

```python
national_parties = ['BJP', 'INC']  

# Grouping by parliamentary constituency and party for both 2014 and 2019
party_votes_2014 = df_2014.groupby(['pc_name', 'party'])['total_votes'].sum().unstack(fill_value=0)
party_votes_2019 = df_2019.groupby(['pc_name', 'party'])['total_votes'].sum().unstack(fill_value=0)

# Calculating vote share change for each party from 2014 to 2019
vote_share_change = party_votes_2019 - party_votes_2014

# Filtering out rows where the vote share change is positive or zero replacing them with null values
vote_share_change[vote_share_change >= 0] = np.nan

# Filtering rows for only the two major national parties
national_party_votes_lost = vote_share_change[national_parties]

# Finding top 5 constituencies where each party lost vote share
top_constituencies_party1_lost = national_party_votes_lost.sort_values(by='BJP', ascending=True).head(5)
top_constituencies_party2_lost = national_party_votes_lost.sort_values(by='INC', ascending=True).head(5)

print("Top 5 constituencies where BJP lost vote share:")
print(top_constituencies_party1_lost)

print("\nTop 5 constituencies where INC lost vote share:")
print(top_constituencies_party2_lost)
```

Ouput - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/369fbddf-37c6-4805-b3e9-4e2f8b3cd5f5)

```python
# Plot horizontal bar chart for top 5 constituencies where BJP Lost Vote Share
plt.figure(figsize=(10, 6))
plt.barh(top_constituencies_party1_lost.index, top_constituencies_party1_lost['BJP'], color='turquoise')
plt.xlabel('Vote Share Lost')
plt.ylabel('Constituency')
plt.title('Top 5 Constituencies where BJP Lost Vote Share')
plt.gca().invert_yaxis()                                                           # Invert y-axis to display top constituencies at the top
plt.show()

# Plot horizontal bar chart for top 5 constituencies where BJP Lost Vote Share
plt.figure(figsize=(10, 6))
plt.barh(top_constituencies_party2_lost.index, top_constituencies_party2_lost['INC'], color='pink')
plt.xlabel('Vote Share Lost')
plt.ylabel('Constituency')
plt.title('Top 5 Constituencies where INC Lost Vote Share')
plt.gca().invert_yaxis()                                                            # Invert y-axis to display top constituencies at the top
plt.show()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/6fbbbb89-1647-4ffa-a8f6-ec4c81b79f21)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/41f4d275-b110-4e58-9dce-e3b0d3846a5a)

**10. Which constituency has voted the most for NOTA?**

```python
filtered_df = df_2014[df_2014['candidate'] == 'None of the Above']                 # Filtering DataFrame to select  rows where candidate is 'NOTA'

sorted_df = filtered_df.sort_values(by='total_votes', ascending=False).head(10)    # Sorting the filtered DataFrame by total_votes in descending order
                                                      
fig = px.bar(sorted_df, x='pc_name', y='total_votes', 
             text='total_votes', height=500,                                     
             title='Total NOTA votes per constituency in 2014')                    # Paasing slice of sorted dataframe to plot bar chart

fig.update_traces(textposition='inside')                                           # Placing the labels inside the bars.

fig.show()                                                                         # Displaying the bar chart
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/f63f925d-213f-4cf5-885e-d2631020b4d6)

```python
filtered_df = df_2019[df_2019['candidate'] == 'NOTA']                              # Filtering DataFrame to select  rows where candidate is 'NOTA'

sorted_df = filtered_df.sort_values(by='total_votes', ascending=False)             # Sorting the filtered DataFrame by total_votes in descending order

num_bars_to_display = 10                                                           # To display top 10 records

fig = px.bar(sorted_df[:num_bars_to_display], x='pc_name', y='total_votes', 
             text='total_votes', height=500,                                     
             title='Total NOTA votes per constituency in 2019')                    # Paasing slice of sorted dataframe to plot bar chart

fig.update_traces(textposition='inside')                                           # Placing the labels inside the bars.

fig.show()                                                                         # Displaying the bar chart
```
Output -

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/79ae50b5-e0d8-4ce8-9422-82d90e322f92)


**11. Which constituencies have elected candidates whose party has less than 10% vote share at state level in 2019**

```python
# Calculating total votes cast for each party in each constituency
party_votes = df_2019.groupby(['pc_name', 'party'])['total_votes'].sum().reset_index()

# Determining the winning party for each constituency
winning_party_indices = party_votes.groupby('pc_name')['total_votes'].idxmax()
winning_party = party_votes.loc[winning_party_indices]

# Calculating total votes cast in each constituency
total_votes_per_constituency = df_2019.groupby('pc_name')['total_votes'].sum().reset_index()

# Merging the two DataFrames to calculate percentage vote share for the winning party in each constituency
winning_party = winning_party.merge(total_votes_per_constituency,on='pc_name',suffixes=('_party', '_total'))

# Calculating percentage vote share for the winning party in each constituency
winning_party['vote_share_percentage'] =(winning_party['total_votes_party']/winning_party['total_votes_total'])*100

# Identifying the constituencies where the winning party's vote share is less than 10% at the state level
constituencies_less_than_10_percent = winning_party[winning_party['vote_share_percentage'] < 10]['pc_name'].unique()

print("Constituencies where the winning party's vote share is less than 10% at the state level in 2019:")
print(constituencies_less_than_10_percent)
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/d7bc035c-b2fa-4f4e-9287-0174d1072a00)

No constituency has winning party vote share less than 10% at state level in 2019.



**12. Is there a correlation between postal votes % and voter turnout % ?**

```python
postal_votes_by_states_2014=df_2014.groupby('state')['postal_votes'].sum()                            # Caluculating postal votes by states
#postal_votes_by_states
total_postal_votes_in_country_2014=df_2014['postal_votes'].sum()                                      # Calculating postal votes by country
#total_postal_votes_in_country
postal_votes_pcnt_2014=round((postal_votes_by_states_2014/total_postal_votes_in_country_2014)*100,2)  # Calculating postal votes % by states
#postal_votes_pcnt_by_states

unique_df_2014 = df_2014.drop_duplicates(subset=['pc_name'])                                          # Dropping duplicate rows based on pc_name 
#unique_df_2014
total_electors_by_state_2014 = unique_df_2014.groupby('state')['total_electors'].sum()                # Calculating total electors by state
#print(total_electors_by_state)
total_votes_by_states_2014 = df_2014.groupby('state')['total_votes'].sum()                            # Calculating total votes by states
#total_votes_by_states
voter_turnout_pcnt_2014=round((total_votes_by_states_2014/total_electors_by_state_2014)*100,2)        # Calculating voter turnout % by states
#voter_turnout_pcnt_2014

correlation_data_2014 = pd.DataFrame({'Postal Votes (%) 2014': postal_votes_pcnt_2014,  
                                 'Voter Turnout (%) 2014': voter_turnout_pcnt_2014})                  # Combining the two Series into a DataFrame
#------------------------------------------------------------------------------------------------------------------------------------------------------#

postal_votes_by_states_2019=df_2019.groupby('state')['postal_votes'].sum()                            # Caluculating postal votes by states
#postal_votes_by_states
total_postal_votes_in_country_2019=df_2019['postal_votes'].sum()                                      # Calculating postal votes by country
#total_postal_votes_in_country
postal_votes_pcnt_2019=round((postal_votes_by_states_2019/total_postal_votes_in_country_2019)*100,2)  # Calculating postal votes % by states
#postal_votes_pcnt_by_states

unique_df_2019 = df_2019.drop_duplicates(subset=['pc_name'])                                          # Dropping duplicate rows based on pc_name 
#unique_df_2019
total_electors_by_state_2019 = unique_df_2019.groupby('state')['total_electors'].sum()                # Calculating total electors by state
#print(total_electors_by_state)
total_votes_by_states_2019 = df_2019.groupby('state')['total_votes'].sum()                            # Calculating total votes by states
#total_votes_by_states
voter_turnout_pcnt_2019=round((total_votes_by_states_2019/total_electors_by_state_2019)*100,2)        # Calculating voter turnout % by states
#voter_turnout_pcnt_2014

correlation_data_2019 = pd.DataFrame({'Postal Votes (%) 2019': postal_votes_pcnt_2019,  
                                 'Voter Turnout (%) 2019': voter_turnout_pcnt_2019})                  # Combining the two Series into a DataFrame
#-------------------------------------------------------------------------------------------------------------------------------------------------------#
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))                                           # Creating a scatter plot with two subplots

correlation_data_2014.plot(kind='scatter', ax=axes[0], color='blue', x='Postal Votes (%) 2014', y='Voter Turnout (%) 2014')        # Plotting the top 5 rows
axes[0].set_title('Correlation between Postal Votes (%) and Voter Turnout (%) in 2014')
axes[0].set_xlabel('Postal Votes (%)')
axes[0].set_ylabel('Voter Turnout Ratio (%)')
 
correlation_data_2019.plot(kind='scatter', ax=axes[1], color='red', x='Postal Votes (%) 2019', y='Voter Turnout (%) 2019')         # Plotting the bottom 5 rows
axes[1].set_title('Correlation between Postal Votes (%) and Voter Turnout (%) in 2019')
axes[1].set_xlabel('Postal Votes (%)')
axes[1].set_ylabel('Voter Turnout Ratio (%)')

plt.tight_layout()                                                                                                                 # Adjusting the layout
plt.show()                                                                                                                         # Displaying the plots
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/daaa63af-42bc-4db7-92dc-aa0bcbea1a11)

**13. Is there a correlation between GDP of a state and voter turnout % ?**

```python
# Correlation data file containing voter turnout percentage is obtained from above
correlation_data_2019 = pd.DataFrame({'Postal Votes (%) 2019': postal_votes_pcnt_2019,  
                                 'Voter Turnout (%) 2019': voter_turnout_pcnt_2019}) 

df_gdp=pd.read_csv(r"C:\Users\91961\Downloads\GDP.csv")                                                             # Reading literacy rate file

correlation_by_gdp=pd.merge(correlation_data_2019,df_gdp,on='state')                                                # Merging both files by state

#correlation_by_gdp.plot(kind='scatter', color='blue', x='GDP', y='Voter Turnout (%) 2019')  

sns.heatmap(correlation_by_gdp[['GDP', 'Voter Turnout (%) 2019']].corr(), annot=True, cmap='coolwarm', fmt=".2f")   # Plotting heat map
plt.title('Correlation Heatmap between GDP and Voter Turnout (%) in 2019')
plt.show()
```

Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/c552a67c-c938-44d7-b66c-bd90acf6214e)




**14. Is there a correlation between literacy % of the state and voter turnout % ?**

```python
# Correlation data file containing voter turnout percentage is obtained from above
correlation_data_2019 = pd.DataFrame({'Postal Votes (%) 2019': postal_votes_pcnt_2019,        
                                 'Voter Turnout (%) 2019': voter_turnout_pcnt_2019}) 

df_lr=pd.read_csv(r"C:\Users\91961\Downloads\literacy rate.csv")                                              # Reading literacy rate file

correlation_by_literacy_rate=pd.merge(correlation_data_2019,df_lr,on='state')                                 # Merging both files by state 

# Plotting  correlation map
sns.jointplot(data=correlation_by_literacy_rate,x='literacy_rate',y='Voter Turnout (%) 2019',kind='hist',,height=8,ratio=4)
plt.title('Correlation between literacy rate (%) and Voter Turnout (%) in 2019')
plt.xlabel('literacy_rate')
plt.ylabel('Voter Turnout (%)')
plt.show()
```
Output - 

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/f1d57335-ea9e-42ec-bf14-423825e8bdd0)


































