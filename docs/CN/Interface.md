# ComfyUI User Interface


## Intro
Node-based interfaces are most commonly found in the 3D Design and VFX industries. You might have encountered them if you've used tools like Maya or Blender3D.

In these interfaces, every node executes some code.

Nodes have inputs, values that are passed to the code, and ouputs, values that are returned by the code.

Using the mouse, users are able to:
- create new nodes
- edit parameters (variables) on nodes
- connect nodes together by their inputs and outputs

In ComfyUI, every node represents a different part of the Stable Diffusion process. By creating and connecting nodes that perform different parts of the process, you can run Stable Diffusion.


## Nodes Explained
To understand nodes, we have to understand a bit about how Stable Diffusion works.

Let's take a look at the default workflow.

![](/docs/media/Workflow_Default.png)

If you're not on the default workflow or you've been messing around with the interface, click Load Default on the right sidebar.

### Load Checkpoint
The .safetensors or .ckpt checkpoint models you use to generate images have 3 main components:
- Unet: to perform the "diffusion" process, the step-by-step processing of images that we call generation
- CLIP model: to convert text into a format the Unet can understand
- VAE: to decode the image from latent space into pixel space (also used to encode a regular image from pixel space to latent space when we are doing img2img)

In the ComfyUI workflow this is represented by the Load Checkpoint node and its 3 outputs (MODEL refers to the Unet).

![](/docs/media/LoadCheckpoint.gif)

### CLIP Text Encode
The CLIP model is used to convert text into a format that the Unet can understand (a numeric representation of the text). We call these embeddings.

![](/docs/media/CLIPTextEncode.gif)

The CLIP Text Encode nodes take the CLIP model of your checkpoint as input, take your prompts (postive and negative) as variables, perform the encoding process, and output these embeddings to the next node, the KSampler.

### Latent Image
The Empty Latent Image node is used to create an empty latent space image. This is the image that the KSampler will start from when generating a new image.

![](/docs/media/EmptyLatentImage.gif)

### KSampler
In Stable Diffusion images are generated by a process called sampling.
In ComfyUI this process takes place in the KSampler node. This is the actual "generation" part, so you'll notice the KSampler takes the most time to run when you queue a prompt.

![](/docs/media/KSampler.gif)

The KSampler takes the following inputs:
- model: MODEL ouput (Unet) from Load Checkpoint node
- positive: the positive prompt encoded by the CLIP model (CLIP Text Encode node)
- negative: the negative prompt encoded by the CLIP model (other CLIP Text Encode node)
- latent_image: an image in latent space (Empty Latent Image node)

Since we are only generating an image from a prompt (txt2img), we are passing the latent_image an empy image using the Empty Latent Image node.
(You can also pass an actual image to the KSampler instead, to do img2img. We'll talk about this below)

### VAE Decode
![](/docs/media/VAEDecode.gif)

The VAEDecode node takes 2 inputs:
- The VAE that came with our checkpoint model (you can also add your own VAE)
- The latent space image that our KSampler has finished denoising.

The VAE is used to translate an image from latent space to pixel space.
It passes this final pixel image to the Save Image node, which is used to show us the image and let us download it.


## Basic Operation

### Add Node
You can add a node by right clicking on blank space -> Add Node.

![](/docs/media/Add_Node.png)

### Search Node
You can double click on blank space to get the list of all nodes and a searchbar.

![](/docs/media/Search_Node.png)

### Select&Drag Nodes
CTRL + drag lets you select multiple nodes. You can move them together with SHIFT + drag.

![](/docs/media/Select&Drag_Nodes.gif)

### Select Node's Color
You can change the color of nodes to help you stay organized. Right click -> Color -> select color.

![](/docs/media/Select_NodeColor.gif)

### Connect Nodes
If you drag and release an input into blank space, you will get a list of compatible nodes.

![](/docs/media/Connect_Nodes.png)

Inputs and outputs are only compatible if they are the same color. Notice how I can connect the purple input to the purple output, but cannot connect any of the rest.

![](/docs/media/Connect_Nodes_ColorMatching.png)

### Bypass Node
You can bypass a node to let it be ignored when executing the workflow. Right click -> Bypass.

![](/docs/media/Bypass_Node.png)

### Execute
When you click Queue Prompt, the workflow passes through the nodes in the order they're connected, starting from Loaders, which have no inputs, only outputs.

![](/docs/media/Execute.png)

If any nodes are missing inputs, you will not be able to run the prompt.

![](/docs/media/Execute_MissingInput.png)


## Advance Operation

### Add Group

### 