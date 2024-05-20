# Election_Data_Analysis 📊

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/e2b2baf9-fd92-4c75-b0c3-d8ca30f6956b)

### Contents 📖
- Problem statement
- Tools used
- Exploring the dataset
- Data cleaning
- Data analysis & Visualization
- Recommendations

### Problem Statement  
AtliQ Media, a private media company wants to telecast a show on Lok Sabha elections in 2024 in India. Unlike other channel they do not want to have debate on who is going to win the elections, they rather wanted to present the insights from 2014 and 2019 elections without any bias and discuss the less explored themes like voter turnout percentage in India.

### Tools used 🛠️

- Programming Language: Python
- Libraries: Pandas, Numpy, Matplotlib, Seaborn
- IDE: Jupyter Notebook

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

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/18fcd2e9-2d43-41c1-bb7d-baf1f815c873)

![image](https://github.com/RamyaSaka/Election_Data_Analysis/assets/121084757/c3e3e63f-fe15-488b-803e-74a76ff912c9)

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





















