#FILE DESCRIPTION
move.nii.gz: original moving image
fix.nii.gz: original target fixed image

fix_uni_res.nii.gz: original moving image -> unified resolution (0.8mm x 0.8mm x 5mm)
move_uni_res.nii.gz: original target fixed image ->  unified resolution (0.8mm x 0.8mm x 5mm)

fix_uni_res_dim.nii.gz: original moving image -> unified resolution -> same dimension (550 x 550 x 100)
move_uni_res_dim.nii.gz: original moving image -> unified resolution -> same dimension (550 x 550 x 100)

linearBCV - fast affine transformation, use all cores of the machine
linearBCVslow - slow affine transformation, only use one CPU core

applyLinearBCVfloat - use to transform Nifti scan image
applyLinearBCV - use to transform manual label image

af_matrix.txt 

Please skip step 1 & 2 if you know how to resample and zero padding
step 1: resample via Freesurfer
export FREESURFER_HOME=/fs4/masi/damons/scratch_mcr_backup/mcr/freesurfer5.3/
source $FREESURFER_HOME/SetUpFreeSurfer.sh
mri_convert -vs 0.8 0.8 5 fix.nii.gz fix_uni_res.nii.gz
mri_convert -vs 0.8 0.8 5 move.nii.gz move_uni_res.nii.gz 

step 2: zeropadding via fsl 
fslroi fix_uni_res.nii.gz fix_uni_res_dim.nii.gz 0 550 0 550 0 100
fslroi move_uni_res.nii.gz move_uni_res_dim.nii.gz 0 550 0 550 0 100

step 3: Affine linear transformation
./linearBCV -F fix_uni_res_dim.nii.gz -M move_uni_res_dim.nii.gz -O af 
# or 
./linearBCVslow -F fix_uni_res_dim.nii.gz -M move_uni_res_dim.nii.gz -O af (slow only use one CPU core)

# it will generate your affine transformation file af_matrix.txt. 

step 4: apply tranformation txt file to your moving image, and generate final output final.nii.gz
./applyLinearBCVfloat -M move_uni_res_dim.nii.gz -D final.nii.gz -A af_matrix.txt


