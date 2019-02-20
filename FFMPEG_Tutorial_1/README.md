README.md
===

## ffmpeg 디코딩

---

### 개발환경

* Windows 10 64bits
* Visual Studio 2017 64bits

---

### 구현

* 추가 포함 디렉터리 : $(ProjectDir)ffmpeg-4.1-win64-dev\ffmpeg-4.1-win64-dev\include
* 추가 라이브러리 디렉터리 : $(ProjectDir)\ffmpeg-4.1-win64-dev\ffmpeg-4.1-win64-dev\lib

```c
// stdafx.h
extern "C"
{
#include "libswscale/swscale.h"
#include "libavformat/avformat.h"
#include "libavformat/avio.h"
#include "libavcodec/avcodec.h"
};

#pragma comment(lib, "avcodec.lib")
#pragma comment(lib, "avformat.lib")
#pragma comment(lib, "avutil.lib")
#pragma comment(lib, "swscale.lib")
```

```c
// CFFMPEGTutorial1Dlg.h

AVCodec*			m_codec;
// 어떤 코덱(MPEG, MJPEG, H264)인지를 나타낸다.
AVCodecContext*		m_context;
// 스트림을 어떻게 디코딩 할 것인지 정보를 넣어 둔다.
```

```c
// CFFMPEGTutorial1Dlg.cpp

m_codec = avcodec_find_decoder(AV_CODEC_ID_H264);
// 디코딩 코덱을 선택
m_context = avcodec_alloc_context3(m_codec);
// m_codec의 정보를 m_context로 넘긴다.
avcodec_get_context_defaults3(m_context, m_codec);
// m_context를 초기화(위와 같은 기능)

m_context->width = 320;
m_context->height = 240;
m_context->codec_id = AV_CODEC_ID_H264;
m_context->codec_type = AVMEDIA_TYPE_VIDEO;
m_context->thread_count = 0;
// 디코딩 정보 입력

avcodec_open2(m_context, m_codec, NULL);
// ffmpeg 라이브러리 설정

AVPacket	avpkt;
av_init_packet(&avpkt);

// avpkt.data = pEncodedeData;
// avpkt.size = EncodedSize;

int ret;
AVFrame* m_picture;

m_picture = avcodec_alloc_frame();

ret = avcodec_decode_video2(m_context, m_picture, &got, &avpkt);

if(got>0)
{
    // ....
}
```

---

### 참고사항

* *.dll파일을 모두 실행파일과 함께 넣어야 함.

---

### 참고사이트

* [사이트](https://killsia.tistory.com/entry/FFMPEG-%EB%94%94%EC%BD%94%EB%94%A9-API-%EC%82%AC%EC%9A%A9%EB%B2%95)