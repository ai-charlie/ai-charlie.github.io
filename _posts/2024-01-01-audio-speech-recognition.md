# FUNASR

- **GitHub库**：[https://github.com/alibaba-damo-academy/FunASR](https://github.com/alibaba-damo-academy/FunASR)，[https://github.com/modelscope/FunASR](https://github.com/modelscope/FunASR)。

- **功能特点**
  - **语音识别（ASR）**：支持多语言高精度语音识别，集成了如Paraformer系列等先进的语音识别模型，能准确地将语音转换为文本。
  - **语音端点检测（VAD）**：自动检测语音信号的开始和结束，适用于实时互动场景，可提高识别效率，减少不必要的处理。
  - **标点恢复**：自动为识别文本添加标点，提升文本的可读性。
  - **语言模型**：利用上下文信息提高识别准确性，使识别结果更符合语言习惯。
  - **说话人验证与分离**：可以识别和验证说话人身份，还能实现多说话人分离，在多人对话场景中准确区分不同说话者的语音。
- **技术优势**
  - **高精度**：采用海量数据训练的工业级语音识别模型，如Paraformer-en等，保证了端到端转写效果的精度。
  - **高推理效率**：语音端点检测、语音识别、标点断句等模型均通过onnx量化导出实现推理加速，ASR模型基于Paraformer的非自回归模型，相比自回归模型推理效率优势明显，可同时支持多线并发。
  - **多语言支持**：支持中文、英语、日语、粤语和韩语等多种语言，满足不同用户的需求。
  - **热词自定义**：允许用户定义特定的术语或专有名词，优化识别结果，提高转录的准确性和实用性。
- **应用场景**：适用于会议记录、访谈转录、语音助手、智能客服等多种需要语音识别的场景，能够为用户提供高效、准确的语音转文本服务。

# 其他语音识别算法库

### 深度学习类

- [whisper](https://github.com/openai/whisper)：OpenAI的创意工具，提供转录和翻译服务。训练使用了来自互联网的68万小时的音频文件，多样化的数据范围提高了工具的鲁棒性，能转录99种语言，并可全部翻译成英语。
- [deepspeech](https://github.com/mozilla/deepspeech)：Mozilla的开源语音转文本引擎，模型参考百度深度语音研究论文，具有端到端的可训练性，支持多种语言音频转录，使用Google的TensorFlow进行训练和实现。
- [speechbrain](https://github.com/speechbrain/speechbrain)：用于促进语音相关技术研究和开发的开源工具包，支持语音识别、增强、分离等任务，使用PyTorch作为开发框架，用户可选择传统或基于深度学习的ASR模型。

### 传统模型类

- [kaldi](https://github.com/kaldi-asr/kaldi)：专门为语音识别研究人员创建的语音识别工具，用C++编写，在Apache2.0许可证下发布。主要专注于使用隐马尔可夫模型、高斯混合模型和有限状态传感器等传统模型的语音识别模型，非常可靠，适合学术和行业相关研究。
- [sphinxbase](https://gitcode.com/gh_mirrors/sp/sphinxbase)：开源的语音处理库，是CMU Sphinx项目的组成部分。提供基础的信号处理和特征提取工具，包含从音频流中提取信息所需的工具，实现了Viterbi解码器等，支持HMM和LM的加载和应用，以进行语音到文本的解码。

### 其他类

- [coqui](https://github.com/coqui-ai/stt)：先进的深度学习工具包，适合培训和部署STT模型，提供预先训练的模型以及示例音频文件，支持多种语言，支持实时转录，延迟极低。
- [PhoneticMatching](https://gitcode.com/gh_mirrors/ph/PhoneticMatching)：由微软维护的语音匹配库，专注于音素匹配，能将英文文本转化为国际音标，提供模糊匹配机制等，可用于提高语音指令的理解准确性、协助评估发音质量等。
