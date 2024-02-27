# 一，在autoDL上测试（未成功）

# 二，在MacOS下载安装并测试(未完成)

**GPT-SoVITS指南**

https://www.yuque.com/baicaigongchang1145haoyuangong/ib3g1e

### 测试结果：

1，声音分割成功；

2，声音训练成功；

3，文字合成声音失败：据UP主说是网络原因，按其方法没能解决。

to be continued……

# 三，在ubuntu上docker里运行GPT-SoVITS(未完成)

docker images: docker pull breakstring/gpt-sovits:latest

```jsx
# run docker container:
docker run --gpus all -it \
       --name mygptsovits \
			 -p 9874:9874 \
			 -p 9873:9873 \
		   -p 9872:9872 \
			 -p 9871:9871 \
       breakstring/gpt-sovits:dev-20240209 bash

# container-id: ba6f73d99fb2

docker run --rm -it --gpus all --env=is_half=False \
	--volume=/home/kemove/Projects/GPT-SoVITS/output:/workspace/output \
  --volume=/home/kemove/Projects/GPT-SoVITS/logs:/workspace/logs \
  --volume=/home/kemove/Projects/GPT-SoVITS/SoVITS_weights:/workspace/SoVITS_weights \
  --workdir=/workspace \
  -p 9880:9880 -p 9871:9871 -p 9872:9872 -p 9873:9873 -p 9874:9874 \
  --shm-size="16G" -d breakstring/gpt-sovits:dev-20240209
```

# 四，在ubuntu的conda环境里运行GPT-SoVITS(成功)

教材：https://www.yuque.com/baicaigongchang1145haoyuangong/ib3g1e/xyyqrfwiu3e2bgyk

按“整合包教程”操作，但是从github下载源码，配置conda环境。

## 1，下载源码

> [git@github.com](mailto:git@github.com):sitexa/GPT-SoVITS.git
> 

## 2，建立conda环境

> 
> 
> 
> ```
> conda create -n GPTSoVits python=3.9
> conda activate GPTSoVits
> bash install.sh
> ```
> 

## 3，安装pytorch&cuda

> `pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118`
> 

## 4，运行过程

（1）使用AU等工具预处理声音源文件,放入文件夹：/home/kemove/audio

（2）折分音频文件，输入：/home/kemove/audio；输出：/home/kemove/Projects/GPT-SoVITS/output/slicer_opt

（3）自动语音识别(asr)，输入：output/slicer_opt；输出：output/asr_opt ⇒ slicer_opt.list

（4）对标，输入：output/asr_opt/slicer_opt.list

（5）训练，输入对标文件：output/asr_opt/slicer_opt.list ；输入音频文件夹：output/slicer_opt

（6）合成，选择声音模型：SoVITS模型及GPT模型

参考音频：/home/kemove/audio/test.wav；

参考文本：“庆历四年春，滕子京谪守巴陵郡，越明年，政通人和，百废俱兴，乃重修岳阳楼。”

要合成的文本：“予观夫巴陵胜状，在洞庭一湖。衔远山，吞长江，浩浩汤汤，横无际涯；朝晖夕阴，气象万千。此则岳阳楼之大观也，前人之述备矣。然则北通巫峡，[南极潇湘](https://www.zhihu.com/search?q=%E5%8D%97%E6%9E%81%E6%BD%87%E6%B9%98&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22678554176%22%7D)，迁客骚人，多会于此，览物之情，得无异乎？”

点击合成，几秒钟就生成声音，试听/下载。
