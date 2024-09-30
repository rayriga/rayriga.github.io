+++
title = "Ollama! My Llama!"
date = 2024-09-29
[extra]
abstract = "The perks of private open source AI toolchains"
+++

The perks of private open source AI toolchains

## First Touch

My first extended experience with modern AI was a "fine-tuned ChatGPT" which was part of a SaaS (Software-as-a-Service) being used for my latest startup venture. This "talking rubber duck" of sorts was helpful in that it provided not the solution, but a reliable _catalysis_ of the correct approach to any given challenge. Alas, this SaaS was not cheap, and the subscription expired.

ChatGPT is an example of a Large Language Model, or LLM. These models are trained on specialized hardware: massive supercomputer clusters far out of the budget of the typical sole proprietor, startup, or SMB. ChatGPT is a proprietary service of the company [OpenAI](https://openai.com), which is a strange name for a company with secret code. To the contrary, some other companies such as Meta open source their LLMs, and some even open source their training data. Meta's series of general purpose LLMs, [Llama](https://github.com/meta-llama/llama-models), is one of the best known. Another AI company called [Hugging Face](https://huggingface.co) is known for hosting various open source AI models and training data including Llama to download for free. 

The models themselves do not require the training data to run. In other words, once a model is trained, all one needs is some less-prohibitively-expensive specialized hardware to run open source AI models at home (or on premise) using an entirely open source and private toolchain. Sounds easy, right? Sorta.

## The Less-Prohibitively-Expensive Specialized Hardware

The thing about these models is, they require specialized hardware to run. If you happen to be a PC gamer, a reasonably modern GPU might be enough to get you going, but I don't fit that bill. I am, however, a "homelab" tinkerer, meaning I like to run servers at home. I've run specialized haradware at home over the years for everything from bulk vinyl digitization to cryptocurrency mining to an unmetered on-premise geocoding API service. My latest homelab project is something called a "compute module cluster": a small-scale version of the distributed computing systems used in enterprises and data centers with container orchestration systems like [Kubernetes](https://kubernetes.io). The compute module cluster, called a [Turing Pi](https://turingpi.com), allows for experiments with these systems at home, at no additional cost besides electricity—and one of the benefits of these small-scale compute module clusters is their energy efficiency. In fact, the whole cluster is powered by something called a PicoPSU which maxes out at only 160W. 

With a compute cluster already in hand, it was time to learn more about the available LLM-ready GPU compute modules on the market, and I soon discovered the [NVIDIA Jetson](https://developer.nvidia.com/embedded-computing) line of products. After some reasearch, I selected a module called the Orin NX 16GB, a midrange model in the line. According to the spec sheet, this module can handle "Up to 100 (Sparse) INT8 TOPs and 50 (Dense) INT8 TOPs," for a retail price of under $1,000. What are TOPs? Trillions of Operations Per second. In this case, 8-bit integer operations. In other words, this unassuming little RAM-looking chip is a BEAST. In practice, this module is able to run LLMs with 8 billion parameters while barely breaking a sweat.

## Meet Orin Jetson

The form factor of the Orin NX is the correct match for the Turing Pi compute cluster. So, I plugged it in and started trying to flash it (in other words, set it up). Doing so was a minor ordeal of bare metal installations of Ubuntu 20.04 and 22.04 back and forth, and switching back and forth between the Turing Pi and a dedicated carrier board for each attempt, and cross-referencing documentation from Stack Overflow, NVIDIA Developer Forums, Turing Pi Discord Forums, and GitHub Gists. This was a summer of bleeding edge open source development for Orin hardware. But, I got it done.

Once the Orin was running in the Turing Pi cluster, it was time to install some LLM software. Initial attempts to directly install and run an open source program called `ollama` as a server failed. Fortunately, there's `jetson-containers`. This is a Docker-based open source package that allows turnkey access to not only `ollama` but many other open source AI programs as well, on Jetson hardware. Once `jetson-containers` was set up, running `ollama` as a server was easy. And with `ollama`, it's just as easy to choose from literally thousands of open source LLMs, AI code completion models, and computer vision models. 

## The Many Clients of Ollama

With the `ollama` server running on the Orin and exposed to the internal local area network on a custom port, it was time to set up some remote clients. First, for my Mac and iOS devices, I installed [Enchanted](https://github.com/AugustDev/enchanted), an open source GUI for `ollama` written in Swift. This allowed for ongoing conversations with a number of models from everyday devices while at home. I can even ask questions (or, in LLM terms, prompts) to Enchanted via speaking.

Next up was the code editor. [Continue](https://www.continue.dev) is an open source AI code assistant that easily integrates with both [VS Codium](https://vscodium.com) (the open source version of Microsoft's VS Code) and the local `ollama` server. 

With that, there are now three ways to interact with this private, fully open source toolchain of home AI:
1. Speak or type prompts to various models (usually `llama3.1` at the moment) via Enchanted
2. Use code suggestion and code completion features via various models (usually `deepseek-coder-v2` at the moment) from VS Codium with Continue
3. Directly `ssh` into the Jetson module to upgrade `jetson-containers` as needed and manage models via the command line `ollama` client
 
## Conclusion

In all honesty, there were several failed attempts over as many months that occurred with setting up this Jetson module before the events described above. There was even a moment of doubt and regret where I thought I'd never get it working. Now, I no longer doubt nor regret the value of this ambitious project. In fact, I like having my very own talking rubber duck!
