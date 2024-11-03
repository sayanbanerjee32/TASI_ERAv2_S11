# GradCam

## Objective

1. Train ResNet18 on Cifar10 for 20 Epochs. The assignment must:
    1. pull master Github code to Google Colab for train, test, util functions etc.
    2. Google colab file must:
        1. train resnet18 for 20 epochs on the CIFAR10 dataset
        2. show loss curves for test and train datasets
        3. show a gallery of 10 misclassified images
        4. show gradcam output on 10 misclassified images. Ensure gradcam is applied at least on channel size of 5x5.
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
3. CoarseDropout - This is applied on half of the images. The images are first padded to 64x64 and then a hole 16x16 is created and then centre crop of 32x32 is applied. Below are some example images after applying all image augmentations.

Example of augmented images  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/adaaed6c-0a37-467f-8d5a-0c5f2b6c810c)



## Model features
[Final notebook is available here](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/blob/main/S11_SayanBanerjee.ipynb) 

### Reusable assets
This notebook uses a [master package](https://github.com/sayanbanerjee32/TASI_vision_master) that implements the following reusable functionalities 
- Image augmentations
- Model architecture
- Train and test functions
- other utility functions including but not limited to grad-cam, LR range test, plotting train and loss and accuracy curves, confusion matrix, plotting misclassified images.
    
### Model Architecture

ResNet18 is used for this model.
 - This has 4 Resnet blocks having 2 convolutions (followed by BatchNorm) each and RELU as activation at the end of each block.
 - Total params: 11,173,962

### Learning Rate Scheduler
Training uses OneCycle Policy for updating learning rate after each iteration.  

    - Number of Epochs: 20  
    - Batch size: 512  
    - Total Iteration: (50,000 / 512) * 20 = 98 * 20 = 1960  
    - Max learning rate at Epoch: 8 (i.e. 0.40 * 20 epoch or 0.40 * 1960 =  iteration 784)  
    - Max learning rate: 0.385  
    - Min learning rate: 0.0385  
    - Optimizer: SGD with momentum of 0.9

Learning rate range test: Suggested LR: 3.85E-01  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/b504b0f1-9a9c-4fa7-898b-273ded02df2e)


Learning rate update at the time of training  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/78e12b0e-49e9-4e81-8fdc-04423bc023d3)


### Performance
    - Train set accuracy: 94.84%
    - Test set Accuracy: 90.88%

Train and test loss and accuracy curve suggests overfitting.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/08201a2f-d32d-4f11-8c73-aa443b02d69b)

Grad-cam images for 10 correct predictions.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/3ae78d19-9753-499c-bf6e-d2cc067956d5)


### Error analysis

There are total 912 wrong classifications out of 10,000 test images. Below is the confusion matrix.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/412bc4fa-b327-48c4-9f5d-fd460fbecd68)

 
Example of 10 misclassified images in descending order of loss.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/7fbb134b-f77b-49c3-81a4-5402ca223225)

Grad-cam images for the same 10 misclassified images below.  
![image](https://github.com/sayanbanerjee32/TASI_ERAv2_S11/assets/11560595/80168962-8906-4a2f-bd83-d8711ae093a7)





