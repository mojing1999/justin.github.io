---

layout: post
title: "video codec"
description: "video decode and encode with intel/nvidia hardware"
category: "Project"
tags: [codec, decode, encode]


---

# video_codec project

## 1. 介绍
video_codec 是Justin开源的多媒体编解码硬件加速项目([Github](https://github.com/mojing1999/video_codec))。 支持Intel集显和Nvidia显卡硬件加速。包含以下模块。仅供个人学习使用，商业使用请联系作者(Justin's Email: mojing1999@gmail.com)

#### 1). 封装Intel Media SDK，实现使用Intel集显处理多媒体编解码硬件加速。
	

	- Intel_dec
	- Intel_enc
	- Intel_transcoding
	
#### 2). 封装Nvidia CUDA SDK，实现使用Nvidia显卡处理多媒体编解码硬件加速。
	- nv_dec
	- nv_enc
	- nv_transcoding


#### 3). 相应的测试例子。
	- test_intel_dec
	- test_intel_enc
	- test_intel_transcoding
	- test_nv_dec
	- test_nv_enc
	- test_nv_transcoding

---

## 2. 基于 Intel 集显的硬件编解码加速
	
### intel_dec

#### - 导出 API
参见 jm_intel_dec.h 头文件。

#### - 使用

参见项目 test_intel_dec 。 工程里包含头文件 jm_intel_dec.h 和导入库 intel_dec.lib

```
#include "jm_intel_dec.h"

#pragma comment(lib,"intel_dec.lib")


```


#### - 调用流程


```
handle_yuv_frame_callback()

...

jm_intel_dec_create_handle()
jm_intel_dec_init()
jm_intel_dec_set_yuv_callback() [OPTIONAL]

loop jm_intel_dec_is_exit()
	if(jm_intel_dec_need_more_data()) {
		len = jm_intel_dec_free_buf_len()
		jm_intel_dec_input_data()

		...
		
		//if no more input data
		jm_intel_dec_set_eof()
	}
	
	jm_intel_dec_output_frame() [OPTIONAL]
	//handle_yuv_frame_callback()

end loop

jm_intel_dec_stream_info()

jm_intel_dec_deinit()

```

流程图

![intel_dec](/media/intel_dec_flow.png)
	 
---

### intel_enc

#### - 导出 API
参见 jm_intel_enc.h 头文件。

#### - 使用

参见项目 test_intel_enc 。 工程里包含头文件 jm_intel_enc.h 和导入库 intel_enc.lib

```
#include "jm_intel_enc.h"
#pragma comment(lib,"intel_enc.lib")
```

#### 支持编码格式

```
	0 - H.264 / AVC
	1 - H.265 / HEVC
	2 - MPEG2 
```


#### test_intel_enc 例子测试结果
```
==========================================
Codec:          H.264
Source: 	1920 x 960
Pixel Format:   NV12
Frame Count:    144
Elapsed Time:   580 ms
Encode FPS:     248.275862 fps
==========================================
```

#### TODO list
- [ ] more codec encode test
- [ ] document



---




## 3. 基于 Nvidia 显卡硬件编解码加速
### nv_dec

#### - 导出 API
参见 jm_nv_dec.h 头文件。

#### - 使用

参见项目 test_nv_dec 。 工程里包含头文件 jm_nv_dec.h 和导入库 nv_dec.lib

```
#include "jm_nv_dec.h"

#pragma comment(lib,"nv_dec.lib")


```


#### - 调用流程


```

jm_nvdec_create_handle()
jm_nvdec_init()

loop jm_nvdec_is_exit()
	read_frame_nalu()
	jm_nvdec_decode_frame()
	jm_nvdec_output_frame()

end loop

jm_nvdec_show_dec_info()

jm_nvdec_deinit()

```

流程图

![nv_dec](/media/nv_dec_flow.png)
	 
---







#### 4. 测试程序
- 开发测试平台
	- OS  : 	Win10
	- CPU : 	i7-6700 
	- 集显：	Intel HD Graphics 530
	- 显卡：	Nvidia GeForce GTX 970
	- Memory: 16 GB







- test_intel_dec


![intel dec](/media/test_intel_dec.png)


- test_nv_dec


![nv dec](/media/test_nv_dec.png)




更多优化测试，目前没有实现更深层的优化。

---

### TODO List

- [x] Intel decode
- [x] Test intel decode
- [x] Nvidia decode
- [x] Test nvdia decode
- [x] intel encode
- [ ] intel transcode
- [ ] nvidia encode
- [ ] nvidia transcode
- [x] FFmpeg integration
- [x] test_player (SDL2)


#### more ...



----
### 如果你喜欢作者的项目或者文章， 请支持原创
微信


![微信](/media/weixin.png)



支付宝


![支付宝](/media/zhifubao.png)