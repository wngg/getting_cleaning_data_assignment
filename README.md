### getting_cleaning_data_assignment

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set.
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set

### start of script
	run_analysis <- function() {
		# load the required packages to perform data manipulation
	    library(plyr)
	    library(dplyr)
    
##### 1. Merges the training and the test sets to create one data set.
	    
		message("reading training data...")
	    train_data <- read.table("./UCI HAR Dataset/train/X_train.txt", sep="", header=FALSE)
	    train_acti <- read.table("./UCI HAR Dataset/train/y_train.txt", header=FALSE)
	    train_subj <- read.table("./UCI HAR Dataset/train/subject_train.txt", header=FALSE)
	    ds_train <- cbind(train_subj, train_acti, train_data)
	    
	    message("reading test data...")
	    test_data <- read.table("./UCI HAR Dataset/test/X_test.txt", sep="", header=FALSE)
	    test_acti <- read.table("./UCI HAR Dataset/test/y_test.txt", header=FALSE)
	    test_subj <- read.table("./UCI HAR Dataset/test/subject_test.txt", header=FALSE)
	    ds_test <- cbind(test_subj, test_acti, test_data)
	    
	    message("merging training and test data...")
	    ds <- rbind(ds_train, ds_test)
	    
##### 4. Appropriately labels the data set with descriptive variable names.
##### Note: Naming the dataset columns here, to make it easier to extract mean() and std() data
#####       using column name
	    
		features <- read.table("./UCI HAR Dataset/features.txt", header=FALSE, stringsAsFactors=FALSE)
	    colnames(ds)[1] <- "Subject"
	    colnames(ds)[2] <- "Activity"
	    
		# column 3 onward are the measurement data
		for(i in 3:length(ds)) {
	        # assign variable names
	        colnames(ds)[i] <- features[i-2,2]
	    }
	    
##### 2. Extracts only the measurements on the mean and standard deviation for each measurement.
	    message("extracting mean() and std()...")
	    ds <- ds[grepl("Subject|Activity|mean[[:punct:]]|std[[:punct:]]", names(ds))]
	
##### 3. Uses descriptive activity names to name the activities in the data set.
##### Note: Activity names are refereneced from file: activity_labels.txt
	    
		activity <- read.table("./UCI HAR Dataset/activity_labels.txt", header=FALSE, stringsAsFactors=FALSE)
	    ds <- merge(x=activity,y=ds,by.x="V1", by.y="Activity", all.y=TRUE)
	    colnames(ds)[1] <- "Activity_Id"
	    colnames(ds)[2] <- "Activity"
	    ds <- select(ds, -Activity_Id)
	    
##### 5. From the data set in step 4, creates a second, independent tidy data set 
#####    with the average of each variable for each activity and each subject.
	    
		message("creating average of each variable group by activity and subject...")
	    ds_avg <- aggregate(. ~ Activity+Subject, data=ds, FUN=mean)
	    ds_avg <- arrange(ds_avg, Activity, Subject)
	
##### ds_avg is not tidy data because column headers are different "measurement" types
##### Each variable you measure should be in one column, 
##### Each different observation of that variable should be in a different row
##### The following script creates the "Measurement" variable and places the average data into variable "Average"
		
	    message("tidying up the data...")
		colname <- names(ds_avg)
	    ds_tidy <- data.frame(matrix(ncol=4, nrow=nrow(ds_avg)*(length(colname)-2)), stringsAsFactors=FALSE)
	    colnames(ds_tidy) <- c("Activity","Subject","Measurement","Average")
	    for(i in 1:nrow(ds_avg)){
	        for(j in 3:length(colname)) {
	            ds_tidy[((i-1)*(length(colname)-2))+(j-2),1] <- ds_avg[i,1]  # Activity
	            ds_tidy[((i-1)*(length(colname)-2))+(j-2),2] <- ds_avg[i,2]  # Subject
	            ds_tidy[((i-1)*(length(colname)-2))+(j-2),3] <- colname[j]  # Measurement
				ds_tidy[((i-1)*(length(colname)-2))+(j-2),4] <- ds_avg[i,j]  # Average
	        }
	    }
	    write.table(ds_tidy, file="./ds_tidy.txt", sep=",", row.name=FALSE)
	}
