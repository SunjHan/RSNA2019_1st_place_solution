# RSNA Intracranial Hemorrhage Detection
This is the source code for the first place solution to the [RSNA2019 Intracranial Hemorrhage Detection Challenge](https://www.kaggle.com/c/rsna-intracranial-hemorrhage-detection).

Solution write up: [Link](https://www.kaggle.com/c/rsna-intracranial-hemorrhage-detection/discussion/117210#latest-682640).

## Solutuoin Overview
![image](https://github.com/SeuTao/RSNA2019_1st_place_solution/blob/master/png/overview.png)

#### Dependencies
- opencv-python==3.4.2
- scikit-image==0.14.0
- scikit-learn==0.19.1
- scipy==1.1.0
- torch==1.1.0
- torchvision==0.2.1

### CODE
- 2DCNN
- SequenceModel

# 2D CNN Classifier

## Preprocessing
![image](https://github.com/SeuTao/RSNA2019_1st_place_solution/blob/master/png/preprocessing.png)

Prepare csv file:

download data.zip:  https://drive.google.com/open?id=1buISR_b3HQDU4KeNc_DmvKTYJ1gvj5-3

1. convert dcm to png
```
python3 prepare_data.py -dcm_path stage_1_train_images -png_path train_png
python3 prepare_data.py -dcm_path stage_1_test_images -png_path train_png
python3 prepare_data.py -dcm_path stage_2_test_images -png_path test_png
```

2. train

```
python3 train_model.py -backbone DenseNet121_change_avg -img_size 256 -tbs 256 -vbs 128 -save_path DenseNet121_change_avg_256
python3 train_model.py -backbone DenseNet169_change_avg -img_size 256 -tbs 256 -vbs 128 -save_path DenseNet169_change_avg_256
python3 train_model.py -backbone se_resnext101_32x4d -img_size 256 -tbs 80 -vbs 40 -save_path se_resnext101_32x4d_256
```

3. predict
```
python3 predict.py -backbone DenseNet121_change_avg -img_size 256 -tbs 4 -vbs 4 -spth DenseNet121_change_avg_256
python3 predict.py -backbone DenseNet169_change_avg -img_size 256 -tbs 4 -vbs 4 -spth DenseNet169_change_avg_256
python3 predict.py -backbone se_resnext101_32x4d -img_size 256 -tbs 4 -vbs 4 -spth se_resnext101_32x4d_256
```

After single models training,  the oof files will be saved in ./SingleModelOutput(three folders for three pipelines). 

After training the sequence model, the final submission will be ./FinalSubmission/final_version/submission_tta.csv

# Sequence Models

## Sequence Model 1
![image](https://github.com/SeuTao/RSNA2019_1st_place_solution/blob/master/png/s1.png)

## Sequence Model 2
![image](https://github.com/SeuTao/RSNA2019_1st_place_solution/blob/master/png/s2.png)

#### Path Setup
Set data path in ./setting.py

#### download 
csv.zip: https://drive.google.com/open?id=1qYi4k-DuOLJmyZ7uYYrnomU2U7MrYRBV

feature samples: https://drive.google.com/open?id=1lJgzZoHFu6HI4JBktkGY3qMk--28IUkC


#### Sequence Model Training
```
CUDA_VISIBLE_DEVICES=0 python main.py
```
The final submissions are in the folder ../FinalSubmission/version2/submission_tta.csv

## Final Submission
### Private Leaderboard:
- 0.04383

### TODO
- [ ] Pre-trained models
- [ ] 2DCNN + SeqModel end-to-end training 
- [ ] 3DCNN training