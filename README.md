## Our manuscript has been accepted by CVPR! 

# Suppressing Uncertainties for Large-Scale Facial Expression Recognition

                                  Kai Wang, Xiaojiang Peng, Jianfei Yang, Shijian Lu, and Yu Qiao
                              Shenzhen Institutes of Advanced Technology, Chinese Academy of Sciences
                                         Nanyang Technological University, Singapore
                                         {kai.wang, xj.peng, yu.qiao}@siat.ac.cn
					 
![image](https://github.com/kaiwang960112/Self-Cure-Network/blob/master/imgs/scn-moti.png)

## Abstract

Annotating a qualitative large-scale facial expression dataset is extremely difficult due to the uncertainties caused by ambiguous facial expressions, low-quality facial images, and the subjectiveness of annotators. These uncertainties lead to a key challenge of large-scale Facial Expression Recognition (FER) in deep learning era. To address this problem, this paper proposes a simple yet efficient Self-Cure Network (SCN) which suppresses the uncertainties efficiently and prevents deep networks from over-fitting uncertain facial images.
	Specifically, SCN suppresses the uncertainty from two different aspects: 1) a self-attention mechanism over mini-batch to weight each training sample with a ranking regularization, and 2) a careful relabeling mechanism to modify the labels of these samples in the lowest-ranked group. Experiments on synthetic FER datasets and our collected WebEmotion dataset validate the effectiveness of our method. 
	Results on public benchmarks demonstrate that our SCN outperforms current state-of-the-art methods with 88.14% on RAF-DB, 60.23% on AffectNet, and 89.35% on FERPlus.
	
![image](https://github.com/kaiwang960112/Self-Cure-Network/blob/master/imgs/SCNpipeline.png)

## Self-Cure Network

Our SCN is built upon traditional CNNs and consists of three crucial modules: i) self-attention importance weighting, ii) ranking regularization, and iii) relabeling, as shown in Figure 2. Given a batch of face images with some uncertain samples, we first extract the deep features by a backbone network. The self-attention importance weighting module assigns an importance weight for each image using a fully-connected (FC) layer and the sigmoid function. These weights are multiplied by the logits for a sample re-weighting scheme. To explicitly reduce the importance of uncertain samples, a rank regularization module is further introduced to regularize the attention weights. In the rank regularization module, we first rank the learned attention weights and then split them into two groups, i.e. high and low importance groups. We then add a constraint between the mean weights of these groups by a margin-based loss, which is called rank regularization loss (RR-Loss). To further improve our SCN, the relabeling module is added to modify some of the uncertain samples in the low importance group. This relabeling operation aims to hunt more clean samples and then to enhance the final model. The whole SCN can be trained in an end-to-end manner and easily added into any CNN backbones.

<img src="http://latex.codecogs.com/gif.latex?\frac{\partial J}{\partial \theta_k^{(j)}}=\sum_{i:r(i,j)=1}{\big((\theta^{(j)})^Tx^{(i)}-y^{(i,j)}\big)x_k^{(i)}}+\lambda \theta_k^{(j)}" />

### Self-Attention Importance Weighting
We introduce the self-attention importance weighting module to capture the contributions of samples for training. It is expected that certain samples may have high importance weights while uncertain ones have low importance. Let $mathbf{F}=[\mathbf{x}_1, \mathbf{x}_2, \ldots, \mathbf{x}_N]\in R^{D\times N}$ denotes the facial features of $N$ images, the self-attention importance weighting module takes $\mathbf{F}$ as input, and  output an importance weight for each feature. Specifically, the self-attention importance weighting module is comprised of a linear fully-connected (FC) layer and a sigmoid activation function, which can be formulated as ,

\begin{equation}
\alpha_i =\sigma( \mathbf{W}_a^\top \mathbf{x}_i ),
\end{equation}
where $\alpha_i$ is the importance weight of the \textit{i}-th sample, $\mathbf{W}_a$ is the parameters of the FC layer used for attention, and $\sigma$ is the sigmoid function. This module also provides reference for the other two modules.



