# typora，php图片上传

/Library/Frameworks/Python.framework/Versions/3.9/bin/python3 /Users/anson/Documents/notebook/up.py

```
/Library/Frameworks/Python.framework/Versions/3.9/bin/python3 /Users/anson/Documents/notebook/up.py



```
#### 复制路径

```
../pic/${filename}
```



Upload.php 

```
<?php
$a=$_POST['message'];
$cont=$_POST['content'];
function base64(){
        //接收base64数据
        $image= $_POST['content'];
        //设置图片名称
        $imageName = "pic_".date("His",time())."_".rand(1111,9999).'.png';
        //判断是否有逗号 如果有就截取后半部分
        if (strstr($image,",")){
            $image = explode(',',$image);
            $image = $image[1];
        }
        //设置图片保存路径
        $path = "mdpic/".date("Ymd",time());
 
        //判断目录是否存在 不存在就创建
        if (!is_dir($path)){
            mkdir($path,0777,true);
        }
 
        //图片路径
        $imageSrc= $path."/". $imageName;
 
        //生成文件夹和图片
        $r = file_put_contents($imageSrc, base64_decode($image));
        if (!$r) {
            return json_encode(['code'=>0,'message'=>'图片生成失败']);
        }else {
            return $imageSrc;
        }
};
echo(base64());
?>

```



Up.py

```
import sys
import base64
import hashlib
import time
import requests
import urllib.parse
import os
from PIL import Image
from PIL import ImageFile

def compress_image(in_file, mb=1000000, quality=85, k=0.9):
    """不改变图片尺寸压缩到指定大小
    :param in_file: 压缩文件保存地址
    :param mb: 压缩目标,默认1000000byte
    :param k: 每次调整的压缩比率
    :param quality: 初始压缩比率
    :return: 压缩文件地址，压缩文件大小
    """
    o_size = os.path.getsize(in_file)  # 函数返回为字节数目
    # print('before_size:{} after_size:{}'.format(o_size, mb))
    if o_size <= mb:
        return in_file

    ImageFile.LOAD_TRUNCATED_IMAGES = True  # 防止图像被截断而报错
    temp_file="/Users/anson/Desktop/ty/temp."+in_file.split(".")[-1] #中间文件路径
    im = Image.open(in_file)
    try:
        im.save(temp_file)
    except Exception as e:
        print(e)

    while o_size > mb:
        im = Image.open(temp_file)
        x, y = im.size
        out = im.resize((int(x*k), int(y*k)), Image.Resampling.LANCZOS)  # 最后一个参数设置可以提高图片转换后的质量
        try:
            out.save(temp_file, quality=quality)  # quality为保存的质量，从1（最差）到95（最好），此时为85
        except Exception as e:
            print(e)
            break
        o_size = os.path.getsize(temp_file) ##1024
    return temp_file


def main():
    target = " http://192.168.0.104/mdpic/uploads.php"
    message = 'typora上传图片  ' #自定义上传的信息,使服务端日志更清晰 
    param = [urllib.parse.unquote(par, 'utf8') for par in sys.argv]  # 把url编码转换成中文
    param.__delitem__(0)  # 第一个参数是脚本文件本身
    if len(param) > 0:
        if not os.path.exists(param[0]):  # 通过判断第一个参数是不是文件来判断是否加了参数 ${filename}
            param.__delitem__(0)
        for i in range(0, len(param)):
            file=compress_image(param[i])#压缩图片，使其保持在1mb一下，防止访问过慢
            # file=param[i] #不压缩图片
            with open(file, "rb") as f:
                content = base64.b64encode(f.read())
                # filename=str(int(time.time()))+"_"+hashlib.md5(content).hexdigest() + param[i][param[i].rfind('.'):]
                data = { 'message': message, 'content': content}
                # print('文件名',filename)
                res = requests.post(target, data)
                # print(res.text)
                print(res.status_code)
                if res.status_code == 200:
                    print('http://192.168.0.104/mdpic/'+res.text)
                else:
                    print('Error uploading Gitee, please check')

if __name__ == '__main__':
    main()

```



![屏幕快照 2023-07-20 下午2.19.32](pic/typora%E7%9A%84%E5%89%AF%E6%9C%AC/pic_063600_2898.png)



![屏幕快照 2023-07-20 下午2.19.27](pic/typora%E7%9A%84%E5%89%AF%E6%9C%AC/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202023-07-20%20%E4%B8%8B%E5%8D%882.19.27.png)

![屏幕快照 2023-07-20 下午2.19.27](pic/typora%E7%9A%84%E5%89%AF%E6%9C%AC/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202023-07-20%20%E4%B8%8B%E5%8D%882.19.27-9836455.png)

