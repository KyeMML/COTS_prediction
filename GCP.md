# GCP
Walkthrough of steps taken to host model on GCP  

### The dataset
The dataset includes a folder labelled train_images, containing the images of COTS, as well as a csv file labelled train, containing the annotations of these images. 
The annotations include labelled object detection boxes for COTS in the images.

### Cloud Storage
Create a new bucket with default settings.
Upload necessary Kaggle data to the cloud storage bucket. It acts as a Data Lake, maintaining the data in its raw format. 

### BigQuery
To query the available data in cloud storage:
- create a new dataset in your project ID in the BigQuery dashboard.
- within that new dataset, create a table. 
  - this will provide a range of options including specifying a source. Here specify cloud storage, and then the specific file

### Preprocessing
Some initial transformations are necessary to enable the data to be accepted for autoML in GCP.

###
