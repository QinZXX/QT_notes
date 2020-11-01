## QStackWidget

可以放置多个widget，显示单个，并且可以切换。



对于360ui项目：



mainwindow.h 。 不需要包含QMouseEvent的？

```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QtGui>
#include <QMenu>
#include <QPushButton>
#include <QtCore>

namespace Ui {
class MainWindow;
}

class MainWindow : public QMainWindow
{
    Q_OBJECT
    
public:
    explicit MainWindow(QWidget *parent = 0);
    ~MainWindow();

protected:
    void paintEvent(QPaintEvent *); // 重绘事件
    void mouseMoveEvent(QMouseEvent *); // 鼠标移动
    void mousePressEvent(QMouseEvent *); // 鼠标按下事件
    
private slots:
    void on_menuBtn_clicked(); // 菜单键按下

    void on_pushButton_clicked(); // 电脑体检

    void on_pushButton_2_clicked(); // 木马查杀

    void on_pushButton_3_clicked();

    void on_pushButton_4_clicked();

    void on_pushButton_5_clicked();

    void on_pushButton_6_clicked();

    void on_pushButton_7_clicked();

    void on_pushButton_8_clicked();

    void on_pushButton_9_clicked();



private:
    Ui::MainWindow *ui;

    QPixmap m_pixmapBg; // 设置pixmap
    QAction *m_AactionAboutQt; // 菜单里加的Aciton
    QMenu *m_menu; // 菜单

    QPoint m_pointStart; // 开始点
    QPoint m_pointPress; // 按下 的点

    QVector <QPushButton* > m_vecBtn; // 按钮的vector

    ///初始化;
    void initData();

    ///设置那啥的风格：Normal？;
    void setNomalStyle();

    ///设置当前的widget;
    void setCurrentWidget();
};

#endif // MAINWINDOW_H

```

mainwindow.cpp     具体实现

```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    initData(); // 初始化数据
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    painter.drawPixmap(m_pixmapBg.rect(), m_pixmapBg); // 整个widget绘制好背景
}

void MainWindow::mouseMoveEvent(QMouseEvent *e)
{
    this->move(e->globalPos() - m_pointStart); // 拖动窗口：拖到 全局点-开始点
}

void MainWindow::mousePressEvent(QMouseEvent *e)
{
    m_pointPress = e->globalPos();
    m_pointStart = m_pointPress - this->pos(); // 鼠标按下，更新这个时刻 按压点和开始点

}

void MainWindow::setNomalStyle() // 设置窗口风格
{  
    QFile styleSheet(":/res/qss/style_360.qss");
    if (!styleSheet.open(QIODevice::ReadOnly))
    {
        qWarning("Can't open the style sheet file.");
        return;
    }
    qApp->setStyleSheet(styleSheet.readAll());
}



void MainWindow::on_menuBtn_clicked()
{
    m_menu->exec(this->mapToGlobal(QPoint(700, 20)));//菜单按钮按下，进入消息循环？
}

void MainWindow::initData()
{
    setWindowFlags(Qt::FramelessWindowHint);
    setAttribute(Qt::WA_TranslucentBackground);

    m_menu = new QMenu();
    m_menu->addAction(tr("第一个"));
    m_menu->addAction(tr("第二个"));
    m_menu->addAction(tr("第三个"));
    m_menu->addSeparator();
    m_menu->addAction(tr("第四个"));
    m_menu->addAction(tr("第五个了啊"));
    m_menu->addAction(tr("360 第六个"));
    m_menu->addAction(tr("第七个这是嗯嗯"));
    m_menu->addAction(tr("这已经是第八个了"));
    m_AactionAboutQt = new QAction(tr("&About Qt"), this);
    connect(m_AactionAboutQt, SIGNAL(triggered()), qApp, SLOT(aboutQt())); // QT4风格
    m_menu->addAction(m_AactionAboutQt);

    ///加载那啥图，frame图;
    m_pixmapBg.load(":/res/image/frame.png");

    m_vecBtn.push_back(ui->pushButton);
    m_vecBtn.push_back(ui->pushButton_2);
    m_vecBtn.push_back(ui->pushButton_3);
    m_vecBtn.push_back(ui->pushButton_4);
    m_vecBtn.push_back(ui->pushButton_5);
    m_vecBtn.push_back(ui->pushButton_6);
    m_vecBtn.push_back(ui->pushButton_7);
    m_vecBtn.push_back(ui->pushButton_8);
    m_vecBtn.push_back(ui->pushButton_9); // 按钮全部加到QVector

    for (int i = 0; i != m_vecBtn.size(); ++i)
    {
        ///每个按钮设置可check 和 自动exclusive;
        m_vecBtn[i]->setCheckable(true); // 设置标记和切换
        m_vecBtn[i]->setAutoExclusive(true); // 自动排他性？每次只能有一个按钮是在那个check状态
    }

    ///这就是那个像进度条的QLabel，按下后跳转到那个360官网首页;
    ui->label_checkUpdate->setText(
                tr("<a href=http://www.360.cn>"
                   "<font color=white>进入官网</font></a>"));
    ui->label_checkUpdate->setOpenExternalLinks(true); // 打开超链接到浏览器的设置

     setNomalStyle(); // 设置风格样式
}


void MainWindow::on_pushButton_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_2_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_3_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_4_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_5_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_6_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_7_clicked()
{
    setCurrentWidget();
}

void MainWindow::on_pushButton_8_clicked()
{
     setCurrentWidget();
}

void MainWindow::on_pushButton_9_clicked()
{
    setCurrentWidget();
}

void MainWindow::setCurrentWidget() // 在QStackWidget中设置当前的显示控件
{
    for (int i = 0; i != m_vecBtn.size(); ++i)
    {
        if ( m_vecBtn[i]->isChecked() )
        {
            ui->stackedWidget->setCurrentIndex(i);// 设置第i个为显示控件
        }
    }
}
```

