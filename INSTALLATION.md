This is the *custom* guide to use the [original stable-diffusion repo](https://github.com/CompVis/stable-diffusion) for apple silicon/m1 macs.

> Note: I have only tested this on my M1 mac with pytorch nightly build from september 01, 2022

---

# Dependency setup

- `git clone https://github.com/NISH1001/stable-diffusion/ --branch feature/apple-silicon`
- create virtual env `python -m venv venv`
	- Maybe setup the `.python-version` to `3.8.13`
- activate `source venv/bin/activate`
- upgrade pip : `pip install pip -U`
- Install dependencies
	- `pip install torch==1.13.0.dev20220901 torchvision==0.14.0.dev20220901 torchaudio==0.13.0.dev20220901 --extra-index-url https://download.pytorch.org/whl/nightly/cpu -r requirements.txt`
	    - Note: The nightly build from september 02, 2022 threw some errors.
- Do editable install for the codebase: `pip install -e .`


## pytorch specifics to do strictly

In the `site-packages/` installation of [[PyTorch]], in `<parent>/site-packages/torch/nn/functional.py` line number **2511**, in the `layer_norm(...)` function, pass the input as contiguous. Something like:

``` python
      return torch.layer_norm(input.contiguous(), normalized_shape, weight, bias, eps, torch.backends.cudnn.enabled)
```

# Download the model

Download model checkpoint `sd-v1-4.ckpt` from https://huggingface.co/CompVis/stable-diffusion-v-1-4-original
	- Copy this to `models/ldm/stable-diffusion-v1/model.ckpt`

# Usage

```bash
python scripts/txt2img.py --plms --n_samples=1 --n_rows=1 --n_iter=1 --prompt "Existential crises guy"
```
