The dataset is created from transformation of raw data obtained from:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Transformation:
==============================
Data files: Raw data consists of 2 sets of data, test and training data.
  - Files containing signals taken from embedded accelerometer and gyroscope:
      - X_test.txt
      - X_train.txt

  - Files containing the activity data when signals was taken:
      - y_test.txt
      - y_train.txt

  - Files containing the subject (volunteers) participating in this experiment:
      - subject_test.txt
      - subject_train.txt

Merging the data
  - 1) Merge the 3 test files (y_test.txt + subject_test.txt + X_test.txt) to get test data
  - 2) Merge the 3 training files (y_train.txt + subject_train.txt + X_train.txt) to get training data
  - 3) Append the training data to the test data
  - 4) Extract out only the mean() and std() features (total 66) and remove all other signals data

Reference files
  - activity_labels.txt (decodes activity data to more readable activity name)
  - features.txt (contains the feature vector ["column name" for signals data])

Grouping and calculating mean
  - 1) Grouped data by "Activity" and "Subject"
  - 2) For each unique Activity-Subject group, calculate the mean of each feature vector

Creating tidy data
  - 1) Creates the "Measurement" variable and places the average data into variable "Average"
  
Variables:
==============================
- prefix 't' is to denote time, signals captured at frequency of 50Hz
- prefix 'f' is to indicate frequency domain signals after applying Fast Fourier Transform
- Body/Gravity is signal classification
- Acc is signal data obtained from accelerometer, in unit g
- Gyro is signal data obtained from gyroscope, in unit radians/second
- Jerk is the time derivation from signal
- Mag is the signal magnitude
- XYZ is used to denote 3-axial signals in the X, Y and Z directions
- mean(): Mean value
- std(): Standard deviation

- 1) Activity
  - 1 WALKING
  - 2 WALKING_UPSTAIRS
  - 3 WALKING_DOWNSTAIRS
  - 4 SITTING
  - 5 STANDING
  - 6 LAYING

- 2) Subject
  - Identification number: 1 to 30

- 3) Measurement
  - tBodyAcc-mean()-X (second)
  - tBodyAcc-mean()-Y (second)
  - tBodyAcc-mean()-Z (second)
  - tBodyAcc-std()-X (second)
  - tBodyAcc-std()-Y (second)
  - tBodyAcc-std()-Z (second)
  - tGravityAcc-mean()-X (second)
  - tGravityAcc-mean()-Y (second)
  - tGravityAcc-mean()-Z (second)
  - tGravityAcc-std()-X (second)
  - tGravityAcc-std()-Y (second)
  - tGravityAcc-std()-Z (second)
  - tBodyAccJerk-mean()-X (second)
  - tBodyAccJerk-mean()-Y (second)
  - tBodyAccJerk-mean()-Z (second)
  - tBodyAccJerk-std()-X (second)
  - tBodyAccJerk-std()-Y (second)
  - tBodyAccJerk-std()-Z (second)
  - tBodyGyro-mean()-X (second)
  - tBodyGyro-mean()-Y (second)
  - tBodyGyro-mean()-Z (second)
  - tBodyGyro-std()-X (second)
  - tBodyGyro-std()-Y (second)
  - tBodyGyro-std()-Z (second)
  - tBodyGyroJerk-mean()-X (second)
  - tBodyGyroJerk-mean()-Y (second)
  - tBodyGyroJerk-mean()-Z (second)
  - tBodyGyroJerk-std()-X (second)
  - tBodyGyroJerk-std()-Y (second)
  - tBodyGyroJerk-std()-Z (second)
  - tBodyAccMag-mean() (second)
  - tBodyAccMag-std() (second)
  - tGravityAccMag-mean() (second)
  - tGravityAccMag-std() (second)
  - tBodyAccJerkMag-mean() (second)
  - tBodyAccJerkMag-std() (second)
  - tBodyGyroMag-mean() (second)
  - tBodyGyroMag-std() (second)
  - tBodyGyroJerkMag-mean() (second)
  - tBodyGyroJerkMag-std() (second)
  - fBodyAcc-mean()-X (Hz)
  - fBodyAcc-mean()-Y (Hz)
  - fBodyAcc-mean()-Z (Hz)
  - fBodyAcc-std()-X (Hz)
  - fBodyAcc-std()-Y (Hz)
  - fBodyAcc-std()-Z (Hz)
  - fBodyAccJerk-mean()-X (Hz)
  - fBodyAccJerk-mean()-Y (Hz)
  - fBodyAccJerk-mean()-Z (Hz)
  - fBodyAccJerk-std()-X (Hz)
  - fBodyAccJerk-std()-Y (Hz)
  - fBodyAccJerk-std()-Z (Hz)
  - fBodyGyro-mean()-X (Hz)
  - fBodyGyro-mean()-Y (Hz)
  - fBodyGyro-mean()-Z (Hz)
  - fBodyGyro-std()-X (Hz)
  - fBodyGyro-std()-Y (Hz)
  - fBodyGyro-std()-Z (Hz)
  - fBodyAccMag-mean() (Hz)
  - fBodyAccMag-std() (Hz)
  - fBodyBodyAccJerkMag-mean() (Hz)
  - fBodyBodyAccJerkMag-std() (Hz)
  - fBodyBodyGyroMag-mean() (Hz)
  - fBodyBodyGyroMag-std() (Hz)
  - fBodyBodyGyroJerkMag-mean() (Hz)
  - fBodyBodyGyroJerkMag-std() (Hz)

- 4) Average
  - Average value of each measurement
		
