/////////////////////Construction Custom Dataset///////////////////////////////////////
//navigate to custom_dataset folder
We tried training on both coco and open images dataset where we extracted images and there respective annotations to build a custom dataset
Steps for coco :
1. Inorder to extract data from coco you must have annoations and images either 2017 or 2014
2. After that you just have to copy the path files of annoations and images folder, paste it in extract.py and run it to get custom dataset in yolo format

Steps for open images dataset :
1. install oid toolkit
2. with the help of oid you will be able to extract specific classes and their labels also their will be csv folder created
3. after geting those images and annotations, you must convert them in yolo format for that run extracting_and_converting_annotations.py
4. Next you must generate train.txt and test.txt for thhat copy the path of images in creating-train-test-txt-files.py
5  Also for yolo format you must create .names and .data files that can be created by pasting image folder path in creating-files-data-and-names.py

###After this copy .data .names in config folder

///////////////////Instruction for running train_oid.py////////////////////////////////

1. After doing all the above just paste all the paths of image,.names,.data,.wts in train_oid.py
3. Run the train_oid.py

///////////////Checkpoints
two checkpoints folder has weight files for the customised and the orignal model which where both
trained on custom dataset

References:
https://github.com/EscVM/OIDv4_ToolKit.git
https://github.com/AlphaArslan/ML_COCO_extract_specific_classes
https://www.udemy.com/course/training-yolo-v3-for-objects-detection-with-custom-data/learn/lecture/
https://towardsdatascience.com/training-yolo-for-object-detection-in-pytorch-with-your-custom-dataset-the-simple-way-1aa6f56cf7d9
https://towardsdatascience.com/object-detection-and-tracking-in-pytorch-b3cf1a696a98
https://github.com/cfotache/pytorch_custom_yolo_training
