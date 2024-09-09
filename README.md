# Build a Large Language Model (From Scratch)

Here is a space for me to work through things myself. 


Link to the official [source code repository](https://github.com/rasbt/LLMs-from-scratch)


DONE:
- setup
- ch02
- ch03

Left off on ch05 - exercise solutions.

<br>
<br>

# Running Notes

  - Lower top-k and temperatures result in less random outcomes, which is desired when creating educational content, technical writing or question answering, data analyses, code generation, and so forth
  - Higher top-k and temperatures result in more diverse and random outputs, which is more desirable for brainstorming tasks, creative writing, and so forth


- In order for the Jupyter notebooks to work, make sure you have the ".venv Python 3.12..." selected
- Maybe try the isolation again in Docker instead?
- Looks like there's CUDA errors, I have torch w/ CUDA installed so it might be an issue with the venv? I hate venv.

- However, in practice, using the softmax function for normalization, which is better at handling extreme values and has more desirable gradient properties during training, is common and recommended. (for getting weights to sum to 1)
attn_weights_2 = torch.softmax(attn_scores_2, dim=0)


- We use dropout of 0.1 above, but it's relatively common to train LLMs without dropout nowadays
- Modern LLMs also don't use bias vectors in the `nn.Linear` layers for the query, key, and value matrices (unlike earlier GPT models), which is achieved by setting `"qkv_bias": False`
- We reduce the context length (`context_length`) of only 256 tokens to reduce the computational resource requirements for training the model, whereas the original 124 million parameter GPT-2 model used 1024 tokens
  - This is so that more readers will be able to follow and execute the code examples on their laptop computer
  - However, please feel free to increase the `context_length` to 1024 tokens (this would not require any code changes)
  - We will also load a model with a 1024 `context_length` later from pretrained weights


- In deep learning, instead of maximizing the average log-probability, it's a standard convention to minimize the *negative* average log-probability value; in our case, instead of maximizing -10.7722 so that it approaches 0, in deep learning, we would minimize 10.7722 so that it approaches 0
- The value negative of -10.7722, i.e., 10.7722, is also called cross-entropy loss in deep learning



- We use a relatively small dataset for training the LLM (in fact, only one short story)
- The reasons are:
  - You can run the code examples in a few minutes on a laptop computer without a suitable GPU
  - The training finishes relatively fast (minutes instead of weeks), which is good for educational purposes
  - We use a text from the public domain, which can be included in this GitHub repository without violating any usage rights or bloating the repository size


- For example, Llama 2 7B required 184,320 GPU hours on A100 GPUs to be trained on 2 trillion tokens
  - At the time of this writing, the hourly cost of an 8xA100 cloud server at AWS is approximately \\$30
  - So, via an off-the-envelope calculation, training this LLM would cost 184,320 / 8 * \\$30 =  \\$690,000


# Cloud Resources

This section describes cloud alternatives for running the code presented in this book.

While the code can run on conventional laptops and desktop computers without a dedicated GPU, cloud platforms with NVIDIA GPUs can substantially improve the runtime of the code, especially in chapters 5 to 7.

&nbsp;

## Using Lightning Studio

For a smooth development experience in the cloud, I recommend the [Lightning AI Studio](https://lightning.ai/) platform, which allows users to set up a persistent environment and use both VSCode and Jupyter Lab on cloud CPUs and GPUs.

Once you start a new Studio, you can open the terminal and execute the following setup steps to clone the repository and install the dependencies:

```bash
git clone https://github.com/rasbt/LLMs-from-scratch.git
cd LLMs-from-scratch
pip install -r requirements.txt
```

(In contrast to Google Colab, these only need to be executed once since the Lightning AI Studio environments are persistent, even if you switch between CPU and GPU machines.)

Then, navigate to the Python script or Jupyter Notebook you want to run. Optionally, you can also easily connect a GPU to accelerate the code's runtime, for example, when you are pretraining the LLM in chapter 5 or finetuning it in chapters 6 and 7.

<img src="https://sebastianraschka.com/images/LLMs-from-scratch-images/setup/README/studio.webp" alt="1" width="700">

&nbsp;

## Using Google Colab

To use a Google Colab environment in the cloud, head over to [https://colab.research.google.com/](https://colab.research.google.com/) and open the respective chapter notebook from the GitHub menu or by dragging the notebook into the *Upload* field as shown in the figure below.

<img src="https://sebastianraschka.com/images/LLMs-from-scratch-images/setup/README/colab_1.webp" alt="1" width="700">


Also make sure you upload the relevant files (dataset files and .py files the notebook is importing from) to the Colab environment as well, as shown below.

<img src="https://sebastianraschka.com/images/LLMs-from-scratch-images/setup/README/colab_2.webp" alt="2" width="700">


You can optionally run the code on a GPU by changing the *Runtime* as illustrated in the figure below.

<img src="https://sebastianraschka.com/images/LLMs-from-scratch-images/setup/README/colab_3.webp" alt="3" width="700">


&nbsp;