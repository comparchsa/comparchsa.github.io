# Author: [Gururaj Saileshwar](https://gururaj-s.github.io/)

Purpose: A document to support future organizers of Meet-A-Senior-Architect (MASA) and Meet-A-Senior-Student (MASS). This document outlines the timeline for setting up MASS/MASA at computer architecture conferences. This is based on the process which was followed for MASS/MASA at  ASPLOS-2021. A similar but less automated process was also followed at MICRO-2020.
MASA is targeted at all student attendees & matches a student with a senior architect (folks in industry/academia)
MASS is targeted at junior student attendees (undergrad/masters,1st/2nd yr PhD students) and matches each junior student with a senior PhD student (3rd year & above).

People Involved: Gururaj Saileshwar, Tianyi Liu & other board-members of CASA (www.comparchsa.org), Joel Emer (MIT/Nvidia) who provided a lot of help, support and timely advice from his experience running MASA for several iterations, George Michelogiannakis (Organizing Committee, ASPLOS-2021) who helped with the registration form and data.



1.5 months before conference: Registration Forms

Create the questions to be added to the registration form. Link to Questions used for ASPLOS CVent form. Ideally this should happen 2 months or more before conference when the registration form is made live.
Note the conference registration sites have poor support for conditional paths, so we need to resort to having qualifiers at the beginning of each question to highlight who it is for (e.g. junior/senior student for MASS)
Some platforms such as Cvent have the ability to separate out registration paths for students and non-students: so it was possible to separate out questions for mentors and mentees for MASA.
Also note, it is critical to provide a list of research areas (checkbox/dropdown) attendees can pick from, if research-area based matching is to be done. It is virtually impossible to do this if each attendee provides their input via textbox.
Preferably avoid any textbox responses even for “Other” category in answers - it will complicate the matching code.
It is best to double-check the registration form after the questions were added. We have found these registration platforms sometimes jumble up questions or miss questions on certain paths altogether due to bugs or human errors.
One learning from ASPLOS MASA/MASA:  Do provide a deadline upfront in the registration form (say 1 week before the conference) after which any responses will not be considered for matching. This is important as you will not be able to provide any matches for folks who register on the last day. Also, provide a disclaimer that the matching will be first-come-first-served as often the number of mentees will be much more than mentors and often we are unable to provide mentors for everyone who registers even before the deadline.
Create a Google Form, in case there are attendees who already registered before the MASS/MASA questions were added to the registration form.
Links to Google Forms: MASA Form, MASS Form
It is essential to ensure that the questions and more importantly the answers are in the same format as the CVent. We had to write some python magic to reconcile the answers from Google-Form and CVent while matching.
Note that some people could provide responses on both platforms Google-Form & Cvent, and show up as duplicates when the responses are combined from both sources. (not sure how this happened at ASPLOS although we took care to only email the form to previously registered attendees). You will need to remove duplicates when you merge the data at the end before matching.
The forms will need to be emailed to attendees who already registered. We used text similar to this:
Greetings ASPLOS Attendee, 
We are reaching out to tell you about mentorship initiatives being planned at ASPLOS this year.
This year, we welcome the Meet-A-Senior-Architect (MASA) and the Meet-A-Senior-Student (MASS) initiatives to ASPLOS’21 with support from the Computer Architecture Student Association (CASA). These mentoring opportunities aim to match students early in their career with Senior Researchers in industry/academia (MASA) and Senior PhD students (MASS) for virtual one-on-one meetings of approximately 30-minutes during the conference week, to allow students to gain advice/mentorship from senior researchers and senior PhD students. 
For Researchers in Industry/Academia: We encourage you to sign up as mentors to students for MASA at this link:  https://tinyurl.com/MASA-ASPLOS21
For Students: 
We encourage you to sign up to meet with senior researchers in industry/academia through MASA at this link:   https://tinyurl.com/MASA-ASPLOS21  
Senior and Junior Students are encouraged to sign up as mentors or mentees respectively for MASS at this link:  https://tinyurl.com/MASS-ASPLOS21
We hope you participate in these initiatives and benefit from them! Hope to see you all virtually during the conference. 
Regards,
		MASS/MASA Organizers

3 weeks before conference: Get ready for the matching

Get a preliminary data-dump from the organizers who control the registration data
This can help you do the following:
Verify that the responses are correctly being entered in the data. 
Get dummy responses that can be used to test matching scripts (see next point)
Check the number of mentors / mentees signing up. Usually there are more mentees than mentors, especially for MASA. While we asked each mentor if they would like 1, 2 or 3 mentees in the form (2 was the popular choice), we still fell short of the mentors needed to match all of our mentees. One option is having someone senior reach out to colleagues known from the registration list to personally encourage additional mentor signups, but this usually has resulted in no more than 5-10 additional mentors


1 week before conference: Perform the Mentor/Mentee Matches & Send Out Emails

Perform the matching with the scripts: The Jupyter Notebook / python scripts we used for ASPLOS can be found here: MASA-Script and MASS Script.
The scripts take into account research-areas for matches and also takes into account mentee’s preference of industry/academia mentor for MASA
Inputs to scripts: ASPLOS21.cvent.csv, ASPLOS21.googleform.MASA.csv - The two csv files containing signups from the registration data & google form
Outputs from the script: mentors_final.csv, mentees_final.csv - The list of mentors with matched mentees and vice-versa. 
The script executes the following steps to decide the matches:
Cleans up data from Cvent Form (ASPLOS Registration Form) & Google Form, making fields consistent across both.
Merges Cvent and Google Form responses, Filters Mentor & Mentee Data-frames to select those who signed up and removes duplicates between the two sets of responses.
Creates multiple entries for mentors who committed to mentor >1 mentee.
Then the actual matching process starts, one iteration matching each for mentees who want industry mentors, academia mentors and those who are agnostic (MASS only has one combined iteration as there are no industry/academia preferences):
In each iteration, the script first separates out mentors into research-area specific lists and matches mentees one-by-one with mentors in one of their research areas.
Any remaining mentees are randomly allotted to mentors and allocation steps when num-mentees-allotted is equal to number-of-commitments by mentors.
Note: We had to manually perform a few swaps to avoid mentor and mentee being from the same affiliation.
Mail-Merge to Send Out Emails to Mentors & Mentees: Once the matches are obtained, the emails need to be sent out. We used thunderbird mail-merge for this:
We created templates for customized emails to be sent out to mentors and mentees communicating the matches. These templates had variables embedded for the mentee name and email and mentor name and email, which would be sourced from CSV files containing the mentor/mentee matches. 
We created four email templates for both MASA & MASS - one each for mentees and mentors with 1, 2, or 3 mentees. Link to email templates.
Mail-merge replaced the variables in these emails with data from CSV files
For mentee email, we directly used the CSV file generated from the previous scripts. 
For the emails to mentors with 1, 2, and 3 mentees, we generated separate CSV files for each of them using a Jupyter Notebook Script that processed the mentors.csv from the previous script and created separate CSV files for mentors with 1,2 and 3 mentees.
We sent the emails from my GT email account and had a mailing list specially created for MASS/MASA organizers that we cced, to ensure replies from any mentor/mentee go to this mail list and reach all the organizers.
For those new to mail-merge with Thunderbird itself, I found this mail-merge tutorial extremely useful
If you use an academic email account to send out the emails, you might encounter timeouts intermittently that stops the mail merge. I followed this helpful link and went to Preferences -> General -> Config Editor and modified the “mailnews.tcptimeout” variable to 1000.
Some emails will bounce, so do watch out for them. For example, in one case we had a typo in the email filled out by a student in the registration form, that we were able to discover when we looked at the email.
During the Conference: Respond to Mentor/Mentor Emails
Common questions/concerns from mentors & mentees:
Most common: I haven’t been able to make contact with my mentor/mentee. My email bounced OR I didn’t hear back from them in the last N days.  In such cases, we tried to advice the mentor/mentee to connect via some other platform (e.g. the conference messaging platform). In some cases, I personally emailed a mentor if I knew them, just to let them know that the mentee tried to get in touch and it worked.
I am not happy with my match: I already know my mentor very well, our timezones dont match, etc. In such cases, we tried our best to swap two mentees who were both unhappy with their matches. But this is not always possible.
I signed up incorrectly: In a couple of cases, a junior student had signed up as a mentor for MASS. So we needed to re-assign the senior student mentor for the mentee.
I would like to withdraw: In some rare scenarios, folks are unable to attend the conference for some reason and could ask to withdraw. We re-allotted their mentors to others who needed them.
 
After 50% of the conference is done: Send Out Reminder Emails
All emails were sent in a non-personalized manner, with all recipients were in BCC. We sent out emails particularly for MASS as this is a newer program than the more well-established MASA.
Emails to mentors urged them to reach out to mentees if they hadn’t heard from them 
Emails to mentees urged them to reach out to mentors if they hadnt done so yet 
Templates for these emails can be found at this link.
