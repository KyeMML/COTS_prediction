# GCP
Walkthrough of steps taken to host model on GCP. A variety of these services may require you to enable their API on GCP. You should be prompted to do this when required.  


### Kaggle dataset
The dataset includes a folder labelled train_images, containing images of COTS, as well as a csv file labelled train.csv, containing the annotations of these images. 
The annotations include labelled object detection boxes for COTS manually identified in the images.


### Preprocessing
The train.csv file providing annotated labels for COTS dataset is not initially in acceptable format for GCP vertexAI object detection.  
The transformation of this data can be completed as ETL or ELT. To consider this, a comparison of these methods is [here](https://github.com/KyeMML/GCP/blob/main/Data_Lakes_Data_Wharehouses/Data_Lakes.md).  

#### Transforming the training data CSV for object detection  
More information on this is provided by google [here](https://cloud.google.com/vision/automl/object-detection/docs/csv-format).  
The CSV file must comply with the following:
- CSV file and the images it points to must be within the same GCP cloud storage bucket
- The csv file must be UTF-8 encoded, .csv extension
- File has one row for each bounding box or one row for each image with no bounding box. 
- The file must contain one image per line. Images with multiple bounding boxes will be repeated with a unique bounding box coordinates on each row.  
An example is:  
```
TRAIN, gs://folder/image1.png,car,0.1,0.1,,,0.3,0.3,,
TRAIN,gs://folder/image1.png,bike,.7,.6,,,.8,.9,,
UNASSIGNED,gs://folder/im2.png,car,0.1,0.1,0.2,0.1,0.2,0.3,0.1,0.3
TEST,gs://folder/im3.png,,,,,,,,,
```
Where image1 is repeated for the bounding box of a car, and again for the bounding box of a bike on the following line.  
The necessary columns include:  
- assignment (train/test/validation/unassigned)
- URI (points to image location on cloud storage bucket)
- label (in our case it would be COTS)
- bouding box (can be specified with two vertices or all 4 vertices) Eachv ertcex is defined by x and y coordinates, These coordinates must be a float and min max normalised across 0-1.

### Cloud Storage
Create a new bucket with default settings.
Upload necessary Kaggle data to the cloud storage bucket. It acts as a Data Lake, maintaining the data in its raw format. 

### BigQuery
To query the available data in cloud storage:
- create a new dataset in your project ID in the BigQuery dashboard.
- within that new dataset, create a table. 
  - this will provide a range of options including specifying a source. Here specify cloud storage, and then the specific file

