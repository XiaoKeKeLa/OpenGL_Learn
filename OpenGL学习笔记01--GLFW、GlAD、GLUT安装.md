**OpenGL学习笔记01--GLFW、GLAD、GLUT安装**

**1.GLFW**

https://www.glfw.org/download.html

进入该网站直接下载[32-bit Windows binaries](https://github.com/glfw/glfw/releases/download/3.3.2/glfw-3.3.2.bin.WIN32.zip)

ps:注意旁边的提示，vs10及以上的下载64位，其他下载32位，下载好后，直接解压。

![1](D:\q\笔记汇总\图片汇总\1.png)



**2.GLAD**

https://glad.dav1d.de/

![2](D:\q\笔记汇总\图片汇总\2.png)

如图所示，其中gl选择最新版的即可。选择好后，Options中勾选 Generate a loader,再点击下方GENERATE。

![3](D:\q\笔记汇总\图片汇总\3.png)

下载后解压，此时你将得到这两个文件夹。

![image-20201011191208599](D:\q\笔记汇总\图片汇总\image-20201011191208599.png)



再新建一个文件夹（例如：OpenGL，不要是中文），新建三个子文件夹，分别为include,lib,src。

将glfw-3.3.2.bin.WIN64->include->GLFW复制到OpenGL->include中

![image-20201011191714672](D:\q\笔记汇总\图片汇总\image-20201011191714672.png)

将glfw-3.3.2.bin.WIN64->lib-vc2019(对应你所使用的版本)->glfw3.lib复制到OpenGL->lib中

![image-20201011191802906](D:\q\笔记汇总\图片汇总\image-20201011191802906.png)

将glad->include下面的两个文件夹全部复制到OpenGL->include中

![image-20201011191957265](D:\q\笔记汇总\图片汇总\image-20201011191957265.png)

将glad->src下的内容复制到OpenGL->src中

![image-20201011192202606](D:\q\笔记汇总\图片汇总\image-20201011192202606.png)

在vs中新建一个空项目

<img src="D:\q\笔记汇总\图片汇总\image-20201011192545752.png" alt="image-20201011192545752" style="zoom:50%;" />

将刚刚建好的OpenGL文件夹复制到和项目同级目录中

![image-20201011192736579](D:\q\笔记汇总\图片汇总\image-20201011192736579.png)

右键项目选择属性，VC++目录->包含目录，将OpenGL中的include文件夹添加进来，再选择VC++目录->库目录，将OpenGL中的lib文件夹添加进来。

ps：记得选择所有配置，所有平台

![image-20201011193306706](D:\q\笔记汇总\图片汇总\image-20201011193306706.png)

![image-20201011193226797](D:\q\笔记汇总\图片汇总\image-20201011193226797.png)

在链接器->输入->附加依赖项选项卡里添加
`opengl32.lib`
`glfw3.lib`
![image-20201011193522438](D:\q\笔记汇总\图片汇总\image-20201011193522438.png)

填写完成后，点击应用，确定后退出。

右键源文件，添加现有项，将OpenGl->src下的glad.c文件添加进来

![image-20201011193656048](D:\q\笔记汇总\图片汇总\image-20201011193656048.png)![image-20201011193712712](D:\q\笔记汇总\图片汇总\image-20201011193712712.png)

一切完成后，新建一个cpp文件，跑测试代码。

```c++
#include <glad/glad.h>
#include <GLFW/glfw3.h>

#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow* window);

// settings
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    // glfw: initialize and configure
    // ------------------------------
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef __APPLE__
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    // glfw window creation
    // --------------------
    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    // glad: load all OpenGL function pointers
    // ---------------------------------------
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }

    // render loop
    // -----------
    while (!glfwWindowShouldClose(window))
    {
        // input
        // -----
        processInput(window);

        // render
        // ------
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // glfw: swap buffers and poll IO events (keys pressed/released, mouse moved etc.)
        // -------------------------------------------------------------------------------
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw: terminate, clearing all previously allocated GLFW resources.
    // ------------------------------------------------------------------
    glfwTerminate();
    return 0;
}

// process all input: query GLFW whether relevant keys are pressed/released this frame and react accordingly
// ---------------------------------------------------------------------------------------------------------
void processInput(GLFWwindow* window)
{
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw: whenever the window size changed (by OS or user resize) this callback function executes
// ---------------------------------------------------------------------------------------------
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    // make sure the viewport matches the new window dimensions; note that width and 
    // height will be significantly larger than specified on retina displays.
    glViewport(0, 0, width, height);
}
```

正确运行结果如下：

![image-20201011194022926](D:\q\笔记汇总\图片汇总\image-20201011194022926.png)

如果运行报错，将这里改为x64后，再运行！！！

![image-20201011194103668](D:\q\笔记汇总\图片汇总\image-20201011194103668.png)

喜大普奔！！！GLFW和GLAD安装成功！！！



**3.GLUT**

![image-20201011195655178](D:\q\笔记汇总\图片汇总\image-20201011195655178.png)

https://www.opengl.org/resources/libraries/glut/

从官网得知目前推荐使用freeglut。这里提供一个OpenGL库，比较方便。https://pan.baidu.com/s/1iXQTo2dBgkD8zLE8AblgqA 提取码：yvtx 
下载后解压，配置OpenGL也就是需要把这include,dll,lib三个文件夹放到安装对应的文件夹中。

按照下图路径找到对应文件夹，下图是vs2019的路径，其他版本可以自行寻找。

![image-20201011200105772](D:\q\笔记汇总\图片汇总\image-20201011200105772.png)

将解压得到的lib文件夹和include文件夹复制到图中对应的文件夹中
再把dll文件夹中的5个dll文件粘贴到C:\Windows\System32和C:\Windows\SysWOW64

再运行测试代码，正确结果如下图所示：

```c++
#include <openGL/glut.h>
 
void init(void)
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
 
	glMatrixMode(GL_PROJECTION);
	gluOrtho2D(0.0, 200.0, 0.0, 150.0);
}
 
void lineSegment(void)
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(1.0, 0.0, 0.0);
	glBegin(GL_LINES);
	glVertex2i(180, 15);
	glVertex2i(10, 145);
	glEnd();
 
	glFlush();
}
 
void main(int argc, char** argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowPosition(50, 100);
	glutInitWindowSize(400, 300);
	glutCreateWindow("Demo OpenGL Program");
 
	init();
	glutDisplayFunc(lineSegment);
	glutMainLoop();
 
}
```

![image-20201011200310469](D:\q\笔记汇总\图片汇总\image-20201011200310469.png)

ps：这里是#include <openGL/glut.h>，因为glut.h头文件保存在openGL文件夹中。网上有的教程的测试代码或者教学代码是#include <GL/glut.h>，直接复制用来跑代码要先修改。



**按照上述操作，你已经可以开始OpenGL学习，无论你是使用蓝宝书学习还是在https://learnopengl-cn.github.io/中学习，都够用了。**