# unimrcp-funasr
@[TOC](使用Unimrcp和Funasr构建呼叫中心语音识别服务端)
经过压力测试，服务很稳定，16c的机器能满足16个实时并发
# 1.编译及运行unimrcp


此次使用的是unimrcp1.6版本，先下载unimrcp-deps-1.6.0以及unimrcp-1.6.0进行构建，此处不过多赘述。

# 2.新增funasr-recog，支持funasr识别
模块新增可参考[智能客服搭建(2) - MRCP Server ASR插件开发
](https://blog.csdn.net/initiallht/article/details/119280960)
此步骤，主要是使用websocket去连接funasr服务端，（funasr的部署自行百度），在`funasr_recog_channel_recognize`创建funasr连接，在`funasr_recog_recognition_complete`处理funasr返回的数据，构建xml返回给freeswitch，此次vad使用的是webrtc vad。
```
void create_xml_output(const char *parsed_text, char *xml_output, size_t xml_output_size) {
    snprintf(xml_output, xml_output_size,
             "<?xml version=\"1.0\" encoding=\"utf-8\" ?>\n"
             "<result>\n"
             "    <interpretation grammar=\"http://127.0.0.1/welcom.grxml\" confidence=\"100\">\n"
             "        <instance>\n"
             "            <speech-to-text confidence=\"100\">%s</speech-to-text>\n"
             "        </instance>\n"
             "        <input mode=\"speech\">%s</input>\n"
             "    </interpretation>\n"
             "</result>",
             parsed_text, parsed_text);
}
```
# 3.启动unimrcp
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/19a8caee4c1e47628a5a38a973baaed6.png)
成功加载funasr识别模块

# 4.启动funasr
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ade7f06c8a9645e6a7c037c387c32e4c.png)
成功启动funasr

# 5. freeswitch呼叫测试
Freeswitch日志
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9873d135b8df4d969d8a5486bcc90ed7.png)
Funasr日志
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/07cb840d981b48dbb40672a61b556db9.png)
Unimrcp日志
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/66dad282e8e94d07893ae8f709dd25df.png)

vx: wxh_assistant
