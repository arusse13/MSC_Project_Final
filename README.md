"# msc-data-science-project-2020_21---files-arusse13" 
# Actional-Structural Graph Convolution Networks Applied to Martial Arts,Dancing and Sports Datasets for Action Recognition
- [x] Objective1 -- Deployment and training of AS-GCN on reduced NTU RGB+D 120 dataSet
- [ ] Objective2 -- Model Evaluation by Hand
- [x] Objective3 -- Deployment and training of AS-GCN on reduced MADS dataset

### This repository contains the following files for each objective of this project for both cross subject (xsub) and cross view (xview) evaluation approachs;
#### config.ymal -- AS-GCN configuration files e.g. batch size, learning rate, GPU devices etc.
#### epoch28_model2.pt -- pytorch model files including all weights and bias, note that there are two models (1 & 2) output at each epoch
#### log.txt -- pytorch log of training and testing including top1 and top5 scores
#### Actional_Structural_Graph_Convolution_Networks_Applied_to_Martial_Arts_Dancing_and_Sports_Datasets_for_Action_Recognition.pdf -- full project techincal report
#### (see https://github.com/arusse13/MSc-Project for the technical project proposal)

### This project uses the source code published by [Li, Maosen et al](https://github.com/limaosen0/AS-GCN)

### Below are the batch commands used to preprocess the MADS dataset using [FFmpeg](http://ffmpeg.org/) and [Openpose](https://github.com/CMU-Perceptual-Computing-Lab/openpose)
### 1. Rename each sequence according to the file naming convention – SsssCcccPpppRrrrAaaa
### 2. Split each seqeuence into 5 second blocks using the following batch command which recursively searchs in all sub-directories for .avi files to process
    FOR /r %%v in (*.avi) DO ffmpeg -i %%v -c copy -map 0 -segment_time 00:00:05 -f segment -reset_timestamps 1 S%%02d%%~nv.avi
### 3. Resize each .avi file to a resolution using the following batch command which recursively searchs in all sub-directories for .avi files to process
    FOR /r %%v in (*.avi) DO Ffmpeg -i %%v -c:v libx264 -s 340x256 %%~nv_DR.avi
### 4. Extract joint (skeleton) data for each sequnce using Openpose (portable) using the following batch command which recursively searchs in all sub-directories for .avi files to process. Note that the output is a JSON file per frame.
    FOR /r %%v in (*.avi) DO md "%%~nv" && bin\OpenPoseDemo.exe --video "%%v" --write_json "%%~nv" --display 0 --render_pose 0
### 5. Use the python files in this [st-gcn-data repo](https://github.com/abcinje/st-gcn-data) to re-merge the JSON files into complete sequences
### 6. Use this [kinetics_gendata.py](https://github.com/yysijie/st-gcn/blob/master/tools/kinetics_gendata.py) python file to preprocess the data fully for AS-GCN. The data will need to be split into training and evaluation portions prior to this.
