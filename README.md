# Optical-Mark-Recognition
**`vector<vector<Point>> getContours(Mat imgage)`**

找到使用函数`findContours`用于查找图像中的闭合轮廓，找到每一个轮廓计算其面积（`contourArea`），我们需要的标出轮廓（使用approxPolyDP进行曲线拟合），我们找到四个角点，找出涂卡区添加到返回的变量中。

**`void drawPoints(Mat image, vector<Point> cPoint, Scalar color)`**

遍历每个轮廓的4个角点，用圆画出并标出序号， circle  的FILLED参数意义为实心圆

**`vector<Point> reorder(vector<Point> points)`**

因为标号的序号是不同的，opencv的矩形标序方式是左上左下右上右下。使用`sumPoints`和`subPoints`，用于存储每个顶点的横纵坐标之和与之差。返回左上右上左下右下顺序的数组

1. - 找到`sumPoints`中最小元素对应的顶点，这是预期左上角的顶点，因为它在坐标系中通常具有最小的横纵坐标之和。
   - 找到`subPoints`中最大元素对应的顶点，这应该是右下角的顶点，因为这个点在坐标轴方向上的差异（x-y）最大。
   - 找到`subPoints`中最小元素对应的顶点，作为预期左下角的顶点。
   - 再次找到`sumPoints`中最大元素对应的顶点，这将是右上角的顶点。

   

**`Mat getWarp(Mat image, vector<Point> points, float w, float h)`**

**`Mat getreWarp(Mat image, vector<Point> points, float w, float h, float sizew, float sizeh)`**

这两个函数是将涂卡区仿射，将涂卡区仿射成俯视图以及将俯视图仿射回原来的图，sizew，sizeh是原图大小

**`vector<Mat> splitBox(Mat img)`**

图像分割，将每一个选项都分别深拷贝一份

**`Mat showAnswer(Mat img, vector<int> index, vector<int> grading, vector<int> ans, int question, int choice)`**

答案展示，根据grading中的数据，对的为1显示绿色，错的显示红色，画圆返回

**Main**

首先我们可以使用摄像头也可以直接读取图片

1. 生成灰度图imggray,生成高斯模糊图imgBlur,使用canny实现边缘检测
2. 找到涂卡区的矩形轮廓返回四个角点到cPoint,画出全部轮廓
3. 重新排序四个点，用红色画出涂卡区和成绩去的矩形轮廓
4. 对涂卡区和成绩区进行仿射变换为俯视图
5. 对两个区域进行二值化，然后分割每一个选项
6. 将每个选项的值（因为二值化，所以途过的选项是白色的，myPixelVal输出的值为选项的白色像素点）放入myPixelVal中，然后输出每一个myPixelVal，myPixelVal越大说明我们选择了这个选项
7. 我们将每一行的选项装进brr这个二维数组，用index来存下我们每行填涂的答案，ans存下正确的答案
8. 我们用grading数组来存下答案是否正确，正确为1，错误为0，并用rightnum来进行评分
9. 展示答案
10. 使用反向仿射变换，然后将图片融合成最终结果
