##arguments used in run_analysis.R are described as follow:

·xtest  to put "x_test.txt"
·ytest to put "y_test.txt"
subtest<-read.table("test/subject_test.txt",header=FALSE,sep=" ")

xtrain<-read.table("train/X_train.txt",header=FALSE,na.strings="",sep="")
ytrain<-read.table("train/y_train.txt",header=FALSE,sep=" ")
subtrain<-read.table("train/subject_train.txt",header=FALSE,sep=" ")

colnames<-read.table("features.txt",header=FALSE,na.strings="",sep="")

colnames(ytest)<- "activityCode"
colnames(ytrain)<- "activityCode"
colnames(subtest)<- "subject"
colnames(subtrain)<- "subject"
colnames(xtest)<- colnames$V2
colnames(xtrain)<- colnames$V2

ytest$activity[ytest$activityCode==1]<-"WALKING"
ytest$activity[ytest$activityCode==2]<-"WALKING_UPSTAIRS"
ytest$activity[ytest$activityCode==3]<-"WALKING_DOWNSTAIRS"
ytest$activity[ytest$activityCode==4]<-"SITTING"
ytest$activity[ytest$activityCode==5]<-"STANDING"
ytest$activity[ytest$activityCode==6]<-"LAYING"

ytrain$activity[ytrain$activityCode==1]<-"WALKING"
ytrain$activity[ytrain$activityCode==2]<-"WALKING_UPSTAIRS"
ytrain$activity[ytrain$activityCode==3]<-"WALKING_DOWNSTAIRS"
ytrain$activity[ytrain$activityCode==4]<-"SITTING"
ytrain$activity[ytrain$activityCode==5]<-"STANDING"
ytrain$activity[ytrain$activityCode==6]<-"LAYING"

test<-cbind(subtest,xtest)
test<-cbind(ytest,test)
train<-cbind(subtrain,xtrain)
train<-cbind(ytrain,train)

sum<-rbind(train,test)
sumclear<-subset(sum,select=c("subject","activity","activityCode","tBodyAccMag-mean()",
                              "tGravityAccMag-mean()","tBodyAccJerkMag-mean()","tBodyGyroMag-mean()",
                              "tBodyGyroJerkMag-mean()","fBodyAccMag-mean()","fBodyBodyAccJerkMag-mean()",
                              "fBodyBodyGyroMag-mean()","fBodyBodyGyroJerkMag-mean()","tBodyAccMag-std()",
                              "tGravityAccMag-std()","tBodyAccJerkMag-std()","tBodyGyroMag-std()",
                              "tBodyGyroJerkMag-std()","fBodyAccMag-std()","fBodyBodyAccJerkMag-std()",
                              "fBodyBodyGyroMag-std()","fBodyBodyGyroJerkMag-std()"))

summary<-data.frame()
for(i in 1:30){
  subjectdata<-subset(sumclear,sumclear$subject==i)
  for(j in 1:6){
    activitydata<-subset(subjectdata,subjectdata$activityCode==j)
    a<-6*(i-1)+j
    summary[a,1]<-i
    summary[a,2]<-j
    if(j==1){summary[a,3]<-"WALKING"}
    if(j==2){summary[a,3]<-"WALKING_UPSTAIRS"}
    if(j==3){summary[a,3]<-"WALKING_DOWNSTAIRS"}
    if(j==4){summary[a,3]<-"SITTING"}
    if(j==5){summary[a,3]<-"STANDING"}
    if(j==6){summary[a,3]<-"LAYING"}
    for(p in 4:21){
      summary[a,p]<-mean(activitydata[,p])
    }
  }    
}
colnames(summary)<-c("subject","activity","activityCode","tBodyAccMag-mean()",
                     "tGravityAccMag-mean()","tBodyAccJerkMag-mean()","tBodyGyroMag-mean()",
                     "tBodyGyroJerkMag-mean()","fBodyAccMag-mean()","fBodyBodyAccJerkMag-mean()",
                     "fBodyBodyGyroMag-mean()","fBodyBodyGyroJerkMag-mean()","tBodyAccMag-std()",
                     "tGravityAccMag-std()","tBodyAccJerkMag-std()","tBodyGyroMag-std()",
                     "tBodyGyroJerkMag-std()","fBodyAccMag-std()","fBodyBodyAccJerkMag-std()",
                     "fBodyBodyGyroMag-std()","fBodyBodyGyroJerkMag-std()")
write.table(summary,"summary.txt",col.names=TRUE)
