

1. **Hugging Face Diffusers**：一个全面的库，支持最新的图像和音频生成的diffusion模型。允许用户轻松定制模型和调度器​ ([GitHub](https://github.com/huggingface/diffusers))​。
    
2. **OpenAI Guided Diffusion**：提供了分类器条件模型和架构改进，具有不同图像大小和配置的变体​ ([GitHub](https://github.com/openai/guided-diffusion))​。
    
3. **Facebook Research DiT**：将diffusion模型与transformers结合，提供了易于实施的扩展性diffusion模型的预训练模型​ ([GitHub](https://github.com/facebookresearch/DiT))​。
    
4. **Stable Diffusion by Stability AI**：特性包括基于文本指导和分类器自由指导比例的潜在diffusion模型，包括对机器生成图像的水印​ ([GitHub](https://github.com/Stability-AI/StableDiffusion))​。
    
5. **Diffusion Models from Scratch**：旨在从零开始学习diffusion模型，专注于理解和实现模型的机制​ ([GitHub](https://github.com/gmongaras/Diffusion_models_from_scratch))​。
    
6. **Diffusion Models Tutorial**：提供了一种易于接触的方法来学习diffusion模型概念，包括DDIM等高级采样方法​ ([GitHub](https://github.com/phykn/diffusion_models_tutorial))​。
    
7. **DiffusionCLIP**：专注于使用diffusion模型进行文本引导的图像操作，需要特定的预训练模型来处理不同的数据集​ ([GitHub](https://github.com/gwang-kim/DiffusionCLIP))​。
    
8. **DiffuSeq**：使用diffusion模型进行序列到序列文本生成的项目，展示了在NLP中应用diffusion技术​ ([GitHub](https://github.com/Shark-NLP/DiffuSeq))​。
    
9. **Microsoft VQ-Diffusion**：基于VQ-VAE的diffusion模型用于文本到图像的生成，展示了与相似参数计数的AR模型相比的性能改进​ ([GitHub](https://github.com/microsoft/VQ-Diffusion))​。
    
10. **Minimal Diffusion**：一个简化的diffusion模型实现，突出了类条件diffusion模型的效率和有效性​ ([GitHub](https://github.com/VSehwag/minimal-diffusion))​。
    
11. **Denoising Diffusion Probabilistic Models**：重点在去噪分数匹配和采样上的PyTorch实现​ ([GitHub](https://github.com/lucidrains/denoising-diffusion-pytorch))​。
    
12. **EDM by NVLabs**：探索基于diffusion的模型的设计空间，提供FID计算和采样器消融的工具​ ([GitHub](https://github.com/NVlabs/edm))​。
    
13. **Diffusion From Scratch**：以简化的方式重建Stable Diffusion模型，旨在教育目的​ ([GitHub](https://github.com/Animadversio/DiffusionFromScratch))​。
    
14. **Pytorch Diffusion Model Tutorial**：提供了关于diffusion概率模型的简单教程，适合初学者​ ([GitHub](https://github.com/Jackson-Kang/Pytorch-Diffusion-Model-Tutorial))​。
    
15. **Google DiffSeg**：利用diffusion模型的注意力信息进行无监督零次分割，展示了diffusion模型除了生成任务之外的多样性​ ([GitHub](https://github.com/google/diffseg))​。
    

每个项目对diffusion模型生态系统的贡献都是独一无二的，范围从基础研究和教育资源到图像生成、NLP和分割的创新应用。虽然它们提供了广泛的能力，潜在用户应考虑每个项目相关的具体要求，如计算资源和数据集的可用性，以及与每个项目相关的学习曲线。




DDPM   Denoising Diffusion Probabilistic Models
[一文带你看懂DDPM和DDIM（含原理简易推导，pytorch代码） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/666552214)
是一种基于扩散过程的生成模型，它利用概率过程模拟数据的生成过程。这类模型通过逐步引入噪声将数据转换成纯噪声，然后通过一个逆过程逐步去除噪声来生成数据
### DDPM的演变和后续模型

DDPM之后，扩散模型经历了多种改进和变体的发展，这些模型在效率、生成质量和多样性等方面都有所提升。一些关键的演变包括：

1. **改进的训练技术**：研究者们引入了各种训练技巧和损失函数的改进，比如改进的参数化方法和采用不同的噪声水平策略，以提高模型的训练效率和生成质量。
    
2. **条件生成扩散模型**：通过引入条件变量（如文本描述、类别标签等），这类模型能够根据给定的条件生成特定的数据。这为文本到图像生成、风格迁移等应用打开了大门。
    
3. **效率提升**：原始的DDPM模型需要经过多个步骤（通常是几百步）来生成数据，这使得生成过程相对缓慢。后续模型通过改进算法（如学习率调度、快速采样方法等）显著提高了采样效率。
    
4. **多模态和跨域应用**：随着模型和算法的进步，扩散模型被扩展到多模态数据生成和跨域学习，如从图像到文本、音频生成等。
    
5. **扩散模型与其他模型的结合**：为了进一步提高生成质量和效率，研究者们尝试将扩散模型与其他类型的生成模型（如GAN、VAE）结合，利用各自的优点来提升性能。


后续的扩散模型发展中，确实出现了许多有影响力的变体和改进，其中一些模型由于其特定的改进和应用而获得了广泛关注。下面列举一些比较著名的后续扩散模型及其特点：

1. **Improved Denoising Diffusion Probabilistic Models (iDDPM)**：这是对原始DDPM模型的一种改进，通过优化训练策略和损失函数，提高了生成质量和效率。
    
2. **Guided Diffusion**：引入了条件信息（如文本描述）来引导生成过程，使得模型能够根据指定的条件生成特定的图像，这在文本到图像生成中尤其有用。
    
3. **Denoising Diffusion Implicit Models (DDIM)**：这种模型通过修改扩散过程中的采样策略，减少了生成所需的采样步骤，从而加快了图像的生成速度，同时保持了生成质量。
    
4. **Conditional Diffusion Models**：这类模型专注于如何更有效地将条件信息（如类别标签、文本描述等）整合到扩散模型中，以控制生成内容的特性。
    
5. **Latent Diffusion Models (LDM)**：通过在潜在空间而非像素空间进行扩散过程，这种模型显著提高了计算效率和生成速度，同时能生成高质量的图像。
    
6. **ADM (Adversarial Diffusion Models)**：结合了对抗性生成网络（GAN）的理念，通过在扩散模型的训练过程中引入对抗性损失，以进一步提高生成图像的真实感和多样性。
    
7. **Score-Based Generative Models**：虽然不完全是扩散模型的直接后续，但这些模型使用了类似的思想，通过估计数据分布的梯度（得分）来生成数据。它们在数学上与扩散模型密切相关，并在图像和音频生成等领域展示了优异的性能。
    

这些模型及其变体各有侧重，有的注重提高生成效率，有的致力于提升生成质量，还有的专注于特定的应用场景，如根据文本提示生成图像。随着研究的深入，扩散模型在生成任务中的应用范围和性能都有了显著的提升。



### FID分数（Frechet Inception Distance）

FID分数是用来评估生成图像质量的一种指标，它通过比较生成图像与真实图像在特征空间中的分布差异来工作。具体来说，FID使用一个预训练的Inception网络提取图像特征，然后计算这些特征的均值和协方差。FID分数是基于这两组特征（生成图像的和真实图像的）的均值和协方差计算出的Frechet距离（也称作Wasserstein-2距离）。FID分数越低，表示生成图像与真实图像的分布越接近，通常意味着图像质量越高。

### CLIP分数

CLIP（Contrastive Language-Image Pre-training）是一个由OpenAI开发的多模态模型，能够理解图像和文本之间的关系。CLIP分数通常用于评估生成图像与给定文本描述的一致性。具体来说，它通过计算图像和文本在CLIP模型嵌入空间中的相似度来实现。相比于FID分数主要评价图像质量，CLIP分数更注重评价生成图像的文本相关性和语义准确性。CLIP分数越高，表示生成的图像与文本描述越匹配。

总的来说，FID分数和CLIP分数各自针对图像生成任务的不同方面进行评估：FID关注于图像的视觉质量和真实性，而CLIP分数关注于图像与文本描述的语义一致性。这两个指标常被并用来全面评估图像生成模型的性能。

## CFG-Scale
CFG-Scale，即“Classification-Free Guidance Scale”，是一个在生成模型中使用的参数，特别是在条件生成模型如DALL·E 2、CLIP-Guided Diffusion Models等中非常重要。这个参数用于调整生成过程中对指定条件（例如文本描述）的依赖程度，从而影响生成图像的质量和与文本描述的一致性。

### 工作原理

在条件生成模型中，模型旨在根据给定的条件（如文本描述）生成相应的图像。CFG-Scale参数控制着模型在遵循给定条件与保持图像多样性之间的平衡。简而言之，它影响着生成图像的创造性与条件的一致性。

- **高CFG-Scale值**：会使模型更加严格地遵循给定的条件，通常会产生与条件更一致、细节更丰富的图像，但可能牺牲一些多样性和创造性。
- **低CFG-Scale值**：则会让模型在生成过程中有更多的自由度，可能产生更多样化的图像，但这些图像可能与给定的条件关联度不够高。

### 应用

CFG-Scale是一个重要的超参数，通过调整它，用户可以根据需求平衡生成图像的创造性与条件一致性。在实际应用中，找到一个合适的CFG-Scale值通常需要通过实验来确定，以确保生成的图像既符合条件也具有一定的多样性和新颖性。