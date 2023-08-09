---
layout: post
title:  Python GUI 만들기
date:   2023-02-12 00:00
image:  
tags:   GUI
---

###### python을 이용하여 어플리케이션을 만들기 위해 PyQT5 라이브러리를 사용하였다.
```py
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtCore import QCoreApplication
from PyQt5.QtGui import QIcon, QFont


class MyApp(QWidget):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        # 툴팁의 폰트, 크기 설정
        QToolTip.setFont(QFont('SansSerif', 10))

        # 버튼 만들기
        btn_quit = QPushButton('Quit', self)
        test = QPushButton('test',self)
        # 버튼의 툴팁 설정
        btn_quit.setToolTip('<b>창을 닫아주는 버튼</b> widget')
        # # 버튼의 절대적 위치 지정
        # btn_quit.move(50, 50)
        # 버튼이 적절한 크기로 유지되도록 설정: (sizeHint())
        btn_quit.resize(btn_quit.sizeHint())
        btn_quit.clicked.connect(QCoreApplication.instance().quit)


        # 수평 박스를 만들고 버튼이나 빈 공간을 추가한다.
        hbox = QHBoxLayout()
        # 두 버튼 양쪽의 stretch factor가 1로 같기 때문에 이 두 빈 공간의 크기는 창의 크기가 변화해도 항상 같습니다.
        hbox.addStretch(1)
        hbox.addWidget(btn_quit)
        hbox.addWidget(test)
        hbox.addStretch(1)
        # 수직 박스를 만든다 
        # 위에서 만들었던 수평 박스를 수직 박스에 넣는다.
        vbox = QVBoxLayout()
        vbox.addStretch(3)
        vbox.addLayout(hbox)
        vbox.addStretch(1)
        # 최종적으로 수직 박스를 창의 메인 레이아웃으로 설정합니다.
        self.setLayout(vbox)

        #창 제목 
        self.setWindowTitle('My First Application')
        # 창의 아이콘 생성
        self.setWindowIcon(QIcon('web.png'))
        # 창의 위치와 사이즈 설정(x,y,width,height)
        self.setGeometry(300, 300, 300, 200)
        self.center()
        self.show()

    def center(self):
        # 창의 위치와 크기 정보를 가져온다.
        qr = self.frameGeometry()
        # 모니터의 center를 파악
        screen_center = QDesktopWidget().availableGeometry().center()
        # 창의 직사각형 위치를 모니터의 center로 이동
        qr.moveCenter(screen_center)
        # 창을 화면 중심으로 이동했던 직사각형의 위치로 이동시킨다.
        self.move(qr.topLeft())

#'__name__'은 현재 모듈의 이름이 저장되는 내장 변수입니다.
# 만약 'moduleA.py'라는 코드를 import해서 예제 코드를 수행하면 __name__ 은 'moduleA'가 됩니다. 
# 그렇지 않고 코드를 직접 실행한다면 __name__ 은 __main__ 이 됩니다. 
# 따라서 이 한 줄의 코드를 통해 프로그램이 직접 실행되는지 혹은 모듈을 통해 실행되는지를 확인합니다.
if __name__ == '__main__':
    # 어플리케이션 객체를 생성
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())


```