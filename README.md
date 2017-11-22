#python 图像处理
<div id="mainbody"><div id="postbody">搭建独立图像处理服务（Thumbor）
<div><div class="cbody"><!--
				<div style="float:left;padding-right:5px">

				</div>
				-->
          
          Thumbor是基于Python的开源的On-Demand图片处理服务，可以实现图片裁剪crop、缩放resize、翻转flip、滤镜filter，甚至是人脸识别。
 <br>
 <br>官网： <a href="https://github.com/thumbor/thumbor" target="_blank">https://github.com/thumbor/thumbor</a>
 <br>
 <br>目前版本： 6.3.2
 <br>
 <br>图像处理是系统开发中的必备组件，各种开发语言都内置图像处理API，也有大量的开源图像处理库做补充。
 <br>
 <br>[1] 图像变换引擎（ <a href="https://www.imagemagick.org/" target="_blank">ImageMagick</a>、 <a href="http://www.graphicsmagick.org/" target="_blank">Graphicsmagick</a>、 <a href="http://opencv.org/" target="_blank">OpenCV</a>）
 <br>[2] 图像优化工具（ <a href="http://www.jpegmini.com/" target="_blank">JPEGMini</a>、 <a href="https://tinypng.com/" target="_blank">TinyPNG</a>、 <a href="https://imageoptim.com/" target="_blank">ImageOptim</a>、 <a href="https://pngmini.com/" target="_blank">ImageAlpha</a>、 <a href="https://pngquant.org/" target="_blank">pngquant</a>、 <a href="https://github.com/mozilla/mozjpeg" target="_blank">MozJPEG</a>）
 <br>[3] 图像处理系统（ <a href="https://github.com/agschwender/pilbox" target="_blank">Pilbox</a>、 <a href="https://github.com/thumbor/thumbor" target="_blank">Thumbor</a>）
 <br>
 <br>Thumbor通过特殊构成的URL来创建缩略图，比如：
 <br> <div>引用</div> <div>http://thumbor.example.com/320x240/http://images.example.com/llamas.jpg</div>
 <br>含义是：Thumbor服务器thumbor.example.com通过HTTP从images.example.com获取图像llamas.jpg，将其缩放到320x240之后返回图像数据。
 <br>
 <br>Thumbor内部支持以下三种图像变换引擎：
 <br>PIL (Python Image Library)、GraphicsMagick (pgmagick)、OpenCV
 <br>
 <br>Thumbor存储图片支持的方式有File、Memcached、MongoDB、Redis。根据实际的使用情况选择存储方式。默认选用文件系统的存储方式。
 <br>
 <br>具体详细的使用方法可以参考官方文档：  <a href="http://thumbor.readthedocs.io/en/latest/index.html" target="_blank">http://thumbor.readthedocs.io/en/latest/index.html</a>
 <br>
 <br>需要注意的是：Thumbor目前还不支持Python3！
 <br> <div>引用</div> <div>&nbsp; File "/usr/local/tensorflow/lib/python3.5/site-packages/thumbor/context.py", line 293
  <br>&nbsp;&nbsp;&nbsp; print "Joining threads...."
  <br>SyntaxError: Missing parentheses in call to 'print'</div>
 <br> <pre>print "Hello world"    # python 2.x
print("Hello world")   # python 3.x</pre>
 <br>
 <br> <strong>（1）安装</strong>
 <br>
 <br> <pre># python -V
Python 2.7.5

# pip search thumbor
thumbor (6.3.2)                   - thumbor is an open-source photo thumbnail service by globo.com

# pip install thumbor
# pip install -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com thumbor</pre>
 <br>
 <br> <strong>（2）启动</strong>
 <br>
 <br> <pre># mkdir /usr/local/thumbor
# thumbor-config &gt; /usr/local/thumbor/thumbor.conf
# cp /usr/local/thumbor/thumbor.conf /usr/local/thumbor/thumbor.conf.default
# thumbor --port=8888 --conf=/usr/local/thumbor/thumbor.conf </pre>
 <br>
 <br>以下警告提示是没有安装opencv。
 <br> <div>引用</div> <div>2017-08-15 15:34:02 thumbor:WARNING Module thumbor.filters.distributed_collage could not be imported: No module named cv2</div>
 <br>
 <br> <strong>（3）测试</strong>
 <br>
 <br>- 原图
 <br>http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5503/ff4063e6-9541-3d93-9066-448e92e0d932.jpg">
 <br>
 <br>http://thumbor-server:8888/unsafe/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5505/893bae3e-3a18-3481-921f-1401d8c74c7f.jpg">
 <br>
 <br>- 保持横纵比，宽度固定300px
 <br>http://thumbor-server:8888/unsafe/300x0/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5507/19458cb3-c98e-3218-83cd-93bb97d92256.jpg">
 <br>
 <br>- 保持横纵比，高度固定300px
 <br>http://thumbor-server:8888/unsafe/0x300/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5509/02830e85-56bb-3f21-826d-30869ddab73b.jpg">
 <br>
 <br>- 指定大小300×300
 <br>http://thumbor-server:8888/unsafe/300x300/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5511/eb7b619d-0e3d-3eb4-9d01-943ecf4de05a.jpg">
 <br>
 <br>- 裁剪指定位置
 <br>http://thumbor-server:8888/unsafe/528x111:1101x657/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5513/4ef58962-95f5-3e7b-b7a8-f857b4f22821.jpg">
 <br>
 <br>- 图像滤镜
 <br>http://thumbor-server:8888/unsafe/400x200/filters:grayscale()/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5515/051b9571-9cfb-3cee-92f0-b58dc62a9454.jpg">
 <br>
 <br>http://thumbor-server:8888/unsafe/200x200/filters:brightness(-20)/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5517/f9a1b838-660e-3336-a159-12644015b25b.jpg">
 <br>
 <br>http://thumbor-server:8888/unsafe/200x200/filters:brightness(40)/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5519/045f0a03-c1ae-3cf8-b396-d5d5f1cee16c.jpg">
 <br>
 <br>http://thumbor-server:8888/unsafe/filters:round_corner(40,255,255,255):grayscale()/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5521/2e385fbe-fceb-3c20-982a-11c3c8cc4a1b.jpg">
 <br>
 <br> <strong>（4）安全模式</strong>
 <br>
 <br>以上都是测试用URL，可以看到URL里包含 <strong>/unsafe/</strong>。
 <br>
 <br>限定来源
 <br> <pre># vi /usr/local/thumbor/thumbor.conf
ALLOWED_SOURCES = [
  'desk.fd.zol-img.com.cn'
]</pre>
 <br>
 <br>访问以下URL会返回 200 OK
 <br>http://thumbor-server:8888/unsafe/300x100/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5523/ac2945de-8e86-3c68-bb19-b55113e44295.jpg">
 <br>
 <br>访问以下URL会返回 400 Bad Request
 <br>http://thumbor-server:8888/unsafe/300x100/https://http.cat/405.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5527/d5721da7-7942-3963-adad-4bff3100a1ef.jpg">
 <br>
 <br>关闭unsafe配置密钥
 <br> <pre># openssl rand -base64 48
JNj9+RxUKfsgrVN+8cH5DVzNdRURiFzBo6S2lL9Q0zcweHmdkU6q1Rf60XX7LnVc
# vi /usr/local/thumbor/thumbor.conf
ALLOW_UNSAFE_URL = False
SECURITY_KEY = 'JNj9+RxUKfsgrVN+8cH5DVzNdRURiFzBo6S2lL9Q0zcweHmdkU6q1Rf60XX7LnVc'</pre>
 <br>
 <br>再次访问以下URL会返回 400 Bad Request，设置的ALLOWED_SOURCES也无效了。
 <br>http://thumbor-server:8888/unsafe/300x100/http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5527/d5721da7-7942-3963-adad-4bff3100a1ef.jpg">
 <br>
 <br> <strong>（5）URL做成</strong>
 <br>
 <br>查看thumbor-url用法
 <br> <pre># thumbor-url -h</pre>
 <br>
 <br>通过KEY获取URL
 <br> <pre># thumbor-url -k JNj9+RxUKfsgrVN+8cH5DVzNdRURiFzBo6S2lL9Q0zcweHmdkU6q1Rf60XX7LnVc http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
URL:
/UJWGMA3XL6X29zF2qgQgBKqz_QM=/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
/UJWGMA3XL6X29zF2qgQgBKqz_QM=/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg</pre>
 <br>
 <br>通过KEY文件获取URL
 <br> <pre># vi /etc/thumbor.key
JNj9+RxUKfsgrVN+8cH5DVzNdRURiFzBo6S2lL9Q0zcweHmdkU6q1Rf60XX7LnVc
# thumbor-url -l /etc/thumbor.key http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
URL:
/UJWGMA3XL6X29zF2qgQgBKqz_QM=/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
/UJWGMA3XL6X29zF2qgQgBKqz_QM=/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg</pre>
 <br>
 <br>访问以下URL会返回 200 OK
 <br>http://thumbor-server:8888/UJWGMA3XL6X29zF2qgQgBKqz_QM=/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5529/a2a95913-7fed-325d-88a9-6e264f0ec98d.jpg">
 <br>
 <br>缩放
 <br> <pre># thumbor-url -l /etc/thumbor.key -w 300 -e 200 http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
URL:
/byeRaXKHyMqfrsWmRZb558J_8iI=/300x200/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
/byeRaXKHyMqfrsWmRZb558J_8iI=/300x200/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg</pre>
 <br>
 <br>访问以下URL会返回 200 OK
 <br>http://thumbor-server:8888/byeRaXKHyMqfrsWmRZb558J_8iI=/300x200/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5531/47bb986d-16d6-315f-bae3-490822e41c97.jpg">
 <br>
 <br> <strong>（6）智能裁剪</strong>
 <br>
 <br>安装
 <br> <pre># yum install -y opencv GraphicsMagick openssl-devel curl-devel GraphicsMagick-c++-devel boost boost-devel boost-python

# pip install pgmagick opencv-python graphicsmagick-engine
# pip install -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com pgmagick opencv-python graphicsmagick-engine</pre>
 <br>
 <br>设置
 <br> <pre># vi /usr/local/thumbor/thumbor.conf
DETECTORS = [
      'thumbor.detectors.face_detector',
      'thumbor.detectors.feature_detector'
    ]</pre>
 <br>
 <br>有无/smart/前后效果对比：
 <br> <pre># thumbor-url -l /etc/thumbor.key -w 300 -e 80 http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
URL:
/twOvaCpM5nAUv3JCdDnNP76Iqz0=/300x80/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
/twOvaCpM5nAUv3JCdDnNP76Iqz0=/300x80/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg</pre>
 <br>
 <br> <pre># thumbor-url -l /etc/thumbor.key -w 300 -e 80 -s http://desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
URL:
/swDVsG0g_nYBE8zHDcjzfyH561w=/300x80/smart/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
/swDVsG0g_nYBE8zHDcjzfyH561w=/300x80/smart/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg</pre>
 <br>
 <br>http://thumbor-server:8888/twOvaCpM5nAUv3JCdDnNP76Iqz0=/300x80/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5533/04e38a85-2ebb-35f6-b8e7-36af5eaa80a7.jpg">
 <br>
 <br>http://thumbor-server:8888/swDVsG0g_nYBE8zHDcjzfyH561w=/300x80/smart/http%3A//desk.fd.zol-img.com.cn/t_s1920x1200c5/g5/M00/01/0E/ChMkJlbKwbyIFnBKAAhTmqigyPUAALGdAHEN-kACFOy827.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5535/35ca7337-6958-3827-8e05-fc207e480217.jpg">
 <br>
 <br> <strong>（7）图像上传</strong>
 <br>
 <br>可以通过REST的方式上传图像：POST http://thumbor-server:8888/image
 <br>
 <br>默认未开启上传功能，会返回‘405: Method Not Allowed’。
 <br>
 <br> <pre># vi /usr/local/thumbor/thumbor.conf
UPLOAD_ENABLED = True

# curl -i -H "Content-Type: image/jpeg" -H "Slug: rensanning.jpg" \
          -XPOST http://thumbor-server:8888/image --data-binary "@/tmp/1348814002177.jpg"
HTTP/1.1 201 Created
Date: Wed, 16 Aug 2017 02:19:59 GMT
Content-Length: 0
Content-Type: text/html; charset=UTF-8
Location: /image/14ffd48c8baa4301b476c4b837568af8/rensanning.jpg
Server: TornadoServer/4.5.1</pre>
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5537/370946a4-3faf-3ce1-8ff2-187fafa7bba8.jpg">
 <br>
 <br>访问以下URL会返回 200 OK。
 <br>http://thumbor-server:8888/image/14ffd48c8baa4301b476c4b837568af8/rensanning.jpg
 <br> <img src="http://dl2.iteye.com/upload/attachment/0126/5539/e234d821-81a5-375e-9240-9f9815bfe5a5.jpg">
 <br>
 <br>同理可以通过PUT、DELETE、GET来操作已有图像。当然默认是不允许的需要修改配置。
 <br> <div>引用</div> <div>#UPLOAD_DELETE_ALLOWED = False
  <br>#UPLOAD_PUT_ALLOWED = False</div>
 <br>
 <br> <strong>（8）其他设置</strong>
 <br>
 <br>默认采用的HTTP加载图像，也就是需要提供一个图像的绝对URL，也可以采用加载本地图像，把图像放入root文件夹即可访问。
 <br>LOADER = 'thumbor.loaders.file_loader' 
 <br>
 <br> <div>引用</div> <div>## The loader thumbor should use to load the original image. This must be the
  <br>## full name of a python module (python must be able to import it)
  <br>## Defaults to: 'thumbor.loaders.http_loader'
  <br>#LOADER = 'thumbor.loaders.http_loader'
  <br>
  <br>## The root path where the File Loader will try to find images
  <br>## Defaults to: '/root'
  <br>#FILE_LOADER_ROOT_PATH = '/root'</div>
 <br>
 <br>默认是不存储转换后的图像的，需要的话需要设置。
 <br>RESULT_STORAGE = 'thumbor.result_storages.file_storage'
 <br>
 <br> <div>引用</div> <div>## The result storage thumbor should use to store generated images. This must be
  <br>## the full name of a python module (python must be able to import it)
  <br>## Defaults to: None
  <br>#RESULT_STORAGE = None
  <br>
  <br>## Path where the Result storage will store generated images
  <br>## Defaults to: '/tmp/thumbor/result_storage'
  <br>#RESULT_STORAGE_FILE_STORAGE_ROOT_PATH = '/tmp/thumbor/result_storage'</div>
 <br>
 <br>参考：
 <br>https://segmentfault.com/a/1190000008656825
 <br>https://ingramchen.io/blog/2015/12/thumbor-tutorial.html
 <br>https://blog.1q77.com/2015/05/using-thumbor-part1
          
           <br> <br>
          
             <a href="http://rensanning.iteye.com/blog/2389847#comments">已有   <strong>0</strong> 人发表留言，猛击-&gt;&gt;  <strong>这里</strong>&lt;&lt;-参与讨论</a>
          
           <br> <br> <br>
ITeye推荐
 <br>
 <ul>  <li>   <a href="http://www.iteye.com/clicks/433" target="_blank">—软件人才免语言低担保 赴美带薪读研！— </a></li></ul>
 <br> <br> <br>
          
        </div></div></div><div class="relate"><h3>相关 [独立 图像处理 服务] 推荐：</h3><div class="post"><h2><a title="搭建独立图像处理服务（Thumbor）" href="/detail/57354-%E7%8B%AC%E7%AB%8B-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86-%E6%9C%8D%E5%8A%A1">搭建独立图像处理服务（Thumbor）</a></h2>
-  - ITeye博客
<div>Thumbor是基于Python的开源的On-Demand图片处理服务，可以实现图片裁剪crop、缩放resize、翻转flip、滤镜filter，甚至是人脸识别. 官网： https://github.com/thumbor/thumbor. 图像处理是系统开发中的必备组件，各种开发语言都内置图像处理API，也有大量的开源图像处理库做补充. </div></div><div class="post"><h2><a title="html5 canvas 图像处理" href="/detail/37964-html5-canvas-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86">html5 canvas 图像处理</a></h2>
-  - HTML5研究小组
<div>前两天无意中看了下《pro html5 programming》,发现html5竟然也能很好的支持图像处理，在此稍稍交代一下. 与matlab处理图像类似的是，这里也是采用图像矩阵的形式. 下面就介绍一个简单的例子:. context1.drawImage(image,0,0);//绘制原始图像，（0,0）表示图像的左上角位与canvas画布的位置. </div></div><div class="post"><h2><a title=" Java图像处理库 Sanselan" href="/detail/54985-java-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86-sanselan"> Java图像处理库 Sanselan</a></h2>
-  - 编程语言 - ITeye博客
<div>Sanselan 是一个纯 Java 的图形库，可以读写各种格式的图像文件，包括快速解析图片信息例如大小/颜色/icc以及元数据等. 尽管因为是Java开发的，在处理速度上会稍微慢一 些，但具备良好的可移植性. 虽然尚未发布1.0 版本，但是已经有多个项目在使用 Sanselan 来处理图像文件. 该项目目前还是 Apache 组织的一个孵化项目. </div></div><div class="post"><h2><a title="神奇的图像处理算法" href="/detail/735-%E7%A5%9E%E5%A5%87-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86-%E7%AE%97%E6%B3%95">神奇的图像处理算法</a></h2>
- etalkr - 博客园新闻频道
<div>这是利用数学算法，进行高难度图像处理的一个例子. 事实上，图像处理的数学算法，已经发展到令人叹为观止的地步. Scriptol列出了几种神奇的图像处理算法，让我们一起来看一下. 数字时代早期的图片，分辨率很低. 尤其是一些电子游戏的图片，放大后就是一个个像素方块. Depixelizing算法可以让低分辨率的像素图转化为高质量的向量图. </div></div><div class="post"><h2><a title="HTML5 Canvas(画布)教程 – 图像处理" href="/detail/50301-html5-canvas-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86">HTML5 Canvas(画布)教程 – 图像处理</a></h2>
-  - Web前端 - ITeye博客
<div>Canvas标记很多年前就被当作一个新的HTML标记成员加入到了HTML5标准中. 在此之前，人们要想实现动态的网页应用，只能借助于第三方的 插件，比如Flash或Java，而引入了Canvas标记后，人们直接打通了通往神奇的动态应用网页的大门. 本教程内容只覆盖了一小部分、但却是非常重 要的canvas标记的应用功能——图像显示和处理. </div></div><div class="post"><h2><a title="一点老东西：神奇的图像处理算法" href="/detail/10176-%E4%B8%9C%E8%A5%BF-%E7%A5%9E%E5%A5%87-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86">一点老东西：神奇的图像处理算法</a></h2>
- $n0wd0wn - 丕子
<div>日期： 2011年8月13日. 几周前，我介绍了相似图片搜索. 这是利用数学算法，进行高难度图像处理的一个例子. 事实上，图像处理的数学算法，已经发展到令人叹为观止的地步. Scriptol列出了几种神奇的图像处理算法，让我们一起来看一下. 数字时代早期的图片，分辨率很低. 尤其是一些电子游戏的图片，放大后就是一个个像素方块. </div></div><div class="post"><h2><a title="PHP图像处理(一) GraphicsMagick介绍与安装" href="/detail/24865-php-%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86-graphicsmagick">PHP图像处理(一) GraphicsMagick介绍与安装</a></h2>
- We_Get - 博客园-首页原创精华区
<div>GraphicsMagick概述. GraphicsMagick号称图像处理领域的瑞士军刀. 短小精悍的代码却提供了一个鲁棒、高效的工具和库集合，来处理图像的读取、写入和操作，支持超过88中图像格式，包括重要的DPX、GIF、JPEG、JPEG-2000、PNG、PDF、PNM和TIFF. 通过使用OpenMP可是利用多线程进行图片处理，增强了通过扩展CPU提高处理能力. </div></div></div></div>



