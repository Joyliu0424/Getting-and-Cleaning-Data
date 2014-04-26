##arguments used in run_analysis.R are described as follow:

xtes--to put "x_test.txt" in 
ytest--to put "y_test.txt" in
subtest--to put "subject_test.txt" in
xtrain--to put "X_train.txt" in 
ytrain--to put "y_train.txt" in
subtrain--to put "subject_train.txt" in

colnames--to put "features.txt"

test--to put subtest, xtest and ytest together
train--to put subtrain,xtrain and ytrain together
sum--to put test and train together

sumclear--to extract "the measurements on the mean and standard deviation for each measurement" from sum

subjectdata--to extract data of same subject from sumclear
activitydata--to extract data of same subject as well as same activityCode from subjectdata

summary--to put "tidy data set with the average of each variable for each activity and each subject" in
