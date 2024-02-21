# A reading note of LaMa's Code
/bin/train.py (*function* make_training_model) **->** \
/saicinpainting/training/trainers/\_\_init_\_.py (*function* get_training_model_class) **->** ../trainers/default.py(*class* DefaultInpaintingTrainingModel) **->** \
../trainers/default.py (*function* get_ramp) **->** /saicinpainting/utils.py (*class* LinearRamp LadderRamp)\ **->** \

**DefaultTrainingModel** \
*function* init  
*function* forward 

**BaseinpaintingTrainingModule** \
if not predict only : not read yet \


