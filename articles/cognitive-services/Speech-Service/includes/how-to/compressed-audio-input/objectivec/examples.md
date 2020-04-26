---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
origin.date: 03/09/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: ae7f90a71c8a2ecf098ef4162bab185d014766ed
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127013"
---
若要以压缩音频格式流式传输到语音服务，请创建 `SPXPullAudioInputStream` 或 `SPXPushAudioInputStream`。

以下代码片段演示如何从 `SPXPushAudioInputStream` 的实例创建 `SPXAudioConfiguration`，并指定 MP3 作为流的压缩格式。

```objectivec
SPXAudioStreamContainerFormat compressedStreamFormat = SPXAudioStreamContainerFormat_MP3;
SPXAudioStreamFormat *audioFormat = [[SPXAudioStreamFormat alloc] initUsingCompressedFormat:compressedStreamFormat];
SPXPushAudioInputStream* stream = [[SPXPushAudioInputStream alloc] initWithAudioFormat:audioFormat];

SPXAudioConfiguration* audioConfig = [[SPXAudioConfiguration alloc] initWithStreamInput:stream];
if (!audioConfig) {
    NSLog(@"Error creating stream!");
    [self updateRecognitionErrorText:(@"Error creating stream!")];
    return;
}
```

下一代码片段演示如何从文件读取压缩的音频数据并将其泵入 `SPXPushAudioInputStream` 中。

```objectivec
NSInputStream *compressedStream = [[NSInputStream alloc] initWithFileAtPath:weatherFile];
[compressedStream open];
NSLog(@"result of opening stream: %@, %lu", compressedStream.streamError, (unsigned long)compressedStream.streamStatus);
if (nil == compressedStream)
{
    NSLog(@"Error while opening file");
    audioConfig = nil;
    return;
}

// start recognizing
[self updateRecognitionStatusText:(@"Recognizing from push stream...")];
[speechRecognizer startContinuousRecognition];

const NSInteger nBytesToRead = 1000;
// push data to stream;
uint8_t *buffer = malloc(nBytesToRead);
NSInteger nBytesRead = 0;
while (1)
{
    // read data
    nBytesRead = [compressedStream read:buffer maxLength:nBytesToRead];
    if (0 == nBytesRead) {
        NSLog(@"end of stream reached");
        [stream close];
        break;
    }
    else if (0 > nBytesRead) {
        NSLog(@"error reading stream (%ld): %@ (%lu)", nBytesRead, compressedStream.streamError, compressedStream.streamStatus);
        [stream close];
        break;
    }
    else
    {
        NSLog(@"Read %lu bytes from file", nBytesRead);
        NSData *data = [NSData dataWithBytesNoCopy:buffer length:nBytesRead freeWhenDone:NO];

        [stream write:data];
        NSLog(@"Wrote %lu bytes to stream", [data length]);
    }

    [NSThread sleepForTimeInterval:0.1f];
}

[speechRecognizer stopContinuousRecognition];
```
