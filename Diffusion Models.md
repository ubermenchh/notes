# Stable-Diffusion from Scratch ([repo](https://github.com/FareedKhan-dev/create-stable-diffusion-from-scratch))
- diffusion models use noise to encode an image, and then use a noise predictor along with reverse diffusion process to put the image back together.
## Architecture of Stable Diffusion
1. Variational Autoencoder
	- consists of encoder and decoder
	- encoder compresses 512x512 pixel image into smaller 64x64 model in latent space
	- decoder restores the model from latent space into full-size 512x512 pixel image
2. Forward Diffusion
	- adds gaussian noise to the image continuously until only random noise remains.
	- used only during training
3. Reverse Diffusion
	- iteratively undoes forward diffusion
4. Noise Predictor (U-Net)
	- utilizes u-net model for denoising images
	- U-Nets are CNNs (SD uses Resnets)
5. Text Conditioning
	- text prompts are common forms of conditioning
	- CLIP tokenizer analyzes each word in a textual prompt and embeds the data into 768-value vector
	- text prompts are fed from the text encoder to U-Net noise predictor using text transformer
