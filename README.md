​
基于github项目进行修改：
源项目地址：github.com/pixelbender/go-sdp

修改目的：
我们拉取rtsp流时，会遇到不规范的rtsp流，所谓不规范，这里遇到的情况就是sdp信息不规范，sdp信息中带有不必要的空行，
导致解析sdp信息失败。
对此对源go-sdp解析sdp的开源项目进行一个修改来适应特殊情况。

修改后的sdp解析包地址为：github.com/dengsir173/go-sdp


修改过的方法为：

func (d *Decoder) Decode() (*Session, error) { .....}

下图为不规范的sdp信息：（带有空行导致会解析失败）规范的sdp信息没有空行

![Snipaste_2022-04-04_22-43-10](https://user-images.githubusercontent.com/64079142/161570643-407b181f-e607-407d-be75-175631575bb3.png)
