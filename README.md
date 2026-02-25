# New NFL Kickoff Data Analysis

Analyze how the 2024 kickoff rule changes impacted return rates and field position.

## Questions
- Did the new rule lead to more returns?
- How do the average starting field position of returned kicks compare with touchbacks? Is there more of an advantage to the kicking team to force a return?
- How does the average starting field position affect the amount of points scored on that drive?

## Data
This project expects a CSV at:
`data/raw/pbp-2024.csv`

(That file is not committed to the repo.)

## Quickstart
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

## What Changed?

First, We have to define what changed about the NFL Kickoff between 2023 and 2024.  The nfl made these changes due to "unnacceptably high" injury rates on the old play, as well as the record lack of returns that occured in the 2023 season. 

From the NFL's operation website, the changes to the rule are listed as follows:

# Alignment
- All kicking team players other than the kicker will line up with one foot on the receiving team’s B40 yard line
- Kicker cannot cross the 50-yard line until ball touches the ground or player in landing zone or end zone
- The 10 kicking team players cannot move until the ball hits the ground or player in the landing zone or the end zone
- The receiving team will line up as follows:
    - Setup Zone – a 5-yard area from the B35 to the B30 yard line where at least 9 receiving team players must line up
    - At least 7 players with foot on the B35 yard line (restraining line) with alignment requirements (outside numbers, numbers to hashes, and inside hashes)
    - Players not on the restraining line must be lined up in setup zone outside the hash marks
- All players in the setup zone cannot move until the kick has hit the ground or a player in the landing zone or the end zone
- A maximum of 2 returners may line up in the landing zone and can move at any time prior to, or during, the kick

# Landing Zone

- Landing zone is the area between the receiving team’s goal line and its 20-yard line.
- Any kick that hits short of the landing zone – treated like kickoff out of bounds and ball spotted at B40 yard line; play would be blown dead as soon as kick lands short of the landing zone
- Any kick that hits in the landing zone - must be returned
- Any kick that hits in the landing zone and then goes into the end zone – must be returned or downed by receiving team – if downed then touchback to B20 yard line
- Kick hits in end zone, stays inbounds - returned or downed – if downed then touchback to B30 yard line
- Any kick that goes out of the back of the end zone (in the air or bounces) – touchback to B30 yard line


## Results

# Touchback Rates
For my first question: did the new rule lead to more returns? I was able to do a simple touchback percentage calculation and see that that 63.3% of kickoffs this year were touchbacks.  From the article found at this link: https://www.forbes.com/betting/football/nfl/kickoff-rule-change/#:~:text=Kick%20returners%20avoided%20these%20hits,in%20NFL%20history%20(21.8%25). , the 2023 touchback percentage was 79.2%.  So after one year with the dynamic kickoff, there was a clear rise in returned kickoffs.

I also wanted to look into team-by-team data to see if there were any teams that were choosing to kick more touchbacks, or any teams that were better at gaining yards on returns.

![Kicking Team Touchback Percentage](reports/figures/kicking_team_touchback_percentage.png)

 The plot above shows the percentage of the time that teams kicked touchbacks in descending order.  Some teams like the Commanders and Saints kicked touchbacks very rarely, and other teams like the Jaguars and Colts Kicked Touchbacks over 80% of the time.

![Return Team Tuchback Taken Percentage](reports/figures/return_team_touchback_taken_percentage.png)

The above plot shows similar information from the perspective of the returning team, how often did they take a touchback?  All teams besides the eagles (who coincidentally won the super bowl the year that this data was taken from), took touchbacks more than 50% of the time.  One thing to notice is the league leading touchback taken rate of the Broncos.  I found in my next analysis that this return unit had the highest yards per return in the league, so it makes sense that teams wouldn't want to kick them the ball.  Teams also may opt to take a high percentage of touchbacks because their return unit isn't very good, and they know that the touchback will provide them better starting field position.

# Starting Field Position
I also plotted kicking and returning team average starting field position on non touchback kickoffs to see what teams had figured out any strategies to exploit this new rule.

![Kicking Team Average Opponent Starting Field Position](reports/figures/kicking_team_average_opponent_starting_field_position_non_touchbacks.png)

![Return Team Average Starting Field Position](reports/figures/return_team_average_starting_field_position_non_touchbacks.png)

From these plots we can see how good teams are at defending kickoff returns and how good teams are at returning kicks.  In 2024, 5 teams had an average return distance past the 30 yard line.  I interpret this to mean that for most teams, its a strategic play to force them to return, since on average your defense would be gaining field position.  The Kicking Team Opponent Starting Field Position Plot shows how good teams are at defending returns.  Again, we only see 5 teams that on average allowed a return past the 30 yard line, indicating a similar strategic trend as the other plot, for most teams it makes sense to put the ball in play.

![Touchback Kicked vs Average Opponent Starting Field Position](reports/figures/touchback_kicked_vs_average_starting_field_position.png)

![Touchback Taken vs Average Return Length](reports/figures/touchback_taken_vs_average_return_length.png)

I investigated the relationship between touchback kicked/taken and return unit success by making scatter plots of the breakdowns above.  We can see that there is a slight correlation between how good a team is on their returns and the amount of touchbacks they take.  Teams are far less likely to allow teams with good return units the chance at a big play, and will do their best to prevent their returner from taking the ball out.  

Another conclusion is that while the ball certainly was more in play this season, the league probably wants the touchback taken % to be even lower, hence moving the touchback starting field position to the 35 yard line for the 2025 season.  With this change, there is now no team that would get better field position on average from returns than taking a touchback, which will force teams to kick into the landing zone more, generating more returns.

# It's only a few yards — why does it matter?

Lastly, I wanted to investigate how starting field position impacts the likelihood of scoring on a drive. I began by calculating the average yards gained per drive across the league.

The average drive gains roughly 34 yards. With the average starting field position after a kickoff being the 29-yard line, that places the typical drive ending around the opponent’s 37-yard line — roughly a 54-yard field goal attempt. That distance sits right at the edge of most kickers’ reliable range (analysis shown below). In other words, the “average” drive already operates near the threshold between attempting points and punting.

This is where a five-yard difference starts to matter. Moving a drive from the 29 to the 34-yard line does not just shift field position slightly — it meaningfully increases the likelihood that an average drive ends within realistic scoring range. Obviously, from team to team and game to game there are many variables that impact whether a team scores more than whether the kickoff was returned or resulted in a touchback. However, by normalizing across the entire league — from elite offenses to struggling ones — we can isolate the structural impact of starting field position itself.

![Field Goal Make Percentage by Yard Line](reports/figures/field_goal_make_percentage_by_yard_line.png)

To further illustrate this effect, I plotted scoring drive percentage by starting field position bins.

![Scoring Drive Percentage by Starting Yard Line](reports/figures/scoring_drive_percentage_by_starting_yard_line_bins.png)

The results show a clear jump in scoring probability once drives begin past the 30-yard line, with scoring rates exceeding 50% for drives starting beyond the 35.

In isolation, five yards may seem insignificant. But league-wide drive data suggests those yards often determine whether a drive ends in a marginal long field goal attempt or a realistic scoring opportunity. Over the course of a season — across hundreds of drives — those small shifts in expected field position compound. In a league defined by narrow margins, five yards is not trivial; it meaningfully shifts the probability of scoring.











