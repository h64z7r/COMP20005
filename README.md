java c
COMP20005 Engineering Computation 
Semester 2, 2024 
Assignment 1 
Due: 4:00 pm Monday 23rd September 2024 
Version 1.0
1 Learning Outcomes 
In this assignment you will demonstrate your understanding of loops and if statements by writing a program that sequentially processes a file of input data. You are also expected to make use of functions and arrays.
2 The Story... The prevalence of smart phones and Internet  of Things  has accumulated huge volumes of human trajecto- ries, that is, sequences of GPS points recording the locations of people or other objects.  For example, Uber has recorded 7.6 billion trips in 2022.In this assignment, we are given a set of trajectory data that records people’s locations at different times. The aim is to identify if any of the trajectories is sufficiently similar to a given  query trajectory. Such com- putations are the basic building block of many location-based applications, such as to find shareable Uber trips (see Figure 1), to make travel recommendations, and, during pandemic, to support contact tracing. You  do  not  need  to  be  an  expert  in  trajectory  data  analysis  to  complete  this  assignment.   This assignment specification will walk you through the tasks step by step.

Figure 1: Application example with human movement trajectories.
Below is a sample list of trajectory records  (the time when the trajectories are recorded is omitted for simplicity).
1.  Each data trajectory starts with a line containing a ‘#’ character followed by a unique six-digit integer representing the ID of the trajectory.
2.  Then, the GPS points captured on the trajectory are stored in the order that they are captured, and they are separated into at least one and up to five lines each containing four points, that is, each trajectory contains up to 20 points.  There are no lines with fewer than four GPS points (except for the trajectory ID lines).
Each GPS point, as separated by a ‘ ; ’, is represented by a pair of latitude and longitude, each of which is a real number with 6 digits after the decimal point.
3.  A line of 10 ‘@’s follows the data trajectory lines to separate them from a query trajectory.
4.  Finally, the query trajectory is represented by up to five lines, each with four GPS points like above. For simplicity, there is no ID for the query trajectory.
#257837
41 .141412  -8 .618643;  41 .141376  -8 .618499;  41 .142511  -8 .620326;  41 .143815  -8.622153;
41 .144373  -8 .623953;  41 .144778  -8 .626686;  41 .144697  -8 .627373;  41 .145210  -8.630226; #761770
41 .151951  -8 .574678;  41 .151942  -8 .574705;  41 .151933  -8 .574696;  41 .151967  -8.574663;
41 .151933  -8 .574723;  41 .151924  -8 .574714;  41 .150934  -8 .575164;  41 .151924  -8.574714;
41 .150232  -8 .577135;  41 .148639  -8 .578538;  41 .147316  -8 .579745;  41 .146173  -8.579358;
41 .145033  -8 .580744;  41 .145127  -8 .582904;  41 .146479  -8 .584388;  41 .145876  -8.610849; #576807
41 .180499  -8 .645994;  41 .180517  -8 .645949;  41 .180049  -8 .646048;  41 .178888  -8.646804;
41 .178465  -8 .649495;  41 .177961  -8 .652152;  41 .177196  -8 .654049;  41 .177925  -8.655012;
41 .177853  -8 .656353;  41 .177277  -8 .659647;  41 .177619  -8 .662518;  41 .179221  -8.664561;
41 .178537  -8 .667432;  41 .176674  -8 .668944;  41 .175182  -8 .671374;  41 .173308  -8.673894;
41 .171841  -8 .676918;  41 .171949  -8 .680032;  41 .173191  -8 .682615;  41 .173776  -8.685441; #484662
41 .140674  -8 .615502;  41 .140926  -8 .614854;  41 .141522  -8 .613351;  41 .140854  -8.609976;
41 .141295  -8 .607537;  41 .141808  -8 .603676;  41 .141916  -8 .599833;  41 .140494  -8.596458;
41 .140008  -8 .592993;  41 .140674  -8 .589384;  41 .142753  -8 .587026;  41 .143644  -8.583831; #941008
41 .145948  -8 .579524;  41 .145039  -8 .580942;  41 .145021  -8 .582706;  41 .146164  -8.584092;
41 .146830  -8 .585466;  41 .147397  -8 .587116;  41 .148018  -8 .586171;  41 .148963  -8.586091;
41 .149368  -8 .588016;  41 .150016  -8 .590401;  41 .150736  -8 .593119;  41 .150853  -8.593506;
41 .150976  -8 .593668;  41 .150232  -8 .595342;  41 .149827  -8 .596087;  41 .148909  -8.597466; #707773
41 .146182  -8 .617563;  41 .145849  -8 .617527;  41 .144832  -8 .616978;  41 .145426  -8.615754; #507479
41 .140557  -8 .611794;  41 .140575  -8 .611785;  41 .140566  -8 .612001;  41 .140503  -8.612622;
41 .140341  -8 .613702;  41 .140386  -8 .614665;  41 .140485  -8 .615844;  41 .140683  -8.615616;
41 .141088  -8 .614566;  41 .141979  -8 .614395;  41 .142942  -8 .613936;  41 .143851  -8.612793;
41 .144787  -8 .611488;  41 .144391  -8 .610543;  41 .143536  -8 .610282;  41 .143401  -8.610255; #223260
41 .140557  -8 .615907;  41 .141088  -8 .614449;  41 .141439  -8 .613522;  41 .140827  -8.609904;
41 .139522  -8 .609301;  41 .138865  -8 .609544;  41 .137551  -8 .610777;  41 .136012  -8.611452; #334953
41 .148009  -8 .619894;  41 .147736  -8 .620164;  41 .148513  -8 .620654;  41 .150313  -8.620927;
41 .151951  -8 .621208;  41 .153517  -8 .621118;  41 .155416  -8 .620884;  41 .155479  -8.620938; @@@@@@@@@@
41 .140534  -8 .612208;  41 .140521  -8 .612235;  41 .140323  -8 .614035;  41 .140353  -8.614809;You may assume that the input is always correctly formatted. There is  always one query trajectory, and there is at least one and up to 100 data trajectories to be compared with the query trajectory. You do not need to check for input data validity.
3 Your Task 
You are given a skeleton code file named program .c. Your task is to add code to it to read the input data and to find the trajectory most similar to the query trajectory. The assignment consists of four stages.
3.1    Stage 1 - Process the First Data Trajectory (6 Marks) Add code to the stage_one function to read the first data trajectory and print out for the trajectory: the trajectory ID, the number of points on the trajectory, the latitude and the longitude of the first point on the trajectory, and the distance (2 digits after the decimal point) between the first two points on the trajectory.Given the coordinates of two points p1  =  ⟨lat1 , long1 ⟩  and p2  =  ⟨lat2 , long2 ⟩, where lati  and longi   (i = 1, 2) represent the latitude and the longitude, the distance (in metres) between p1  and p2 , represented by dist(p1 , p2 ), is calculated based on the haversine formula as follows.dist(p1.P2) = 6371000 · angle_distance, where angle distance = 2- atan2(sgrt(chord length), sqrt(1 - chord_length)), chordJength = sin (to_radian(lat2 — latı)/2)+ (1) cos(to radian(lat1)) - cos(to radian(lat2)) - sin2(to radian(long2 - longi)/2) In the equation above, to radian(x) = x · (3.14159/180) is a function that converts a latitude or longitude coordinate x to its radian value.  You need to implement this function and  define  the  constants  properly in your code.  Note that you should not use the constant M_PI provided by the math.h library as it is not supported by the assignment submission system under the assignment compilation settings.
The functions atan2(),  sqrt(),  sin(), and cos() are provided by the math.h library.   If you use this library, make sure to add the “-lm” flag to the “gcc” command at compilation on your own machine.
The output of this stage given the above sample input should be (where “mac:” is the command prompt):
mac:  ./program  < test0 .txt
Stage  1
==========
Trajectory:  #257837
Number  of  points:  8
First  point:  <41 .141412, -8 .618643>
Distance  between  first  two  points:  12 .71
Here, 12.71 is the distance between the first two points on the first trajectory, <-41 .141412, -8 .618643> and <-41 .141376, -8.618499>, as calculated using the haversine formula above.Hint:  To ensure the result accuracy, use d代 写COMP20005 Engineering Computation Semester 2, 2024 Assignment 1R
代做程序编程语言ouble’s for storing the coordinates and calculating the distance value. Use %05 .2f for the distance output formatting. You may also read all input data before printing out for Stage 1.  You may  (and should)  add more functions  as  appropriate.As this example illustrates, the best way to get data into your program is to save it in a text file (with a “ .txt” extension, any text editor can do this), and then execute your program from the command line, feeding the data in via input redirection  (using <).In your program, you will still use the standard input functions such as scanf() to read the data. Input redirection will direct these functions to read the data from the file, instead of asking you o type it in by hand. This will be more convenient than typing out the test  cases manually every time you test your pro-gram. Our auto-testing system will feed input data into your submissions in this way as well. To simplify the marking, your program should not print anything except for the data requested to be output (as shown in the output example).We will provide a sample test file “test0.txt” on LMS. You may download this file directly and use it to test your code before submission.  This file is created under Mac OS. Under Windows systems, the entire file may be displayed in one line if opened in the Notepad app. You do not need to edit this file to add line breaks. The ‘\n’ characters are already contained in the file. They are just not displayed properly but the scanf() function in C can still recognise them correctly.
3.2 Stage 2 - Process the Rest of Data Trajectories (5 Marks) Now add code to the stage_two function so that the length of each data trajectory (not the query trajectory) is computed and visualised. You may assume that the distance values are within the range of (0 , 4000) (use %07 .2f for the length output formatting).  The length of a trajectory is the sum of the distance between every two adjacent points (in the input order) on the trajectory.
On the same sample input data, the additional output for this stage should be (you need to work out how the visualisation works based on this example):
Stage  2
==========
#257837,  len:  1121 .89  | ----+-
#761770,  len:  3812 .16  | ----+----+----+----+
#576807,  len:  3957 .92  | ----+----+----+----+
#484662,  len:  2875 .40  | ----+----+----+
#941008,  len:  2043 .64  | ----+----+-
#707773,  len:  0281 .15  | --
#507479,  len:  1180 .29  | ----+-
#223260,  len:  1121 .64  | ----+-
#334953,  len:  0913 .29  | ----+
Here, the length of trajectory #257837 is 1121.89.
3.3    Stage 3 - Read the Query Trajectory (5 Marks) Now add code to the stage_three function to read the query trajectory and calculate its  dissimilarity with the first data trajectory.  Consider a query trajectory Tq  with points {pq,1,pq,2,..., pq,nq } and a data trajectory Td  with points {pd,1,pd,1,..., pd,nd }, where nq  and nd  are the number of points on Tq   and Td , respectively. Their dissimilarity dissim(Tq , Td ) is defined as follows:
Intuitively, for every point pq,i  on Tq , we need to find and calculate the distance to its closest point on Td. The sum of such distances for all points on Tq  is the dissimilarity for Tq  with Td.
Note that the definition of dissimilarity is asymmetric, that is, the dissimilarity for Tq  with Td  may be different from that for Td  with Tq   (dissim(Tq , Td )  dissim(Td ,Tq )).
On the same sample input data, the additional output for this stage should be (use % .2f for the dissimilarity value output formatting):
Stage  3
==========
Dissimilarity  with  trajectory  #257837:  1789 .16
Wherever appropriate, code should be shared between stages through the use of functions.  In particular, there should not be long stretches of repeated code appearing in different places in your program.
3.4    Stage 4 - Find the Most Similar Data Trajectory (4 Marks) In this stage, we find the data trajectory most similar to the query trajectory. Add code to the stage_four function to calculate the dissimilarity for the query trajectory with each data trajectory.   The function should output the ID of the data trajectory of the smallest dissimilarity value (that is, the data trajectory most similar to the query trajectory), and the corresponding dissimilarity value. You may assume that there will not be ties in the dissimilarity values.
On the same sample input data, the additional output for this stage should be (use % .2f for the dissimilarity value output formatting; note the final ‘\n’ in the output):
Stage  4
==========
Most  similar  data  trajectory:  #507479 Dissimilarity:  78.48
4    Submission and Assessment 
This assignment is worth 20% of the final mark. A detailed marking scheme will be provided on LMS.Submitting your code. To submit your code, you will need to:  (1) Log in to LMS subject site, (2) Nav- igate to “Assignment 1” in the “Assignments” page, (3) Click on “Load Assignment 1 in a new window”, and (4) follow the instructions on the Gradescope “Assignment 1” page and click on the “Submit” link to make a submission.  You can submit as many times as you want to.  Only the  last submission made  before  the deadline will be marked.  Submissions made after the deadline will be marked with late penalties as detailed at the end of this document.  Do not  submit after the deadline unless a late submission is intended.  Two hidden tests will be run for marking purposes. Results of these tests will be released after the marking is done.
You can (and should) submit both early and often – to check that your program compiles correctly on our test system, which may have some different characteristics to your own machines.Testing on your own computer. You will be given a sample test file test0 .txt and the sample output test0-output .txt. You can test your code on your own machine with the following command and compare the output with test0-output .txt:
mac:  ./program  < test0.txt /* Here ‘<’ feeds the data from test0.txt into program */
Note that we are using the following command to compile your code on the submission testing system (we name the source code file program.c).
gcc  -Wall  -std=c17  -o  program  program .c  -lmThe flag “-std=c17” enables the compiler to use a modern standard of the C language – C17.  To ensure that your submission works properly on the submission system, you should use this command to compile your code on your local machine as well.
You may discuss your work with others, but what gets typed into your program must be individual work, not from anyone else.
• Do not give (hard or soft) copies of your work to anyone else.
• Do not “lend” your memory stick to others.
• Do not leave your laptop unlocked in the lab or library.
• Do not upload your assignment solutions onto Github or any other public code repositories.
•  Do not ask others to give you their programs “just so that I can take a look and get some ideas, I won’t copy, honest” .The best way to help your friends in this regard is to say a very firm “no” when they ask for a copy of, or to see, your program, pointing out that your “no”, and their acceptance of that decision, is the only thing that will preserve your friendship.  A  sophisticated program  that  undertakes  deep  structural  analysis  of  C code  identifying  regions  of similarity  will  be  run  over  all  submissions  in   “compare  every  pair”  mode.   See https://academichonesty.unimelb.edu.au for more information.Deadline: Programs not submitted by 4:00 pm Monday 23rd September 2024 will lose penalty marks at the rate of 3 marks per day or part day late.  Late submissions after 4:00 pm Friday 27th September 2024 will not be accepted.





         
加QQ：99515681  WX：codinghelp
