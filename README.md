# ComfyUI-PixtralLlamaVision + Molmo
 Do not clone it, this is for debug and test only, please clone it from SeanScripts/ComfyUI-PixtralLlamaVision
 For loading and running Pixtral and Llama 3.2 Vision models
Molmo Model:
https://huggingface.co/cyan2k/molmo-7B-D-bnb-4bit

Includes six nodes:
- Load Molmo Model
- Generate Text with Molmo
- Load Pixtral Model
- Generate Text with Pixtral
- Load Llama Vision Model
- Generate Text with Llama Vision

Along with some utility nodes for working with text:
- Parse Bounding Boxes
- Regex Split String
- Regex Search
- Regex Find All
- Regex Substitution
- Join String
- Select Index
- Slice List

## Installation

Available in [ComfyUI-Manager](https://github.com/ltdrdata/ComfyUI-Manager) as ComfyUI-PixtralLlamaVision. When installed from ComfyUI-Manager, the required packages will be installed automatically.

If you install by cloning this repo into your custom nodes folder, you'll need to install `transformers >= 4.45.0` to load Pixtral and Llama Vision models, and you'll also need to make sure `accelerate`, `bitsandbytes`, and `torchvision` are updated. You can install these in the windows portable version of ComfyUI with:
`python_embeded\python.exe -m pip install -r ComfyUI\custom_nodes\ComfyUI-PixtralLlamaVision\requirements.txt`

Models should be placed in the `ComfyUI/models/pixtral` and `ComfyUI/models/llama-vision` folders, with each model inside a folder with the `model.safetensors` file along with any config files and the tokenizer.

You can get a 4-bit quantized version of Pixtral-12B and/or Llama-3.2-11B-Vision-Instruct which is compatible with these custom nodes here:


[https://huggingface.co/SeanScripts/pixtral-12b-nf4](https://huggingface.co/SeanScripts/pixtral-12b-nf4)

[https://huggingface.co/SeanScripts/Llama-3.2-11B-Vision-Instruct-nf4](https://huggingface.co/SeanScripts/Llama-3.2-11B-Vision-Instruct-nf4)

Unfortunately, the Pixtral nf4 model has considerably degraded performance on some tasks, like OCR. The Llama Vision model seems to be better for this task.

## Examples
Example of Molmo D (based on Qwen2-7B) BNB (you need to install tensorflow)
![image](https://github.com/user-attachments/assets/a91d3d75-19e0-4091-846f-42232395c4d7)

Example Pixtral image captioning (not saving the output to a text file in this example):
![Example Pixtral image captioning workflow](pixtral_caption_example.jpg)

Both models should work very well for image captioning, even in 4-bit quantization. You can also customize your captioning instructions.

Example Pixtral image comparison:
![Example Pixtral image comparison workflow](pixtral_comparison_example.jpg)

I haven't been able to get image comparison to work well at all with Llama Vision. It doesn't give any errors, but the multi-image understanding just isn't there. The image tokens have to be **before** the question/instruction and consecutive for the model to even be able to see both images at once (I found this out by looking at the image preprocessor cross-attention implementation), and even then, it seems to randomly mix up which is the first/second, left/right, the colors between them and other details. It doesn't seem usable for purposes involving two images in the same message, in my opinion. Not sure whether the non-quantized model is better at this.

Since Pixtral directly tokenizes the input images, it's able to handle them inline in the context, with any number of images of any aspect ratio, but it's limited by token lengths, since each image can be around 1000 tokens.

Example Llama Vision object detection with bounding box:
![Example Llama Vision object detection with bounding box workflow](llama_vision_bounding_box_example.jpg)

Both models kind of work for this, but not that well. They definitely have some understanding of the positions of objects in the image, though. Maybe it needs a better prompt. Or a non-quantized model. Or a finetune. But it does sometimes work.
