# TASI_ERAv2_S11

## Objective

1. Train ResNet18 on Cifar10 for 20 Epochs. The assignment must:
    1. pull master Github code to Google Colab for train, test, util functions etc.
    2. Google colab file must:
        1. train resnet18 for 20 epochs on the CIFAR10 dataset
        2. show loss curves for test and train datasets
        3. show a gallery of 10 misclassified images
        4. show gradcam output on 10 misclassified images. Ensure gradcam is applied at least on chanel size of 5x5.
2. Apply these transforms while training:
    1. RandomCrop(32, padding=4)
    2. CutOut(16x16)

## Dataset - CIFAR10

1. Training data size -50,000, Test data size - 10,000
2. Image shape - 3 x 32 x 32 (colour image - RGB)
3. Number of classes 10 - plane, car, bird, cat, deer, dog, frog, horse, ship, truck

Example images from training set  
![image](https://github.com/sayanbanerjee32/TSAI_ERAv2_S8/assets/11560595/711aed42-d235-45f3-b7e1-729fbb8a01fe)

## Augmentation Strategies
Following modules of albumentations library is used for image augmentations
1. RandomCrop - This is applied on half of the images. The images are first padded to 40x40 and then random crop is applied to arrive at 32x32 size.
2. HorizontalFlip - this flips half of the images horizontally
3. CoarseDropout - This is applied on half of the images. The images are first padded to 48x48 and then a hole 8x8 is created and then centre crop of 32x32 is applied. Below are some example images after applying all image augmentations.

Example of augmented images  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/assets/11560595/c43ee567-3639-4f06-9ef3-3de0fc575f26)


## Model features
[Final notebook is available here](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/blob/main/S10_SayanBanerjee.ipynb) 

### Reusable assets
This notebook uses a [master package](https://github.com/sayanbanerjee32/TASI_vision_master) that implements the following reusable functionalities 
- Image augmentations
- Model architecture
- Train and test functions
- other utility functions
    
### Learning Rate Scheduler
Training uses OneCycle Poliy for updating learning rate after each iteration.  

    - Number of Epochs: 24  
    - Batch size: 512  
    - Total Iteration: (50,000 / 512) * 24 = 98 * 24 = 2352  
    - Max learning rate at Epoch: 5 (i.e. 0.21 * 24 epoch or 0.21 * 2352 =  iteration 494)  
    - Max learning rate: 0.00254  
    - Min learning rate: 0.000254  

Learning rate range test: Suggested LR: 2.54E-03  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/assets/11560595/3a5b805c-3482-4d15-a3a3-1e58b92062ca)

Learning rate update at the time of training  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/assets/11560595/86f287c4-5782-40d8-8d41-194e9dcef5bb)

### Performance
    - Number of parameters: 6,573,120
    - Train set accuracy: 98.13%
    - Test set Accuracy: 93.05%

Train and test loss and accuracy curve suggests overfitting.  Dropout is used in all layers in the model. Use of `weight_decay` of 0.001 could not improve overfitting. Use of `weight_decay` of 0.01 was impacting test accuracy badly.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/assets/11560595/75184809-a166-4116-97fd-571d5b0fb226)


### Error analysis

There are total 695 wrong classifications out of 10,000 test images. Top 5 confused cases of prediction
|target|prediction|number of images|
|------|----------|----------------|
|dog|cat|66|
|cat|dog|57|
|truck|car|28|
|cat|deer|28|
|cat|frog|26|  
  
Example of 9 misclassified images in descending order of loss.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S10/assets/11560595/a0ee0cc2-b071-46b2-a944-e928cb78cb5f)



