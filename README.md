# Election_Data_Analysis 

**Problem Statement** - AtliQ Media, a private media company wants to telecast a show on Lok Sabha elections in 2024 in India. Unlike other channel they do not want to have debate on who is going to win the elections, they rather wanted to present the insights from 2014 and 2019 elections without any bias and discuss the less explored themes like voter turnout percentage in India.

**Tools used** - Python for Data cleaning, Exploratory Data Analysis and Data Visualization.

**Insights**

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
top_5_2014.plot(kind='bar', ax=axes[0], color='skyblue')                                     # Plotting the top 5 rows
axes[0].set_title('Top 5 constituencies with highest voter turnout ratio in 2014')
axes[0].set_xlabel('Parliamentary Constituency')
axes[0].set_ylabel('Voter Turnout Ratio (%)')
bottom_5_2014.plot(kind='bar', ax=axes[1], color='salmon')                                   # Plotting the bottom 5 rows
axes[1].set_title('Bottom 5 constituencies with lowest voter turnout ratio in 2014')
axes[1].set_xlabel('Parliamentary Constituency')
axes[1].set_ylabel('Voter Turnout Ratio (%)')
plt.tight_layout()                                                                           # Adjusting the layout
plt.show()                                                                                   # Displaying the plots```



