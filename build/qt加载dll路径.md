---
layout: post
title: qt dll��̬���ӿ�����·��
category: Qt
group: IT
tags: [Qt]
keywords:[Qt]
description: qt dll��̬���ӿ�����·��

author: lileilei
revision:
    editor: lileilei
    date: 2018-11-10
---

����õ�qtӦ�ó��򣬻���pyqt���룬���Լ����������кܺã���̨���Ծ;���������Ҳ���dll�����߳��֡�could not find or load the Qt platform plugin windows�������ⶼ����Ϊ�Ҳ�����Ҫqt��dll�ļ�������ȡ�����������������ڴ��¸�����������ô���¡�


# qtӦ�ó�������dll�ķ�ʽ

## qt��dll���ӿ���ļ���Ϊ����

+ ��1�֣�����������ʱ���Ҫ���صģ�����Qt5Core.dll�ȣ�
+ ��2�֣���������������Ҫ��ʱ��̬���صģ�����Qt Plugins�ȣ�

## qt��̬����dll ·��������ʽ

��1��(����������ʱ���Ҫ���ص�)qt dll��Ӧ�ó�������Ŀ¼��path��������·��������

## qt��̬����plugins dll ·��������ʽ

��2��(��������������Ҫ��ʱ��̬���ص�)����Ҫ��plugins dll, ��������������ַ����б�`QStringList QCoreApplication::libraryPaths()`�����б���·�����������̬�⡣

QCoreApplication::libraryPaths()��ô�����أ���Դ�������֪���������μ������������ݣ�

1.  
�� QLibraryInfo::location(QLibraryInfo::PluginsPath) ·������
2.  ȷ����Ӧ�ó���������·��QCoreApplication::applicationFilePath()����

3.  ��QT_PLUGIN_PATH ��������ָ����·�����μ���

����QLibraryInfo::location(QLibraryInfo::PluginsPath) ������ô�����أ�����������Ӱ�죺

+ ����з��������� qt.conf �ļ�����ʹ�ø��ļ�������
+ ���û�������� qt.conf �ļ�����ʹ�ñ���Qtʱд��������

д��������ͨ������ֱ�Ӱ�װQt��Ŀ¼���������ڷ����Լ�����������Կ���ͨ���Զ���conf �ļ��޸�Qt �ṩ��һЩ������qt.conf�ļ���ʽ���£�

[Paths]
Prefix = .
Plugins= plugins

PrefixָQtĿ¼��Ŀ¼��ͨ����bin��plugins����Ŀ¼���ֱ�ŵ�1�ֿ��plugins��

��ôqt.confӦ�÷���ʲôλ���أ����ͨ��������������·������conf�ļ���

1. ����Ȼ���qrc��Դ�ļ���Ѱ��:/qt/etc/qt.conf��ʹ����Դϵͳʱ��
2. MAC OS X�У� ��Ӧ�ó���Ŀ¼�µ� ResourceĿ¼�� 
3. Ӧ�ó����ִ���ļ�����Ŀ¼��Ҳ����QCoreApplication::applicationDirPath() + QDir::separator() + "qt.conf"

����Ҫָ����2�ֶ�̬�⣨plugins�ȣ���λ�ã�����ʹ���±߼��ַ�ʽ��

1. ֱ��ʹ��QCoreApplication::addLibraryPath()��QCoreApplication::setLibraryPaths()�޸�QCoreApplication::libraryPaths
2. ��дqt.conf�����ļ�������Ӧ�ó�������Ŀ¼����ϵͳ��Դ�ļ���
3. ����QT_PLUGIN_PATH��������

# pyqt����qt dll����

����PyQtӦ�ó���qt.conf������Ӧ�÷���python.exe��ͬ��Ŀ¼�¡������Ҵ���û�гɹ���������Ϊ�˼򵥣�����ʹ��QCoreApplication.addLibraryPath()��������QT_PLUGIN_PATH��������

�����Լ������pyqt5�о���addlibrarypath�������qt dll·�����������£�

+ ������������qt dll����bin�ļ�����
+ ������plugin dll����plugins�ļ�����
+ ��bin��plugins�ļ��зŵ�pyqt5 python���ļ�����
+ ��pyqt5���__init__.pyβ��������´���

```
import os
dir=os.path.join(os.path.dirname(__file__),'bin;')
os.environ['path']=dir+os.environ['path']

from PyQt5.QtWidgets import QApplication
QApplication.addLibraryPath(os.path.join(os.path.dirname(__file__),"plugins"));
```

�����ڵ���pyqt5��ʱ������ִ��__init__.py��Ҳ����˵�ڵ���pyqt5ǰ������ִ���������룬���ú�qt dll����·����

# C++ qtӦ�ó������qt dll����

�����c++��д��qtӦ�ó��򣬿���ʹ��windeployqt.exe�Զ����������������д�ĳ���Ϊbin\myapp.exe���������������Զ�����������qt dll��

``` bat
windeployqt.exe bin\myapp.exe --release --force --compiler-runtime -libdir bin -dir bin\plugins
```

+ ��1���ļ�(����������ʱ���Ҫ���ص�dll)��ŵ�binĿ¼��
+ ��2���ļ�(��������������Ҫ��ʱ��̬���ص�dll)��ŵ�bin\pluginsĿ¼��

# �ο�����

+ <https://stackoverflow.com/questions/28298507/qt-qcoreapplication-addlibrarypath-use>
+ <https://blog.csdn.net/dbzhang800/article/details/6543489>