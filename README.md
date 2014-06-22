DataCleanProject
================

Data Clean Project Assignment

#Run_analysis.R
#
##This Code file will laod the data from the UCI HAR Dataset.
##It will consolidate data and features, and activity labels
##It will create a file called "tidydata.txt"
#
##Source Data is found here:
##Https://d396qusza40orc.cloudfront.net/getdata%Fprojectfiles%2FUCI%20%HAR%20Dataset.zip
#
##Use of this dataset in publications must be acknowledged by referencing #the following publication [1] 
#
##[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. #Reyes-Ortiz. 
##Human Activity Recognition on Smartphones using a Multiclass #Hardware-Friendly Support 
##Vector Machine. International Workshop of #Ambient Assisted Living (IWAAL 2012). 
##Vitoria-Gasteiz, Spain. Dec 2012
#
##This dataset is distributed AS-IS and no responsibility implied or #explicit can be 
##addressed to the authors or their institutions for its use #or misuse. Any commercial 
##use is prohibited.
#
##Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita.November 2012.
#
##Adjust datadir to point to an appropriate data source directory.

datadir<-"~/DSCert/Cleaning Data/data/UCI HAR Dataset"

##Read in datasets, combine features and Labels

activitylabels <-read.table(paste(datadir,"activity_labels.txt", sep="/"))[,2]
features <-read.table(paste(datadir,"features.txt", sep='/'))[,2]

##Read data sets and combine train and test

xtestfit <-read.table(paste(datadir,"test/X_test.txt",sep="/")
xtrainfit <-read.table(paste(datadir,"train/X_train.txt", sep="/")
x<- rbind(xtestfit, xtrainfit)    

stestfit <-read.table(paste(datadir,"test/subject_test.txt",sep="/")
strainfit <-read.table(paste(datadir,"train/subject_train.txt", sep="/")
s<- rbind(stestfit, strainfit)    

ytestfit <-read.table(paste(datadir,"test/y_test.txt",sep="/")
ytrainfit <-read.table(paste(datadir,"train/y_train.txt", sep="/")
y<- rbind(ytestfit, ytrainfit)  

##Assign the features to x
headers<-make.names(features,unique=TRUE) 
names(x)<-headers

##create vector for mean or std features
##meanstd_features will have to much, so we will extrad just the mean and std deviations 
meanstd_features<-grepl("mean\\(\\)?std\\(\\)",features)
x<-x[,meanstd_features]

##combine into one data frame
combined<-cbind(s,x,y)
names(combined)[1]<-"SubjectID"
names(combined)[2]<-"Activity"

##Aggregate function provides data set required.
fit <-aggregate(.~SubjectID+Activity,data=combined,FUN=mean)

##Update the activity names
fit$Activity <-factor(fit$Activity, labels=activies)

##Create output table
write.table(fit,file="./tidydata.txt",sept"\t",row.names=FALSE)
