Tennis Transcript and Commentary Dataset 
========================================

This dataset contains transcripts for tennis singles post-match press conferences for major tournaments between 2007 to 2015, gathered from ASAP sports' website [1]. These transcripts are matched with corresponding match information, such as game outcome and player ranking, as provided by Tennis-Data [2]. Also included in the dataset is a gender-balanced set of live-text play-by-play commentaries from matches played at recent Grand Slam tournaments, collected from the website Sports Mole [3]. 

URL: http://www.cs.cornell.edu/~liye/tennis.html
Authors: Liye Fu <lf383@cornell.edu>
              Cristian Danescu-Niculescu-Mizil <cristian@cs.cornell.edu>
              Lillian Lee <llee@cornell.edu>
Contact: Liye Fu <lf383@cornell.edu>
Last Updated: July 21, 2016
Version 1.0 

The dataset is further described in our paper:
	Tie-breaker: Using language models to quantify gender bias in sports journalism
	Liye Fu, Cristian Danescu-Niculescu-Mizil, Lillian Lee
	Proceedings of the IJCAI workshop on NLP meets Journalism, 2016

Files
-----

* questions_matchinfo.json - a JSON file containing the press conference question snippets together with player ranking and game result. 
* transcripts_matchinfo.json - a JSON file containing full transcripts with more complete match information.
* text_commentaries.json - a JSON file containing commentary data. 
* README.txt - this readme


File Descriptions & Formats
---------------------------


* Question data (questions_matchinfo.json)

Post-match press conferences in tennis take place shortly after each match. Players enter a press conference room to face a group of reporters from different news agencies and answer questions posed to them. 

This file contains a slightly processed version of the transcript data which we use in our study. It covers a total of 6467 post-match press conferences. Duplicate questions from the same interview are removed. 

It is in the form of a JSON file with five fields:
	'gender': player gender
	'player': name of the player being interviewed 
	'questions': list of question snippets asked in the press conference. Each entry represents one turn from one reporter. 
	'ranking': ranking of the player
	'result': 1 indicates the player being interviewed is the match winner; 0 otherwise. 

For the full transcript and more complete match information, refer to the transcript data described below.


* Transcript data (transcripts_matchinfo.json)


The dataset provide the full transcript (i.e. both questions and player responses), in the format of a JSON file: 

	>>> import json
	>>> with open("transcripts_matchinfo.json", "r") as f:
	...		interviews = json.load(f)

	The following is an example for one transcript, with each field explained: 
	
	>>> interview = interviews['0']
	0: {
		'QandA': ...  
			# Interview transcripts in the format of a list of question answer pairs.   
		'date': '2015-06-21',  # date the match is played
		'gender': 'M', # 'F' indicates women's singles match, 'M' indicates men's. 
		'opponent': 'Kevin Anderson', # opponent in the match 
			(available only if the opponent has at least one interview recorded in our dataset.)
  		'player': 'Andy Murray', # player being interviewed 
  		'ranking': 3, # ranking of the player
  		'result': 1,  # 1 indicates the player being interviewed has won the match; 0 otherwise. 
    		'stage': 'The Final', # stage of the tournament
  		'tournament': 'AEGON CHAMPIONSHIPS' # tournament name
		'tournament_type': 'ATP500', # type of the tournament, indicating tournament prestige. 
	}

Note that: 
<> These questions and answers are transcribed exactly the way the journalists and the players put it; they are not further edited for grammatical correctness. There are, although relatively rarely, questions that are marked as (Off microphone.), (Indiscernible) or even (Translated from X) where X is a language other than English. 
<> Information about which journalist asked which question is not available. 
<> This transcript data does not contain all available singles press conference transcripts available at ASAP sports as we only included transcripts that we could find corresponding match information for. In addition, since transcripts data and match results are matched by date and player last name, and we did not manually check for every match, it is possible to have a few matching errors. 


* Commentary data (text_commentaries.json)

The commentary data contains 3962 pieces of live-text play-by-play commentaries, split evenly between men and women singles matches. It is a JSON file with three fields: 

	'commentary': the actual text from the live updates. It describes the process of the game. 
	'gender': 'F' indicates the update is from a women's match; 'M' means it is from men's game. 
	'scoreline': the score when the text update is posted. * indicates the player who is serving at the moment. 

<> While we used Sports Mole's commentaries for our study, ByTheMinute [4] could be another source to gather similar data.


References
----------

[1]http://www.asapsports.com/
[2]http://www.tennis-data.co.uk/
[3]http://www.sportsmole.co.uk/
[4]http://www.bytheminute.co/