
# Photogrammetry and Gaussian Splatting on Lunar Apollo 17 Imagery

This project explores **Gaussian Splatting for 3D Reconstruction** using Apollo 17 imagery and evaluates how **novel view synthesis** using splatting can enhance **photogrammetric modeling**. The experiment follows a two-part investigation: comparing rendered outputs from splatting with ground truth images and evaluating their utility in improving textured mesh reconstruction.

---

## 📌 Objectives

- Train a **Gaussian Splatting model** using a small Apollo 17 image set.
- Generate **novel views** via neural rendering.
- Perform **photogrammetric modeling** using:
  - Original images only (N=15)
  - Augmented dataset with 10 novel views (N=25)
- Conduct **qualitative and quantitative evaluations** using PSNR, SSIM, and mesh visual comparison.

---

## 📁 Project Structure

```
/
│
├── colmap/
│   ├── colmap/              # Original 15 Apollo 17 images
│   ├── Generated_Images/    # New Views generated using different camera positions
│   ├── images/              # Collection of all the images
│
├── New_Generated/           #Has all the images generated
├── Orginal_image/           #Original Lunar Images
|
├── Part_A/                  #This contains the part A of the assignment which involves generating an Agisoft model.
├── Part_B/                  #This contains part B of assignment which involves generating new views using gaussian splating and then comparing that to the original view.
```

---

## ⚙️ Environment Setup

### System Requirements

- Ubuntu 22.04 (AWS EC2 or local)
- NVIDIA GPU with CUDA support
- Python 3.12
- PyTorch 2.6+
- COLMAP preprocessed `.txt` files


## Part A: 
### 1. Agisoft Photogrammetry (15 Images)
Import Images- Allign images- Build Point Cloud with COnfidence - Build Model - Add texture - Export project and Camera features.
![Moon_model](https://github.com/user-attachments/assets/3e15359b-0939-419a-a1eb-30bacb77a498)


# Workflow Overview

1. **Model Training**  
   -> Used to train the model  

2. **Rendering Trained Outputs**  

   Trained model  
   -> Loads config  
   -> Saves rendered images to `renders/gaussian_images_output`

3. **Quantitative Comparison (PSNR/SSIM)**  
   Rendered images + Ground truth  
   -> Computes PSNR and SSIM  
   -> Outputs metrics


## 🛰️ Part B: Novel Views

# Novel View Generation and New Photogrammetry Workflow

1. **Generate Novel Views**  
   Command:  
   `ns-render camera-path --load-config outputs/apollo17-splating/unnamed/splatfacto/<timestamp>/config.yml --output-path renders/novel_views`

   Trained model  
   -> Generates `novel_views.mp4` video of unseen views

2. **Convert Novel Views to Images**  
   Command:  
   `mkdir -p /home/ubuntu/colmap/images3 && ffmpeg -i novel_views.mp4 -vf "select='not(mod(n\\,3))'" -frames:v 10 /home/ubuntu/colmap/images3/novel_%02d.png`

   `novel_views.mp4`  
   -> Extracts every 3rd frame  
   -> Saves 10 images to `colmap/images3/`

3. **New Photogrammetry**  
   Tool: **Agisoft Metashape**

   Original 15 images  
   + 10 novel images  
   -> Reconstructed a new textured mesh  
   -> Standard photogrammetry pipeline used

4. **Novel View Images**  

![novel_01](https://github.com/user-attachments/assets/d82b6cf1-50ba-4ece-ae60-f12902766da6)

![novel_02](https://github.com/user-attachments/assets/19d3edd0-f919-49c2-8ae2-a6bf7c459db0)

![novel_03](https://github.com/user-attachments/assets/777dbd01-2e29-4846-bbc2-a8c5c7065ab1)

![novel_04](https://github.com/user-attachments/assets/2b1b120f-fd57-4f86-9345-20efc463152c)


![novel_05](https://github.com/user-attachments/assets/1d8668d3-0bb8-4f83-9830-13dde23eb703)

![novel_06](https://github.com/user-attachments/assets/2d5f00ff-3211-4286-a5ab-fd41a77a7657)

![novel_07](https://github.com/user-attachments/assets/b3cb07a9-ea56-4eed-8217-2c961309136d)

![novel_08](https://github.com/user-attachments/assets/0b90472e-ccfd-41bb-84fb-d258432a687c)

![novel_09](https://github.com/user-attachments/assets/c0071feb-6f63-44ca-90b9-5187d855607a)

![novel_10](https://github.com/user-attachments/assets/7f53edce-2ab6-4e8f-a339-954b9c7e9c46)

## Qualitative Comparison of Models

| Method                   | Observation                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| Original Photogrammetry  | Mesh appears rough with noticeable holes and poor texture coverage from oblique views |
| Augmented with Splatting | Surfaces are more complete, textures align better, and occluded regions are more accurately reconstructed |


