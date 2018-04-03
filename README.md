# pytorch np-transforms
Pytorch transforms based on numpy arrays. I decided to re-write some of the standard pytorch transforms using only numpy operations that allow for High Dynamic Range image manipulation.
The file `exr_data.py` include some methods for loading HDR images in `exr` format into numpy arrays and writing numpy arrays into `exr` files.

Create a dataset that loads a dataset if hdr images in `.exr` format:

```python
import exr_data

trf = np_transforms.Compose([
    np_transforms.Scale(size=(256, 256)),
    np_transforms.RandomCrop(size=(224, 224)),
    np_transforms.RandomVerticalFlip(prob=0.5),
    np_transforms.RandomHorizontalFlip(prob=0.5),
    np_transforms.RotateImage(angles=(-15, 15)),
    np_transforms.ToTensor(),
])

data_train = exr_data.exrData(root=os.path.join(ROOT_DIR, 'train'),
                                  loader=exr_data.exr_loader,
                                  transform=trf)
                                  
```

Write a numpy array into a `.exr` file:

```python
import exr_data

# let's assume we have a numpy array called 'pic' with the image stored in the form [HxWxC]

(Rs, Gs, Bs) = [pic[:, :, channel].tostring() for channel in range(pic.shape[-1])]
exr_data.exr_writer(out_path, size=im_size, pixel_values={'R': Rs, 'G': Gs, 'B': Bs})

```
