# 基于 Qt 的 NX二次开发环境配置

collected by wxb

> 由于网络上几乎没有基于Qt的NX二次开发环境配置教程，基本都是基于VS的。在配环境的过程中，我也踩了一些坑，特此记录，希望能帮到后来者。



## NX DESIGN 下载并安装

1. 首先，要下载并安装NX DESIGN 软件（B站上有完整的教程和网盘链接）。

2. 下载完成后，进入对应的下载版本，找到 `UGOPEN`文件夹，里面包含了所有 `.lib`, `.h`, `.c` 文件，也就是我们需要调用第三方库所在的位置。

    除此以外，还可以在这个文件夹内找到一些 sample ，帮助学习。

## Qt 项目配置

1. 下载并安装 Qt（网络上教程很多，此处略过）

2. 创建一个项目，console / wizard 都可以，根据自己的需要即可。

3. 在 `.pro` 文件中配置 `INCLUDEPATH` 和 `LIBS` 文件，其实就是写qmake。

    ```makefile
    INCLUDEPATH += $$quote('</UGOPEN path>'); 
    # 使用 '' 可以防止文件路径中有空格导致识别失败。
    # '' 内填入至 UGOPEN 的绝对路径，例 'D:\Program Files\Siemens\NX1980\UGOPEN'
    LIBS += '<..\UGOPEN\*.lib>'
    # * 为通配符，表示将该目录下的所有 .lib 文件都加入
    # '' 内填入至 UGOPEN 的绝对路径，例 'D:\Program Files\Siemens\NX1980\UGOPEN\*.lib'
    ```

4. 完成以后，可以进入 `main.c` 文件，使用 `#include <UF.h>`(或者其他的头文件)将需要的头文件 include 进项目中，如果是 NXOpen 的 API，则不要忘记引用命名空间 `using namespace NXOPEN;`

5. 完成以上步骤后即可构建项目并运行。

6. 此处提供一个示例

    ```c
    #include "mainwindow.h"
    
    #include <QApplication>
    
    #include <uf.h>
    #include <uf_object_types.h>
    #include <uf_part.h>
    #include <uf_modl.h>
    #include <uf_obj.h>
    #include <uf_std.h>
    
    // Internal+External Includes
    #include <NXOpen/Annotations.hxx>
    #include <NXOpen/Assemblies_Component.hxx>
    #include <NXOpen/Assemblies_ComponentAssembly.hxx>
    #include <NXOpen/Body.hxx>
    #include <NXOpen/BodyCollection.hxx>
    #include <NXOpen/Face.hxx>
    #include <NXOpen/Line.hxx>
    #include <NXOpen/NXException.hxx>
    #include <NXOpen/NXObject.hxx>
    #include <NXOpen/Part.hxx>
    #include <NXOpen/PartCollection.hxx>
    #include <NXOpen/Session.hxx>
    #include <NXOpen/Features_BlockFeatureBuilder.hxx>
    #include <NXOpen/Features_FeatureCollection.hxx>
    
    // Std C++ Includes
    #include <iostream>
    #include <sstream>
    
    using namespace NXOpen;
    using namespace NXOpen::Features;
    using std::string;
    using std::exception;
    using std::stringstream;
    using std::endl;
    using std::cout;
    using std::cerr;
    
    int main(int argc, char *argv[])
    {
        // TODO: add your code here
        UF_initialize();
        tag_t tagPrt = NULL_TAG;
        UF_PART_new("D://test.prt", 1, &tagPrt);
    
        UF_FEATURE_SIGN sign = UF_NULLSIGN;
        // UF_NULLSIGN  NULL
        // UF_POSITIVE  SUM
        // UF_NEGATIVE DIFF
        // UF_UNSIGNED  INTER
        double dPoint[3] = { 0, 0, 0 };
        char charH[256] = "20";
        char charD[256] = "10";
        double dVector[3] = { 0, 0, 1 };
        tag_t tagCylinderTemp = NULL_TAG;
    
        UF_MODL_create_cylinder(sign, NULL_TAG, dPoint, charH, charD, dVector, &tagCylinderTemp);
    
        std::vector<tag_t> SolidVector;
        tag_t ObjectTag = NULL_TAG;
    
        UF_OBJ_cycle_objs_in_part(UF_PART_ask_display_part(), UF_solid_type, &ObjectTag);
    
        //打开用于二进制输出的STL文件
        void* file_handle = NULL;
        char* file_name = (char *)"D:\\test.stl";
        UF_STD_open_binary_stl_file(file_name, true, (char *)"", &file_handle);
    
        //将实体放入STL文件中
        int num_errors;
        UF_STD_stl_error_p_t  error_info;
        UF_STD_put_solid_in_stl_file(file_handle, NULL_TAG, ObjectTag, 0, 10000, 0.0001, &num_errors, &error_info);
    
        //关闭STL文件
        UF_STD_close_stl_file(file_handle);
        UF_OBJ_set_color(ObjectTag, 186);
        SolidVector.push_back(ObjectTag);
    
        UF_PART_save();
    
        UF_terminate();
    }
    ```

    运行以后可以在D盘得到 test.prt, test.stl，绘制了一个圆柱体。



### 坑

即使完成以上4布，也有可能出现 `undefined reference`的情况。可以尝试将编译器换成 MSCV，使用cdb debug。

具体操作：tools -> 选项 -> manage kit，修改 compiler 为 MSCV。