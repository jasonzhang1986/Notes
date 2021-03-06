>问题描述：如何从大量的图片中过滤掉亮度低的图片？

这个是今天在工作中遇到一个小问题，虽然很快找到了解决方案，但其中有一些心得在这里分享一下。
首先想到的是，是否有方法直接获得图片的亮度值？这样就可以根据获取的亮度值来定一个阈值，来过滤掉亮度低于阈值的数据。
最近在学 Python，就想到直接用 py 啊，这还不简单？然后打开 IDE， new python file，啪啪啪几行代码就搞定
```python
from PIL import Image, ImageStat
import os
def brightness(im_file):
   im = Image.open(im_file).convert('L')
   stat = ImageStat.Stat(im)
   return stat.mean[0]

#获取亮度
bright = brightness('abc.jpg')
print(bright)
#打印结果
if bright > 100:
    print('bright mode')
else:
    print('dark mode')
```
是不是很 Easy， 测试了下运行正常，分分钟搞定。

结果跑去告诉领导，领导说服务端同学使用的是 Java 语言，有没有 Java 版本的实现？好吧，我来撸。Python 有很多优秀的库，可以很方便的实现，Java 是否也有呢？俗话说外事问 Google 房事问 Baidu，打开 Google 搜索下，发现了 ImageIO 的类。

通过 ImageIO 读取 Image，然后读取每个像素的 RGB， 然后算出每个像素的亮度，最后求取平均值。
```java
public void brightness(String path, String imageName, float threshold) {
        File file = new File(path);
        try {
            BufferedImage image = ImageIO.read(file);
            int width = image.getWidth();
            int height = image.getHeight();
            float luminance_count = 0;
            for(int x=0; x<width; x++) {
                for (int y=0; y<height; y++) {
                    int color = image.getRGB(x, y);
                    // extract each color component
                    int red   = (color >>> 16) & 0xFF;
                    int green = (color >>>  8) & 0xFF;
                    int blue  = (color >>>  0) & 0xFF;

                    // calc luminance in range 0.0 to 1.0; using SRGB luminance constants
                    float luminance = (red * 0.2126f + green * 0.7152f + blue * 0.0722f) / 255;
                    luminance_count += luminance;
                }
            }

            float avg = luminance_count / (width*height);
               // choose brightness threshold as appropriate:
            if (avg >= threshold) {
                // bright color
                System.out.println("bright color" );
            } else {
                // dark color
               System.out.println("dark color" );
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
最后也实现了同样的效果，但无论从代码量还是实现功能花费的时间都比 python 要多一些。

然后是并发执行，因为图片比较多，所以采用多线程来处理，这里发现 python 的多线程不熟悉，摸索着搞了好久最后才搞定，而 Java 的线程池知识早已熟悉，啪啪啪几句话搞定。

看来没有绝对的好与坏，只有适合的、满足需求的才是最好的。不说了，我去学 python 啦。