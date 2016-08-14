# GettingCleaningData
###Final project for Coursera Getting and Cleaning Data course
####D. Spence

  
This folder contains the following files:  
  

**run_analysis.R**  
--The script that performs the analysis described below  
**TidyData.txt**  
--The output file created by the script (also submitted directly on Coursera)  
**CodeBook.md**  
--Description of all variables in the output file  


The analysis script is self-documenting; comments explain each section. Below
are a summary of the process and some notes about how the task was approached.  


The script combines the training and test data sets. Each of these data sets 
resides in a separate folder and is comprised of 3 files, as follows:  
  
**subject_train.txt (or subject_test.txt)**   
--contains the research subjects from whom each row of data were collected 
   (identified by numbers from 1 to 30)  
**y_train.txt (or y_test.txt)**  
--contains the specific activity in which the subject was engaged when the
   measurements were taken  
**X_train.txt (or X_test.txt)**  
--contains the measurements themselves  


The 3 files described above have a row-for-row correspondence; for instance,
the 4th row of the "subject"" file identifies a research subject; the 4th row
of the "y" file identifies an activity that the subject was engaged in at 
some point; and the 4th row of the "X" file contains the measurements collected
from that subject during that activity at that particular time.  


There are additional data in another folder (Inertial Signals) for each data 
set, but since the task involves retaining only those variables that are means 
or standard deviations, these data are not needed.  


The columns of the measurement data are named using the descriptive text names 
given in the *features.txt* file. These names are also the basis for filtering
the data set down so that only the columns containing means or standard 
deviations are retained. From this analysis, there were 79 such columns.  
Two notes are especially important about this process:  

1. The project instructions say to combine the data sets (step 1) and to 
   extract the mean and std deviation columns (step 2). However, it is much
   more efficient to extract those columns we intend to keep, and THEN to
   combine those data sets.  The resulting data set is the same, but doing
   tasks in this order prevents unnecessary extra processing and memory usage.
2. My first inclination was to retain only those columns identified with
   mean() and std(), based on the project instructions. However, I read a 
   post on the class discussion board, in which a student had lost points by
   not including the 'meanFreq' fields. I agree with the student who  was
   submitting the complaint-- the meanFreq fields are part of the data that
   was used to compute the measurement in question, but they are not the mean
   of the measurement. The instructions specifically say to extract the mean
   and std. deviation "for each measurement". But since it appears open to
   (uninformed) interpretation, I am playing it safe and have included the 
   meanFreq columns. Such is the hazard of peer review. **:/**  


The script builds one dataframe for each setting (training and test) by 
combining the data from the 3 files for that data set. Since the rows 
correspond among the three independent tables, a column bind can be used to put 
them together. Subjects remain identified by number (as given). Activities, 
which were given as codes from 1 to 6, are converted to descriptive text, using 
the *activity_labels.txt* file (e.g., 1 translates to WALKING).  

 
Once the test and training dataframes have been assembled, they are merged
into a single dataframe, retaining all records from each separate data frame.
This merged dataframe is the basis for the summary tidy data table that is
constructed. All rows identifying a particular subject doing a particular 
activity are grouped together and summarized as an average on a single row in 
the summary dataframe. For instance, all the rows corresponding to subject #7 
walking upstairs are grouped together, and the mean of each measurement column
is computed across that group of records, creating a single row in the summary
data set. Since there are 30 subjects and 6 activities, there are 180 records
in this summary table-- one for each subject performing each activity.

