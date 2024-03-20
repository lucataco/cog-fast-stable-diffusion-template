# Fast-Stable-Diffusion Cog template

## About
This is a generic fast-stable-diffusion [Cog](https://github.com/replicate/cog) template to push huggingface models as a Cog model.

This template uses hf-transfer by default, with the option to use [pget](https://github.com/replicate/cog) to fetch the weights at setup() for shorter cold-boots.

## Setup

### Setup Cog wrapper
First, find a huggingface SDXL model that you want to run on Replicate. For this example we'll choose [playgroundai/playground-v2.5-1024px-aesthetic](https://huggingface.co/playgroundai/playground-v2.5-1024px-aesthetic).

From the huggingface model page you'll see the recommended `num_inference_steps` as `50`, and the `guidance_scale` as `3`. We'll use the slug(playgroundai/playground-v2.5-1024px-aesthetic), `25`, and `3.0` for lines `#15`, `#16`, and `#17` in the predict.py file:

```python
# Change model name here:
MODEL_NAME = "playgroundai/playground-v2.5-1024px-aesthetic"
DEFAULT_INFERENCE_STEPS = 25
DEFAULT_GUIDANCE_SCALE = 3.0
```

For examples of other models, see the [examples.txt](./examples.txt) file.

### Upload to Replicate
Next, create the namespace for the new model [in Replicate](https://replicate.com/create).

Then, after `cog login`, upload your model via:

    cog push r8.im/<username>/<model-name>

### Run your model
Finally, You should be able to run SDXL with relatively 
![alt text](output.0.jpg)

### Pget
To use our development tool: [pget](https://github.com/replicate/pget) instead of hf-transfer, uncomment the lines in `predict.py` that refers to pget.

Then run `git lfs install` and `git clone` the huggingface repo to download the weights

Once downloaded you can tar the files in the folder. `cd` into the weights directory and run:

    tar -cf playground-v2.5.tar ./*

Then you can upload your tar file to your own GCP storage solution with gsutil. For example:

    gsutil cp playground-v2.5.tar gs://replicate-weights/playgroundai/playground-v2.5-1024px-aesthetic.tar

Be sure to use the full path of the uploaded tar file in line `#23` of `predict.py`. For example:

```python
MODEL_CACHE_URL = "https://weights.replicate.delivery/default/playgroundai/playground-v2.5-1024px-aesthetic.tar"
```