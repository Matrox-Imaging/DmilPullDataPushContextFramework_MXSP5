# DmilPullDataPushContextFramework_MXSP5

Date: 01/19/2022

**MIL Version** MIL X SP5  

**Type** New example

**Description**  
This program pulls the data to a server and push a new context to one client or multipule clients using Distributed MIL.

The project structure, including the xml and png files, aims to be copied in "\Users\Public\Documents\Matrox Imaging\MIL\Examples\Processing\Classification\DmilPullDataPushContextFramework" of the MIL installation directory to be displayed by the MIL example launcher.

**Link**  
https://github.com/Matrox-Imaging/DmilPullDataPushContextFramework_MXSP5

**Story line**  
Imagine a factory with 10 production lines, each line has a smart camera that performs classification. Therefore there is a trained classification context involved. For various reasons like adding a new class, or new setup conditions, we might need to update the context. At the moment to update the context we need to stop the application, replace the context and run the application/line which could costs a lot.  Here we are showing a new method to update the context transparently on the go. Meaning that the cameras will be connected to a Client and using distributed MIL we upload the context into the cameras, preprocess them and change the used context without any delay. 
Now imagine that we want to automate finetuning process as well. Hence we need to automatically gather new images that are critical in our opinion, for example an image with low prediction score. This means all the cameras will be saving images after days or weeks we will have a new dataset of images that are challenging in our solution. In the next step, the client gathers all the images from all the cameras. Note that the saved images will be labeled by the system but an operator needs to verify the labels and fix the mislabeled ones. The more labels we fix the better will be the improvement. 
After preparing the new dataset merged with the original dataset, we can finetune the context and upload it to each camera. 

**Note**  
1-This example is developed based on MIL classification module, Deap Learning technology and edge-cloud arthitecture. However it can be used for most of context based modules. 
2-THis example uses the 10 pasta images and the context from MIL example "Mclass".
3-In the mega-data images UUID_MD.mim that are saved with the original low-scored images UUID_IM.mim, we save an array that contains prediction index, score, and time stamp, which can be opened in MIL Copilot.

**Server side (system that grabs live images)**
There are three folders:
1- Contexts:  All the contexts including the original and the updated will be placed in this folder.
2- Inference: The original solution running on the server.
3- SavedImages: The location to save all the critical original images that have low prediction score, named "UUID_IM.mim. For each image, a UUID_MD.mim that contains mega data are saved: prediction index, score amd time.

Tasks on the server side:
1- Save the images: This part could vary based on the original solution. 
Since the client is constantly looking for new files and it could start downloading a image file that has not been completely saved on camera, we save the images first with ".tmp" extension and then rename them to ".mim" when the file is completely save. 
The saving process needs to be done using a low priority thread in order to prevent any slow down. 
Makes sure it does not save more that a number of images in order to not to overflow the disk. 
2- Update the context: 
This process is also done using a low priority thread to prevent any slow down. 
It Constantly looks for new uploaded contexts. After detecting the context, it replaces the original context with the updated one on the hard drive. 
Then it loads the updated context, preprocess it, wwitch the ID of used context, and Free the previous context. 

**Client side**
There are two applications:
1- Uploader:  upload updated contexts to the server.
2- Collector: download the low-scored images and mega-data images from all the servers, then delete all those images that saved on the servers.

**Test systems**
"The example has been tested with Windows 10 64-bit and VS2017 x64 config.
