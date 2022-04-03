基于github项目进行修改：
源项目地址：github.com/pixelbender/go-sdp

修改目的：
我们拉取rtsp流时，会遇到不规范的rtsp流，所谓不规范，这里遇到的情况就是sdp信息不规范，sdp信息中带有不必要的空行，
导致解析sdp信息失败。
对此对源go-sdp解析sdp的开源项目进行一个修改来适应特殊情况。

修改后的sdp解析包地址为：github.com/dengsir173/go-sdp


修改过的方法为：

func (d *Decoder) Decode() (*Session, error) {
line := 0
sess := new(Session)
var media *Media

	for {
		line++
		s, err := d.r.ReadLine()
		if err != nil {
			if err == io.EOF && sess.Origin != nil {
				break
			}
			return nil, err
		}
		if len(s) == 0 && sess.Origin == nil {
			break
		}
		/*if len(s) < 2 || s[1] != '=' {
			return nil, &errDecode{errFormat, line, s}
		}*/
		var f uint8
		var v string

		if s != "" {
			f, v = s[0], s[2:]
		} else {
			f, v = 0, ""
		}

		if f == 'm' {
			media = new(Media)
			err = d.media(media, f, v)
			if err == nil {
				sess.Media = append(sess.Media, media)
			}
		} else if media == nil {
			err = d.session(sess, f, v)
		} else {
			err = d.media(media, f, v)
		}
		if err != nil {
			return nil, &errDecode{err, line, s}
		}
	}
	return sess, nil
}
