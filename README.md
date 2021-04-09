# DLC_Bonsai_Workflows

 This Repo Contains:
 - Bonsai Workflow: Inputs camera feed from a pylon camera, tracks an animal's position using a DLC node, defines a region of interest in pixels for opto stim, and outputs a logical string to a specified COM Port (True when animal is in ROI, False when animal is not in ROI)
 - Additional Bonsai Outputs:
   - Video
   - CSV with position xy coordinates of animal
   - CSV with timestamps corresponding to position coordinates
   - CSV with timestamps corresponding to logical output of bonsai
 - Arduino Code: Reads logical string from output of bonsai, if True sets digital output arduino pin to HIGH, if False sets digital output ardunio pin to LOW

Dependencies:
- Spike2
- DeepLabCut https://github.com/DeepLabCut/DeepLabCut/blob/master/docs/installation.md
- DeepLabCut-live https://github.com/DeepLabCut/DeepLabCut-live
- Pylon viewer version 5.1 installed (if using pylon camera)
- Bonsai https://bonsai-rx.org/docs/installation/ (also see https://github.com/bonsai-rx/deeplabcut)
  - Once installed, go to manage packages in Bonsai interface and add the following packages:
   - Pylon Basler
   - Deeplabcut
   - Ephys

 General Steps:\
     - Create a video of animal\
     - Label frames of video in deeplabcut gui and train model\
     - Export Model: deeplabcut.export_model('config_path\config.yaml', iteration=None, shuffle=1, trainingsetindex=0, snapshotindex=None, TFGPUinference=True, overwrite=False, make_tar=True)\
     - In bonsai workflow adjust parameters of nodes\
           - Crop: Specify cropping parameters of camera (may not be necessary)\
           - DetectPose: Specify modelFilename and modelPoseConfig, both found in exported models folder within your dlc model folder\
           - ConfidenceThreshold: Specify confidence of dlc model for pose estimation\
           - GetBodyPart: Define a bodypart of interest that was trained in deeplabcut\
           - Specify ROI in pixels in greaterThan and lessThan nodes (Measure something in pixels of known length to make conversion)\
           - Specify paths to save CSV and video files to\
     - In spike2, create a new channel to read in the HIGH and LOW digital output from arduino & in the graphical editor make conditionals for creating pulses\            
