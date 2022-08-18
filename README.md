# Election_Analysis
Overview of Project
This weeks challenge helps us build upon our skills learned in the python module. For deliverables, the client wanted additional information analyzed from the election 
results. To complete the audit, for loops, conditional statements with memberships and logical operators were used to investigate parameters such as percentage vote by 
county, voter and county turnout. To concluded the summary, a text file was generated to record the findings.

Results:
In this congressional election, there were 369,711 votes that were cast. Provided below are the counties with the largest number of votes in decending order:

Denver: 306,055 votes(82.8%)
Jefferson: 38,855 votes (10.5%)
Arapahoe: 24,801 votes (6.7%)

The following list is a breakdown of the number of votes and their respective percentage of the total votes each candidate recieved. The canidate that won was Diana 
DeGette and the runners up in decending order by total votes are shown below:

Diana DeGette: 272,892 votes
Charles Casper Stockham: 85,213 votes
Raymon Anthony Doane: 11,606 votes (3.1%)

Election Results Summarry:
![Election_Results](https://user-images.githubusercontent.com/109875421/185286584-05ecde93-b333-4de8-917c-f024c4e17fb3.png)

CODE:
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.("Resources", "election_results.csv")
file_to_load = os.path.join("election_results.csv")
# Add a variable to save the file to a path.
file_to_save =  os.path.join("analysis","election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county_turnout = ""
largest_county_vote = 0

# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_results:
    reader = csv.reader(election_results)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:
        # Add to the total vote count
        total_votes = total_votes + 1
        # Get the candidate name from each row.
        candidate_name = row[2]
        # 3: Extract the county name from each row.
        county_name = row[1]
        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:
            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)
            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0
        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:
            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)
            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0
        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1

# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in county_votes:
        # 6b: Retrieve the county vote count.
        county_vote_count = county_votes[county_name]
        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(county_vote_count) / float(total_votes)*100
        county_results=(f"{county_name}: {county_vote_percentage:.1f}% ({county_vote_count:,})\n")
        # 6d: Print the county results to the terminal.
        print(county_results)
        # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
        # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote_count > largest_county_vote):
            largest_county_vote = county_vote_count
            largest_county_turnout = county_name

    # 7: Print the county with the largest turnout to the terminal.
    largest_county_turnout = (f"\n-------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}\n"
        f"-------------------------\n")
    print(largest_county_turnout)
    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_turnout)
    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:
        #Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")
        # Print each candidate's voter count and percentage to the terminal.  
        print(candidate_results)
        #   Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage
            
    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
    
SUMMARRY:
Given how this code is supposed to function, it's propose can be used for other elections big or small. It would be an interesting parameter to see the geographical 
location of each person who voted and create a heat map. This will give valuable information of where to do more outreaching to get a more wholistic representation of 
the local population. The limitations of this dataset are that they only limit a few parameters. It would also be interesting to see the age groups and genders that 
participated in the election so that the parties can go about better strategizing for target audiences they struggled.
